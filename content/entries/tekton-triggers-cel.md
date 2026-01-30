---
title: "Tekton Triggers CEL"
date: 2022-02-27T14:49:03+01:00
lastmod: 2022-02-27T14:49:03+01:00
description: "Master Tekton Triggers with Common Expression Language (CEL) for advanced webhook processing. Learn how to use CEL interceptors to manipulate git references and automate pipeline triggers in Kubernetes."
summary: "Use CEL interceptors in Tekton Triggers to transform git references and automate webhook-based pipeline execution."
tags: ["Tekton", "CEL", "Kubernetes", "CI/CD", "Webhooks", "GitLab", "GitHub", "Automation"]
topics: ["Tekton", "CI/CD", "Kubernetes", "Automation"]
category: "DevOps & Infrastructure"
author: "Marco Paga"
keywords: "Tekton Triggers, CEL, Common Expression Language, Kubernetes, webhooks, git automation, interceptors, pipeline automation"
---
_or how to convert a git ref to a branch name_

[Tekton Triggers](https://github.com/tektoncd/triggers) is a Tekton project which allows to receive webhooks and react appropriately to those. You can follow along [my article on the codecentric blog](https://blog.codecentric.de/en/2022/03/tekton-triggers-in-practice/) to see what is possible.

Tekton triggers includes a conecpt which is called [Interceptors](https://github.com/tektoncd/triggers/blob/main/docs/interceptors.md) that provide a way to implement cross cutting concerns. 
To give you a quick example: If you integrate Github with Tekton you hopefully want to validate if __your__ project is calling the tekton installation since you don't want anybody stumbling on your tekton installation to run pipelines. How hard can it be? Just have a look at the [documentation](https://docs.github.com/en/developers/webhooks-and-events/webhooks/securing-your-webhooks) and see that you need to exchange secrests and need to run HMAC on the encoded request. Long story short: Just integrate the [GitHub](https://github.com/tektoncd/triggers/blob/main/docs/interceptors.md#github-interceptors) interceptor and you are done.

And now another simple problem. When gitlab triggers a webhook the branch name is included in the request but contained in a git reference. This is great but what if I just need the branch name?
One way to handle it is to create a task that gets the reference as a parameter and returns just the branch name as an out parameter. This is quite a big overhead so maybe there is a simpler solution.

## Common Expression Language Interceptor

The common expression language is a simple expression language based on protobuf types. The implementation used in the context of tekton is the [go implementation called cel-go](https://github.com/google/cel-go/).
If you integrate the CEL interceptor in the EventListener you can manipulate the request and find the results in the trigger bindings in a field called __extensions__.

Find a short example here where I configured the interceptor to return the branch name:

```
 triggers:
    - name: gitlab-push-events-trigger
      interceptors:
        - name: "verify-gitlab-payload"
          ref:
            name: "gitlab"
            kind: ClusterInterceptor
          params:
            - name: secretRef
              value:
                secretName: "gitlab-secret"
                secretKey: "secretToken"
            - name: eventTypes
              value:
                - "Push Hook"
        - ref:
            name: cel
          params:
            - name: "overlays"
              value:
                - key: branch_name
                  expression: "body.ref.split('/')[2]"
      bindings:
        - name: git-revision
          value: $(body.checkout_sha)
        - name: git-repository-url
          value: $(body.repository.git_http_url)
        - name: git-branch
          value: $(extensions.branch_name)

```

The branch is extracted in the last two lines so these can be sent over to the triggered pipeline.

## Recap

As you can see we simply changed a request field to make it easily consumable from our pipeline.
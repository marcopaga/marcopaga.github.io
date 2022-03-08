---
title: "Gitops"
date: 2022-03-08T18:17:03+01:00
draft: true
---

# GitOps
## Was ist GitOps?
GitOps betrachtet den Auslieferungsprozess von Software auf den verschiedenen Stages von einer Entwicklungsumgebung bis hin zu einer produktiven Umgebung. Wie der Name schon vermuten lässt, wird git als Versionskontrollsystem verwendet und bildet damit die revisionssichere Historie ab. Alle Änderungen werden über diesen nachvollziehbaren Weg in eine Umgebung gebracht. Weiter gedacht wird die Umgebung an sich genau so erstellt und verwaltet.
## Warum?
Agile Softwareentwicklung und Lean Ansätze im allgemeinen Leben von einem schnellen Feedbackzyklus. Anforderungen werden umgesetzt und schnell in die produktive Nutzung überführt. so dass wir aus der Benutzung lernen können. Daher soll die Auslieferung von Software so einfach wie möglich erfolgen können. Wie kann man das erreichen?



## Was war nochmal Infastructure as Code?
Ein Ansatz der schon lange dafür genutzt werden kann ist Infrastructure as Code oder kurz IaC. Die Infrastruktur, dazu gehören beispielsweise virtuelle Maschinen, Cloud Funktionen und weitere Services genauso wie auch die auszuführende Software, wird in einer Tool-spezifischen Beschreibungssprache definiert. Diese Beschreibung wird interpretiert um den gewünschten Zielzustand herzustellen.
Zur Kategorisierung dieser Tools sind zwei Dimensionen nutzbar. 
Eine Dimension ist die Unterscheidung zwischen Push und Pull basierten Tools und zur weiteren Unterscheidung die prozedural arbeitenden Tools und beschreibenden Tools sinnvoll.
Push-basierte Tools wie z.B. Terraform und Ansible werden durch einen Nutzer oder Prozess aktiv gestartet um den aktuell definierten Stand auf den definierten Zielumgebungen herzustellen. Pull-basierte Werkzeuge hingegen sind ständig aktiv und werden einen neuen Zielzustand herstellen wenn dieser bekannt gemacht wurde; ein Beispiel hierfür ist Chef.
Jetzt zum nächsten Unterscheidungsmerkmal und zwar geht es dabei um die Herstellung des Zielzustands, denn auch hierbei gibt es zwei unterschiedliche Herangehensweisen. 
Ansible beschreibt prozedural den Weg zum Zielzustand wohingegen Terraform den Zielzustand deklarativ beschreibt. Bei Ansible schreibe ich - stark vereinfacht dargestellt - idempotente Kommandos welche Schritt für Schritt Tools ausführen und den Status des Systems beeinflussen.
Für Terraform definiere ich hingegen in der Beschreibungssprache ein System und das Tool erstellt beim Aufruf die Sicht auf den aktuellen Zustand und bildet eine Differenz zu dem gewünschten Zielzustand. Dieser Unterschied zum Zielzustand wird dann durch das Tool angepasst.
Die Versionierung des IaC Codes ist in der Praxis normal und gehört zu einem professionell gepflegten Softwareprojekt dazu genauso wie es normal ist den Sourcecode an sich in git zu hinterlegen. Auf diese Weise erhalten wir im Zusammenspiel ein reproduzierbares Deployment der Anwendungen von der Entwicklungsumgebung bis zur produktiven Umgebung wenn alles über IaC Tools abgebildet ist und manuelle Eingriffe nicht vorkommen.

## GitOps
Nachdem wir uns einen Überblick der IaC Tools verschafft haben können wir jetzt mit einem gemeinsamen Verständnis einen Blick auf GitOps werfen. Wie der Name schon sagt wird Git als Versionskontrollsystem (VCS) hier eingesetzt, wobei git an der Stelle auch durch andere VCS genutzt werden können wenn man bereit ist das Tooling selbst zu bauen. Ops meint in diesem Fall die Bereitstellung der benötigten Infrastruktur und Software für einen Endnutzer.
In Summe haben wir damit eine versionierte Deploymentumgebung mit der dazugehörigen Software.
Entstanden und gewachsen ist diese Idee im Kubernetes Umfeld. Kubernetes arbeitet mit Beschreibungen des Zielzustandes, so dass alles innerhalb eines Clusters beschrieben wird und Operator-Komponenten im Hintergrund den gewünschten Zustand anlegen. Damit haben wir die erste Gemeinsamkeit zu deklarativen IaC Tools. Da GitOps Tools selbst in Kubernetes beheimatet sind ist keine konzeptuelle Lücke zwischen dem Tool und der Umgebung vorhanden denn diese sind homogen eingebettet.
Allgemein arbeiten die GitOps Tools pull basiert und rollen die Änderungen automatisch aus. Eine Kontrolle des Roll-Outs erfolgt in diesem Fall durch das Branch Management innerhalb des Git Repositories welche zum Beispiel durch Branch-Permissions und Pull-Requests mit Qualitätschecks sicher gestellt werden können. Auf diese Weise ist es möglich ein qualitätsgesichertes, reproduzierbares und nachvollziehbares Deployment zu leben.

## Flux CD
FluxCD ist ein CNCF Incubating Projekt das einen GitOps Kubernetes Operator bereitstellt, den ich gerne im folgenden nutzen möchte.
Vorweg eine Einordnung der Historie des Projekts weil es sonst bei einer Websuche leicht zur Verwirrung kommen könnte.
## Flux Version 2
Mit Argo CD, ebenfalls ein GitOps Tool, wurde  Ende 2019 eine Kooperation bekannt gegeben, daher hatte ich vermutet, dass diese beiden Tools verschmelzen würden. Das ist nicht passiert jedoch haben FluxCD und Argo  CD gemeinsam an einem zentralen Bestandteil und zwar der GitOps-Engine[FN](https://github.com/argoproj/gitops-engine) gearbeitet. Das damit verbundene Ziel dieses Projekts war die Gemeinsamkeiten der verschiedenen GitOps Tools einmal zu implementieren und diese Engine in den Tools zu verwenden. Das Flux Team hat sich im Laufe des Projekts gegen dessen Verwendung entschieden uns stattdessen das GitOps Toolkit implementiert und in Flux eingesetzt. Bei genauer Betrachtung der Flux Manifeste findet man Referenzen auf „gotk“ womit das „GitOps ToolKit“ gemeint ist. Das FluxCD Projekt hat stattdessen die wichtigsten Erkenntnisse aus diesem Projekt genutzt um Flux 2 von Grunde auf neu zu implementieren.
Wichtig hervorzuheben ist die Neuimplementierung der Version 2, denn bei einer Websuche nach Flux gelangt man erratisch zu Beiträgen die sich auf die Vorversion beziehen die mich immer wieder in die Irre geleitet haben.
## Komponenten
Die Hauptkomponente ist der Flux Operator, der in einem Kubernetes Cluster deployed wird. Dieser Operator ist eine aktive Komponente und agiert im Rahmen des Kubernetes Clusters mit den zugewiesenen Berechtigungen, wie jeder andere Workload. Der Operator wird bei der Installation mit einem Git-Repository verknüpft, so dass die Konfiguration innerhalb des Clusters angewendet wird. Die Verknüpfung bezieht sich auf ein Verzeichnis in einem Branch der den Zielzustand definiert.
Genau gesagt handelt es sich bei Flux um eine Sammlung von Controllern die zusammenarbeiten um die Aufgabe umzusetzen. Dies entspricht dem Kubernetes Konzept und ist auch bei den verschiedenen Kernkomponenten so zu umgesetzt. Zu nennen sind der source-controller, notification-controller, kustomize-controller und helm-controller. 
Interessant zu erwähnen ist auch noch, dass die Flux Installation an sich ebenfalls automatisch in dem Git Repository abgelegt wird. Durch diesen einfachen Schritt ist die Installation des Tools an sich ebenfalls nachvollziehbar abgelegt und entspricht damit auch den GitOps Prinzipien.
## Konzepte
An dieser Stelle ist ein Blick auf die grundlegenden Konzepte von Flux wichtig um zu verstehen wie Flux arbeitet und zu konfigurieren ist. 
Das wichtigste Element ist ein `Source` das genutzt wird um die Quelle für ein Deployment zu definieren. Diese Quellen werden periodisch von Flux angefragt und auf Änderungen überprüft.
Das kann, wenig überraschend, ein `GitRepository`sein und darüber hinaus auch noch ein `HelmRepository`und ein `Bucket`sein.
Ein neuer Git-Commit auf dem Quellbranch wird automatisch erkannt und verarbeitet. Wenn ein `HelmRepository` konfiguriert ist wird Flux es im Rahmen der Konfiguration überwachen und je nach konfiguriertem Versionsparameter kann Flux sogar eine neue Version automatisch nutzen und ausrollen.
Ein weiteres Element ist eine `Kutomization` die auch in Form einer CRD abgebildet ist, welche verwendet wird um zu deployende Ressourcen zu definieren. Diese Kustomizations werden regelmäßig überprüft und automatisch in den Zustand des GitRepositories überführt falls eine Abweichung eingetreten ist.
## „Hello, World!“
Zum Start schauen wir uns ein einfaches Beispiel an: Das Ziel ist es eine Applikation über einfache Kubernetes Manifeste in ein Cluster zu deployen und mit Flux zu aktualisieren.
![](Paga-Gitops-Abbildung%201.png)
Was brauchen wir dafür?
1) Git Repository
Es bietet sich an ein öffentliches GitHub Repository für einen ersten Test zu verwenden. Der Zugriff auf das Cluster muss für die Flux Installation möglich sein um auch Änderungen wieder zurückzuspielen, daher muss für das Repository ein Personal Access Token (PAT) mit den Scopes `repo_status`und`public_repo`angelegt werden. Das erstellte Token werden wir für das Bootstrapping verwenden.
2) Kubernetes Cluster
Für einen ersten lokalen Test ist ein k3d Cluster völlig ausreichend. Die Installation erfolgt z.B. mit `brew install k3d`. Nach der Installation ist es einfach mit einem Kommando ein Cluster mit einer Portfreigabe zu erstellung und zu starten: `k3d cluster create -p "8081:80@loadbalancer" --agents 2`.
3) Flux CLI
Als letztes benötigen wir noch die Flux CLI die ebenfalls über ein Befehl installierbar ist:  `brew install fluxcd/tap/flux`

Jetzt haben wir erstmal ein leeres GitHub-Repository erstellt und ein Cluster ohne Workload erstellt. Jetzt können wir diese drei Elemente durch einen Flux Bootstrap verbinden. Dies sorgt für die Installation von Flux in dem lokalen k3d Cluster mit einer Verknüpfung zum angegebenen GitHub Repository.

```bash
GITHUB_TOKEN=YOUR_PAT_TOKEN flux bootstrap github \
```
  --owner=OWNER
  --repository=REPOSITORY_NAME_
  --branch=main \\
  --private=false \\
  --personal=true \\
  --token-auth \\
  --path=clusters/local`
`
Im Cluster sind in dem Namespace `flux-system` schon die Controller aufgetaucht und aktiv.
Schau mal in das GitHub Repository nachdem das Kommando durchgelaufen ist und wundere dich nicht, dass da in dem Verzeichnis `clusters/local/flux-system` Dateien aufgetaucht sind. Diese beschreiben die Flux Installation und sind für dich der Startpunkt um die eigenen Installationen vorzunehmen.
Bei einem Blick in die Dateien sieht man sehr gut wie alles aufgebaut ist. Die Datei `kustomization.yaml` ist der Einsprungspunkt und referenziert nacheinander die Dateien `gotk-components.yaml` und `gotk-sync.yaml`aus dem gleichen Verzeichnis.
Die Datei `gotk-components.yaml`beinhaltet alle Elemente der Flux Installation angefangen vom Namespace, über CRDs und NetworkPolicies bis hin zu Controller-Deployments.
Die Datei `gotk-sync.yaml` nutzt CRDs aus der ersten Datei und referenziert das oben angegebene GitRepository. Darüber hinaus ist dort die erste `Kustomization` zu finden die ausgewertet wird und zwar referenziert diese das `clusters/local` Verzeichnis und erlaubt uns damit dort unsere Konfiguration abzulegen.

Deployment anlegen
Irgendwann mal zum Ende kommen

## Ausblick

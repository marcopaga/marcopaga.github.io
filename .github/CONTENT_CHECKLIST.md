# Content Quality Checklist

Use this checklist when creating or updating blog posts and articles to ensure AI Engine Optimization (AEO) and SEO best practices.

## Frontmatter Completeness

**Required Fields:**
- [ ] `title` - Clear, descriptive title (50-60 characters ideal)
- [ ] `date` - Publication date in ISO format
- [ ] `lastmod` - Last modified date (update when content changes)
- [ ] `description` - Unique 120-160 character summary for meta tags
- [ ] `summary` - Compelling 1-2 sentence overview
- [ ] `author` - Author name (default: "Marco Paga")

**Recommended Fields:**
- [ ] `tags` - 3-8 relevant tags (e.g., "Kubernetes", "GitOps", "DevOps")
- [ ] `topics` - Main subject areas (2-4 topics)
- [ ] `category` - Content category (e.g., "DevOps & Infrastructure", "Community & Content")
- [ ] `keywords` - SEO keywords (5-10 comma-separated terms)
- [ ] `image` - Featured image path (if applicable)

**Validation:**
- [ ] Description is unique (not copied from other pages)
- [ ] Tags are relevant and consistent with site taxonomy
- [ ] Keywords match actual content topics

## Content Quality

**Structure:**
- [ ] Clear H1 heading (automatically generated from title)
- [ ] Logical heading hierarchy (H2, H3, H4)
- [ ] Table of contents for long articles (use `{{<tableOfContents>}}` shortcode)
- [ ] Introductory paragraph explaining what the article covers
- [ ] Conclusion or summary section

**Readability:**
- [ ] Paragraphs are 2-4 sentences max
- [ ] Complex topics broken into sections
- [ ] Code examples are syntax-highlighted and explained
- [ ] Lists and bullet points used for scannability
- [ ] Technical jargon explained or linked to definitions

**Links & References:**
- [ ] External links to authoritative sources (CNCF, official docs, etc.)
- [ ] Internal links to related blog posts
- [ ] All links use descriptive anchor text (not "click here")
- [ ] External links open in same tab (standard web behavior)

## E-E-A-T Signals (Experience, Expertise, Authoritativeness, Trustworthiness)

**Experience:**
- [ ] First-hand experience or practical examples included
- [ ] Real-world implementation details or case studies
- [ ] Specific outcomes or results mentioned

**Expertise:**
- [ ] Author credentials relevant to topic
- [ ] Technical depth appropriate for subject matter
- [ ] Demonstrates understanding of best practices

**Authoritativeness:**
- [ ] Citations to official documentation or specifications
- [ ] References to industry standards (CNCF, W3C, etc.)
- [ ] Links to authoritative sources (not just blog posts)
- [ ] Statistics or data with sources

**Trustworthiness:**
- [ ] Accurate publication and modification dates
- [ ] No broken links
- [ ] Consistent author attribution
- [ ] Contact information available (via About page)

## AI Optimization

**Structured Data:**
- [ ] Frontmatter fields complete (enables JSON-LD generation)
- [ ] Unique meta description (not site-wide default)
- [ ] Keywords/tags defined for better categorization

**Content Clarity:**
- [ ] Clear topic focus (AI agents should understand main subject)
- [ ] Specific technical terms used (not vague language)
- [ ] Actionable information or takeaways provided
- [ ] Questions answered directly (good for Q&A-style AI responses)

**Citation-Worthiness:**
- [ ] Original insights or perspectives included
- [ ] Verifiable facts with sources
- [ ] Practical examples or code snippets
- [ ] Timeless content or clearly dated content (AI can determine relevance)

## Technical Validation

**Before Publishing:**
- [ ] Run `hugo server -D` locally to preview
- [ ] Check that all images load correctly
- [ ] Verify code blocks are properly formatted
- [ ] Test all external links
- [ ] Review meta description in page source
- [ ] Validate frontmatter YAML syntax

**After Publishing:**
- [ ] Verify page appears in sitemap.xml
- [ ] Check structured data with [Google Rich Results Test](https://search.google.com/test/rich-results)
- [ ] Validate Schema.org markup at [schema.org validator](https://validator.schema.org/)
- [ ] Test Open Graph preview with [OpenGraph.xyz](https://www.opengraph.xyz/)

## Content Freshness

**For New Content:**
- [ ] Set `date` to current date
- [ ] Set `lastmod` to current date
- [ ] Set `draft: false` when ready to publish

**For Updated Content:**
- [ ] Update `lastmod` to current date
- [ ] Add update note if significant changes made
- [ ] Review all links for currency
- [ ] Update statistics or data points if outdated
- [ ] Verify technical instructions still accurate

## Performance & Sustainability

**Following Site Principles:**
- [ ] No third-party scripts added (maintain zero 3rd-party requests)
- [ ] Images optimized and compressed
- [ ] No unnecessary JavaScript
- [ ] Minimal CSS additions (use theme styles)
- [ ] Content follows site performance budget

## Pre-Commit Checklist

- [ ] All required frontmatter fields present
- [ ] Unique description written
- [ ] Tags and topics assigned
- [ ] Content reviewed for quality and accuracy
- [ ] Links verified and working
- [ ] Hugo build succeeds locally
- [ ] No spelling or grammar errors

## Resources

- **Hugo Documentation:** https://gohugo.io/documentation/
- **Schema.org Types:** https://schema.org/
- **Google Rich Results Test:** https://search.google.com/test/rich-results
- **Open Graph Debugger:** https://www.opengraph.xyz/
- **Sitemap Validator:** https://www.xml-sitemaps.com/validate-xml-sitemap.html

---

**Last Updated:** 2026-01-30

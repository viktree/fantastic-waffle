---
title: "Etsy's Take on Documentation (old)"
date: "2018-10-29"
draft: true
---

This week, my team and me are busy polishing our documentation. These are the markdown files in the `docs/` folder of our git repo and the confluence pages for communicating across teams. These are the functional specs that people reference daily and the SRE Playbook entries we pray we don’t need to look at. We have documentation in all shapes and sizes. When there is so much stuff to manage, making sure that documentation doesn’t become stale or irrelevant can easily become cumbersome.

A little while back, Etsy’s engineering blog “Coding as a Craft” illuminated Etsy manages their documentation. Their unusual method of managing, came about when their engineers stumbled upon an interesting insight: **As the system they work with changes, the operational details and technical composition change with it. However, the reasons behind the decisions that took place along the way are far less likely to become irrelevant.**

Before Etsy employees write docs, they try and think about the type of questions the document will answer. Etsy has two main types of documentation: The WHY-docs and the HOW-docs.

The WHY-docs are things like design decisions, choice of libraries and framework explanations. Like at most other companies, this information is stored as README.md files.

What is interesting is how the HOW-docs are managed. Some of these questions might be “How do I perform a rollback on the latest deployment?” or “How should I test my code?”. Unlike their counterparts, the answers of the questions posed by HOW-docs tend to be shorter tidbits of information. Similar to what you would find on a site like stack-overflow. Hence, Etsy decided to use the slack channeler bot tag posts that answers HOW-doc type questions. The Etsy engineers can then search for these tags and get the most up to date information bubbling up in their slack.

While I don’t think the slack-channeler solution for managing HOW-docs is a suitable one for the way ecobee operates, I have found value in separating these two types of questions. When writing docs, I keep thinking about what parts of the document answer how questions and what parts of the document answers why questions. I try to supplement the operational details with reasons why. For example, last Friday I wrote an onboarding document on how to get setup with `gcloud` and `kubectl`. While writing, I realised that the commands for installing `kubectl` and hooking it up with our GKE cluster might change. Yet, why the steps are there will still be relevant. So a year from now, if our tooling changes and I revisit the document, it will be easier for me to decide what parts of the document need to change to maintain its freshness.

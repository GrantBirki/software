---
title: "Software: How to build it"
author: "Grant Birkinbine"
description: "A high level software design manifesto on how to effectively build production software, deploy it, and maintain it."

# The summary is for search engines
summary: A high level software design manifesto on how to effectively build production software, deploy it, and maintain it.

date: 2025-02-11T00:00:00-00:00 # date of creation

# disableAnchoredHeadings: false

draft: false

ogTitle: Grant Birkinbine
ogDescription: "Software: How to build it"
ogImage: /default.png # comment this out to force an SVG generation and usage then you can run `script/images` to make the SVG become a PNG and use it here
---

## Introduction

This document is a high level software design manifesto on how to effectively build production software, deploy it, and maintain it. It is a living document, and an opinionated one. It is strongly inspired by the core engineering principles of [GitHub](https://github.blog/engineering/engineering-principles/).

There are four key principles that this document will discuss:

1. [**Communication**](#communication)
2. [**Project Architecture**]((#project-architecture))
3. [**Deployment**](#deployment)
4. [**Maintenance**](#maintenance)

## Communication

Effective communication is the foundation for all other pillars of software engineering. This section will go over the key principles of communication in the context of software engineering. They are adapted from [*How GitHub Engineering communicates*](https://github.com/github/how-engineering-communicates/blob/bf86e6b5e397e0122280b4881652248a10aebb80/quick-ref.md).

### 1. Be Asynchronous First

Working asynchronously[^4] is a key part of any software engineering team. It allows for more flexibility in work schedules, for teams to be more inclusive of different time zones, and for people to have more time to work uninterrupted. Interrupt work is a major productivity killer due to context switching. Additionally, it allows for more thoughtful and well-considered responses to questions and issues.

Asynchronous mediums necessitate a distributed workflow and those mediums generally lead to durable artifacts which leads to our next principle.

### 2. Everything should have a URL

URLs are the foundation of the internet. Having a permalink to a converation, decision, pull request, snippet of code, or any other artifact used to build software is a key part of effective communication. Having URLs that are durable is the lifeblood of a distributed team that builds software. It allows teams to build and collaborate asynchronously.

Ensure that the tools you adopt for communication have durable URLs. For example, Slack threads are not durable, but a link to a GitHub issue is.

> "*While it’s certainly hard to make the case that absolutely every decision should be rigorously memorialized, it’s even harder to make the case that your organization would be better off if they weren’t.*" - Ben Balter

Ben Balter's blog post [Why URLs](https://ben.balter.com/2015/11/12/why-urls/) goes into more detail on this principle.

### 3. Work in the Open

### 4. Write it Down

### 5. Collaborate

### 6. Communicate Openly, Honestly, and Authentically

## Project Architecture

### 1. Scripts to Rule Them All

Projects should follow the [**scripts to rule them all**](https://github.com/github/scripts-to-rule-them-all) pattern. This ensures a consistent set of scripts for tasks like testing, building, and deploying any project[^1]. Developers don't need to remember the specific commands for each project as the scripts are consistent across all projects.

### 2. Language Version Managers

Developers should use *shim*-based language version managers[^3]. At a high level, these version managers intercept commands using shim executables injected into your `PATH`, determines which version has been specified by your application, and passes your commands along to the correct language installation.

All projects should use a corresponding `.<lang>-version` file at the root of the repository so that all developers are using the same version of the language.

For example, a Ruby project should have a `.ruby-version` file at the root with a value such as `3.3.0` inside of it. When a developer enters the project directory, the version manager will automatically switch to the correct version of Ruby.

Here are some examples of *shim*-based language version managers: [rbenv](https://github.com/rbenv/rbenv), [pyenv](https://github.com/pyenv/pyenv), [goenv](https://github.com/go-nv/goenv), [tfenv](https://github.com/tfutils/tfenv), [nodenv](https://github.com/nodenv/nodenv), [asdf](https://asdf-vm.com/).

### 3. Vendor Dependencies

### 4. Build Systems are Production Systems

## Deployment

## Maintenance

[^1]: A software project that could be a library, service, or application. This term will be interchanged with "repository" often in this document.
[^2]: The *live* environment of an application or service. A production environment is where the final product is delviered to the end user. This could be a website, mobile app, api, etc.
[^3]: Language version managers use a directory of shims at the front of your `PATH` - Read more about it [here](https://github.com/nodenv/nodenv/blob/8948584145f2ce1853967337c91f2e09996aa1c3/README.md?plain=1#L64-L104).
[^4]: Asynchronous communication is any kind of communication where there is a delay between the information being provided by the sender and the time when the recipient accepts the information and acts on it.

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
2. [**Project Architecture**](#project-architecture)
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

Ensuring that the work you do is open and *visible* to the rest of the team is crucial. In the context of software engineering, this doesn't mean that you need everything to be public in the sense of *open source* but rather that other members of your organization can discover your work. This means using public Slack channels, pushing your code frequently, and opening pull requests early in the development process to get feedback.

Working in private channels, excluding team members from conversations, or working in a silo can lead to duplicated work, or worse, work that is not in line with the rest of the team's goals.

Not working in the open prevents others from learning about your work, and prevents you from learning about theirs.

Fostering a culture of working in the open leads to a more inclusive team and one that supports transparency and collaboration.

### 4. Write it Down

When used in combination with the [**Everything should have a URL**](#2-everything-should-have-a-url) and [**Work in the Open**](#3-work-in-the-open) principles, writing things down becomes incredibly powerful. Writing things down allows for a durable artifact to be created that can be referenced at any time. This helps to reduce the amount of cognitive load that is required to remember every detail and instead only focus on the most important ones. If you need to remember something, write it down and then you can forget about it because you know where to find it via durable URLs.

### 5. Collaborate

When writing software, it is important to remember that you are not working in a vacuum. You will either have team members directly working on the same project as you, other teams that will interact with your project, consumers of your project, open source contributors, or all of the above. Collaboration allows for others to bring their unique perspectives to the table and will ultimately lead to a better product.

When you collaborate, you are opening the door to:

- **Feedback**: This can be in the form of code reviews, design reviews, or any other form of feedback.
- **Questions**: Questions often have the side effect of leading to better documentation.
- **Learning**: You will learn from the people you collaborate with, and they will learn from you.
- **Preventing Silos**: Collaboration helps to prevent silos from forming. Silos are dangerous because they can lead to duplicated work or complete gaps in knowledge that makes it challenging for others to contribute/maintain/troubleshoot to your work.

When collaboration is used in combination with the [**Write it Down**](#4-write-it-down), [**Work in the Open**](#3-work-in-the-open), [**Everything should have a URL**](#2-everything-should-have-a-url), and [**Be Asynchronous First**](#1-be-asynchronous-first) principles, it becomes incredibly powerful. It allows for a team to work asynchronously, for that work to be durable/discoverable, and for all members of the team to be able to contribute to the conversation.

A North Star example would be linking every pull request you are working on in a public Slack channel for your team. This allows for other members on the team to see your work, optionally review it, and provide feedback. It also allows for members to contribute in an asynchronous manner that gaps time zones. A bonus of this example is that it also creates a timeline of the work you have done as you go so others can follow along and be get caught up on the work that is in flight and serve as a changelog. By doing this, you are working in the open, writing it down, and allowing for collaboration to happen - all at the same time.

### 6. Communicate Honestly and Authentically

Say what you mean, in a respectful way, and listen to understand each other ([ref](https://github.com/github/how-engineering-communicates/blob/bf86e6b5e397e0122280b4881652248a10aebb80/how-github-engineering-communicates.md?plain=1#L64)). Building software is a team sport and it is important to remember that we are humans working with other humans. If you think something is a bad idea, say so, but do it in a respectful way. Foster a culture of psychological safety where everyone feels comfortable sharing their ideas and opinions.

Also remember that it is okay to say "I don't know" or "I need help". No one knows everything and it is important to ask for help when you need it. This is a sign of strength, not weakness.

Emoji and animated GIFs are the facial expressions and body language of online writing - use them.

## Project Architecture

### 1. Scripts to Rule Them All

Projects should follow the [**scripts to rule them all**](https://github.com/github/scripts-to-rule-them-all) pattern. This ensures a consistent set of scripts for tasks like testing, building, and deploying any project[^1]. Developers don't need to remember the specific commands for each project as the scripts are consistent across all projects. Using this pattern allows for Language Version Managers to work quite effectively, which is the next principle.

### 2. Language Version Managers

Developers should use *shim*-based language version managers[^3]. At a high level, these version managers intercept commands using shim executables injected into your `PATH`, determines which version has been specified by your application, and passes your commands along to the correct language installation.

All projects should use a corresponding `.<lang>-version` file at the root of the repository so that all developers are using the same version of the language.

For example, a Ruby project should have a `.ruby-version` file at the root with a value such as `3.3.0` inside of it. When a developer enters the project directory, the version manager will automatically switch to the correct version of Ruby.

Here are some examples of *shim*-based language version managers: [rbenv](https://github.com/rbenv/rbenv), [pyenv](https://github.com/pyenv/pyenv), [goenv](https://github.com/go-nv/goenv), [tfenv](https://github.com/tfutils/tfenv), [nodenv](https://github.com/nodenv/nodenv), [asdf](https://asdf-vm.com/).

### 3. Vendor Dependencies

### 4. Build Systems are Production Systems

### 5. Testing

## Deployment

## Maintenance

[^1]: A software project that could be a library, service, or application. This term will be interchanged with "repository" often in this document.
[^2]: The *live* environment of an application or service. A production environment is where the final product is delviered to the end user. This could be a website, mobile app, api, etc.
[^3]: Language version managers use a directory of shims at the front of your `PATH` - Read more about it [here](https://github.com/nodenv/nodenv/blob/8948584145f2ce1853967337c91f2e09996aa1c3/README.md?plain=1#L64-L104).
[^4]: Asynchronous communication is any kind of communication where there is a delay between the information being provided by the sender and the time when the recipient accepts the information and acts on it.

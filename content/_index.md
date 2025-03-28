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

Vendoring dependencies is an often overlooked aspect of software development. Many software languages today have package managers that allow for native dependency vendoring.

The term *Dependency Vendoring* refers to the practice of including a copy of a given dependency in your project's repository. Ruby is truly a shining example of this by being able to configure `bundler` (the Ruby package manager) to vendor all dependencies locally within a project. Here is an example of vendoring a gem (rack) within a project: [`vendor/cache/rack-3.1.12.gem`](https://github.com/GrantBirki/ruby-template/blob/c3a4f59585545b1e4289fd7045c215e922cf9153/vendor/cache/rack-3.1.12.gem). When a developer goes to configure their environment, their package manager will install the dependencies from the vendored directory. Additionally, when a CI/CD pipeline runs, it will also install the dependencies from the vendored directory. This leads to an extremely stable and reproducible build process that is not dependent on the availability of the dependency registry except for the first time the dependency is vendored or when the dependency is updated.

The reason that Ruby is such a great example here is due to the way that Gems are structured as a single file. This makes it easy to vendor the dependency and commit it to the repository. Updating a Ruby Gem that has been vendored typically involves updating a single file (the Gem). Doing the same in GoLang might result in updating a directory of files (could be thousands of files).

Vendoring Dependencies should be adopted as a standard practice in **all** projects. Vendoring dependencies has many benefits that lead to a more stable and secure codebase:

- **Reproducibility**: When you vendor dependencies, you are ensuring that the code you are running is the same code that was running when the dependency was last updated.
- **Security**: Vendoring hardens your application against supply chain attacks. The version that is committed to your repository is the version that will be used. If a dependency is compromised, projects that don't vendor their dependencies are at risk of being compromised as well since they pull in the dependency at build time.
- **Availability**: If a dependency is removed from the registry, or the registry goes down, you can still build your project because the dependency is vendored. Additionally, you can even build your project without an internet connection.
- **Performance**: When you vendor dependencies, you are caching them locally. This means that you don't have to download the dependency every time you build your project. This can lead to a significant performance improvement, especially in CI/CD environments.

Without vendoring, it makes it difficult (or in some cases impossible without tooling) to ensure that the application running locally on your machine is the same as the one that gets build in CI/CD and deployed to production.

The North Star example of this principle would be to pass what I call the "**airplane test**"[^5]. If you were to be on an airplane with no internet connection, you should be able to: bootstrap a project, write some code, run unit tests, and build (or run) the project[^6]. If you can do all of that, then you have passed the airplane test and are correctly vendoring your dependencies.

### 4. Build Systems are Production Systems

### 5. Testing

## Deployment

## Maintenance

[^1]: A software project that could be a library, service, or application. This term will be interchanged with "repository" often in this document.
[^2]: The *live* environment of an application or service. A production environment is where the final product is delviered to the end user. This could be a website, mobile app, api, etc.
[^3]: Language version managers use a directory of shims at the front of your `PATH` - Read more about it [here](https://github.com/nodenv/nodenv/blob/8948584145f2ce1853967337c91f2e09996aa1c3/README.md?plain=1#L64-L104).
[^4]: Asynchronous communication is any kind of communication where there is a delay between the information being provided by the sender and the time when the recipient accepts the information and acts on it.
[^5]: The airplane test is a test that I made up to put a projects dependency vendoring to the test. It is a test that I believe all projects should be able to pass. If a project cannot pass the airplane test, then it is almost certainly at risk of having build failures due to dependency resolution issues. Whether those issues are due to network availability, a package registry going down, or the package being remove (or compromised) from the registry, the airplane test will help to ensure that your project shielded from these issues. Go ahead and try it out at home! Turn off your network connection and see if you can write code, run tests, and build your project without the pesky internet.
[^6]: In the context of vendoring dependencies, building a project could mean any of the following: compiling a binary, build a library (like a Ruby Gem), running a service or application, etc. In some cases, building a project is synonymous with running a project.

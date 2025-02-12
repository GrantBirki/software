---
title: "Software: Project Architecture, Deployment, and Maintenance"
author: "Grant Birkinbine"
description: "A high level software design manifesto on how to effectively build production software, deploy it, and maintain it."

# The summary is for search engines
summary: A high level software design manifesto on how to effectively build production software, deploy it, and maintain it.

date: 2025-02-11T00:00:00-00:00 # date of creation

# disableAnchoredHeadings: false

draft: false

ogTitle: Grant Birkinbine
ogDescription: "Software: Project Architecture, Deployment, and Maintenance"
ogImage: /default.png # comment this out to force an SVG generation and usage then you can run `script/images` to make the SVG become a PNG and use it here
---

## Introduction

This document is a high level software design manifesto on how to effectively build production software, deploy it, and maintain it. It is a living document, and an opinionated one. It is strongly inspired by the core engineering principles of [GitHub](https://github.blog/engineering/engineering-principles/).

Some common language that is used throughout this document:

- **Project**: A software project that could be a library, service, or application. This term will be interchanged with "repository" often in this document.
- **Production**: The *live* environment of an application or service.

## Project Architecture

### 1. Scripts to Rule Them All

Projects should adhear to the [**scripts to rule them all**](https://github.com/github/scripts-to-rule-them-all) pattern. This pattern is a set of common scripts that are used across all repositories through an organization. This pattern is used to ensure that all projects have a consistent set of scripts that are used to perform common tasks such as testing, building, and deploying.

Using this pattern enables developers to immediately be productive in a new repository as they can run a single command to get a working development environment.

CI builds also greatly benefit from this pattern as they use the same scripts that developers use to build and test the project.

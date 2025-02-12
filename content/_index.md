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

Projects should follow the [**scripts to rule them all**](https://github.com/github/scripts-to-rule-them-all) pattern. This ensures a consistent set of scripts for tasks like testing, building, and deploying any project.

This pattern allows developers to quickly get a working development environment with a single command and ensures CI builds use the same scripts for building and testing. Developers don't need to remember the specific commands for each project as the scripts are consistent across all projects. No matter the project, language, or framework, calling `script/bootstrap` should always install the project's dependencies.

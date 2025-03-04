---
title: "Tarball Deploy"
author: "Grant Birkinbine"
description: "todo"

# The summary is for search engines
summary: todo

date: 2025-03-03T00:00:00-00:00 # date of creation

draft: false

ogTitle: Grant Birkinbine
ogDescription: "Tarball Deploy"
ogImage: /posts/tarball-deploy/og.png # comment this out to force an SVG generation and usage then you can run `script/images` to make the SVG become a PNG and use it here
---

## Why Use Tarball-Based Deployment?

Tarball-based deployments involve packaging your application and its dependencies into a compressed `.tar.gz` archive and extracting it on the target system. This approach works well in various scenarios, especially when you need a simple, reliable, and versioned deployment method.

### When Tarball-Based Deployments Work Well

In almost all cases, its better to just use containers. However, there are some cases where tarball-based deployments still work well. Most cases involve replacing Git based deployments with tarball based deployments. Here are some examples:

1. **Self-Contained Applications** – If your application is a single binary or a small set of files, tarballs are a simple way to package and deploy it. Examples include Go, Rust, or even Ruby applications that package all dependencies into a single `vendor` directory.
2. **Efficient Transfer and Deployment** – Tarballs are highly compressed and can be quickly transferred over the network and extracted in place.
3. **Non-Containerized Environments** – If you're not using containers but still want a reproducible and controlled deployment method, tarballs offer a lightweight alternative.
4. **Zero-Downtime Deployments** – By extracting the tarball to a new directory and updating a symlink, you can achieve zero-downtime deployments with minimal configuration.

Certain services/applications that runs directly on hosts (or are just super simple) can benefit from tarball-based deployments.

### When NOT to Use Tarball-Based Deployments

- **When Using Containers or Kubernetes** – If you're deploying to Kubernetes, it's better to use container images rather than managing tarball extraction manually.
- **Large, Complex Applications with OS Dependencies** – If your application requires system-wide dependencies (e.g., databases, libraries, or OS-specific packages), it is almost always better to just package the application in a container image. This ensures that the application is deployed in a consistent and reproducible environment.
- **Dependency Isolation** – If you need to isolate dependencies between different applications or services, containers are a better choice.

## What about Git Deployments?

Tarball based deployments are actually a good **alternative** to "git deployments". Git deployments involve having a server pull down the latest code from a git repository, and then building and deploying the application. This can be slow, and can be error prone if the server is not in a clean state. Additionally, git deployments create a hard dependency on the git repository and where it is hosted.

When doing a Git based deployment, your server needs both the **build** environment and the **runtime** environment. For example, if you are doing a Git based deployment of a Ruby application, your server might need to have certain `apt` packages installed, like `libssl-dev`, `libreadline-dev`, and `zlib1g-dev`. These might be needed to **build** the Ruby application. However, these are not always needed to **run** the Ruby application. If you were to use a tarball based deployment, you could build the Ruby application in a container, and then deploy the tarball to the server. This would mean that the server only needs the runtime dependencies, and not the build dependencies. This can make the server more secure, and can make the deployment faster.

Lastly, Git is truly not a deployment tool. It is a version control system. While it can be coerced into a deployment tool, it is not the best tool for the job.

Using Git as a deployment tool is like launching a plane into the air with no engines via a sling shot. Sure, it will fly for a bit, but it will eventually crash and burn.

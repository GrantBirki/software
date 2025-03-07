---
title: "Tarball Deploy"
author: "Grant Birkinbine"
description: "About Tarball-based deployments"

# The summary is for search engines
summary: Tarball Deployments - A simple deployment alternative to Git based deployments when containers might not be an option.

date: 2025-03-03T00:00:00-00:00 # date of creation

draft: false

ogTitle: Grant Birkinbine
ogDescription: "Tarball Deploy"
ogImage: /posts/tarball-deploy/og.png # comment this out to force an SVG generation and usage then you can run `script/images` to make the SVG become a PNG and use it here
---

## Why Use Tarball-Based Deployment?

Tarball-based deployments involve packaging your application and its dependencies into a compressed `.tar.gz` archive and extracting it on the target system. This approach works well in various scenarios, especially when you need a simple, reliable, and versioned deployment method.

Tarball deployments are largely considered a legacy deployment method, but they are still used today and many systems still rely on them.

### When Tarball-Based Deployments Work Well

In almost all cases, its better to just use containers. However, there are some cases where tarball-based deployments still work well. Most cases involve replacing Git based deployments with tarball based deployments. Here are some examples:

1. **Self-Contained Applications** – If your application is a single binary or a small set of files, tarballs are a simple way to package and deploy it. Examples include Go, Rust, or even Ruby applications that package all dependencies into a single `vendor` directory.
2. **Efficient Transfer and Deployment** – Tarballs are highly compressed and can be quickly transferred over the network and extracted in place.
3. **Non-Containerized Environments** – If you're not using containers but still want a reproducible and controlled deployment method, tarballs offer a lightweight alternative.
4. **Zero-Downtime Deployments** – By extracting the tarball to a new directory and updating a symlink, you can achieve zero-downtime deployments with minimal configuration.
5. **Host-Based Services** – Many systems today are containerized, but some are still services running directly on hosts doing Git based deployments. Using tarball-based deployments can be a good alternative to Git based deployments if you are deploying to a host-based service.
6. **Simple Services** – Very simple services that don't need to be containerized can benefit from tarball-based deployments. For example, a very simple web server that serves static files could be deployed using tarball-based deployments, and a lot of the time, this is how it is done.
7. **Pure Configuration Changes** - If you are deploying a change consisting of only configuration files (ex: json, yaml, etc), then a tarball based deployment can also work well. Especially if you pair this with symlinks, you can quickly switch between different configurations.

> *A tarball, sometimes its all you need.*

### When NOT to Use Tarball-Based Deployments

- **When Using Containers or Kubernetes** – If you're deploying to Kubernetes, it's better to use container images rather than managing tarball extraction manually.
- **Large, Complex Applications with OS Dependencies** – If your application requires system-wide dependencies (e.g., databases, libraries, or OS-specific packages), it is almost always better to just package the application in a container image. This ensures that the application is deployed in a consistent and reproducible environment.
- **Dependency Isolation** – If you need to isolate dependencies between different applications or services, containers are a better choice.
- **Security** – If you need to ensure that the deployment environment is secure and compliant, containers offer better isolation and control over the runtime environment.

## How to Implement Tarball-Based Deployments

This section will go into a high-level overview of how to implement tarball-based deployments for deploying a simple application to a single server.

The following steps should all take place in a CI/CD pipeline and this takes place during a **deployment** stage.

1. **Build the Tarball** – Compile the application and package it into a tarball in a clean environment.
2. **Transfer the Tarball** – Transfer the tarball to the target server(s) using a secure and reliable method. SSH, SCP, are your classic options.
3. **Extract the Tarball** - Once the tarball exists on the server, the most common method is to have the final step of the deployment process be to connect to the server (ex: via SSH) and extract the tarball to the correct directory.
4. **Restart the Application** - After the tarball is extracted, the application needs to be restarted to pick up the new changes. Now you are done!

### Best Practices for Tarball-Based Deployments

- **Pack Commit Metadata** - Since tarballs should not contain the `.git` directory, it will not contain Git data by default. Therefore, you should ensure you add some metadata into the tarball. For example, adding the Git [SHA or branch](https://github.com/GrantBirki/ruby-template/blob/ef2e68f2aeeeb1284879b8df5fa7b5545affeed2/script/build-deploy-tarball#L20-L22) could be useful here.
- **File Naming Convention** - Use a consistent naming convention for your tarballs to make it easy to identify the platform, version, and architecture. For example: `linux-aarch64-bookworm-sha123abc.tar.gz`
- **Cleanup Old Tarballs** - To avoid running out of disk space, you should periodically clean up old tarballs on the server.
- **Checksums and Signatures** - To ensure the integrity of the tarball, you should generate a checksum and/or signature for the tarball and verify it on the server before extracting it.

## What about Git Deployments?

Tarball based deployments are actually a good **alternative** to "git deployments". Git deployments involve having a server pull down the latest code from a git repository, and then building and deploying the application. This can be slow, and can be error prone if the server is not in a clean state. Additionally, git deployments create a hard dependency on the git repository and where it is hosted.

When doing a Git based deployment, your server needs both the **build** environment and the **runtime** environment. For example, if you are doing a Git based deployment of a Ruby application, your server might need to have certain `apt` packages installed, like `libssl-dev`, `libreadline-dev`, and `zlib1g-dev`. These might be needed to **build** the Ruby application. However, these are not always needed to **run** the Ruby application. If you were to use a tarball based deployment, you could build the Ruby application in a container, and then deploy the tarball to the server. This would mean that the server only needs the runtime dependencies, and not the build dependencies. This can make the server more secure, and can make the deployment faster.

Lastly, Git is truly not a deployment tool. It is a version control system. While it can be coerced into a deployment tool, it is not the best tool for the job.

Using Git as a deployment tool is like launching a plane into the air with no engines via a sling shot. Sure, it will fly for a bit, but it will eventually crash and burn.

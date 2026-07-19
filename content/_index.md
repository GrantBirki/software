---
title: "Software: How to build it"
author: "Grant Birkinbine"
description: "A high level software design manifesto on how to effectively build production software, deploy it, and maintain it."

# The summary is for search engines
summary: A high level software design manifesto on how to effectively build production software, deploy it, and maintain it.

date: 2025-02-11T00:00:00-00:00 # date of creation

# disableAnchoredHeadings: false

draft: false

homePageIsPost: true

ogTitle: Grant Birkinbine
ogDescription: "Software: How to build it"
ogImage: /default.png
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

### 3. Less is Better

The safest and easiest dependency to maintain is one that a project never adds. Package managers make dependencies feel free, but every one adds code you did not write, maintainers you must trust, releases you must review, and an API your project must carry. A single direct dependency can also hide a much larger transitive graph, multiplying those costs.

I strongly agree with the approach described by Obsidian in their blog post [*Less is safer*](https://obsidian.md/blog/less-is-safer/). Their strategy is wonderfully simple: avoid depending on third-party code in the first place. A shallow dependency graph means fewer paths for a malicious update, fewer upstream decisions affecting your project, and less code to audit. It also improves reliability and maintainability: a dependency that does not exist cannot disappear from a registry, abandon an API, introduce a vulnerability, or demand an emergency upgrade.

This principle applies to **all** dependencies, not only those shipped with the final application. Build tools, linters, development tools, and testing frameworks still execute on developer machines or in CI/CD environments, where they may access source code, credentials, or release artifacts. Calling something a "development dependency" describes when it is used; it does not erase its supply chain risk or maintenance cost.

Programming languages with excellent standard libraries deserve a special shout out because they make this principle practical. Go is an *exceptional* example. Its [`net/http`](https://pkg.go.dev/net/http) package can power a complete web service without a third-party framework, while [`testing`](https://pkg.go.dev/testing) provides tests, benchmarks, and examples without a separate testing framework. A strong standard library supports substantial features while keeping the dependency graph small, understandable, and well maintained.

Before adding a dependency, use this decision ladder:

1. **Start with the standard library**: Use the built-in capability when it fits; it usually has fewer trust, compatibility, and update boundaries.
2. **Own small and understandable behavior**: A few clear lines of local code are often easier to maintain than a package and its transitive dependencies.
3. **Internally fork medium sized modules carefully**: Bring code into the project only when its license permits it and the team will own its maintenance. This exchanges upstream trust for direct responsibility; it does not eliminate work.
4. **Make large dependencies earn their place**: Accept complex or specialized dependencies when recreating them would be riskier. Inspect their maintainers, release history, license, install behavior, and full transitive graph. Their value must outweigh the code and trust they add.

Less is better is a strong default, not an argument for rewriting everything. Homegrown cryptography or casually replacing a mature security-critical parser is likely to create more risk than a carefully selected dependency. Require justification proportional to the dependency's cost, while avoiding the reflex to outsource simple, well-understood behavior.

Revisit this decision during every upgrade. The original need may be gone, while a routine version bump may add sub-dependencies, install scripts, or maintainers. Treat meaningful upgrades as accepting the dependency again. If it still earns its place, pin it, vendor it, and update it deliberately.

### 4. Vendor Dependencies

Once a dependency has earned its place in a project, it should be vendored wherever the language supports it. Vendoring dependencies is an often overlooked aspect of software development. Many software languages today have package managers that allow for native dependency vendoring.

The term *Dependency Vendoring* refers to the practice of including a copy of a given dependency in your project's repository. Ruby is truly a shining example of this by being able to configure `bundler` (the Ruby package manager) to vendor all dependencies locally within a project. Here is an example of vendoring a gem (rack) within a project: [`vendor/cache/rack-3.1.12.gem`](https://github.com/GrantBirki/ruby-template/blob/c3a4f59585545b1e4289fd7045c215e922cf9153/vendor/cache/rack-3.1.12.gem). When a developer goes to configure their environment, their package manager will install the dependencies from the vendored directory. Additionally, when a CI/CD pipeline runs, it will also install the dependencies from the vendored directory. This leads to an extremely stable and reproducible [hermetic build process](/posts/hermetic-builds/) that is not dependent on the availability of the dependency registry except for the first time the dependency is vendored or when the dependency is updated.

The reason that Ruby is such a great example here is due to the way that Gems are structured as a single file. This makes it easy to vendor the dependency and commit it to the repository. Updating a Ruby Gem that has been vendored typically involves updating a single file (the Gem). Doing the same in GoLang might result in updating a directory of files (could be thousands of files).

Vendoring Dependencies should be adopted as a standard practice in **all** projects. Vendoring dependencies has many benefits that lead to a more stable and secure codebase:

- **Reproducibility**: When you vendor dependencies, you are ensuring that the code you are running is the same code that was running when the dependency was last updated.
- **Security**: Vendoring hardens your application against supply chain attacks. The version that is committed to your repository is the version that will be used. If a dependency is compromised, projects that don't vendor their dependencies are at risk of being compromised as well since they pull in the dependency at build time.
- **Availability**: If a dependency is removed from the registry, or the registry goes down, you can still build your project because the dependency is vendored. Additionally, you can even build your project without an internet connection.
- **Performance**: When you vendor dependencies, you are caching them locally. This means that you don't have to download the dependency every time you build your project. This can lead to a significant performance improvement, especially in CI/CD environments.

Without vendoring, it makes it difficult (or in some cases impossible without tooling) to ensure that the application running locally on your machine is the same as the one that gets build in CI/CD and deployed to production.

The North Star of this principle would be to pass what I call the "**airplane test**"[^5]. If you were to be on an airplane with no internet connection, you should be able to: bootstrap a project, write some code, run unit tests, and build (or run) the project[^6]. If you can do all of that, then you have passed the airplane test and are correctly vendoring your dependencies.

### 5. Build Systems are Production Systems

The system that decides what reaches production is part of production. A build system is not just a compiler or a `script/build` command. It includes the repository-owned scripts, CI workflows, packaging, release automation, and deployment controls that turn source code into a running change. This system selects the source, dependencies, tools, credentials, configuration, and final artifact. When it fails, releases stop. When it is compromised, it can produce malicious output that every downstream system trusts.

Build systems should follow four rules:

1. **Use one reviewed path**: Developers and CI should call the same repository-owned `script/*` entrypoints. Keeping the real build logic beside the application makes it reviewable and prevents local, CI, and release builds from quietly becoming different systems. This follows the [scripts to rule them all](https://github.com/github/scripts-to-rule-them-all) pattern.
2. **Make trust explicit**: Pin runtimes, tools, dependencies, and CI Actions to immutable references. Use least-privilege permissions, and do not expose deployment credentials to code from a pull request. See GitHub's [Secure use reference](https://docs.github.com/en/actions/reference/security/secure-use) and if using branch deployments, see the [trusted checkouts docs](https://github.com/github/branch-deploy/blob/de4d10ee17c3117a2076aff489ba03fadf225f35/docs/trusted-checkouts.md).
3. **Prove the artifact**: Build the exact reviewed commit, verify the package contents and checksums, and exercise it the way a consumer will. [SLSA provenance](https://slsa.dev/spec/v1.2/provenance) records where, when, and how an artifact was produced. A reproducible build allows another party to recreate it *bit-for-bit* from the same inputs.
4. **Control deployment**: Preview destructive changes, require explicit authorization, serialize shared mutations, deploy the verified artifact or exact commit, report the result, and preserve a rollback path. A deployment must not silently rebuild something different from what passed review. Notably, release builds should not use GitHub Actions caching [due to the risk of cache poisoning](https://adnanthekhan.com/2024/05/06/the-monsters-in-your-build-cache-github-actions-cache-poisoning/#dont-use-actions-caching-in-release-builds).

These rules are visible in [`go-template`'s release workflow](https://github.com/GrantBirki/go-template/blob/cdf5f7d3e204cd088d638723c732689aa3c6211c/.github/workflows/release.yml), which uses shared scripts, vendored inputs, checksummed artifacts, provenance, and verification. The [`dns` branch deployment workflow](https://github.com/GrantBirki/dns/blob/b607836ab05879ec3333e24236afb2bca8bd3e69/.github/workflows/branch-deploy.yml) keeps control code trusted, operates on an exact candidate commit, separates `.noop` from `.deploy`, serializes production changes, reports the outcome, and documents rollback. Neither system is magic; their value comes from making the important boundaries explicit and reviewable.

A green test suite proves that the tests passed. It does not prove that the reviewed code became the artifact running in production. The build system must close that gap.

### 6. Testing

An exceptional test suite should aim for 100% line, branch, and function coverage. This is the North Star. It forces a project to account for every path through its code instead of quietly accepting unknown behavior.

Coverage does not prove correctness; it only proves that code executed. Tests still need meaningful assertions across successful paths, failures, edge cases, and the branches where safety decisions are made. Every behavior change should include tests, and every bug fix should add a regression test that fails without the fix.

Strong tests are the lifeblood of a healthy open source project (and private projects too). They give maintainers a reliable way to evaluate a contribution without reconstructing the entire system during review. They also give contributors the confidence to make a change and know whether it broke unrelated behavior. Without that shared safety net, contributions become slower, riskier, and less likely.

Every project should expose one consistent test command. `script/test` should be the entrypoint that developers and CI both call. Unit tests should be fast, focused, deterministic, and isolated from the network. A suite that is slow or unreliable teaches people not to run it.

Languages with excellent built-in testing tools make this much easier. Go's `testing` package is an excellent example: a project can write tests, benchmarks, and executable examples without adding another testing dependency.

Acceptance tests are the next level. Projects with an executable, service, or other consumer-facing boundary should exercise the product through the same interface a consumer uses. Run these tests through `script/acceptance`, both locally and in CI. Keep the environment controlled with mock APIs, emulators, or disposable containerized services instead of live external systems. A GitHub integration can use a mock GitHub API, an LDAP-backed service can use a disposable LDAP container, and an Action can be invoked through `uses: ./`. These tests prove that the parts work together while remaining repeatable and independent of someone else's availability or data.

## Deployment

### 1. Deploy Before Merge

The safest time to discover that a change does not work in production is before it becomes part of `main`. Merge-then-deploy reverses this order: it records a change as accepted and only then discovers whether it can run. That makes every deployment a gamble and turns `main` into a collection of assumptions instead of a known-good branch.

Branch deployment should be the strong default. Open a pull request, pass the selected CI checks, explicitly authorize a production deployment, validate the running change, and merge only after it works. The order matters. Testing and review establish that the change should work; the deployment proves that it does work in the environment that matters. The merge then records a state that has already been proven.

This model also gives `main` a useful and durable meaning: it is the known-good, deployable branch. A broken candidate can be fixed or abandoned without first removing it from `main`, and a rollback has an obvious target. The [Branch Deploy Model](https://blog.birki.io/posts/branch-deploy/) describes this workflow in more detail, while GitHub's article on [enabling branch deployments through IssueOps](https://github.blog/engineering/engineering-principles/enabling-branch-deployments-through-issueops-with-github-actions/) shows how the same idea can work across a large engineering organization.

Merging should not automatically trigger a redundant deployment when production already contains the exact tree that was validated. If the merge result matches the deployed change, there is nothing new to prove. Reconcile from `main` only when the merge commit, an intervening deployment, or another repository change means that production differs from the newly merged tree. The upstream [merge-commit strategy](https://github.com/github/branch-deploy/blob/de4d10ee17c3117a2076aff489ba03fadf225f35/docs/merge-commit-strategy.md) provides a practical model for making that decision without discarding the safety of deploy-before-merge.

### 2. Deploy Immutable References

People deploy pull request branches; machines should deploy exact commits. A branch gives humans the context they need: the review, discussion, CI results, permissions, and deployment history. The actual deployment target must be the full commit SHA selected from that pull request. This separates a useful human workflow from the immutable identity required by a reliable system.

Every deployment should retain that source SHA. When the build produces a container, package, binary, or other artifact, the deployment should also use and retain its immutable digest or checksum. Branches, tags, versions, and releases are valuable labels, but they are not sufficient identities. They can move, be recreated, or resolve differently over time. An exact SHA and artifact digest make it possible to answer a basic question long after the event: what, precisely, is running?

This is not an argument for allowing arbitrary commit SHAs to bypass the normal workflow. The SHA should still be selected through a pull request, checked by CI, constrained by permissions, and deployed only after explicit authorization. The [commit-SHA deployment guidance](https://github.com/github/branch-deploy/blob/de4d10ee17c3117a2076aff489ba03fadf225f35/docs/deploying-commit-SHAs.md) preserves the pull request as the trusted context while ensuring that the selected candidate cannot change between approval and deployment.

Deployment control code needs its own trust boundary. Workflows, scripts, and helpers that decide how production changes should come from a trusted revision of the default branch. Only the candidate application or infrastructure should come from the pull request's exact SHA. Otherwise, a pull request can change both the thing being deployed and the mechanism that grants credentials and deploys it. The pinned [trusted-checkout guidance](https://github.com/github/branch-deploy/blob/de4d10ee17c3117a2076aff489ba03fadf225f35/docs/trusted-checkouts.md) demonstrates how to keep those responsibilities separate.

### 3. Make Deployments Safe and Reversible

A production deployment should require two things: the selected CI checks must pass, and an authorized operator must explicitly request the change. Passing tests alone should not silently mutate production, and deployment authority should not bypass the project's required checks. Pull request approval requirements can remain project-specific; different teams and risk levels justify different review policies.

When a deployment system can produce a meaningful noop, plan, preview, or dry run, show it before applying the change. This is especially valuable for infrastructure and other destructive operations because it gives the operator a chance to catch an unexpected mutation. It is not a universal requirement. A misleading preview or one that merely repeats the input adds ceremony without adding safety.

Deployments that compete for the same environment or shared mutable state must be serialized. Two individually safe changes can become unsafe when they race, overwrite each other, or apply from different assumptions about the current state. Independent and additive releases do not need a global lock simply for consistency's sake. Concurrency should follow the actual collision boundary.

The result of every deployment should remain visible and attributable. A maintainer should be able to find the exact source and artifact identity, the target environment, whether the deployment succeeded, and the output needed to understand the result. The specific metadata format matters less than keeping a durable connection between the reviewed change and the production event.

Rollback must be a normal and practiced deployment operation. When a candidate fails, redeploy the stable state of `main`, resolved to its exact source SHA and artifact identity. Do not wait for a revert pull request to merge before restoring service. The repository can record the corrective change afterward; production recovery should use the known-good state that already exists.

These principles do not require any particular command interface or CI provider. They require a deployment system that preserves context, deploys immutable inputs, protects its control path, proves changes in production, and can recover quickly. [`github/branch-deploy`](https://github.com/github/branch-deploy) is one reference implementation of this model.

## Maintenance

[^1]: A software project that could be a library, service, or application. This term will be interchanged with "repository" often in this document.
[^2]: The *live* environment of an application or service. A production environment is where the final product is delviered to the end user. This could be a website, mobile app, api, etc.
[^3]: Language version managers use a directory of shims at the front of your `PATH` - Read more about it [here](https://github.com/nodenv/nodenv/blob/8948584145f2ce1853967337c91f2e09996aa1c3/README.md?plain=1#L64-L104).
[^4]: Asynchronous communication is any kind of communication where there is a delay between the information being provided by the sender and the time when the recipient accepts the information and acts on it.
[^5]: The airplane test is a test that I made up to put a projects dependency vendoring to the test. It is a test that I believe all projects should be able to pass. If a project cannot pass the airplane test, then it is almost certainly at risk of having build failures due to dependency resolution issues. Whether those issues are due to network availability, a package registry going down, or the package being remove (or compromised) from the registry, the airplane test will help to ensure that your project shielded from these issues. Go ahead and try it out at home! Turn off your network connection and see if you can write code, run tests, and build your project without the pesky internet.
[^6]: In the context of vendoring dependencies, building a project could mean any of the following: compiling a binary, build a library (like a Ruby Gem), running a service or application, etc. In some cases, building a project is synonymous with running a project.

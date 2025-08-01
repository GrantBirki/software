---
title: "Hermetic Builds"
author: "Grant Birkinbine"
description: "About hermetic builds and dependency vendoring"

# The summary is for search engines
summary: Hermetic Builds - A battle tested way to ensure your builds are reproducible and air-gapped by vendoring dependencies.

date: 2025-07-31T00:00:00-00:00 # date of creation

draft: false

ogTitle: Grant Birkinbine
ogDescription: "Hermetic Builds"
ogImage: /posts/hermetic-builds/og.png # comment this out to force an SVG generation and usage then you can run `script/images` to make the SVG become a PNG and use it here
---

## What are Hermetic Builds?

A hermetic build is a build process that is self-contained, deterministic, and completely air-gapped from the internet. It produces the same output given the same inputs, regardless of the environment in which it runs. The term "hermetic" comes from the concept of a hermetically sealed container – nothing gets in or out except what is explicitly defined.

In the context of software development, hermetic builds ensure that your application can be built consistently across different environments without relying on external resources during the build process. This is primarily achieved through **dependency vendoring** – the practice of including all necessary dependencies directly within your project repository.

Think of hermetic builds as the "airplane test" for your entire build system. If you can build your project on an airplane with no internet connection, no access to external systems, and the only machine involved in the build is the one on your lap, you've achieved a truly hermetic build.

## Why Hermetic Builds Matter Beyond Security

While supply chain security is often the headline reason for adopting hermetic builds, the benefits extend far beyond just protecting against malicious dependencies. Hermetic builds fundamentally change how you think about software reliability, debugging, and team productivity.

### 1. Debugging and Troubleshooting Become Tractable

When a build fails in a non-hermetic environment, you're often left wondering: *Was it a network issue? Did a dependency registry go down? Did someone push a breaking change to a transitive dependency?*

With hermetic builds, you eliminate an entire class of failure modes. When something breaks, you know it's either:

- Something in your code
- Something in your vendored dependencies (which you explicitly chose to update)
- Something in your build environment (e.g., a compiler version change)

This dramatically reduces the scope of investigation and makes debugging more predictable. No more debugging sessions that end with "it must have been a network hiccup" or "try running it again."

### 2. Freezing Your Dependencies in Time

Hermetic builds create a **time capsule** for your dependencies. When you vendor dependencies, you're preserving the exact state of the software ecosystem at the time of your last dependency update. This has profound implications:

- **Historical Builds**: You can checkout any commit from your repository's history and build it successfully, even if half the dependencies no longer exist on the internet.
- **Regression Analysis**: When debugging issues, you can bisect through your Git history knowing that each commit represents a buildable state.
- **Archaeological Debugging**: Years later, when investigating a production issue, you can build the exact version that's running in production without worrying about dependency evolution.

This is particularly valuable for teams maintaining long-lived applications or working in regulated industries where you need to prove exactly what was running at any given time.

### 3. Performance and Productivity Gains

The performance benefits of hermetic builds compound over time:

- **CI/CD Speed**: No time wasted downloading dependencies from potentially slow or overloaded registries
- **Developer Onboarding**: New team members can bootstrap and build the project immediately
- **Consistent Build Times**: Build duration becomes predictable since it's not dependent on network conditions
- **Offline Development**: Developers can work productively without internet access (or in limited connectivity scenarios)

Consider a typical CI/CD pipeline that spends 2-3 minutes downloading dependencies on every run. Over hundreds of builds per day across a team, this time adds up to hours of wasted compute and developer waiting time.

### 4. Eliminating "Works on My Machine" Syndrome

Non-hermetic builds are susceptible to subtle environmental differences:

- Different versions of system packages
- Varying network conditions affecting which mirror a package manager uses
- Timing-dependent dependency resolution
- Registry inconsistencies between different geographic regions

Hermetic builds eliminate these variables. If the build works in one environment, it will work in all environments with the same base toolchain.

## The Supply Chain Security Bonus

While not the only reason to adopt hermetic builds, supply chain security benefits are substantial:

### Protection Against Dependency Confusion Attacks

When you vendor dependencies, you're protected against attackers who might upload malicious packages with similar names to popular libraries. Your build process won't accidentally pull in a compromised dependency because it's using the specific versions you've explicitly vendored.

### Immutable Dependency Snapshots

Vendored dependencies create an immutable snapshot of your dependency tree. Even if an attacker compromises a package registry and replaces legitimate packages with malicious ones, your builds continue using the known-good versions you've vendored.

### Audit Trail and Compliance

Every dependency update becomes an explicit, reviewable change in your version control system. This creates a clear audit trail of what changed, when, and who approved it – crucial for compliance in regulated industries.

## Implementing Hermetic Builds: Language-Specific Strategies

### Ruby: The Gold Standard

Ruby's Bundler makes hermetic builds straightforward:

```bash
bundle install --local
```

This assumes you have a `vendor/cache` directory containing all your gems and a `.bundle/config` file like this:

```yaml
---
BUNDLE_BIN: "bin"
BUNDLE_PATH: "vendor/gems"
BUNDLE_CACHE_PATH: "vendor/cache"
BUNDLE_CACHE_ALL: "true"
BUNDLE_SPECIFIC_PLATFORM: "true"
BUNDLE_NO_INSTALL: "true"
```

Ruby gems are single compressed files, making them ideal for vendoring without repository bloat.

Here is an example of a Ruby project that uses vendoring effectively: [GrantBirki/ruby-template](https://github.com/GrantBirki/ruby-template/tree/7200c62db6c178fd972c54f522d1cc4d6d61ea65)

### Go: Built-in Vendoring Support

Go has excellent built-in support for hermetic builds:

```bash
# Create vendor directory with all dependencies
go mod vendor

# Build using only vendored dependencies
go build -mod=vendor
```

> You might end up with a pull request containing +/- 100,000 lines of code, but this is the price of reliability.

Here is an example of a Go project that uses vendoring effectively: [github/gh-combine](https://github.com/github/gh-combine/tree/e2ef6bfa8cb006e0134b5e39d8a08b8b5033e81f).

## The Airplane Test: Your Hermetic Build Validation

Here's how to validate that your builds are truly hermetic:

1. **Disconnect from the internet** (or work from a flight!)
2. **Clone your repository** to a clean directory
3. **Bootstrap your development environment** using your standard setup scripts
4. **Run your tests** and **build your application**
5. **Start your application** (if applicable)

If any step fails due to network dependencies, your build isn't hermetic yet.

## Common Objections and Responses

### "But vendoring increases repository size!"

Yes, vendoring increases repository size, but consider the trade-offs:

- Storage is cheap; developer time and build reliability are expensive
- Modern Git handles large repositories well with features like LFS and shallow clones
- The productivity gains from reliable builds far outweigh storage costs

### "Dependencies become stale!"

This is actually a feature, not a bug. Dependencies should be updated intentionally and deliberately, not automatically. Hermetic builds force you to be conscious about dependency updates, leading to more stable software.

### "What about security updates?"

Hermetic builds don't prevent security updates – they make them explicit and reviewable. When a security issue is discovered, you update the affected dependencies, review the changes, and commit the new vendored versions.

## Best Practices for Hermetic Builds

1. **Automate dependency updates**: Use tools like Dependabot or Renovate to propose dependency updates as pull requests
2. **Regular dependency audits**: Periodically review and update dependencies, don't let them become too stale
3. **Document your vendoring process**: Make it easy for new team members to understand how to update dependencies
4. **Test dependency updates thoroughly**: Since updates are explicit, you can afford to test them more rigorously

## Hermetic Builds and SLSA Level 3

The [Supply Chain Levels for Software Artifacts (SLSA)](https://slsa.dev/) framework provides a set of security guidelines for securing software supply chains. SLSA Level 3 represents a significant milestone in supply chain security, and hermetic builds are a **critical component** for achieving this level.

### What SLSA Level 3 Requires

SLSA Level 3 builds upon the requirements of lower levels (SLSA L1/L2) and adds several key guarantees:

- **Hosted Build Platform**: Builds must run on a hosted platform that generates and signs provenance
- **Build Isolation**: Build runs cannot influence each other (preventing cross-contamination between builds)
- **Signed Provenance**: The build process must produce cryptographically signed provenance that can be verified as authentic

### How Hermetic Builds Enable SLSA Level 3

Hermetic builds directly support two of the three core SLSA Level 3 requirements:

1. **Build Isolation**: By vendoring all dependencies and eliminating external network calls during the build process, hermetic builds naturally prevent builds from influencing each other. Each build operates in a completely self-contained environment.

2. **Provenance Integrity**: Since hermetic builds use only explicitly vendored dependencies, the provenance can accurately capture the complete set of inputs used in the build. There are no hidden or implicit dependencies that could affect the build outcome.

When combined with a SLSA Level 3 compliant build platform (like GitHub Actions with the appropriate generators), hermetic builds provide the foundation for achieving the highest level of supply chain security currently defined by the SLSA framework.

### The Path Forward

If your organization is pursuing SLSA Level 3 compliance, implementing hermetic builds should be one of your first priorities. They provide the build isolation and reproducibility guarantees that SLSA Level 3 requires, while also delivering the practical benefits discussed throughout this post.

## The Bottom Line

Hermetic builds represent a fundamental shift in how we think about software reliability. They transform builds from fragile, network-dependent processes into predictable, reliable operations. While the security benefits are significant, the real value lies in the dramatically improved developer experience and system reliability.

In a world where software systems are increasingly complex and interconnected, hermetic builds provide a foundation of stability that teams can build upon with confidence. They're not just a nice-to-have feature – they're a prerequisite for professional software development and a critical component for achieving advanced supply chain security standards like SLSA Level 3.

> *Hermetic builds: Because your software should build the same way every time, everywhere.*

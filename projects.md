---
layout: page
title: "Projects"
permalink: /projects
---
## Projects 📽️

### [🛥️🐳 KSail](https://github.com/devantler/ksail)

A CLI tool for provisioning GitOps-enabled K8s clusters in Docker. It is especially useful for local development and in CI pipelines.

Locally it allows you to spin up a K8s cluster with GitOps enabled to instantly deploy your Flux Kustomizations. This enables you to develop and test your applications in a K8s environment almost instantly.

In CI pipelines, it allows you to spin up a K8s cluster with GitOps enabled and test that it successfully deploys your applications.

### [#️⃣ .NET Commons](https://github.com/devantler/dotnet-commons)

Various .NET libraries I built to simplify common tasks I encounter in my day-to-day work. The libraries are built to be as simple as possible to use, and often wrap more complex libraries to simplify common use cases in .NET. As such, they are not as flexible as the libraries they wrap, but they are much easier to use.

As I encounter more complex use cases, these libraries will grow in complexity, but I will always strive to keep them as simple as possible without solving something that has already been solved.

I use and maintain these libraries regularly, but I really appreciate contributions and feedback to find out whether they can be useful to others.

### [🏠 Homelab](https://github.com/devantler/homelab)

A Flux GitOps-based Kubernetes cluster that I run on a Mac Mini and a set of RPIs in my home. It demonstrates a dev-friendly approach to working with Kubernetes.

I use this cluster to learn and experiment with new technologies and approaches to working with Kubernetes. As such I strive to implement the latest and greatest technologies to keep my skills sharp and up-to-date.

I also plan to use this cluster to run services that I use in my day-to-day life, for entertainment, personal projects, or self-hosted services to cut costs and own my own data.

### [🚚 OCI Artifacts](https://github.com/devantler/oci-artifacts)

A spin-off project from my Homelab project, that simplifies my GitOps deployments by distributing my Kustomize and Flux HelmRelease components through OCI. Doing this allows me to deploy my applications in a single line of code, and still have the flexibility of regular Kustomize and Helm charts. The approach was inspired by shared libraries in regular programming languages, and I felt that Helm charts and Kustomize components required way too much duplication of code when deploying in multiple environments.

Most of the OCI Artifacts are dependent on Flux Kustomizations, and they use Flux Post Build Variables to injects values into the components. This allows me to expose values for the component, that a user can provide when deploying the component in typical settings.

I strive to keep the OCI Artifacts as simple as possible, such that they are very close to the original Kustomize and Helm charts default values. But in many cases, Helm Chart maintainers do not provide default values that enable the component to be deployed out-of-the-box. In those cases, I enforce some post-build variables to be set to ensure that the component can be deployed.

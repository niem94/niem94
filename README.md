![](https://komarev.com/ghpvc/?username=devantler)

# Hi there 👨🏻‍💻🤘🏻

<p align="center">
  <img alt="profile-image" src="https://github.com/devantler/devantler/assets/26203420/60c5ee86-ce7e-4962-b459-e40d991589f1" width="500">
</p>

## About Me 📝

As a software engineer, I have a strong passion for modern software development practices and technologies. I truly believe in the power of open-source software, especially its ability to spark innovation and encourage collaboration. I'm a big fan of the Cloud Native Computing Foundation (CNCF) and appreciate their work in promoting the use of cloud-native technologies. I believe that by working together on important software, we can tackle the challenges we face today and in the future.

```csharp
public class Profile {
  public string Name { get; init; } = "Nikolai Emil Damm";
  public string Alias { get; set; } = "devantler";
  public string Job { get; set;} = "Software Developer in the Digital Incubator at Energinet";
  public string Education { get; set; } = "MSc in Software Engineering";
  public FavLang FavLang { get; set; } = FavLang.CSharp;
  public string[] Skills { get; set; } = { "C#/.NET", "Go", "K8s", "Docker", "CNCF", "And much much more" };
  public string[] Interests { get; set; } = { "GitOps", "Meshing", "Observability", "Modern approaches to bridge OT and IT" };
  public string[] PersonalInterests { get; set; } = { "Running", "Gaming", "Technology" };
}
```

## Professional Experience 💼

### Software Developer in the Digital Incubator at Energinet

**Duration**: August 2023 - Present

**Key Responsibilities**:

- Developing and maintaining K8s infrastructure.
- Developing and maintaining GitOps tooling.
- Developing and maintaining CI/CD pipelines.
- Developing and maintaining IT and OT integrations.
- Supporting the organization in adopting modern software development practices like GitOps, DevOps, and cloud-native.

## Open Source Projects 🚀

- [🛥️🐳 KSail](https://github.com/devantler/ksail) - A CLI tool for provisioning GitOps-enabled K8s clusters in Docker.
- [#️⃣ .NET Commons](https://github.com/devantler/dotnet-commons) - Various .NET libraries built to simplify common tasks like Code Generation.
- [🏠 Homelab](https://github.com/devantler/homelab) - A Flux GitOps-based Kubernetes cluster that I run on a Mac Mini and a set of RPIs in my home. It demonstrates a dev-friendly approach to working with Kubernetes.
- [🚚 OCI Artifacts](https://github.com/devantler/oci-artifacts) - Popular Kustomize and Flux HelmRelease components that are distributed through OCI.

## Live Stats 📊

<div align="center">
  <a href="https://github.com/anuraghazra/github-readme-stats">
    <img alt="github-readme-stats-top-langs" align="center" src="https://github-readme-stats-pt7yj2vy3-devantler.vercel.app/api/top-langs/?username=devantler&theme=aura_dark&langs_count=8&layout=compact&role=OWNER,COLLABORATOR&&exclude_repo=software-engineering-f22-shared" />
  </a>
  <a href="https://github.com/anuraghazra/github-readme-stats">
    <img alt="github-readme-stats" align="center" src="https://github-readme-stats-pt7yj2vy3-devantler.vercel.app/api?username=devantler&show_icons=true&theme=aura_dark&count_private=true&include_all_commits=true&role=OWNER,COLLABORATOR"/>
  </a>
</div>

<!-- {% comment %} -->

## For Maintainers 🛠️

My Curriculum Vitae is also a central hub for all my active projects. As such this repository is a monorepo that contains all my active projects as submodules. This allows me to keep all my projects in one place and easily manage them. If you want to know more about how to work with this monorepo, please see the Getting Started Guide below.

<details>
  <summary>Show/Hide Getting Started Guide</summary>

### Initializing the Monorepo

When you clone the monorepo for the first time, you need to initialize the submodules:

```bash
git submodule update --init --recursive
```

Alternatively, you can clone the monorepo with the `--recurse-submodules` flag:

```bash
git clone --recurse-submodules git@github.com:energinet-digitalisering/[department-name].git
```

Make sure that all submodules are checked out on the correct branch the first time you clone the monorepo. Otherwise, you might risk loosing changes as the submodule will be in a detached head state.

> [!NOTE]
> Submodules are configured to clone with SSH, so it requires adding your public SSH key to DevOps and GitHub, respectively. You will not be able to clone the submodules with HTTPS. This decision was made, as HTTPS will require authentication on every request, where as SSH can do this automatically when the public key is shared.

### Adding a submodule

```sh
git submodule add -b <branch> <ssh-url> <path>
```

### Updating a submodule

All submodules are configured to automatically update to the latest commit on the branch they are tracking.

### Removing a submodule

```sh
# Remove the submodule entry from .git/config
git submodule deinit -f <path>

# Remove the submodule directory from the superproject's .git/modules directory
rm -rf .git/modules/<path>

# Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule
git rm -f <path>
```

</details>

<details>
  <summary>Show/Hide folder structure</summary>

<!-- readme-tree start -->
```
.
├── .github
│   └── workflows
├── .vscode
├── _posts
├── github-configuration
└── projects
    ├── dotnet-commons
    ├── homelab
    ├── ksail
    └── oci-artifacts

10 directories
```
<!-- readme-tree end -->

</details>

<!-- {% endcomment %} -->

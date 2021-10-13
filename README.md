# Workflows

![GitHub Actions](https://img.shields.io/badge/githubactions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![Gradle](https://img.shields.io/badge/Gradle-02303A.svg?style=for-the-badge&logo=Gradle&logoColor=white)
![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=java&logoColor=white)

A collection of fairly useful GitHub Actions Workflows for Gradle Java Projects

## Usage

To add these actions to a gradle project, this repository can be added as a submodule by running the followning command in the root directory of the project repository:

```bash
git submodule add git@github.com:edurso/workflows.git .github/workflows
```

## List of Actions

- Build
- Deploy to Package Registry
- Generate Javadocs & Deploy to GitHub Pages

### Gradle Build

`build.yml`

Out of the box, this will use JDK11 to run a simple `./gradlew build` in a linux environment.
This will also set up any submodules within a project.

This action will run on pushes and PRs.

### Javadoc

`javadoc.yml`

This action will use gradle to generate the javadocs for a gradle project and will deploy to GitHub Pages.

The GitHub Pages for the project should be configured to run from the root of the branch `gh-pages` (default).

### Gradle Deploy

`deploy.yml`

This action must run manually and provided a version number and version name.

There are two parts to this action:

1. Create a new release with the version name and number.
2. Publish a package to the [GitHub Gradle Package Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry)

Published packages can then be used as dependencies in other projects as follows:

#### Using Packages on GitHub Gradle Package Registry

To use a package from the GitHub Registry, the GitHub Registry must be added to `build.gradle`.

```groovy
repositories {
    maven {
        name = "GitHubPackages"
        url = "https://maven.pkg.github.com/<USER>/<PACKAGE>"
        credentials {
            username = project.findProperty("gpr.user")
            password = project.findProperty("gpr.key")
        }
    }
}
```

Where `<USER>` is the publisher (user or organization) of the package and `<PACKAGE>` is the name of the package.

`gpr.user` and `gpr.key` refer to the authentication information about the person using the package.
The `~/.gradle/gradle.properties` (Unix) or `%USERPROFILE%\.gradle\gradle.properties` (Windows) file should contain the GitHub username as well as a [GitHub Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

```groovy
gpr.user=<USERNAME>
gpr.key=<TOKEN>
```

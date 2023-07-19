# Workflows

A collection of fairly useful GitHub Actions Workflows for random things.

## Usage Notes About `gradle/deploy.yml`

This action must run manually and provided a version number and version name.

There are two parts to this action:

1. Create a new release with the version name and number.
2. Publish a package to the [GitHub Gradle Package Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry)

Published packages can then be used as dependencies in other projects as follows:

### Using Packages on GitHub Gradle Package Registry

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

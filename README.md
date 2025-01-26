# xemantic-project-template

A template repository for Xemantic's Kotlin multiplatform projects

[//]: # (TODO replace title and description)

## Why?

Creating new gradle projects, with all the conventions we are using at [Xemantic](https://xemantic.com), might be a hassle.

[//]: # (TODO replace with the rationale behind the new project)

[//]: # (TODO everything starting from here can be removed in your project)

## How?

1. When creating new GitHub project choose this repository as a template
2. Follow the [CHECKLIST](CHECKLIST.md)

## Updating this template project

From time to time, it is worth to:

### Update gradlew wrapper

```shell
./gradlew wrapper --gradle-version 8.12 --distribution-type bin
```

### Update all the dependencies to the latest versions

All the gradle dependencies are managed by the [libs.versions.toml](gradle/libs.versions.toml) file in the `gradle` dir.

It is easy to check for the latest version by running:

```shell
./gradlew 
```

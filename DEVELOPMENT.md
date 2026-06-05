# Development

Maintenance notes for working on this project itself. From time to time, it is worth to update
the build tooling and dependencies.

## Update gradlew wrapper

```shell
./gradlew wrapper --gradle-version latest --distribution-type bin
```

## Update all the dependencies to the latest versions

All the gradle dependencies are managed by the [libs.versions.toml](gradle/libs.versions.toml) file in the `gradle` dir.

It is easy to check for the latest versions by running:

```shell
./gradlew dependencyUpdates --no-parallel
```

This only reports the available updates. To apply them automatically to
[libs.versions.toml](gradle/libs.versions.toml), run the
[version-catalog-update](https://github.com/littlerobots/version-catalog-update-plugin) plugin:

```shell
./gradlew versionCatalogUpdate
```

To review and pick the updates one by one instead of applying them all, use the interactive mode:

```shell
./gradlew versionCatalogUpdate --interactive
```

then apply the staged changes with:

```shell
./gradlew versionCatalogApplyUpdates
```

> [!NOTE]
> The plugin is configured in [build.gradle.kts](build.gradle.kts) to preserve the manual ordering of
> `libs.versions.toml` (`sortByKey = false`) and to keep the `kotlinTarget`, `javaTarget`, and `asm`
> version constants, which have no `version.ref` and would otherwise be removed as unused.
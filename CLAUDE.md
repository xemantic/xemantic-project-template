# CLAUDE.md

This file captures only what cannot be inferred from the codebase itself.

## Rules for editing this file

Both developers and AI agents are expected to add entries as they encounter surprises.

- **Add an entry** when you encounter something unexpected: a build quirk, a non-obvious constraint, a dependency gotcha, or any behavior that would surprise the next agent or developer.
- **Add an entry** when a developer flags an anti-pattern produced by AI — describe the anti-pattern and the preferred alternative.
- **Do not** add codebase overviews, directory listings, or anything discoverable by reading the source.
- Keep entries concise: one line per lesson, grouped under a heading if a theme emerges.

## Conventions

### Markdown authoring

Markdown files use [semantic line breaks](https://sembr.org/):
break a line after a sentence,
and optionally at clause boundaries within a long sentence,
so that diffs stay meaningful and reviewable.

There is no column width limit —
never reflow or hard-wrap a paragraph to fit some character count.
Modern editors soft-wrap Markdown visually,
see the [README](README.md#markdown-soft-wrapping-in-the-ide) for how to enable it.

## Known gotchas

- After upgrading the Gradle wrapper, `jvmTest` may fail with `NoSuchFileException: build/test-results/jvmTest/binary/in-progress-results-generic.bin`, because the results of the previous Gradle version are stale — delete `build/test-results` (or run `clean`) and retry.
- The `rootPackageJson` and `wasmRootPackageJson` tasks do not track yarn `resolution(...)` entries as inputs,
  so after changing them the lock files stay stale unless these tasks are forced with `--rerun`
  (see the comment above `npmResolutions` in `build.gradle.kts` for the full command).
- Dependabot alerts on `kotlin-js-store/yarn.lock` and `kotlin-js-store/wasm/yarn.lock` concern only the Kotlin/JS test tooling (mocha, karma, webpack),
  none of which ships in the published artifacts —
  dismiss them on GitHub as "vulnerable code is not actually used" instead of forcing versions via yarn resolutions,
  which pins majors the tooling never declared support for.
- The `Reporter option 'alsoWithHtml' has no effect` warning printed by JS/Wasm test tasks
  comes from JetBrains' own `kotlin-web-helpers` npm package ([kotlin-web-helpers#11](https://github.com/Kotlin/kotlin-web-helpers/issues/11)) —
  nothing in this build causes it and there is no knob to suppress it, so leave it alone.
- After a fresh Gradle daemon starts, the first build reports `configuration cache cannot be reused because file '.../caches/jreleaser/jreleaser/<version>/marker.txt' has changed` —
  the JReleaser plugin bumps a banner counter in that file behind Gradle's back on every configuration,
  so the cache is invalidated once per daemon lifetime and reused normally afterwards; it is harmless.

## Anti-patterns to avoid

- Do not add content to this file that is already discoverable by reading the source or build scripts — that inflates context without adding signal, reducing AI agent task success rates (see [arxiv 2602.11988](https://arxiv.org/abs/2602.11988)).

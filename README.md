# lib

The official weir module library: modules the weir project maintains,
written in weir, one file each, tested and tagged.

## What this is

A module here is vendored into a project by its bare name:

```text
weir add module semver@v0.1.0
```

That is the same as
`weir add module github.com/weir-shell/lib//semver.weir@v0.1.0 --as semver`
— the short spelling is sugar; weir expands it and does what `add
module` always does. The module lands in `.weir/modules/semver.weir`,
the lock (`.weir/lock.json`) records the raw URL at the full commit
sha, and the script imports it:

```text
import "weir:semver" as Semver
```

`weir verify` and `weir restore` treat it like any other vendored
module. `--as` renames it (`weir add module semver@v0.1.0 --as sv`).

This is not a package manager. There is no registry, no dependency
resolution and no version ranges — a module is a file you review, pin
by sha, and update on purpose.

## The modules

| name | since | what |
|---|---|---|
| semver | v0.1.0 | Semantic Versioning 2.0.0: `parse`/`tryParse`, `toString`, `build`, `isPrerelease`, `core`, `bumpMajor`/`bumpMinor`/`bumpPatch`; `[<Ord>]` — `Seq.sort`, `<`, `min`/`max` follow the spec's precedence |

## Versions

The whole repo is tagged `v0.x` (`v0.1.0`, `v0.1.1`, …); one tag pins
every module at once. Under `v0.x` any module may change shape between
tags; a project moves by re-running `add` with the new tag and
reviewing the diff of the vendored file. The tag list is the
changelog: each tag's GitHub release notes name the modules it
touched. There is no `latest`, no ranges and no branch to track —
point at a tag.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for what a module must be, and
run `weir tools/test.weir` before opening a PR.

# lib

The official weir module library: modules the weir project maintains,
written in weir, one file each, tested and tagged.

## What this is

A module here is vendored into a project by its bare name:

```text
weir add module semver
```

That picks the module's latest release and is the same as
`weir add module github.com/weir-shell/lib//modules/semver.weir@semver/v0.1.0 --as semver`
— the short spelling is sugar; weir expands it and does what `add
module` always does. The module lands in `.weir/modules/semver.weir`,
the lock (`.weir/lock.json`) records the raw URL at the full commit
sha, and the script imports it:

```text
import "weir:semver" as Semver
```

`weir verify` and `weir restore` treat it like any other vendored
module. `--as` renames it (`weir add module semver --as sv`), and
`weir add module semver@v0.1.0` pins one release.

This is not a package manager. There is no registry, no dependency
resolution and no version ranges — a module is a file you review, pin
by sha, and update on purpose.

## The modules

| name | since | what |
|---|---|---|
| semver | semver/v0.1.0 | weir ≥ v0.0.79. Semantic Versioning 2.0.0: `parse`/`tryParse`, `toString`, `isPrerelease`, `core`, `bumpMajor`/`bumpMinor`/`bumpPatch`; `[<Ord n>]` on the key — `Seq.sort`, `<`, `min`/`max` follow the spec's precedence, build metadata rides the record unnumbered |

## Versions

Each module is versioned on its own: a release is a tag
`<name>/v<semver>` (`semver/v0.1.0`, `yaml/v0.2.3`) on the commit
that changes `modules/<name>.weir`, and the tag is the version. A
tag names one module; there are no repo-wide tags.

- `weir add module semver` vendors the latest stable release — the
  highest `semver/v*` tag by semver precedence, read from the repo's
  own tag list; a prerelease (`semver/v1.0.0-rc.1`) is skipped unless
  no stable release exists. weir prints which one it picked.
- `weir add module semver@v0.1.0` pins that release.
- The tag list filtered to a module is its changelog:
  [tags?q=semver/](https://github.com/weir-shell/lib/tags?q=semver/).

Under `v0.x` a module may change shape between releases; a project
moves by re-running `add` (bare for the latest, or with the new tag)
and reviewing the diff of the vendored file. There are no ranges and
no branch to track.

A module names the weir release it needs; CI runs the latest release.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for what a module must be, and
run `weir tools/test.weir` before opening a PR.

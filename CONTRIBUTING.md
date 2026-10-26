# Contributing a module

A library module is a file a stranger will vendor by name and trust
by reading. The bar below is what `tools/test.weir` and weir itself
enforce, stated here so an author never discovers it at a user's
`add`.

## The module bar

- **One file, `modules/<name>.weir`**, starting with
  `module` (or `module Name`). A module is declarations only — a
  command or a bare expression at import is refused by weir, so this
  is stated, not policed.
- **Self-contained: no `import` at all.** Not `weir:`, not relative.
  `weir add module` refuses a `weir:` import inside a vendored file,
  and would vendor a relative one as a closure; the library wants
  neither until weir's manifest/MVS design
  (dev/plans/DESIGN-external-contracts.md in the weir repo, "customer
  three") lands. The runner greps for `import ` so the refusal is
  here, not at the user's `add`.
- **Exports are signatures.** `let name : <ty>` on its own line, then
  the implementation as a plain `let name … = …`. Anything unsigned is
  private to the module. Every signature carries a `///` doc on the
  line above it — the signature is the doc's one home (hover and
  completion read it there).
- **`let pure` where the body allows it**, so hover shows the
  `(pure)` badge. Effects where needed are fine — weir is
  effect-normal — and `weir check --can <name>.weir` shows them; that
  report is the review surface for a PR.
- **A name weir can vendor bare.** The derived alias (the name
  capitalised) must not be a builtin module: `retry`, `graph`, `tree`,
  `poll`, `log`, `seq`, `str`, `args`, `bytes`, `color`, `dir`,
  `duration`, `env`, `file`, `float`, `frontier`, `http`, `instant`,
  `json`, `map`, `net`, `option`, `path`, `plan`, `proc`, `result`,
  `secret`, `self`, `server`, `size`, `table`, `term`, `uuid`, `yaml`
  are taken — weir refuses the bare add with "derives the alias
  '…', a builtin module". The name is also the file and the import
  alias, so it is a letter followed by letters, digits or `_` (no
  hyphen, no dot). The list is checked because a future builtin could
  take a name the library already uses; the long spelling would still
  work, but the bare one would not.
- **A test, `tests/<name>.weir`.** It starts with
  `import "../modules/<name>.weir" as X`, asserts with `fail`, and exits 0 on
  green. Both files must be `weir check` clean and `weir fmt --check`
  clean.
- **The PR adds a row to README's table** (`name | since | what`),
  with `since` the tag that will carry it. A release is a tag.

## Running the checks

```text
weir tools/test.weir
```

The runner uses the weir on `PATH` (CI installs the current release),
runs `weir check` and `weir fmt --check` on every module and test,
rejects a module with an `import` line, a module without a test and a
test without a module, then runs each test. It prints one line per
check and fails with the red count.

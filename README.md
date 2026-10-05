<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->
# ruby_quality

Shared Ruby quality baseline. The repo to maintain; the place from which to pin.

Templates are parameterized by project (`gem_name`, `module_name`,
`gem_name_upcase`, `description`, `license`, `rust`); anything that only fit one
project was cut rather than parameterized.

Pure-Ruby projects are the default. Pass `--rust` to `bin/scaffold` to opt
into the RbSys native-extension Rakefile, the `rake-compiler` dependency,
and the Rust quality gates.

## Layout

* `config/rubocop.yml` — the shared RuboCop baseline. Consumers pin it by URL
  via their thin `.rubocop.yml` (`templates/rubocop.yml.erb`), so rule changes
  propagate without per-repo syncs.
* `templates/` — ERB scaffold templates (`AGENTS.md`, `Rakefile`, `Gemfile`,
  `Steepfile`, `mise.toml`, `REUSE.toml`, `.rubocop.yml`, `.gitignore`,
  `.pre-commit-config.yaml`, `bin/setup`, `bin/console`, `tasks/*.rake`,
  `tasks/*_exceptions.rb`, plus skeleton templates for the gemspec, `lib/`,
  `sig/`, `test/`, `exe/`, and `CHANGELOG.md`).
* `bin/scaffold` — renders `templates/` into a new or existing repo.
* `bin/propagate` — re-renders *shared* files into the repos you name
  (dirs or globs), skipping any whose `.rubocop.yml` doesn't pin
  ruby_quality. One command, no per-repo visits.

## Shared vs scaffold-once

*Shared* (overwritten by `bin/propagate`): `.rubocop.yml`, `Steepfile`,
`tasks/shared/*`, `bin/setup`, `.pre-commit-config.yaml`, `mise.toml`.
They exist so quality gates mean the same thing in every repo.
`.rubocop.yml` is a URL pin to this repo's `config/rubocop.yml` — rule
changes propagate with no per-repo sync. `mise.toml` and Rake tasks
have no remote-include mechanism, so those stay propagate-vendored copies.
The Rust tasks no-op without a `Cargo.toml`, so pure-Ruby consumers stay
green on shared files alone.
*Scaffold-once* (written once by `bin/scaffold`, never touched by propagate):
`AGENTS.md`, `Rakefile`, `Gemfile`, `REUSE.toml`, `bin/console`,
`.gitignore`, `tasks/rbs_exceptions.rb`, `tasks/clippy_exceptions.rb`.
*Skeleton* (written only when missing, never overwritten or propagated):
the gemspec, `lib/` structure, `sig/` types, `test/` smoke test, `exe/`
placeholder, and `CHANGELOG.md`. Scaffolding into an existing repo never
clobbers hand-written code.

Adding new files anywhere (e.g. custom `tasks/*.rake`) is always fine —
propagate only re-renders the shared list above.

## Usage

Scaffold a new project (pure Ruby by default):

```bash
bin/scaffold my_gem --dir ~/Developer --author "Kerrick Long <me@kerricklong.com>"
```

Opt into the Rust native extension:

```bash
bin/scaffold my_gem --dir ~/Developer --rust
```

Preview without writing:

```bash
bin/scaffold my_gem --dir ~/Developer --dry-run
```

Propagate shared files to named repos:

```bash
bin/propagate
```

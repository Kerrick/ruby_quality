<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->
# ruby_quality

Shared Ruby quality baseline. The repo to maintain; the place from which to pin.

Templates are parameterized by project (`gem_name`, `module_name`,
`gem_name_upcase`, `description`, `license`); anything that only fit one
project was cut rather than parameterized.

## Layout

* `config/rubocop.yml` — the shared RuboCop baseline. Consumers pin it by URL
  via their thin `.rubocop.yml` (`templates/rubocop.yml.erb`), so rule changes
  propagate without per-repo syncs.
* `templates/` — ERB scaffold templates (`AGENTS.md`, `Rakefile`, `Gemfile`,
  `Steepfile`, `mise.toml`, `REUSE.toml`, `.rubocop.yml`,
  `.pre-commit-config.yaml`, `bin/setup`, `bin/console`, `tasks/*.rake`).
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
*Scaffold-once* (written once by `bin/scaffold`, never touched by propagate):
`AGENTS.md`, `Rakefile`, `Gemfile`, `REUSE.toml`, `bin/console`,
`lib/` structure, gemspec.

Adding new files anywhere (e.g. custom `tasks/*.rake`) is always fine —
propagate only re-renders the shared list above.

## Usage

Scaffold a new project:

```bash
bin/scaffold my_gem --dir ~/Developer --author "Kerrick Long <me@kerricklong.com>"
```

Propagate shared files to named repos:

```bash
bin/propagate
```

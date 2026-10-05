# frozen_string_literal: true

#--
# SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
# SPDX-License-Identifier: AGPL-3.0-or-later
#++

require_relative "lint"

# Rust gates no-op in repos without a Cargo.toml (pure-Ruby default).
def rust_available?
  File.exist?("Cargo.toml")
end

def skip_rust(task_name)
  puts "Skipping #{task_name} (no Cargo.toml — pure-Ruby repo)"
end

namespace :rust do
  desc "Check Rust formatting with cargo fmt (skipped without Cargo.toml)"
  task :fmt do
    skip_rust("rust:fmt") unless rust_available?
    sh "cargo fmt --check" if rust_available?
  end

  namespace :fmt do
    desc "Auto-fix Rust formatting with cargo fmt (skipped without Cargo.toml)"
    task :fix do
      skip_rust("rust:fmt:fix") unless rust_available?
      sh "cargo fmt" if rust_available?
    end
  end

  desc "Run Clippy with strict settings, configured in Cargo.toml (skipped without Cargo.toml)"
  task :clippy do
    skip_rust("rust:clippy") unless rust_available?
    sh "cargo clippy -- -D warnings" if rust_available?
  end

  desc "Fast type checking with cargo check (skipped without Cargo.toml)"
  task :check do
    skip_rust("rust:check") unless rust_available?
    sh "cargo check" if rust_available?
  end

  desc "Forbid #[allow(clippy::...)] except approved exceptions (skipped without Cargo.toml)"
  task :no_allows do
    skip_rust("rust:no_allows") unless rust_available?
    next unless rust_available?

    exceptions = File.expand_path("../clippy_exceptions.rb", __dir__)
    unless File.exist?(exceptions)
      abort "tasks/clippy_exceptions.rb missing — re-scaffold to create it"
    end
    Lint.load(exceptions).enforce!(
      glob: "ext/**/*.rs",
      pattern: /#\[allow\(clippy::/,
      violation_name: "#[allow(clippy::...)]"
    )
  end
end

namespace :lint do
  desc "Run all Rust linting (clippy + fmt check + no unapproved allows)"
  task rust: %w[rust:clippy rust:fmt rust:no_allows]
end

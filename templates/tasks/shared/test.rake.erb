# frozen_string_literal: true

#--
# SPDX-FileCopyrightText: 2025 Kerrick Long <me@kerricklong.com>
# SPDX-License-Identifier: AGPL-3.0-or-later
#++

require "minitest/test_task"

# Clear the default test task created by Minitest::TestTask if it exists
Rake::Task["test"].clear if Rake::Task.task_defined?("test")

desc "Run all tests (Rust first, then Ruby; Rust is skipped without Cargo.toml)"
task test: %w[test:rust test:ruby]

namespace :test do
  desc "Run Rust tests (skipped without Cargo.toml)"
  task :rust do
    unless File.exist?("Cargo.toml")
      puts "Skipping test:rust (no Cargo.toml — pure-Ruby repo)"
      next
    end
    Rake::Task[:compile].invoke if Rake::Task.task_defined?(:compile)
    sh "cargo test"
  end

  # Create a specific Minitest task for Ruby core tests
  Minitest::TestTask.create(:core) do |t|
    t.test_globs = ["test/**/*.rb"]
  end

  desc "Run all Ruby tests"
  task ruby: ["test:core"]
end

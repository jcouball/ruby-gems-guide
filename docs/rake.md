# [Rake](https://github.com/ruby/rake)

Rake is the de facto Ruby build tool and is required for the ruby/gem Screwdriver
template to function.

To integrate `rake` into the project:

1. Declare a development dependency in your gemspec to the `rake` gem:

        :::ruby
        spec.add_development_dependency 'rake', '~> 13.0'

    and then run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing rake 13.0.1
        ...
        $

2. Write a `Rakefile` in the project root directory.  See the documentation for
each [build step](../build-steps) and  [build tool](../integrating-build-tools) your
project requires.

    A Rakefile for a new project which has not yet integrated other build tools
    should contain only:

        :::ruby
        # Copyright (c) 2020 Verizon
        # frozen_string_literal: true

    A complete Rakefile (depending on what build tools have been
    integrated) might contain:

        :::ruby
        # Copyright (c) 2020 Verizon
        # frozen_string_literal: true

        # Bump

        require 'bump/tasks'

        # Bundler Audit

        require 'bundler/audit/task'
        Bundler::Audit::Task.new

        # Bundler Gem Build

        require 'bundler'
        require 'bundler/gem_tasks'

        # RSpec

        require 'rspec/core/rake_task'

        RSpec::Core::RakeTask.new do |t|
          t.rspec_opts = %w[
            --require spec_helper.rb
            --color
            --format RspecSonarqubeFormatter --out rspec-report.xml
            --format documentation
          ]
        end

        CLEAN << 'coverage'
        CLEAN << '.rspec_status'
        CLEAN << 'rspec-report.xml'

        # Rubocop

        require 'rubocop/rake_task'

        RuboCop::RakeTask.new do |t|
          t.options = %w[
            --format progress
            --format json --out rubocop-report.json
          ]
        end

        CLEAN << 'rubocop-report.json'

3. Test by running `bundle exec rake -AT` in your project's root directory.

    This command lists the available Rake tasks. If you are starting a new project and
    have an empty Rakefile, no tasks will be listed yet.

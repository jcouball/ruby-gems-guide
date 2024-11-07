# [Rubocop](https://github.com/rubocop/rubocop)

RuboCop is a source code analyzer for Ruby.  See [the RuboCop
documentation](https://rubocop.readthedocs.io/) for more information about RuboCop.

To integrate RuboCop into your project:

1. Declare a development dependency in your gemspec to the `rubocop` gem (your
   version may vary):

        :::ruby
        spec.add_development_dependency 'rubocop', '~> 1.24'

    and then run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing rubocop 1.24.1
        ...
        $

2. Add the `rubocop` task to the Rakefile:

        :::ruby
        # Rubocop

        require 'rubocop/rake_task'

        RuboCop::RakeTask.new do |t|
          t.options = %w[
            --format progress
            --format json --out rubocop-report.json
          ]
        end

        CLEAN << 'rubocop-report.json'

3. Ensure that the `rubocop` Rake tasks were added by running `bundle exec rake -AT |
   grep 'rake rubocop'`

        :::shell
        $ bundle exec rake -AT | grep 'rake rubocop'
        rake rubocop                              # Run RuboCop
        rake rubocop:auto_correct                 # Auto-correct RuboCop offenses
        $

4. Define exceptions to the default rules in the `.rubocop.yml` file.  Here are the
recommended exceptions:

        :::yaml
        AllCops:
          NewCops: enable
          # Output extra information for each offense to make it easier to diagnose:
          DisplayCopNames: true
          DisplayStyleGuide: true
          ExtraDetails: true
          SuggestExtensions: false
          # RuboCop enforces rules depending on the oldest version of Ruby which
          # your project supports:
          TargetRubyVersion: 2.7

        # The default max line length is 80 characters
        Layout/LineLength:
          Max: 120

        # The DSL for RSpec and the gemspec file make it very hard to limit block length:
        Metrics/BlockLength:
          Exclude:
            - "spec/**/*_spec.rb"
            - "*.gemspec"

        # When writing minitest tests, it is very hard to limit test class length:
        Metrics/ClassLength:
          Exclude:
            - "test/**/*_test.rb"

5. Exclude the rubocop work files from Git by adding the following to the project
   `.gitignore`:

        :::text
        /rubocop-report.json

6. Run RuboCop via `bundle exec rake rubocop:auto_correct` in the project's root
   directory

        :::shell
        $ bundle exec rake rubocop:auto_correct
        Running Rubocop...
        Inspecting 8 files
        ...
        8 files inspected, 61 offenses detected, 61 offenses corrected
        $

    This should auto-correct all existing offenses. Manually fix any remaining
    offenses.

    Run `bundle exec rubocop` to make sure that all offenses have been cleaned up:

        :::shell
        $ bundle exec rake rubocop
        Running RuboCop...
        Inspecting 8 files
        ........

        8 files inspected, no offenses detected
        $

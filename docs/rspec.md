# [RSpec](https://rspec.info)

RSpec is a popular unit testing framework for Ruby.  See [rspec.info](http://rspec.info)
for more information about writing RSpec tests.

To integrate RSpec tests into your project:

1. Declare a development dependency in your gemspec to the `rspec` gem (your version may vary):

        :::ruby
        spec.add_development_dependency 'rspec', '~> 3.10'
        spec.add_development_dependency 'simplecov', '~> 0.21'

    The `simplecov` gem will be used to measure and report unit test code coverage.

    Run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing rspec 3.10.0
        Installing simplecov 0.21.2
        ...
        $

2. Add the `spec` task to the Rakefile:

        :::ruby
        # RSpec

        require 'rspec/core/rake_task'

        RSpec::Core::RakeTask.new

        CLEAN << 'coverage'
        CLEAN << '.rspec_status'
        CLEAN << 'rspec-report.xml'

3. Ensure that the `spec` Rake task was added by running
   `bundle exec rake -AT | grep 'rake spec'`

        :::shell
        $ bundle exec rake -AT | grep 'rake spec'
        rake spec    # Run RSpec code examples
        $

4. Configure code coverage with simplecov by replacing the `spec/spec_helper.rb`
   with the following content:

        :::ruby
        # frozen_string_literal: true

        RSpec.configure do |config|
          # Enable flags like --only-failures and --next-failure
          config.example_status_persistence_file_path = '.rspec_status'

          # Disable RSpec exposing methods globally on `Module` and `main`
          config.disable_monkey_patching!

          config.expect_with :rspec do |c|
            c.syntax = :expect
          end
        end

        # Setup simplecov

        require 'simplecov'

        SimpleCov.formatters = [SimpleCov::Formatter::HTMLFormatter]

        # Fail the rspec run if code coverage falls below the configured threshold
        #
        test_coverage_threshold = 100
        SimpleCov.at_exit do
          unless RSpec.configuration.dry_run?
            SimpleCov.result.format!
            if SimpleCov.result.covered_percent < test_coverage_threshold
              warn "FAIL: RSpec Test coverage fell below #{test_coverage_threshold}%"
              exit 1
            end
          end
        end

        SimpleCov.start

        # Make sure to require your project AFTER SimpleCov.start
        #
        require 'my_project'

5. Exclude the rspec-report.xml file from Git by adding the following to the project
   `.gitignore`:

        ::text
        /rspec-report.xml

6. Run rspec tests via `bundle exec rake spec` in the project's root directory.

    If you are setting up a new project, the spec 'MyProject does something useful' will
    fail. Let's remove this spec so that our tests succeed. Remove (or comment out)
    the following lines in `spec/my_project_spec.rb`:

        :::ruby
        it 'does something useful' do
          expect(false).to eq(true)
        end

    Run the tests again and make sure they pass.

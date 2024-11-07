# [Bundler Audit](https://github.com/rubysec/bundler-audit)

This tool provides the `bundle:audit` rake task.  This task checks to see if any of the
projects dependencies contain vulnerabilities.

To integrate audits into your project:

1. Declare a development dependency in your gemspec to the `bundler-audit` gem:

        :::ruby
        spec.add_development_dependency 'bundler-audit', '~> 0.9'

    and then run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing bundler-audit 0.9.0.1
        ...
        $

2. Add the `bundle:audit` task to your Rakefile:

        :::ruby
        # Bundler Audit

        require 'bundler/audit/task'
        Bundler::Audit::Task.new

3. Ensure that the `bundle:audit` Rake task was added by running
   `bundle exec rake -AT | grep 'rake bundle:audit'`

        :::shell
        $ bundle exec rake -AT | grep 'rake bundle:audit'
        rake bundle:audit          #
        rake bundle:audit:check    # Checks the Gemfile.lock for insecure dependencies
        rake bundle:audit:update   # Updates the bundler-audit vulnerability database
        $

4. Test by running `bundle exec rake bundle:audit` in the project's root directory

        :::shell
        $ bundle exec rake bundle:audit
        ...
        No vulnerabilities found
        $


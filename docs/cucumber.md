# [Cucumber](https://github.com/cucumber/cucumber-ruby)

Cucumber is a tool that supports Behaviour-Driven Development(BDD).  It reads
executable specifications written in plain text and validates that the software
does what those specifications say.  See
[the Cucumber documentation](https://docs.cucumber.io) for more
information about writing Cucumber tests.

To integrate Cucumber into your project:

1. Declare a development dependency in your gemspec to the `cucumber` gem (your version
   may vary):

        :::ruby
        spec.add_development_dependency 'cucumber', '~>; 5.0'

    and then run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing cucumber 5.0.0
        ...
        $

2. Add the `cucumber` task to the Rakefile:

        :::ruby
        require 'cucumber/rake/task'

        Cucumber::Rake::Task.new

3. Write cucumber tests in the `features` directory.

4. Run the cucumber tests via `bundle exec rake cucumber` in the project's root directory.

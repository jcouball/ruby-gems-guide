# [Bump](https://github.com/gregorym/bump)

The bump tool provides the `bump:patch` rake task to increment and commit the project's
gem version number.

To integrate `bump` into the project:

1. Declare a development dependency in the gemspec to the `bump` gem (your version may vary):

        :::ruby
        spec.add_development_dependency 'bump', '~> 0.10'

    and then run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing bump 0.10.0
        ...
        $

2. Add the bump tasks to your Rakefile:

        :::ruby
        # Bump

        require 'bump/tasks'

3. Ensure that the `bump` Rake tasks were added by running
   `bundle exec rake -AT | grep 'rake bump'`

        :::shell
        $ bundle exec rake -AT | grep 'rake bump'
        rake bump:current[no_args]                # Show current gem version
        rake bump:file[no_args]                   # Show version file path
        rake bump:major[no_args]                  # Bump major part of gem version
        rake bump:minor[no_args]                  # Bump minor part of gem version
        rake bump:patch[no_args]                  # Bump patch part of gem version
        rake bump:pre[no_args]                    # Bump pre part of gem version
        rake bump:set                             # Sets the version number using the VERSION environment variable
        rake bump:show-next[no_args]              # Show next major|minor|patch|pre version
        $

4. Test by running `bundle exec rake bump:current` in the project's root directory

        :::shell
        $ bundle exec rake bump:current
        0.1.0
        $

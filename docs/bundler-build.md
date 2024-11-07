# Bundler Build

This tool provides the `build` rake task.  This task builds the gem based on the
project's gemspec and places it in the `pkg` directory.

To integrate gem building into the project:

1. Add the `build` task to the Rakefile:

        :::ruby
        # Bundler Gem Build

        require 'bundler'
        require 'bundler/gem_tasks'

        begin
          Bundler.setup(:default, :development)
        rescue Bundler::BundlerError => e
          warn e.message
          warn 'Run `bundle install` to install missing gems'
          exit e.status_code
        end

        CLEAN << 'pkg'
        CLOBBER << 'Gemfile.lock'

2. Ensure that the `build` Rake task was added by running
   `bundle exec rake -AT | grep 'rake build'`

        :::shell
        $ bundle exec rake -AT | grep 'rake build'
        rake build                                # Build my_project-0.1.0.gem into the pkg directory
        rake build:checksum                       # Generate SHA512 checksum if my_project-0.1.0.gem into the checksums directory
        $

3. If you are building a gem, you should exclude `Gemfile.lock` from Git by
   adding the following to the project `.gitignore`:

        ::text
        /Gemfile.lock

4. Test by running `bundle exec rake build` in the project's root directory.

        :::shell
        $ bundle exec rake build
        my_project 0.1.0 built to pkg/my_project-0.1.0.gem.
        $

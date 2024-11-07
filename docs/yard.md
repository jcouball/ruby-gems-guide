# [YARD](https://yardoc.org)

YARD is a documentation generation tool for the Ruby programming language. It
enables the user to generate consistent, usable documentation that can be
exported to a number of formats very easily.

To integrate RuboCop into your project:

1. Declare a development dependency in your gemspec to the `yard` and `yardstick`
   gems (your versions may vary):

        :::ruby
        spec.add_development_dependency 'github_pages_rake_tasks', '~> 0.1'
        spec.add_development_dependency 'redcarpet', '~> 3.5'
        spec.add_development_dependency 'yard', '~> 0.9'
        spec.add_development_dependency 'yardstick', '~> 0.9'

    and then run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing github_pages_rake_tasks 0.1.8
        Installing redcarpet 3.5.1 with native extensions
        Installing yard 0.9.27
        Installing yardstick 0.9.9
        ...
        $

2. Add the `yard` tasks to the Rakefile:

        :::ruby
        # YARD

        require 'yard'

        YARD::Rake::YardocTask.new do |t|
          t.files = %w[lib/**/*.rb examples/**/*]
        end

        CLEAN << '.yardoc'
        CLEAN << 'doc'

        # Yardstick

        desc 'Run yardstick to show missing YARD doc elements'
        task :'yard:audit' do
          sh "yardstick 'lib/**/*.rb'"
        end

        # Yardstick coverage

        require 'yardstick/rake/verify'

        Yardstick::Rake::Verify.new(:'yard:coverage') do |verify|
          verify.threshold = 100
        end

        # Publish YARD documentation to GitHub

        require 'github_pages_rake_tasks'

        GithubPagesRakeTasks::PublishTask.new do |task|
            # task.doc_dir = 'documentation'
            task.verbose = true
        end

3. Ensure that the `yard` Rake tasks were added by running
   `bundle exec rake -AT | grep 'rake yard'`

        :::shell
        $ bundle exec rake -AT | grep 'rake yard'
        rake yard                                 # Generate YARD Documentation
        rake yard:audit                           # Run yardstick to show missing YARD doc elements
        rake yard:coverage                        # Verify that yardstick coverage is at least 100%
        $

4. Exclude the files generated from YARD from Git by adding the following to the project `.gitignore`:

        :::text
        /.yarddoc
        /doc

5. Run `bundle exec rake yard yard:audit yard:coverage` to make sure the `yard` tasks work:

        :::shell
        $ bundle exec rake yard yard:audit yard:coverage
        Files:           2
        Modules:         1 (    1 undocumented)
        Classes:         1 (    1 undocumented)
        Constants:       1 (    1 undocumented)
        Attributes:      0 (    0 undocumented)
        Methods:         0 (    0 undocumented)
        0.00% documented
        yardstick 'lib/**/*.rb'

        YARD-Coverage: 100.0%  Success: 0  Failed: 0  Total: 0
        YARD-Coverage: 100.0% (threshold: 100%)
        $

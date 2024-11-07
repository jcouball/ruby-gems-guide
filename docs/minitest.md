# [Minitest](https://github.com/minitest/minitest)

Minitest is a popular unit testing framework for Ruby.  See
[the minitest documentation](https://github.com/seattlerb/minitest) for more
information about writing minitest tests.

To integrate Minitest tests into your project:

1. Declare a development dependency in your gemspec to the `minitest` gem (your
   version may vary):

        :::ruby
        spec.add_development_dependency 'minitest', '~> 5.14'

    Run `bundle install` to install the new gems:

        :::shell
        $ bundle install
        ...
        Installing minitest 5.14.1
        ...
        $

2. Add the `test` task to the Rakefile:

        :::ruby
        require 'rake/testtask'

        Rake::TestTask.new do |t|
          t.verbose = true
          t.libs << 'test'
          t.libs << 'lib'
          t.test_files = FileList['test/**/*_test.rb']
        end

3. Write tests in the `test` directory.  Test files should be named `*_test.rb`.

4. Run minitest test via `bundle exec rake test` in the project's root directory.


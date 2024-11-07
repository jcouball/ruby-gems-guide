# Getting Started

Follow these instructions to build a new Ruby Gem project from scratch.

At the end of these instructions, you will have:

* a new Ruby Gem project in GitHub a repository
* a Screwdriver Pipeline which builds a gem and publishes it
* a new Ruby Gem published to your project's Artifactory repository

This new project will make it easy to share code and deploy shell scripts.

**Shared code** should be placed in the `lib` directory following the conventions for
Ruby Gem projects. Following this guide is a good start in following the conventions
for Ruby Gem projects.

**Shell scripts** should be placed in the `exe` directory.  Scripts from this directory
will be installed with your gem in such a way that users can run from the command line.
You must ensure that these files are executable.

## Pick a gem name

Follow [the conventions given in rubygems.org for gem naming](https://guides.rubygems.org/name-your-gem/)

The most important conventions are:

1. The gem name, the project's main .rb file, and the main class or module should relate
   to one another as described in the conventions document
2. Use underscores and dashes appropriately
3. Do not use uppercase letters

## Pick a unit test framework

Choose the test framework to be used for this project: **RSpec** or **minitest**.

Both RSpec and minitest are popular testing frameworks for Ruby that provide a complete
suite of tools that support Test Driven Development (TDD), Behavior Driven Development
(BDD), and mocking.

Either testing framework would be a good choice and it mostly comes down to personal
preference.

### RSpec

In RSpec, tests are implemented with a domain-specific language (DSL). Its tests are
written in a "Tests as Specification" manner.

RSpec is considered to be more complete than minitest. Because its DSL gives
more structure to tests than an xUnit style framework, it is considered easier maintain
tests for complex projects. On the other hand, the DSL means there is the additional
cognitive overhead when using RSpec.

Where to learn more about RSpec:

* [The Definitive RSpec Tutorial With Examples](https://www.rubyguides.com/2018/07/rspec-tutorial/)
  is a good guide for getting started with RSpec.
* [Better Specs](https://www.betterspecs.org) is a collection of best practices for
  testing with RSpec.
* [The Official Documentation for RSpec](https://relishapp.com/rspec) has detailed information
  about every aspect of RSpec.
* [Effective Testing with RSpec 3](https://www.pragprog.com/titles/rspec3/effective-testing-with-rspec-3/)
  is a great book about RSpec.
* [Rubocop RSpec](https://github.com/rubocop-hq/rubocop-rspec) is a RuboCop extension
  focused on enforcing RSpec best practices and coding conventions.

### minitest

minitest is an xUnit style framework which has assertion functions in the style
of xUnit.

minitest is "just Ruby". Because it does not have the overhead of a custom DSL, it is
considered much easier to learn and runs a lot faster than RSpec.

Where to learn more about Minitest:

* [Getting Started with Minitest](https://semaphoreci.com/community/tutorials/getting-started-with-minitest)
  is a good guide for getting started with Minitest.
* [Assert Yourself: An Introduction to Minitest](https://launchschool.com/blog/assert-yourself-an-introduction-to-minitest)
  is another good tutorial for getting started with Minitest.
* [The Official Documentation for minitest](https://github.com/seattlerb/minitest) has detailed
  information about every aspect of minitest.
* [Rubocop Minitest](https://github.com/rubocop-hq/minitest-style-guide) is a RuboCop extension
  focused on enforcing Minitest best practices and coding conventions.

## Create a skeletal project

1. Use the `bundle gem` command to create a skeletal Ruby Gem project. Review the options for
   creating your project by examining the output of `bundle gem --help`.

    Use the `--test` argument to indicate the unit testing framework to use:
    `rspec` or `minitest`:

        :::bash
        $ bundle gem my_project --no-mit --test=rspec --linter=rubocop
        Creating gem 'my_project'...
        RuboCop enabled in config
        Initializing git repo in /Users/couballj/my_projec
              create  my_project/Gemfile
              create  my_project/lib/my_project.rb
              create  my_project/lib/my_project/version.rb
              create  my_project/sig/my_project.rbs
              create  my_project/my_project.gemspec
              create  my_project/Rakefile
              create  my_project/README.md
              create  my_project/bin/console
              create  my_project/bin/setup
              create  my_project/.gitignore
              create  my_project/.rspec
              create  my_project/spec/spec_helper.rb
              create  my_project/spec/my_project_spec.rb
              create  my_project/.rubocop.yml
        Gem 'my_project' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
        $

2. Change directory to the project directory:

        :::bash
        $ cd my_project
        $

## Review the Gemfile

Remove all lines that start with `gem` from the generated `Gemfile`. For example:

    :::ruby
    gem "rake", "~> 13.0"
    gem "rspec", "~> 3.0"
    gem "rubocop", "~> 1.21"

All runtime and development dependencies will be tracked in the project's `gemspec`
file instead.

## Review the gemspec

Edit the `*.gemspec` file in your project's root directory:

1. Fill in anything that has a TODO with valid values
2. Set `allowed_push_host` to something other than `rubygems.org`.  For example:

        :::ruby
        spec.metadata["allowed_push_host"] = "N/A"

    The new gem will be sent to Artifactory via ssh instead using the typical rubygems
    interface.

3. Set the `licenses` setting to `Nonstandard`.  For example:

        :::ruby
        spec.licenses = ['Nonstandard']

4. Test by running `gem build && echo SUCCESS` in the project's root directory and fix
   any errors reported.

## Add Rake to the project

1. Remove all lines in the existing `Rakefile`. Lines will be added back to the `Rakefile`
   by following this guide.
2. Follow [the directions for integrating rake](../integrating-build-tools#rake) but DO NOT
   add any additional lines to the Rakefile
3. Test by running `bundle exec rake -AT && echo SUCCESS` in the project's root directory

    This command lists the available Rake tasks. Since the `Rakefile` is empty, no tasks
    will be listed yet.

## Add bundle:audit to the project

1. Follow [the directions for integrating bundler-audit](../integrating-build-tools#bundler-audit)
2. Test by running `bundle exec rake bundle:audit` in the project's root directory

## Add bundler gem build to the project

1. Follow [the directions for integrating bundler gem build](../integrating-build-tools#bundler-build)
2. Test by running `bundle exec rake build` in the project's root directory

## Add bump to the project

1. Follow [the directions for integrating bump](../integrating-build-tools#bump)
2. Test by running `bundle exec rake bump:current` in the project's root directory.

## Add a test framework to the project

For RSpec:

1. Follow [the directions for integrating RSpec](../integrating-build-tools#rspec)
2. Test by running `bundle exec rake spec`

For minitest:

1. Follow [the directions for integrating Minitest](../integrating-build-tools#minitest)
2. Test by running `bundle exec rake test`

## Add RuboCop to the project

1. Follow [the directions for integrating RuboCop](../integrating-build-tools#rubocop)
2. Auto-correct all the offenses by running `bundle exec rake rubocop:auto_correct`
3. Test by running `bundle exec rubocop` in the project's root directory

    All offenses should have been auto-corrected in the previous step. Manually fix any
    remaining offenses reported.

## Add YARD to the project

1. Follow [the directions for integrating YARD](../integrating-build-tools#yard)

2. Test by running `bundle exec yard yard:audit yard:coverage` in the project's root directory

3. View the project's documentation by running `open doc/index.html` from the project's
   root directory.

## Add the `default` rake task

The `default` rake task should run the same tasks that your Continuous Integration
build runs. These tasks will vary depending on what test framework you choose and
if you integrated YARD documentation into the build so you will need to tailor the
list of tasks to suite your needs.

1. Add the default task definition to the project's `Rakefile`. Place these lines at the top
   of the file after the `frozen_string_literal` comment (if it appears).

        :::ruby
        # The default task

        desc 'Run the same tasks that the CI build will run'
        task default: %w[spec rubocop yard yard:audit yard:coverage bundle:audit build]

2. Test by running `bundle exec rake`. This task should succeed without errors.  Fix any
   any errors/warnings that are reported.

## Configure the Screwdriver build

1. Add a minimal `screwdriver.yaml` file in the root directory of the project:

        :::yaml
        jobs:
          main:
            requires: [~pr, ~commit]
            template: ruby/gem

## Add the project to GitHub

1. Create a new repository in GitHub -- **DO NOT** select to add a `README.md` or `.gitignore`

2. Perform the initial commit in the local Git repository, attach your local repository to
the GitHub repository, and then push the initial commit to GitHub

        :::bash
        git add .
        git commit -m 'Initial commit'
        git remote add origin <project uri>
        git push -u origin master

    !!! Note "NOTE"
        This is the **ONLY** time you should push to master without an approved pull request.
        In order for this not to be flagged by Ministry of Code, you need to create the
        repository **WITHOUT** a README.md or .gitignore.

## Create a Screwdriver Pipeline

1. Enter your GitHub URI on [the Screwdriver Pipeline creation page](https://screwdriver.ouroath.com/create)
to create the project's Screwdriver pipeline.

2. Press the 'Start' button to attempt the first build of the project.

2. Any errors in creating the pipeline must be corrected before proceeding.

    !!! Note "NOTE"
        The Screwdriver job is not expected to completely succeed yet.

    If you followed the instructions correctly to this point, everything up to the
    `save-version-number` step will succeed. Correct any errors in the steps prior to
    this step before proceeding.

## Configure Artifactory

Follow the directions in the [publish-gem](../build-steps#publish-gem) step to configure where
the project will publish gems.

## Allow the Screwdriver Pipeline to Write to GitHub

In order to save the incremented gem version number back to the project's GitHub repository,
you will need to create an `sd.allow` file in the project's root directory.

Create the `sd.allow` file as detailed in the [save-version-number](../build-steps#save-version-number)
step.

## Add a Build Badge to the README.md

Add a link to the Screwdriver Pipeline in your README.md.  Replace `<pipeline_id>`
with your actual Screwdriver Pipeline id:

    :::text
    [Screwdriver Pipeline](https://screwdriver.ouroath.com/pipelines/<pipeline_id>)<br/>
    [![Build Status](https://screwdriver.ouroath.com/pipelines/<pipeline_id>/badge)](https://screwdriver.ouroath.com/pipelines/<pipeline_id>)

## The First Successful Build

We will use the changes to `sd.allow`, `screwdriver.yaml`, `README.md`, and
`sonar-project.properties` to trigger our first successful build.
To do this, create a branch (named `screwdriver` in the examples below) and merge it
into the `master` branch with an approved Pull Request (PR).

1. Create the branch:

        :::bash
        $ git checkout -b screwdriver
        $

2. Commit and push the changes:

        :::bash
        $ git add .
        $ git commit -m 'Finish configuring the Screwdriver build'
        $ git push --set-upstream origin screwdriver
        $

3. In the GitHub UI, create a Pull Request (PR) for the change
    * After the PR is created, a PR build should automatically be triggered
    * This build will not try to make any change outside the build environment: it won't update
      version number in GitHub or publish the new gem to Artifactory.
    * Wait for the build to finish
    * Fix any errors before proceeding.  Should you encounter any error, read the advice
      in the [Fixing build errors](../fixing-build-errors) section.

4. Have a co-worker approve your PR and the merge the PR to master.
    * A new build should be triggered.
    * This build will update the version number in GitHub and publish the new gem to Artifactory.
    * Fix any errors before proceeding.  Should you encounter any error, read the advice
      in the [Fixing build errors](../fixing-build-errors) section.

## What to do next

**Shared code** should be placed in the `lib` directory following the conventions for
Ruby Gem projects. See [Your First Gem](https://guides.rubygems.org/make-your-own-gem/#your-first-gem)
for more details on the conventions that Ruby Gems are expected to follow.

**Shell scripts** should be placed in the `exe` directory.  Scripts from this directory
will be installed with your gem in such a way that users can run from the command line.
You must ensure that these files are executable.

**Test code** should be placed in the `spec` or `test` directory depending on the testing
framework used by the project.

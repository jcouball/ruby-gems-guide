# Build Steps

This table gives a brief summary of each step.  Each step has a detail section below the table.
If you have a problem with a step, consult that step's detail section for more information.

| Steps (run in this order)    | Run In PR Build? | Description |
| :--------------------------- | :--------------: | :---------- |
| show-support-info           | Yes | Need help?  See the output of this step. |
| show-env                    | Yes | Show all environment variables -- useful for debugging |
| restore-bundle-from-cache   | Yes | Restore vendor/bundle from cache to avoid installing gems from scratch for every build |
| show-bundle-config          | Yes | Shows the output of `bundle config`.  Useful for troubleshooting `bundle install`. |
| [install-dependencies](#install-dependencies) | Yes | Install gems (and dependencies) listed in the Gemfile and/or gemspec using `bundle install` |
| [audit-dependencies](#audit-dependencies) | Yes | Fails the build if the project's gem dependencies contain known vulnerabilities |
| [show-available-rake-tasks](#show-available-rake-tasks) | Yes | Show all rake tasks -- useful for debugging |
| [test](#test)               | Yes | Runs the rake tasks listed in the `TEST_TASKS` environment variable.  Default tasks are `spec`, `test`, and `cucumber`. Undefined tasks are skipped. |
| [analyze-code](#analyze-code) | Yes | Runs the rake tasks listed in the `ANALYZE_CODE_TASKS` environment variable.  Default task is `rubocop`. Undefined tasks are skipped. |
| [increment-version-number](#increment-version-number)  | Yes | Runs the rake tasks listed in the `INCREMENT_VERSION_NUMBER_TASKS` environment variable.  Default task is `bump:patch`.  If this task is not defined, the build will fail. |
| [build-gem](#build-gem)     | Yes | Runs the rake tasks listed in the `BUILD_GEM_TASKS` environment variable.  Default task is `build`.  If this task is not defined, the build will fail. |
| [build-documentation](#build-documentation) | Yes | Runs the rake tasks listed in the `GENERATE_DOCUMENTATION_TASKS` environment variable.  Default tasks are `rdoc` and `yard`. Undefined tasks are skipped. |
| [save-version-number](#save-version-number) | No  | Push the new version number to Github. |
| [publish-gem](#publish-gem) | No  | Publish the gem found in the `pkg` directory to `${ARTIFACT_REPOSITORY}/gems/${GEM}` |
| [publish-documentation](#publish-documentation) | No  | Runs the rake tasks listed in the `PUBLISH_DOCUMENTATION_TASKS` environment variable.  The default task is `publish-github-pages`. |
| save-bundle-to-cache        | No  | Save vendor/bundle to the cache for the next build |

## install-dependencies

This step installs the build's gem dependencies using `bundle install`.
You will need to provide a Gemfile and a gemspec in your project's root directory following
these directions:

* [bundle-install](https://bundler.io/v2.0/man/bundle-install.1.html)
* [Gemfile](https://bundler.io/v2.0/man/gemfile.5.html)
* [.gemspec](https://guides.rubygems.org/specification-reference/)

### Configuring Bundler

Bundler options that apply to BOTH local builds and Screwdriver builds should be
set in a `.bundle` file in the project's root directory.

Screwdriver specific bundler options can be set by using the `BUNDLE_INSTALL_OPTIONS`
environment variable in your `screwdriver.yaml` file.

The Screwdriver build sets the `BUNDLE_PATH` environment variable to
`vendor/cache`. This directory is cached between builds to save time when installing
gems.  This will override the path setting in the `.bundle` file, but will not
override the path set in `BUNDLE_INSTALL_OPTIONS`.

See [bundle-config](https://bundler.io/v2.0/man/bundle-config.1.html) for more information
about configuring Bundler.

## show-available-rake-tasks

This step will list the tasks defined in your Rakefile using `bundle exec rake -AT`.
This can be helpful in understanding why some steps were skipped.

For this step to succeed, you must integrate rake into this project:

* [Integrate Rake](../integrating-build-tools#rake)

## audit-dependencies

This step is implemented with `rake`.  See the section about
[Build Steps Implemented With Rake](#build-steps-implemented-with-rake).

For this step to succeed, you must integrate a dependency scanning tool:

* [Integrate bundler-audit](../integrating-build-tools#bundler-audit)

## test

This step is implemented with `rake`.  See the section about
[Build Steps Implemented With Rake](#build-steps-implemented-with-rake).

Tests are optional, but highly recommended. If you choose to run tests,
you must integrate one or more testing frameworks such as:

* [Integrate minitest](../integrating-build-tools#minitest)
* [Integrate RSpec](../integrating-build-tools#rspec)
* [Integrate Cucumber](../integrating-build-tools#cucumber)

## analyze-code

This step is implemented with `rake`.  See the section about
[Build Steps Implemented With Rake](#build-steps-implemented-with-rake).

Static code analysis is optional but highly recommended.  If you choose
to run code analysis, you must integrate one or more code analysis tools
such as:

* [Integrate RuboCop](../integrating-build-tools#rubocop)

## build-gem

This step is implemented with `rake`.  See the section about
[Build Steps Implemented With Rake](#build-steps-implemented-with-rake).

This step is expected to create the gem in the `pkg` directory.  This is the
default behavior of the rubygems `build` rake task.

For this step to succeed, you must integrate a gem building tool into the project:

* [Integrate bundler gem build](../integrating-build-tools#bundler-gem-build)

## build-documentation

This step is implemented with `rake`.  See the section about
[Build Steps Implemented With Rake](#build-steps-implemented-with-rake).

Building documentation is optional.  If you choose
to build documentation, you must integrate one or more documentation building tools
into the project:

* [Integrate RDoc](../integrating-build-tools#rdoc)
* [Integrate Yard](../integrating-build-tools#yard)

## publish-documentation

This step is implemented with `rake`.  See the section about
[Build Steps Implemented With Rake](#build-steps-implemented-with-rake).

Publishing documentation is optional.

## increment-version-number

This step is implemented with `rake`.  See the section about
[Build Steps Implemented With Rake](#build-steps-implemented-with-rake).

The new version number is saved in an environment variable `GEM_VERSION` which can be
referenced in later build steps.

The `save-version-number` step pushes this commit back to Github.

For this step to succeed, you must integrate the bump tool into the project:

* [Integrate bump](../integrating-build-tools#bump)

## save-version-number

This step pushes the commit created in the `increment-version-number` step to Github
so that the next build will know the last version number.

1. In order to grant Screwdriver the permission to push to your Github repository, you will
need to create an `sd.allow` file containing the following (substitute your Screwdriver
project number):

        :::yaml
        version: 1
        push: ["screwdriver:1005684"]

    Failure to do this will result in the following error during the `save-version-number`
    step:

        :::text
        Error: sd.allow not found
        Error: SSH principal 'screwdriver:1037004' is not allowed to push to 'couballj/my_project'

2. In GitHub, give the `by-screwdriver` user write access to the project's repository.

    Failure to do this will result in the following error during the `save-version-number`
    step:

        :::text
        ERROR: Permission to couballj/my_project.git denied to by-screwdriver.

## publish-gem

This publishes the gem found in the `pkg` directory to https://rubygems.org.

**TODO**: add instructions here

## Build Steps Implemented With Rake

The `audit-dependencies`, `test`, `analyze-code`, `build-gem`, `build-documentation`,
and `publish-documentation` build steps are all implemented by calling `rake`.

There are two Screwdriver environment variables for each step:
* one that defines the rake task(s) to be run and what order they are run
* the other defines the behavior of the build if any of those tasks are not defined
in the `Rakefile`.

These variable names have the step name as the root of the name and a suffix defining which
variable it is.  The step name transformed to upper-case and has dashes changed to
underscores.  The general form is:

* `<STEP_NAME>_TASKS`: a comma delimited list of rake tasks
* `<STEP_NAME>_IS_MANDATORY`: either "y" or "n"

For instance, for the `build-documentation` step, the variables are:

    :::yaml
    BUILD_DOCUMENTATION_TASKS: rdoc, yard
    BUILD_DOCUMENTATION_IS_MANDATORY: n

Set the `<STEP_NAME>_TASKS` variable to an empty string to bypass that step.

If the `<STEP_NAME>_IS_MANDATORY` variable is 'y', the build will fail if any task listed in
the `<STEP_NAME>_TASKS` variable are not defined in the `Rakefile`.  Otherwise, undefined
tasks will be skipped without a build error.

The default definition for each of these build steps is:

| Step                    | Default TASKS        | Default IS_MANDATORY | Provided By |
| :---------------------- | :------------------- | :------------------- | :---------- |
| `audit-dependencies`    | bundle:audit         | y                    | [bundler-audit](../integrating-build-tools#bundler-audit) |
| `tests`                 | test, spec, cucumber | n                    | [minitest](../integrating-build-tools#minitest), [rspec](../integrating-build-tools#rspec), [cucumber](../integrating-build-tools#cucumber) |
| `analyze-code`          | rubocop              | n                    | [rubocop](../integrating-build-tools#rubocop) |
| `increment-version`     | bump:patch           | y                    | [bump](../integrating-build-tools#bump) |
| `build-gem`             | build                | y                    | [bundler gem build](../integrating-build-tools#bundler-gem-build) |
| `build-documentation`   | rdoc, yard           | n                    | [rdoc](../integrating-build-tools#rdoc), [yard](../integrating-build-tools#yard) |
| `publish-documentation` | publish-gh-pages     | n                    | TODO |

In order for these tasks to be successful, the build dependencies for each task
need to be defined in the project's gemspec.  See [Integrating Build Tools](../integrating-build-tools)
for instructions for specific build tools like rspec, rubocop, etc.

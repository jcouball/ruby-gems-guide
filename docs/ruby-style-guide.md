# Ruby Style Guide

This guide can be accessed at [yo/rubystyleguide](http://yo/rubystyleguide)

As a prerequisite to a code review, all Ruby code at Verizon Media must:

* Follow the [rubocop-hq Ruby Style Guide](https://rubystyle.guide)
* Pass the default [RuboCop](https://github.com/rubocop-hq/rubocop) checks (with Exceptions
  and Additions noted below)

!!! Note "Skip the Lecture"
    Click here to go directly to
    [the directions for integrating RuboCop into your project](../integrating-build-tools#rubocop)

## rubocop-hq Ruby Style Guide

The [rubocop-hq Ruby Style Guide](https://rubystyle.guide) is the defacto standard style guide
in the Ruby community.  It is the Ruby equivalent of the
[PEP-8 Python Style Guide](https://legacy.python.org/dev/peps/pep-0008/)
or the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html).

The rubocop-hq Ruby Style Guide differs in many ways from
[Verizon Media's Generic Style Guide](https://docs.google.com/document/d/1E5ZvphTQWxDqQoAr5RB09V-0tbb7iY1gKXVX6e4dlNo/edit).
Where there are differences, favor the rubocop-hq Ruby Style Guide.

## RuboCop

[RuboCop](https://github.com/rubocop-hq/rubocop) is a static code analyzer that is
the defacto standard for checking compliance to the rubocop-hq Ruby Style Guide.

See [Integrating RuboCop](../integrating-build-tools#rubocop) for instructions to integrate
RuboCop into a project.  The instructions include configuration for the exceptions and additions
mentioned below.

## Exceptions to RuboCop Defaults

The following exceptions are allowed from the RuboCop defaults.

### Line Length

RuboCop defaults to a max line length of 80 characters.  Many people wanted this to be
wider.  In other languages, they have selected 100 characters, so that is what we will
choose here.  This exception is configured by adding the following to the project's
`.rubocop.yml` file:

    # The default max line length is 80 characters
    Metrics/LineLength:
      Max: 100

### Block size for RSpec and gemspec files

The DSL for RSpec and gemspec files make it very hard to limit block length.  It is
acceptable to exclude RSpec and gemspec files from the Block Length check by adding
the following to the projects `.rubocop.yml` file:

    # The DSL for RSpec and the gemspec file make it very hard to limit block length:
    Metrics/BlockLength:
      Exclude:
        - "spec/**/*_spec.rb"
        - "*.gemspec"

### Class size for Test::Unit and Minitest files

Test::Unit and Minitests are implemented in classes.  It can be very hard to keep
class size of these files to the same recommendation as other files.  It is
acceptable to exclude these files by adding the following to the project's
`.rubocop.yml` file:

    # When writing minitest tests, it is very hard to limit test class length:
    Metrics/ClassLength:
      Exclude:
        - "test/**/*_test.rb"

## Additions to RuboCop Defaults

The following Verizon Media specific checks should be run in addition to the
default RuboCop cops.

### Require Copyright

Verizon Media's generic style guide requires that each source code file include a
copyright notice.  This is accomplished by adding the following the project's
`.rubocop.yml` file:

    :::yaml

    Style/Copyright:
      Enabled: true
      Notice: 'Copyright (\(c\) )?2019 Oath Inc. All Rights Reserved'
      AutocorrectNotice: "# Copyright (c) 2019 Oath Inc. All Rights Reserved\n"

After the end of 2019, use this configuration:

    :::yaml

    Style/Copyright:
      Enabled: true
      Notice: 'Copyright (\(c\) )?2020 Verizon'
      AutocorrectNotice: "# Copyright (c) 2020 Verizon Media Inc.\n"

## Running RuboCop Locally

Ideally, you should run RuboCop as you make changes.

[Guard](https://github.com/guard/guard) is a tool that can be configured (using
[guard-rubocop](https://github.com/yujinakayama/guard-rubocop)) to run RuboCop in
a terminal window everytime a source code files changes.

Source code editors can be configured to run RuboCop in the background and
highlight RuboCop offenses.  Examples include:

* [RubyMine](https://www.jetbrains.com/help/ruby/rubocop.html)
* [IntelliJ](https://www.jetbrains.com/help/idea/robocop.html)
* [Sublime](https://gist.github.com/velveetachef/d161a1c31a339e3f5f2d)
* [Atom](https://atom.io/packages/linter-rubocop)
* [VIM](https://github.com/ngmy/vim-rubocop)

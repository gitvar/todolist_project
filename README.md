\# Course 130: Ruby Foundations: More Topics:

\# Lesson 3.6: Packaging Code into a Project:

\# todolist_project

 

Preparing A Rubygem
===================

 

Most Ruby projects use Rubygems as the distribution mechanism. This requires
that you observe certain practices when building your project. Specifically, you
must use a common directory structure and you must supply a `.gemspec` file.
Most Gems also include both a `Gemfile` and a `Rakefile`, but don't require
them.

As it happens, we've been using the Rubygems layout in this lesson: we've put
our main source code in the `lib` directory, our tests in the `test` directory,
and we've set up a `Gemfile` and a `Rakefile`.

When you're ready to prepare your project for distribution, you should
investigate the requirements for Rubygems. You can find useful information in
the [Rubygems documentation](http://guides.rubygems.org); it provides the
details you'll need to finish preparing your project as a Rubygem. The steps
required include:

-   Read the documentation.

-   Prepare any additional directories that you need.

-   Prepare a `README.md` file.

-   Write documentation if necessary.

-   Prepare your `.gemspec` file. Note that the actual `.gemspec` file has a
    name like `project.gemspec`, where "project" is your project name. See below
    for an example `.gemspec`.

-   Add a `gemspec` statement to your `Gemfile`.

Gemfile:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
source 'https://rubygems.org'

ruby '~> 2.2'

gemspec
gem 'stamp'
gem 'minitest', '~> 5.10'
gem 'minitest-reporters', '~> 1.1'
gem 'rake'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remember to now run `bundle install` again in order to recreate the
`Gemspec.lock` file.

 

### A Simple .gemspec

The following shows a simple `.gemspec` file for our Todo List Manager project:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
todolist_project.gemspecGem::Specification.new do |s|
  s.name        = 'todolist_project'
  s.version     = '1.0.0'
  s.summary     = 'Todo List Manager!'
  s.description = 'This is a simple todo list manager!'
  s.authors     = ['Pete Williams']
  s.email       = 'pw@example.com'
  s.homepage    = 'http://example.com/todolist_project'
  s.files       = ['lib/todolist_project.rb', 'test/todolist_project_test.rb']
end
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The name of this file is `todolist_project.gemspec`.

 

### Modify Your Rakefile

Add the following statement near the top of your `Rakefile`:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
require "bundler/gem_tasks"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

### Building your new gem

The `require bundler/gem_tasks` line in the Rakefile adds several tasks to your
Rakefile that are common to Rubygems (`rake build`, `rake install` and `rake
release` are the most important of these tasks).

 

`rake build`:

Constructs a `.gem` file in the `pkg` directory. This file contains the actual
Rubygem that you will distribute.

If you now run `bundle exec rake build`, the directory structure will look like
this:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
├── Gemfile
├── Gemfile.lock
├── README.md
├── Rakefile
├── lib
│   └── todolist_project.rb
├── pkg
│   └── todolist_project-1.0.0.gem
├── test
│   └── todolist_project_test.rb
└── todolist_project.gemspec
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The new gem is in the **./pkg** directory.

 

### Installing the new gem on your system

`rake install`:

Runs `rake build` then installs the program in your Ruby's Gem directory. This
way, you can test the Gem without having to load information from your project
directory.

 

Now run the `bundle exec rake install` command and **check** the **gems
directory** to see that the new gem was actually installed:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ ll ~/.rvm/gems/ruby-2.2.0/gems

total 0
drwxr-xr-x  18 bennie  staff   612B Mar 11 20:10 ./
drwxr-xr-x  11 bennie  staff   374B Mar  4 20:28 ../
drwxr-xr-x   6 bennie  staff   204B Mar 11 12:01 activesupport-4.2.1/
drwxr-xr-x  12 bennie  staff   408B Mar  4 20:34 ansi-1.5.0/
drwxr-xr-x  10 bennie  staff   340B Mar  4 20:35 builder-3.2.3/
drwxr-xr-x  21 bennie  staff   714B Mar 11 12:26 bundler-1.14.6/
drwxr-xr-x  14 bennie  staff   476B Mar 11 12:01 gemspec-0.3.1/
drwxr-xr-x   7 bennie  staff   238B Mar 11 12:01 i18n-0.8.1/
drwxr-xr-x  22 bennie  staff   748B Mar 11 12:01 json-1.8.6/
drwxr-xr-x  11 bennie  staff   374B Mar 11 12:01 memoist-0.12.0/
drwxr-xr-x  10 bennie  staff   340B Mar  4 20:35 minitest-5.10.1/
drwxr-xr-x  14 bennie  staff   476B Mar  4 20:36 minitest-reporters-1.1.14/
drwxr-xr-x  17 bennie  staff   578B Mar 11 20:06 rake-12.0.0/
drwxr-xr-x   7 bennie  staff   238B Mar  4 20:35 ruby-progressbar-1.8.1/
drwxr-xr-x  14 bennie  staff   476B Mar  5 23:47 stamp-0.6.0/
drwxr-xr-x  16 bennie  staff   544B Mar 11 12:01 thread_safe-0.3.6/
drwxr-xr-x   4 bennie  staff   136B Mar 11 20:10 todolist_project-1.0.0/
drwxr-xr-x  10 bennie  staff   340B Mar 11 12:01 tzinfo-1.2.2/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See that the 2nd last gem listed above is the new gem?

 

### Testing your new gem

Now create a new directory and copy the `todolist_project_test.rb` file to that
directory:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ mkdir ~/my_gem_test_directory
$ cp ./test/todolist_project_test.rb  ~/my_gem_test_directory
$ cd ~/my_gem_test_directory
$ ll
total 8
drwxr-xr-x    3 user  staff   102B Mar 12 14:54 ./
drwxr-xr-x+ 107 user  staff   3.6K Mar 12 14:53 ../
-rw-r--r--    1 user  staff   3.8K Mar 12 14:54 todolist_project_test.rb
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

Change the code in your `todolist_project_test.rb` file to reference the new gem
and not the code in the original lib directory:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
require 'bundler/setup'
require 'minitest/autorun'
require 'minitest/reporters'
require 'date'
# require_relative '../lib/todolist_project' # Old line commented out.
require 'todolist_project' # New line added to require the new gem.

Minitest::Reporters.use!

class TodoListTest < MiniTest::Test
  def setup
    @todo1 = Todo.new('Buy milk')
    ...
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

Run the code in the new directory to check that the gem is correctly used:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ ruby ~/my_gem_test_directory/todolist_project_test.rb

Started with run options --seed 46062
  23/23: [============================================================================] 100% Time: 00:00:00, Time: 00:00:00
Finished in 0.00338s
23 tests, 39 assertions, 0 failures, 0 errors, 0 skips
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

**Voilà! New gem used correctly!**

 

`rake release`:

Send your `.gem` file to the remote Rubygems library for the world to download.
**Don’t do this now.**

 

\< end of file \>

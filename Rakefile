# require 'rake/testtask'
# require 'find'
require "bundler/gem_tasks"

# desc 'Display inventory of all files'
# task :inventory do
#   Find.find('.') do |name|
#     next if name.include?('/.') # Excludes files and directories with . names
#     puts name if File.file?(name)
#   end
# end

desc 'Say hello'
task :hello do
  puts "Hello there! This is the 'hello' task."
  puts
end

desc 'Run tests'
task :tests do
  sh 'ruby ./test/todolist_project_test.rb'
end

desc 'default'
task default: :tests

# Rake::TestTask.new(:test) do |t|
#   t.libs << "test"
#   t.libs << "lib"
#   t.test_files = FileList['test/**/*_test.rb']
# end

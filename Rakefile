require 'bundler/setup'
require 'bundler/gem_tasks'
require 'rake/testtask'
require 'rdoc/task'
autoload :Parallel, 'parallel'

Rake::RDocTask.new do |rdoc|
  if File.exist?('VERSION')
    version = File.read('VERSION')
  else
    version = ''
  end

  rdoc.rdoc_dir = 'rdoc'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.title = "timecop #{version}"
  rdoc.rdoc_files.include('README*')
  rdoc.rdoc_files.include('History.rdoc')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

task :test do
  test_files = Dir['test/*_test.rb']
  thread_count = ENV['DEPARALLELIZE'] ? 1 : 8
  failed = Parallel.map(test_files, in_threads: thread_count) do |test|
    command = "ruby #{test}"
    puts
    puts command
    command unless system(command)
  end.compact
  abort "#{failed.count} Tests failed\n#{failed.join("\n")}" if failed.any?
end

desc 'Default: run tests'
task :default => [:test]

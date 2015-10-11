require 'rubygems'
require 'cucumber'
require 'cucumber/rake/task'
require 'parallel'
require 'json'
require 'yard'
require 'fileutils'

ADB_SERIAL = "your_serial_number"

task :cleanup do
puts " ========Deleting old reports ang logs========="
FileUtils.rm_rf('reports')
File.delete("cucumber_failures.log") if File.exist?("cucumber_failures.log")
File.new("cucumber_failures.log", "w")
Dir.mkdir("reports")
end

task :parallel_run do
puts "===== Executing Tests in parallel"
system "bundle exec parallel_cucumber features/ -o \"-p parallel -p poltergeist -p pretty\" -n 10"
puts " ====== Parallel execution finished and cucumber_failure.log created ========="
end

task :rerun do
if File.size("cucumber_failures.log") == 0
puts "==== No failures. Everything Passed ========="
else
puts " =========Re-running Failed Scenarios============="
system "bundle exec cucumber @cucumber_failures.log -f pretty"
end
end

task :parallel_cucumber => [ :cleanup, :parallel_run, :rerun  ]

# Rake::Task["parallel_cucumber"].invoke

YARD::Rake::YardocTask.new(:yard) do |t|
t.files   = ['features/**/*.feature', 'features/**/*.rb']
end

Cucumber::Rake::Task.new(:selenium) do |t|
  t.cucumber_opts = "features -p selenium --format pretty --profile html "
end


Cucumber::Rake::Task.new(:poltergeist) do |t|
  t.cucumber_opts = "features -p poltergeist --format pretty --profile html "
end

Cucumber::Rake::Task.new(:chrome) do |t|
  t.cucumber_opts = "features -p chrome --format pretty --profile html "
end

Cucumber::Rake::Task.new(:sauce) do |t|
  t.cucumber_opts = "features -p sauce --format pretty --profile html "
end

Cucumber::Rake::Task.new(:browserstack) do |t|
  t.cucumber_opts = "features -p browserstack --format pretty --profile html "
end

Cucumber::Rake::Task.new(:testingbot) do |t|
  t.cucumber_opts = "features -p testingbot --format pretty --profile html "
end

Cucumber::Rake::Task.new(:appium) do |t|
  puts "===== Make sure you have started Appium server in the background====="
  puts "===== You can start it with rake task start_appium ....====="
  t.cucumber_opts = "features -p appium_android_web ADB_SERIAL=#{ADB_SERIAL} --format pretty --profile html "
end

task :cuke_sniffer do
  sh "bundle exec cuke_sniffer --out html reports/cuke_sniffer.html"
end

task :rubocop do
  sh "bundle exec rubocop features/"
end

task :start_appium do
puts "===== Installing Appium with NodeJS====="
sh "npm install"
sh " ./node_modules/.bin/appium > /dev/null 2>&1"
end

task :police => [ :cleanup, :cuke_sniffer]

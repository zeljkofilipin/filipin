desc 'run all tasks'
task default: %i[rubocop yaml_lint]

desc 'run markdown linter'
task :md_lint do
  sh "find . -regex '.*md' | xargs mdl -c .mdlrc"
end

require 'rubocop/rake_task'
desc 'run ruby linter'
RuboCop::RakeTask.new

desc 'run yaml linter'
task :yaml_lint do
  sh "find . -not -path './vendor/*' -regex '.*yml' | xargs yaml-lint"
end

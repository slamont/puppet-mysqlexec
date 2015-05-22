require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'
PuppetLint.configuration.send('disable_80chars')
PuppetLint.configuration.ignore_paths = ["spec/**/*.pp", "pkg/**/*.pp"]

desc "Validate manifests, templates, and ruby files"

task :cleaner do
  Dir['manifests/**/*.pp'].each do |manifest|
    puts 'WARNING: ALWAYS Review files after running this task !'
    puts ' --> Running cleaner ...'
    sh "puppet-clean -a -b -e -l -m -o -r -t2 -v -w #{manifest} > #{manifest}.new"
    puts " --> Backing up original file #{manifest} to #{manifest}.bak ..."
    sh "mv #{manifest} #{manifest}.bak"
    puts ' --> Putting new file in place ...'
    sh "mv #{manifest}.new #{manifest}"
  end
  puts 'THIS TASK DOES AGRESSIVE CLEANUP'
  puts 'ALWAYS REVIEW THE CHANGES THAT WAS MADE'
  puts 'BACKUPS END WITH .bak'
end

task :validate do
  Dir['manifests/**/*.pp'].each do |manifest|
    sh "puppet parser validate --noop #{manifest}"
  end
  Dir['spec/**/*.rb','lib/**/*.rb'].each do |ruby_file|
    sh "ruby -c #{ruby_file}" unless ruby_file =~ /spec\/fixtures/
  end
  Dir['templates/**/*.erb'].each do |template|
    sh "erb -P -x -T '-' #{template} | ruby -c"
  end
end

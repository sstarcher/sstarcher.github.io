require 'rubygems'
require 'tmpdir'

require 'bundler/setup'
require 'jekyll'

GITHUB_REPONAME = 'sstarcher/sstarcher.github.io'.freeze

desc 'Build site with Jekyll'
task :serve, [:production] do |t, args|
    puts 'Rakefile: Building with a development configuration.'
    sh 'jekyll serve --trace --config _config.yml --watch --incremental'
end

desc 'Generate blog files'
task :generate do
  Jekyll::Site.new(Jekyll.configuration(
    source:      '.',
    destination: '_site'
  )).process
end

desc 'Download recipe updates'
task :recipes do
  system 'git checkout source'
  system 'rm -f ~/github/paprika-exporter/_data/recipes_status.json'
  system 'cd ~/github/paprika-exporter/ && pipenv run ~/github/paprika-exporter/export.py'
  system 'rm -rf images/recipes && mv ~/github/paprika-exporter/assets/images/recipes/ images/'
  system 'cp ~/github/paprika-exporter/_data/recipes.yaml _data/'
  system 'rm -rf assets'
end

desc 'Generate and publish blog to gh-pages'
task publish: [:generate] do
  Dir.mktmpdir do |tmp|
    cp_r '_site/.', tmp

    pwd = Dir.pwd
    Dir.chdir tmp

    system 'git init'
    system 'git add .'
    message = "Site updated at #{Time.now.utc}"
    system "git commit --author='Shane Starcher <shane.starcher@gmail.com>' -m #{message.inspect}"
    system 'git remote add origin git@github.com:sstarcher/sstarcher.github.io.git'
    system 'git push origin master --force'

    Dir.chdir pwd
  end
end

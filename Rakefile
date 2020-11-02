# frozen_string_literal: true

require 'json'
require 'openssl'
require 'open-uri'
require 'yaml'
require 'time'
require 'nokogiri'
require 'filewatcher'
require 'dotenv/load'

def save_data(name, object)
  file = "_data/#{name}.yml"
  File.write(file, object.to_yaml)
  puts "saved to #{file}"
end

def load_data(name)
  file = "_data/#{name}.yml"
  YAML.load_file(file)
end

def github_fetch(url)
  options = {
    http_basic_authentication: ['sobstel', ENV['GITHUB_PAT']],
    ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE
  }
  open(url, options).read
end

desc 'Dev/watch'
task :watch do
  sh 'bundle exec jekyll serve'
end
task dev: :watch

desc 'Publish live'
task :publish, :message do |_, args|
  args.with_defaults message: YAML.load_file('quotes.yml')['quotes'].values.flatten.sample
  message = args[:message]

  task(:import_gists).execute

  sh 'git add --all .'
  sh "git commit . --message=\"#{message}\""
  sh 'git pull --rebase'
  sh 'git push'
end

desc 'Import GitHub gists'
task :import_gists do
  gists = load_data('gists')
  since = gists.first['date'].to_s

  url = format('https://api.github.com/users/%s/gists?since=%s', 'sobstel', URI.encode(since))
  puts "fetch from #{url}"

  new_gists = JSON.parse(github_fetch(url)).select do |gist|
    gist['public']
  end

  new_gists.reject do |gist|
    gist['created_at'] <= since
  end.reverse.each do |gist|
    gists.unshift(
      'title' => gist['description'],
      'url' => gist['html_url'],
      'date' => gist['created_at']
    )
  end

  save_data('gists', gists)
end

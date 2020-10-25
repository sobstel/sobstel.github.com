# frozen_string_literal: true

require 'json'
require 'openssl'
require 'open-uri'
require 'yaml'
require 'time'
require 'nokogiri'
require 'filewatcher'
require 'gemoji-parser'
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

def fetch_repos(url)
  JSON.parse(github_fetch(url)).reject do |repo|
    repo['archived']
  end.sort_by do |repo|
    repo['pushed_at']
  end.collect do |repo|
    repo.select do |key, _|
      %w[name full_name description html_url fork languages stargazers_count].include? key
    end.tap do |repo|
      repo['description'] = EmojiParser.detokenize(repo['description']) if repo['description']
    end
  end.reverse
end

def generate_ronn_page(file)
  sh({ 'RONN_LAYOUT' => '_layouts/_ronn.html' }, "ronn --html #{file}")
end

task :watch do
  # task(:generate_ronn_pages).execute
  # thread = Thread.new(Filewatcher.new(['*.ronn'])) do |fw|
  #   fw.watch { |file| generate_ronn_page(file) }
  # end
  sh 'bundle exec jekyll serve'
  thread.join
end
task dev: :watch

desc 'Publish live'
task :publish, :message do |_, args|
  args.with_defaults message: YAML.load_file('quotes.yml')['quotes'].values.flatten.sample
  message = args[:message]

  task(:generate_ronn_pages).execute
  task(:import_github_repos).execute
  task(:import_gists).execute
  task(:import_github_contributions).execute
  # task(:import_medium_posts).execute

  sh 'git add --all .'
  sh "git commit . --message=\"#{message}\""
  sh 'git pull --rebase'
  sh 'git push'
end

desc 'Generate ronn pages'
task :generate_ronn_pages do
  Dir.glob('*.ronn').each do |file|
    generate_ronn_page(file)
  end
end

desc 'Import GitHub repos'
task :import_github_repos do
  url = format('https://api.github.com/users/%s/repos', 'sobstel')
  puts "fetch from #{url}"

  all_repos = fetch_repos(url)

  repos, forks = all_repos.partition { |repo| !repo['fork'] }
  popular_repos, other_repos = repos.partition do |repo|
    next repo['stargazers_count'] >= 9
  end

  save_data('popular_repos', popular_repos)
  save_data('other_repos', other_repos)
  save_data('forks', forks)
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

desc 'Import GitHub contributions'
task :import_github_contributions do
  github_contributions = load_data('contribs')

  # last 3 months
  3.downto(0) do |i|
    from = DateTime.now - 30 * i
    to = DateTime.now - 30 * (i - 1)

    url = format('https://github.com/%s?tab=contributions&from=%s&to=%s', 'sobstel', from.to_s, to.to_s)
    puts "fetch from #{url}"

    html_doc = Nokogiri::HTML(github_fetch(url))
    contributions = html_doc.css('.contribution-activity-listing a[data-hovercard-type="repository"]')

    contributions.each do |contribution|
      github_contributions << contribution.text.strip
    end
  end

  github_contributions.uniq!
  github_contributions.reject! { |repo| repo.start_with?('sobstel', 'golazon', 'hydropuzzle') }
  github_contributions.sort_by!(&:downcase)

  save_data('contribs', github_contributions)
end

desc 'Import Medium posts'
task :import_medium_posts do
  url = 'https://medium.com/feed/@sobstel'
  puts "fetch from #{url}"

  feed = Nokogiri::XML(open(url).read)
  medium_posts = feed.xpath('//item').collect do |item|
    {
      'title' => item.xpath('title').text,
      'url' => item.xpath('link').text,
      'date' => item.xpath('pubDate').text
    }
  end

  save_data('medium_posts', medium_posts)
end

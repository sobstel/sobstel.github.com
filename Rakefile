require 'json'
require 'openssl'
require 'open-uri'
require 'yaml'
require 'time'
require 'nokogiri'

def save_data(name, object)
  file = "_data/#{name}.yml"
  File.write(file, object.to_yaml)
  puts "saved to #{file}"
end

def load_data(name)
  file = "_data/#{name}.yml"
  YAML.load_file(file)
end

def fetch(url)
  params = {
    http_basic_authentication: %w[sobstel 486e5d6a8f22b65aac97f271262b7bec9a788979],
    ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE
  }
  open(url, params).read
end

desc 'Publish live'
task :publish, :message do |_, args|
  args.with_defaults message: YAML.load_file('quotes.yml')['quotes'].values.flatten.sample
  message = args[:message]

  task(:generate_ronn_pages).execute
  task(:import_github_repos).execute
  task(:import_gists).execute

  sh 'git add --all .'
  sh "git commit . --message=\"#{message}\""
  sh 'git pull --rebase'
  sh 'git push'
end

desc 'Generate ronn pages'
task :generate_ronn_pages do
  sh 'RONN_STYLE=css ronn --html --style print index.ronn'
  sh 'RONN_STYLE=css ronn --html --style print examples.ronn'
  sh 'mv examples.html examples/index.html'
end

desc 'Import GitHub repos'
task :import_github_repos do
  url = format('https://api.github.com/users/%s/repos', 'sobstel')
  puts "fetch from #{url}"

  all_repos = JSON.parse(fetch(url)).sort_by do |repo|
    repo['pushed_at']
  end.collect do |repo|
    repo.select do |key, _|
      %w[name full_name description html_url fork languages stargazers_count].include? key
    end
  end.reverse

  repos, forks = all_repos.partition { |repo| !repo['fork'] }
  popular_repos, other_repos = repos.partition { |repo| repo['stargazers_count'] >= 7 }

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

  new_gists = JSON.parse(fetch(url)).select do |gist|
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

  (2017..Time.now.year).each do |year|
    (1..12).each do |month|
      from = Date.new(year, month, 1).to_s
      to = Date.new(year, month, 1).next_month.prev_day.to_s

      next if year >= Time.now.year && month > Time.now.month

      url = format('https://github.com/%s?tab=contributions&from=%s&to=%s', 'sobstel', from, to)
      puts "fetch from #{url}"

      html_doc = Nokogiri::HTML(fetch(url))
      contributions = html_doc.css('.contribution-activity-listing li a:first-child')

      contributions.each do |contribution|
        text = contribution.text.strip
        matches = %r{(.+)/(.+)}.match(text)

        next unless matches

        vendor = matches[1]
        name = matches[2]

        repo = format('%s/%s', vendor, name)

        github_contributions << repo
      end

      sleep(1)
    end

    sleep(3)
  end

  github_contributions.uniq!
  github_contributions.sort_by!(&:downcase)

  save_data('contribs', github_contributions)
end

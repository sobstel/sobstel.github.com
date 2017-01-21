require 'json'
require 'openssl'
require 'open-uri'
require 'yaml'
require 'time'
require 'nokogiri'
require 'quotey'

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
    http_basic_authentication: %w(sobstel 486e5d6a8f22b65aac97f271262b7bec9a788979),
    ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE
  }
  open(url, params).read
end

desc 'Publish live'
task :publish, :message do |_, args|
  args.with_defaults message: Quotey::Quoter.new.get_quote.encode('UTF-8').delete('"')
  message = args[:message]

  task(:import_github_repos).execute
  task(:import_gists).execute
  task(:generate_tech_stack_cloud).execute

  sh 'git add --all .'
  sh "git commit . --message=\"#{message}\""
  sh 'git pull --rebase'
  sh 'git push'
end

desc 'Import GitHub repos'
task :import_github_repos do
  url = format('https://api.github.com/users/%s/repos', 'sobstel')
  puts "fetch from #{url}"

  all_repos = JSON.parse(fetch(url)).sort_by do |repo|
    repo['pushed_at']
  end.collect do |repo|
    puts "- fetch languages from #{repo['languages_url']}"
    repo.merge('languages' => JSON.parse(fetch(repo['languages_url'])))
  end.collect do |repo|
    repo.select do |key, _|
      %w(name full_name description html_url fork languages stargazers_count).include? key
    end
  end.reverse

  repos, forks = all_repos.partition { |repo| !repo['fork'] }
  popular_repos, other_repos = repos.partition { |repo| repo['stargazers_count'] >= 4 }

  save_data('popular_repos', popular_repos)
  save_data('other_repos', other_repos)
  save_data('forks', forks)
end

desc 'Import GitHub gists'
task :import_gists do
  gists = load_data('gists')
  meta = load_data('meta')
  since = meta['gists-last-read'].to_s

  url = format('https://api.github.com/users/%s/gists?since=%s', 'sobstel', URI.encode(since))
  puts "fetch from #{url}"

  new_gists = JSON.parse(fetch(url)).reject do |gist|
    gist['public']
  end

  new_gists.reject do |gist|
    gist['created_at'] < since # github uses "updated at"
  end.reverse.each do |gist|
    gists.unshift(
      'title' => gist['description'],
      'url' => gist['html_url'],
      'created_at' => gist['created_at']
    )
  end

  save_data('gists', gists)

  meta['gists-last-read'] = Time.now.utc.iso8601
  save_data('meta', meta)
end

desc 'Import GitHub contributions'
task :import_github_contributions do
  github_contributions = load_data('contribs')

  (2016..Time.now.year).each do |year|
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
        excluded_vendors = %w(sobstel golazon tripit wbond)

        github_contributions << repo unless excluded_vendors.include?(vendor)
      end

      sleep(1)
    end

    sleep(3)
  end

  github_contributions.uniq!
  github_contributions.reverse!

  save_data('contribs', github_contributions)
end

desc 'Generate tech stack cloud'
task :generate_tech_stack_cloud do
  html_doc = Nokogiri::HTML(File.read('work.html'))
  html_stacks = html_doc.css('.stack')

  stack = {}

  html_stacks.each do |html_stack|
    html_stack.text.split(', ').each do |tech|
      stack[tech] = 0 unless stack[tech]
      stack[tech] += 1
    end
  end

  stack_arr = stack.to_a
  stack = stack_arr.sort_by { |k, v| [-v, stack_arr.index([k, v])] }.to_h

  # puts stack.to_a.to_yaml

  save_data('stack', stack)
end

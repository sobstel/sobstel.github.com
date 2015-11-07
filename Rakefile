require "json"
require "openssl"
require "open-uri"
require "yaml"
require "time"
require "nokogiri"

def save_data(name, object)
  file = "_data/#{name}.yml"
  File.write(file, object.to_yaml)
  puts "saved to #{file}"
end

def load_data(name)
  file = "_data/#{name}.yml"
  YAML.load_file(file)
end

task :publish, :message do |t, args|
  args.with_defaults :message => Time.now.rfc2822
  message = args[:message]

  task(:import_github_repos).execute
  task(:import_gists).execute

  sh "git add --all ."
  sh "git commit . --message=\"#{message}\"";
  sh "git pull --rebase"
  sh "git push"
end

task :import_github_repos do
  url = sprintf("https://api.github.com/users/%s/repos", "sobstel")
  puts "fetch from #{url}"

  all_repos = JSON.parse(open(url, {ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE}).read).reject do |repo|
    ["skeptic"].include? repo["name"]
  end.sort_by do |repo|
    repo["pushed_at"]
  end.collect do |repo|
    repo.select do |key, value|
      ["name", "full_name", "description", "html_url", "fork", "language"].include? key
    end
  end.reverse

  repos = all_repos.reject { |repo| repo["fork"] }
  forks = all_repos.select { |repo| repo["fork"] }

  save_data("repos", repos)
  save_data("forks", forks)
end

task :import_gists do
  gists = load_data("gists")
  meta = load_data("meta")
  since = meta["gists-last-read"].to_s

  url = sprintf("https://api.github.com/users/%s/gists?since=%s", "sobstel", URI::encode(since))
  puts "fetch from #{url}"

  new_gists = JSON.parse(open(url, {ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE}).read).reject do |gist|
    gist["public"] === false
  end

  new_gists.reject do |gist|
    gist['created_at'] < since # github uses "updated at"
  end.reverse.each do |gist|
    gists.unshift({
      'title' => gist["description"],
      'url' => gist["html_url"],
      'created_at' => gist["created_at"]
    })
  end

  save_data("gists", gists)

  meta["gists-last-read"] = Time.now.utc.iso8601
  save_data("meta", meta)
end

task :import_github_contributions do
  github_contributions = load_data("contribs")

  (2015..Time.now.year).each do |year|
    (1..12).each do |month|
      from = Date.new(year, month, 1).to_s
      to = Date.new(year, month, 1).next_month.prev_day.to_s

      url = sprintf("https://github.com/%s?tab=contributions&from=%s&to=%s", "sobstel", from, to)
      puts "fetch from #{url}"

      html_doc = Nokogiri::HTML(open(url, {ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE}).read)
      contributions = html_doc.css(".contribution-activity-listing .title")

      contributions.each do |contribution|
        text = contribution.text.strip
        matches = /.+\s(.+)\/(.+)/.match(text)

        next unless matches

        vendor = matches[1]
        name = matches[2]

        repo = sprintf("%s/%s", vendor, name)
        excluded_vendors = ["sobstel", "golazon", "tripit", "wbond"]

        github_contributions << repo unless excluded_vendors.include?(vendor)
      end
    end
  end

  github_contributions.uniq!.reverse!

  save_data("contribs", github_contributions)
end

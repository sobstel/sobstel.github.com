require "configuration"
require "json"
require "openssl"
require "open-uri"
require "yaml"
require "time"

Configuration.for('github') {
  user "sobstel"
  repos {
    include_only ["agnostic", "metaphore", "sesshin", "jsonp.js", "AsyncHTTP", "scru.js"]
  }
}

task :publish, :message do |t, args|
  args.with_defaults :message => Time.now.rfc2822
  message = args[:message]

  task(:import_github_repos).execute
  task(:import_gists).execute

  puts message

  sh "git add --all ."
  sh "git commit . --message=\"message\"";
  sh "git pull --rebase"
  sh "git push"
end

task :import_github_repos do
  url = sprintf("https://api.github.com/users/%s/repos", Configuration.for('github').user)
  puts "fetch from #{url}"

  repos = JSON.parse(open(url, {ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE}).read).select do |repo|
    Configuration.for('github').repos.include_only.any? do |name|
      repo["name"].include? name
    end
  end.sort_by do |repo|
    repo["stargazers_count"]
  end.reverse.collect do |repo|
    repo.select do |key, value|
      ["name", "full_name", "description", "html_url"].include? key
    end
  end

  file = "_data/repos.yml";
  File.write(file, repos.to_yaml)
  puts "saved to #{file}"
end

task :import_gists, :limit do |t, args|
  args.with_defaults :limit => 1
  limit = [10, args[:limit].to_i].max

  url = sprintf("https://api.github.com/users/%s/gists?per_page=%d", Configuration.for('github').user, limit)
  puts "fetch from #{url}"

  gists = JSON.parse(open(url, {ssl_verify_mode: OpenSSL::SSL::VERIFY_NONE}).read).reject do |gist|
    gist["public"] === false
  end

  gists.each do |gist|
    output = "---\r\n"
    output += "title: \"#{gist["description"]}\"\r\n"
    output += "created_at: #{gist["created_at"]}\r\n"
    output += "updated_at: #{gist["updated_at"]}\r\n"
    output += "---\r\n\r\n";

    gist["files"].each do |name, file|
      output += "<strong>#{file["filename"]}</strong>\r\n\r\n"
      content = open(file["raw_url"]).read.gsub(/^/, "    ")
      output += "#{content}\r\n\r\n"
    end

    output += "[fork or comment at github](#{gist["html_url"]})\r\n"

    file = "_gists/#{gist["id"]}.md"

    File.write(file, output)

    puts "saved to #{file}"
  end

  recent_gists = gists[0..9].collect do |gist|
    gist.select do |key, value|
      ["id", "description"].include? key
    end
  end
  file = "_data/recent_gists.yml";
  File.write(file, recent_gists.to_yaml)
  puts "saved to #{file}"
end

# DEPRECATED
# task :import_posts_from_nanoc do
#   source_dir = "/Users/sobstel/Projects/sobstel.org/content/blog"
#   target_dir = "/Users/sobstel/Projects/sobstel/_posts"

#   Dir.foreach source_dir do |f|
#     next if f == "." || f == ".."

#     frontmatter = YAML.load_file(source_dir + "/" + f)

#     output = "---\r\ntitle: \"#{frontmatter["title"]}\"\r\n---";

#     content = File.read(source_dir + "/" + f)
#     content.gsub! /---(.|\n)*?---/, ''

#     output += content

#     File.write(target_dir + "/" + frontmatter["date"].strftime("%Y-%m-%d") + "-" + f, output)
#   end
# end

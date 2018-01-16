require 'logger'
require 'pathname'

LOGGER = Logger.new(STDOUT)

desc 'Publish the site'
task :publish, [:commit_msg] => [:build] do |t, args|
  args.with_defaults(:commit_msg => "Update site")
  commit_msg = args[:commit_msg].gsub(/'/, "\\\\'")
  system("git add . && git commit -a -m '#{commit_msg}' && git push")
end

desc 'Build the site'
task :build => [:tags, :minify]

desc 'Generate tags page'
task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read
  site.tags.sort.each do |tag, posts|
    html = ''
    html << <<-HTML
---
layout: tag-index
title: Posts tagged "#{tag}"
---
<dl>
    HTML
    posts.each do |post|
      post_data = post.to_liquid
      html << <<-HTML
  <dt>
    <a href="#{post_data['url']}">#{post_data['title']}</a>
    <span class="post-date">&bull; #{post_data['date'].strftime('%d %b %Y')}</span>
  </dt>
      HTML
      if post_data['summary']
        html << "<dd>#{post_data['summary']}</dd>"
      end
    end
    File.open("_tags/#{tag}.md", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end

desc 'Minify assets'
task :minify do
  require 'sprockets'

  sprockets = Sprockets::Environment.new do |env|
    env.logger = LOGGER
    env.css_compressor = :scss
    env.append_path("_assets/css")
  end

  sprockets["application.scss"].write_to("public/css/application.css")
end

desc 'Generate tags page'
task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
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
    html << '</dd>'
    File.open("_tags/#{tag}.md", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end
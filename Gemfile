source "https://rubygems.org"
gem "jekyll", "~> 4.2.2"
gem "minima", "~> 2.5"
gem "json"
gem "hash-joiner"
gem 'html-proofer'
gem 'jekyll-sitemap'
gem 'jekyll-redirect-from'
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins

# plugins
group :jekyll_plugins do
  gem 'jekyll-last-modified-at'
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
gem "tzinfo" if Gem.win_platform?
gem "tzinfo-data" if Gem.win_platform?
gem "webrick" if Gem.win_platform?
# Performance-booster for watching directories on Windows
gem "wdm" if Gem.win_platform?

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

# TODO: Remove when this gets fixed in Jekyll
gem "webrick"
gem "csv"
gem "base64"
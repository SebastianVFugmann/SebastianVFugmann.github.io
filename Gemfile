# Gemfile
source "https://rubygems.org"

# Use GitHub Pages gem
gem "github-pages", group: :jekyll_plugins

# Theme
gem "jekyll-remote-theme" # Keep this for remote theme support

# Jekyll plugins group
group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-feed"
  gem "jekyll-include-cache" # Required by Minimal Mistakes theme
  gem "jekyll-sitemap"
  gem "jekyll-gist" # For GitHub gist embedding
  gem "jemoji" # For emoji support
end

# Windows and JRuby support
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1", :platforms => [:mingw, :x64_mingw, :mswin]

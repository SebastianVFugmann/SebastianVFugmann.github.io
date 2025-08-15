# Gemfile - Fixed for GitHub Pages compatibility
source "https://rubygems.org"

# Use GitHub Pages gem instead of Jekyll directly for GitHub Pages deployment
gem "github-pages", group: :jekyll_plugins

# Theme - Remove local theme gem since we're using remote_theme
# gem "minimal-mistakes-jekyll" # Commented out - conflicts with GitHub Pages
gem "jekyll-remote-theme" # Keep this for remote theme support

# Jekyll plugins group
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-include-cache" # Required by Minimal Mistakes theme
  gem "jekyll-sitemap" # Recommended for SEO
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

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

# Libraries no longer part of the default gems (Ruby 3.0+)
gem "csv"
gem "base64"
gem "logger"

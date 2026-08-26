require "rake"

CHROME_PATHS = [
  "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
  "/usr/bin/google-chrome",
  "/usr/bin/chromium"
].freeze

def chrome_path
  ENV["CHROME_PATH"] || CHROME_PATHS.find { |path| File.exist?(path) }
end

task :build do
  sh "bundle exec jekyll build"
end

desc "Build the site and convert cv.html to cv.pdf using headless Chrome"
task pdf: :build do
  chrome = chrome_path
  abort "Google Chrome or Chromium not found. Set CHROME_PATH to point at your browser binary." unless chrome

  cv_html = File.expand_path("_site/cv.html", __dir__)
  cv_pdf = File.expand_path("cv.pdf", __dir__)
  abort "Expected #{cv_html} after the build; nothing to convert." unless File.exist?(cv_html)

  sh %Q{"#{chrome}" --headless=new --disable-gpu --no-sandbox \
        --no-pdf-header-footer --print-to-pdf="#{cv_pdf}" "file://#{cv_html}" 2>/dev/null}

  abort "Conversion failed: #{cv_pdf} was not created." unless File.exist?(cv_pdf)
  puts "Wrote #{cv_pdf} (#{File.size(cv_pdf)} bytes)"
end

task default: :pdf
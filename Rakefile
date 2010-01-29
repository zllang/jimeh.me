
$server = "new.jimeh.me"
$user = "jimeh"
$path = "#{$server}/public"

task :clean do
  system "rm -rf ./public"
  build
end

task :build do
  build
end

task :assets do
  rsync "assets/", "public/"
end

# task :server do
#   system "jekyll --server --auto"
# end

task :deploy => "deploy:site"

namespace :deploy do
  task :site do
    rsync "public/", "#{$user}@#{$server}:#{$path}"
  end
  
  task :assets do
    rsync "assets/", "#{$user}@#{$server}:#{$path}"
  end
  
  task :all do
    rsync ["public/", "assets/"], "#{$user}@#{$server}:#{$path}"
  end
  
  task :reset do
    rsync ["public/", "assets/"], "#{$user}@#{$server}:#{$path}", ["--delete"]
  end
  
  task :clean do
    system "ssh #{$user}@#{$server} 'cd \"#{$path}\" && rm -rf ./* && rm -rf ./.*'"
    rsync ["public/", "assets/"], "#{$user}@#{$server}:#{$path}"
  end
end

def build
  system "jekyll ./source/site ./public"
  system "jekyll ./source/blog ./public/blog"
end

def rsync(source, dest, options = [])
  if source.is_a?(Array)
    source.map! { |dir, i| "\"#{dir}\"" }
    source = source.join(" ")
  end
  options << "--exclude='.DS_Store'"
  options << "--exclude='.git*'"
  system "rsync -vr #{options.join(" ")} #{source} #{dest}"
end
require 'bundler'
Bundler.require

# The name include in the PDF filename
def name
  # TODO: customize this:
  "JohnDoe"
end

task default: [:html, :pdf]

desc "Build static HTML"
task :html do
  puts "Compiling to HTML..."
  status = system("bundle exec middleman build") do
    puts status ? "OK" : "FAILED"
  end
end

desc "Build PDF from build directory"
task :pdf do
  puts "Compiling HTML TO PDF..."

  find_html_files.reverse.each do |file|
    build_pdf file
  end
end

desc "Run the middleman server at http://localhost:4567"
task :server do
  system("bundle exec middleman")
end

desc "Remove the build directory"
task :clean do
  puts "Deleting the 'resume' directory and its contents..."
  system("rm -rf resume")
end

desc "Upload to Amazon S3"
task :upload do
  puts "Uploading build to Amazon S3..."

  verify_env_variables
  bucket = s3.directories.create(key: fog_directory, public: true)

  build_dir_files.each do |file, etag|
    case etag
    when :directory
      puts "Directory #{file}"
      bucket.files.create(key: file, public: true)
    else
      puts "Uploading #{file}"
      bucket.files.create(key: file, public: true, body: File.open(file))
    end
  end
end

def s3
  @s3 ||= Fog::Storage.new(
    provider: :aws,
    aws_access_key_id: ENV['AWS_ACCESS_KEY'],
    aws_secret_access_key: ENV['AWS_SECRET_KEY']
  )
end

def verify_env_variables
  %w[
    AWS_ACCESS_KEY
    AWS_SECRET_KEY
    FOG_DIRECTORY
  ].each do |env_variable|
    unless ENV[env_variable]
      puts "#{env_variable} required"
      exit 1
    end
  end
end

def fog_directory
  ENV['FOG_DIRECTORY']
end

def build_pdf(file)
  html = File.open(file).read
  kit = PDFKit.new(html,
    :page_size => 'Letter',
    :encoding => "UTF-8"
  )
  kit.stylesheets << 'resume/stylesheets/normalize.css'
  kit.stylesheets << 'resume/stylesheets/base.css'
  kit.to_file("resume/#{name}Resume.pdf")
end

def build_dir_files
  files = FileList["resume/**/*"].inject({}) do |hash, path|
    if File.directory? path
      hash.update("#{path}/" => :directory)
    else
      hash.update(path => File.read(path))
    end
  end

  files
end

def find_html_files
  require 'find'
  html_file_paths = []

  Find.find('resume') do |path|
    html_file_paths << path if path =~ /.*\.html$/
  end

  html_file_paths
end

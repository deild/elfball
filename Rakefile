namespace :book do
  desc 'prepare build'
   task :prebuild do
     Dir.mkdir 'images' unless Dir.exists? 'images'
     Dir.glob("book/**/*.jpg").each do |image|
       FileUtils.copy(image, "images/" + File.basename(image))
     end
     Dir.glob("book/**/*.png").each do |image|
       FileUtils.copy(image, "images/" + File.basename(image))
     end
   end

  desc 'build basic book formats'
  task :build => :prebuild do
  #task :build do
    puts "Converting to HTML..."
    `bundle exec asciidoctor elfball.adoc`
    puts " -- HTML output at elfball.html"

    puts "Converting to EPub..."
    `bundle exec asciidoctor-epub3 elfball.adoc`
    puts " -- Epub output at elfball.epub"

    puts "Converting to Mobi (kf8)..."
    `bundle exec asciidoctor-epub3 -a ebook-format=kf8 elfball.adoc`
    puts " -- Mobi output at elfball.mobi"

    puts "Converting to PDF... (this one takes a while)"
    `bundle exec asciidoctor-pdf elfball.adoc 2>/dev/null`
    puts " -- PDF  output at elfball.pdf"
  end

  desc 'archive basic book formats'
  task :archive => :build do
    puts "Compress HTML..."
    `7za a -t7z -mx9 elfball.html.7z elfball.html images/**/*.png images/**/*.jpg`
    puts " -- output at elfball.html.7z"
    puts "Compress EPub..."
    `7za a -t7z -mx9 elfball.epub.7z elfball.epub`
    puts " -- output at elfball.epub.7z"
    puts "Compress Mobi..."
    `7za a -t7z -mx9 elfball.mobi.7z elfball.mobi`
    puts " -- output at elfball.mobi.7z"
    puts "Compress PDF..."
    `7za a -t7z -mx9 elfball.pdf.7z elfball.pdf`
    puts " -- output at elfball.pdf.7z"
  end

  desc 'delete generate files'
  task :clean do
    puts "Delete Generate binaries..."
    `rm -f *.7z`
    puts " -- 7z files deleted"
    `rm -f elfball.html`
    puts " -- elfball.html files deleted"
    `rm -f elfball*.epub`
    puts " -- elfball.epub files deleted"
    `rm -f elfball.mobi`
    puts " -- elfball.mobi files deleted"
    `rm -f elfball.pdf`
    puts " -- elfball.pdf files deleted"
    #puts "Delete copy images..."
    # `rm -rf images`
    # puts " -- images folder deleted"
  end

  desc 'Upload archive on dropbox'
  task :upload do
    puts "Upload on dropbox..."
    date = Time.now.strftime "%Y%m%d_%H%M"
    # find tools here : https://github.com/andreafabrizi/Dropbox-Uploader
    sh "~/Applications/dropbox_uploader.sh -p mkdir Elfball/#{date}/"
    sh "~/Applications/dropbox_uploader.sh -p copy Elfball/elfball.epub.7z Elfball/#{date}/"
    sh "~/Applications/dropbox_uploader.sh -p copy Elfball/elfball.pdf.7z Elfball/#{date}/"
    sh "~/Applications/dropbox_uploader.sh -p copy Elfball/elfball.mobi.7z Elfball/#{date}/"
    sh "~/Applications/dropbox_uploader.sh -p copy Elfball/elfball.html.7z Elfball/#{date}/"
    sh "~/Applications/dropbox_uploader.sh -p upload *.7z Elfball/"
    sh "~/Applications/dropbox_uploader.sh -p share Elfball/#{date}/"
    puts " -- archives files upload on dropbox"
  end
end

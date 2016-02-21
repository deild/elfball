namespace :book do
  task :download do
    puts "Download from dropbox..."
    # find tools here : https://github.com/andreafabrizi/Dropbox-Uploader
    sh "~/Applications/dropbox_uploader.sh -p download Elfball/elfball.html.7z"
    puts " -- archives files download on dropbox"
    puts "Extract HTML..."
    sh "7za x -y -t7z elfball.html.7z"
    puts " -- output HTML"
    puts "Rename elfball.html..."
    sh "mv elfball.html index.html"
    puts " -- to index.html "
  end
end

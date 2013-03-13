!SLIDE
# The all mighty Rake #

!SLIDE bullets incremental
# Why rake? #

* it comes with a ruby package..
* ..or is easy installed with gem..
* ..and is widely used

!SLIDE bullets incremental
# Why not make? #
* Which make? (BSD, GNU, Qt...)?
* The syntax is not easy to start with

!SLIDE bullets incremental
# When not rake? #
* When performance is an issue
* On OpenBSD :)

!SLIDE bullets
# Lets start #
* $EDITOR Rakefile

!SLIDE code veel-code
# first task #
	@@@ ruby
	desc "Print current git commit hash"
	task :version do
	  sh "git rev-parse HEAD"
	end

!SLIDE code veel-code
# make it default
	@@@ ruby
	task :default => [ :version ]

!SLIDE code veel-code
# save it to a file
	@@@ ruby
 	desc "Check synatx of foo"
	task :syntax do
	  begin
	    sh 'ruby -c -w -W2 foo.rb > foo.synatx'
	  rescue
	    puts "Error: #{$!}"
	    exit 1
	  end
	end

!SLIDE
# but wait there is more!!!

!SLIDE code veel-code
# we can do better
	@@@ ruby
        RUBY_SCRIPTS = FileList['**/*.rb']
        RUBY_SCRIPTS.each do |src|
          target = src.ext("syntax")
          file target => src do
            begin
              sh "ruby -c -w -W2 #{src} 2> #{target} > /dev/null"
              puts File.read target
            rescue
              puts "Error: #{$!}"
              exit 1
            end
          end
          desc "Check syntax of all ruby files"
          task :syntax => target
        end

!SLIDE code veel-code
# what is FileList

	@@@ ruby
	RUBY_SCRIPTS = FileList['**/*.rb']

* collect all ruby files in this directory
  and all subdirectories

!SLIDE code veel-code
	@@@ ruby
	RUBY_SCRIPTS.each do |src|
  	  target = src.ext("syntax")

* for every file we found we create a
  target file

!SLIDE code veel-code
# the file task
	@@@ ruby
	file target => src do

!SLIDE code veel-code
# ..and in the end.
	@@@ ruby
	  task :syntax => target

!SLIDE code veel-code
# once again:
	@@@ ruby
        RUBY_SCRIPTS = FileList['**/*.rb']
        RUBY_SCRIPTS.each do |src|
          target = src.ext("syntax")
          file target => src do
            begin
              sh "ruby -c -w -W2 #{src} 2> #{target} > /dev/null"
              puts File.read target
            rescue
              puts "Error: #{$!}"
              exit 1
            end
          end
          desc "Check syntax of all ruby files"
          task :syntax => target
        end



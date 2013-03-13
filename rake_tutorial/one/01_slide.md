!SLIDE bullets incremental
# The all mighty Rake #
* A simple ruby build program with capabilities similar to make.

!SLIDE bullets incremental
# or 1000 times invented wheel #
* Author: Jim Weirich
* Project page: http://rubyforge.org/projects/rake

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

!SLIDE  veel-code
# first task #
	@@@ ruby
	desc "Print current git commit hash"
	task :version do
	  sh "git rev-parse HEAD"
	end

!SLIDE  veel-code
# make it default #
	@@@ ruby
	task :default => [ :version ]

!SLIDE  veel-code
# save it to a file #
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
# but wait there is more!!! #

!SLIDE  veel-code
# we can do better #
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

!SLIDE  veel-code
# what is FileList #

	@@@ ruby
	RUBY_SCRIPTS = FileList['**/*.rb']

* collect all ruby files in this directory
  and all subdirectories

!SLIDE 
# but wait there is more!!! #
	@@@ ruby
	FileList = FileList['**/*.rb'].
	  exclude("vendor/")

!SLIDE  veel-code
# manipulate the file name #
	@@@ ruby
	RUBY_SCRIPTS.each do |src|
  	  target = src.ext("syntax")
* for every file we found we create a
  target file

!SLIDE 
# but wait there is more!!! #
	@@@ ruby
	"src/foo.rb".pathmap("%{^src,bin}X.rb")
	# => "bin/foo.rb"

!SLIDE  veel-code
# the file task #
	@@@ ruby
	file target => src do

!SLIDE  veel-code
# ..and in the end. #
	@@@ ruby
	  task :syntax => target

!SLIDE 
# Dependencies #
	@@@ ruby
	task :second do
  	#second's body
	end

	task :first do
	  #first's body
	end
	task :second => :first 

!SLIDE  veel-code
# remember? #
	@@@ ruby
	task :default => [ :version, :syntax ]

!SLIDE  veel-code
# once again: #
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

!SLIDE
# lets clean up #
	@@@ ruby
	require 'rake/clean'
	CLEAN.include("**/*.syntax")
* rake clean
* removes all files added with 'CLEAN.include'

!SLIDE
# but wait there is more!!! #
	@@@ ruby
        CLOBBER.include("**/*.")
* rake clobber
* removes all files added with 'CLEAN.include'
* removes all files added with 'CLOBBER.include'

!SLIDE
# Reference #
## rake: [http://rake.rubyforge.org/](http://rake.rubyforge.org/)
## Martin Fowler: [http://martinfowler.com/articles/rake.html](http://martinfowler.com/articles/rake.html)


!SLIDE
# Thanks. Last questions? #
## github: [http://github.com/kmerz](http://github.com/kmerz)
## twitter: [http://twitter.com/k_merz](http://twitter.com/k_merz)

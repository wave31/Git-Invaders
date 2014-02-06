Git-Invaders
============

	   #   #       ####        ##    
	  #######     ######     ######  
	 ########## ##########  ## ## ###
	### ### ### ### ## ### ##########
	# ####### # ########## #  #  #  #
	# #     # #   # ## #     # ## #  
	             #      #   # #  # # 


Git invaders is the collection of software I will use to maintain a
space-invaders github graph. 

## hax!

Yeah, I know, my github graph is the sacred record of my productivity, and I
shouldn't mess around with it or people won't take me seriously. I'm sorry, but
having had this idea, I can't not do it, so here's the deal: 

I'm not going to generate false commits to make this work, all the work you see
on the graph was real work done by me and not some shell script drawing pretty
pictures by adding spaces to a text file. 

## How?

tldr; by automatically delaying some of my commits by redirecting them to a
private github server.

There are three pieces to this puzzle. The first is a Go program that models
the 'invaders' data structure, and given epoc values for the start day and
today, returns 0 if it's ok to git push today, and 1 if it's not ok to git push
today. 

The second is a shell wrapper for git that intercepts my push commands and, if
I shouldn't push to github today, merges the github repo with a private copy
and then pushes to the private copy instead. 

The third is a cron script that runs against the private staging server and
attempts to dump any changes I've staged. It's not in the spirit of this to
"save up" my commits and spread them or anything like that; I'm just delaying
my commits on certain days of the week, and if I don't have the commit traffic
to pull this off, then there will be holes in the graph. I'll live with it. I'm
just a developer evangalist, so I don't expect a day of delay here and there to
kill anyone. If it gets in the way I'll stop doing it. 

## invaders.go

The only part of this written so far.  It takes two arguments, a start day and
'today' in epoc seconds. You may omit the epoc value for today, but the start
day is required.  Simply put, tell it when you started, and it'll exit 0 if you
should push today, or 1 if you shouldn't. 

Install it like this: 

	go get github.com/djoephsen/Git-Invaders 

Test is as if you started 1 month ago like this on
linux: 

	invaders $(date -d '1 month ago' +%s)

or like this on Darwin:

	invaders $(date -v-1M +%s)

use it in a shell script like this: 

	if invaders ${START}; then echo YES!; else echo NO!; fi

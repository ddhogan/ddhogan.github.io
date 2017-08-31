---
layout: post
title:  "On creating my first CLI Gem"
date:   2017-08-31 03:52:57 +0000
---


You code the way you do anything else in life.  So for me, that means spending roughly three times as long agonizing over the thing than actually doing the thing.  However, I’m not too sad, as learning to code has brought that factor down (you should have seen me in grad school…)

Often, I spend so long in the planning phase trying to decide which idea is “best”, that I lose sight of the objective.  My Flatiron classmates have been enormously helpful in this process, reminding me that for this, my first project as a web software developer, I’m not expected to re-create Google (or even Altavista).  And that as a beginner, I won’t be able to make anything useful until I can make something usable.  Sometimes it’s easy for me to get discouraged when something I make isn’t immediate useful or novel, and this gets in the way of even starting.

But, one evening I made the initial commit, and had a functioning CLI gem the next evening.  It was a puny little thing, and I still had a “To Do” list and “should maybe do” and “would be nice” lists.  But I was surprised, all the same.  Avi’s walk-through videos proved very useful, and even through I had watched them through at least once before, following along with my content helped me learn even more still.  

### Into the weeds…

For example, all this time I thought I had to stay with Ruby 2.3.1 for the labs because most gems didn’t work with later versions.  But, in going “off-road”, I ran into a similar problem with Nokogiri, despite uninstalling and reinstalling, .  Several dives into Stackoverflow and github forums later, I found this gem (pardon the pun, I'll see myself out): `gem install –user-install nokogiri`.  Gems were being installed in a place ruby wasn’t expecting (or maybe rvm wasn’t expecting?)

### The product (approximately)

My gem accepts a user’s zipcode and tells you the single closest location from each of two different popular drugstore chains in the US.  Obviously, this forces the user to willfully suspend disbelief for a few minutes and forget that the rest of the internet doesn’t exist and search engines aren’t a thing.  But I can already see how these smaller gears can fit into a larger machine with access to databases, a more intuitive UI, and more complex object models.

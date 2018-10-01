---
layout: post
title:      "On the React-on-Rails portfolio project"
date:       2018-09-30 20:19:00 +0000
permalink:  on_the_react_on_rails_portfolio_project
---

It's always kinda bothered me that one can't edit tweets on Twitter.  
Naturally, my React-on-Rails portfolio project is yet another little Twitter clone (called [Butter Emails](https://github.com/ddhogan/butter-emails)). I spent almost as much time on implementing the edit/update actions as I did on everything else.  I almost left it for version 2, but the completionist in me just couldn't it it go (it's C-R-U-D not C-R-D!) So, I'm disproportionately proud of that.  

One of the things that makes it tricky is how the state tree is structured.  Currently, it has a branch for auth (which includes information about the current user and where they're logged in), and one for posts (posts are fetched as an array of objects in `state.posts`).  This choice meant I didn't easily have access to setting an inital state (from Redux's perspective) of `editing: false` for individual posts.  I resorted to having a nested return statement in the postsReducer, which, for now, works just as well, though isn't as elegant.  

The next biggest effort on this project was the JWT/authentication strategy.  Since I was designing my own API I didn't want to use an OmniAuth provider, and instead chose to use the 'Knock' gem.  I discovered how quietly opinionated this system is.  It works, but it seems to lock you into using email addresses for login, which was something I was hoping to avoid.  I want to collect as little information as possible from the user (I know, crazy, right?!) but still only have the user's display name listed next to their posts.  Still, it was nice to not need to re-invent the wheel on this part.  

Lastly, getting the two servers to work together was the next largest effort.  I followed a couple good tutorials, but still couldn't get the API server and client server to be managed by Foreman, and that ended up just being solved by specifying a more recent version of the gem. (Update your dependencies, kids!)
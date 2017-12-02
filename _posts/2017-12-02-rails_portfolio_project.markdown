---
layout: post
title:      "Rails Portfolio Project"
date:       2017-12-02 19:33:51 +0000
permalink:  rails_portfolio_project
---


This Rails portfolio project borrows the theme from my Sinatra project, but is rebuilt from scratch to have nested resources, more complex relationships, and more features.  My Sinatra project was only for CRUDing starships.  This project has two models (Crew and Ships) that have a bi-directional has_many_through association via Assignments, which acts as the join table, and can be thought of as one particular crew person's role on one particular ship.  This schema gives us easy access to listing all the crew member's assigned to a ship, or where are crew member is currently assigned.  Sounds so reasonable!
<br></br>

Quick tip: when generating seed data for users with has_secure_password through BCrypt, make the password_digest attribute: `BCrypt::Password.create('password')`, and then you can use `password` to log in when you're testing your app!  
<br></br>

Also, forms are more than just a pretty face!  Rails form helpers sometimes makes me forget that the form dictates the structure of the params hash.  At first I had checkboxes for crew people in the new assignments form, but kept getting an error message about the crew_id not existing (which it totally did), but as soon as I switched to a drop down menu the problem was solved.  Makes sense in retrospect since an assignment only has room for one crew_id, so checkboxes make no sense in that context, and set the params hash up incorrectly.  
<br></br>

Check out the [GitHub repo](https://github.com/ddhogan/starfleeter), or my [video](https://youtu.be/STRKKznhV9Y) walk-through !

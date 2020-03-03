---
layout:     post
title:      "Publishing an eleventy site to Github Pages"
date:       2020-03-03 01:00:00 +0000
permalink:  publishing_eleventy_site_to_github_pages
---

Eleventy is a relatively new static site generator.  I've heard it's "[almost fascinatingly simple](https://css-tricks.com/a-site-for-front-end-development-conferences-built-with-11ty-on-netlify/)", and I have a situation at work that is perfect for this tool.  

## My Situation

I have a small collection of one-off "posts" that are hosted on the repo's Github Pages site.  Some have a template that I just shamelessly lifted from some of the org's other communications for visual continuity.  The problem, of course, is that this is repetitive: if I ever have to make a change to that template (and I have) it must be made on each post.  Additionally, I want to still be able to host it on gh-pages (to avoid making this a whole huge thing by changing), so I made an extremely minimal silly little test case to see how Eleventy plays with Github Pages. I'm sure Netlify is amazing, but I really want to change only one thing at a time (and I also just want to see if this will work 😁)

## Initializing

I started by following along with their [getting started guide in the docs](https://www.11ty.dev/docs/getting-started/).
I added a single HTML template (just an `<h1>` element, basically), and two posts: one in Markdown and the other in HTML (since my use-case involves both).  Just some text and a little image in each.  I put the posts in the `src` directory and the template in the `_includes` directory, just following along with the gorgeously easy to understand docs.  So far easy-peasy!

## Bringing it to GitHub Pages

`0.` After [pushing to Github](https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line), make a branch called `gh-pages` (or whatever you'd like) and go into the settings and [enable Github Pages](https://help.github.com/en/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

`1.` Then, back in your terminal, install the gh-pages package: `npm install gh-pages --save-dev`.

`2.` Because of this package's [command line utility](https://www.npmjs.com/package/gh-pages#command-line-utility), we can add this item to `package.json`: 
```json
"scripts": {
    "deploy": "gh-pages -d _site"
},
"homepage": "/silly-eleventy-demo",
```  
This allows us to use the command `npm run deploy` to push to our gh-pages branch from our `_site` directory (which is effectively the build/dist), and, if our account is already mapped to a custom domain, add the repo name to the url so we still land on the correct `index.html`.

`3.` Add this to `.eleventy.js`:
```js
return {
    pathPrefix: "/silly-eleventy-demo"
}
```
This gives us a handy redirect for serving up links within the project.

`4.` Add the file `_data/site.json` with this:
```json
{
  "pathPrefix": "/silly-eleventy-demo"
}
```
Which gives us access to that variable for when we write out links in a post.

`5.` Then, assuming you have permalinks in the front matter of the md or html content, make your links like this:
`href="{{site.pathPrefix}}/myFirstPost/"`

And finally, run `npx @11ty/eleventy` (if you haven't since your last edits, you can add the `--serve` if you want to see it live, as described in the docs) to generate the `_site` directory, and then run `npm run deploy`.  The page is ready to go at `https://{{yourGitHubName}}.github.io/{{theRepoName}}` 🎊
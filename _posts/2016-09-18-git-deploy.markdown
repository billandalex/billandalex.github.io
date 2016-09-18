---
layout: post
title:  "Automated Git Deploy for WordPress Theme Development"
date:   2016-09-18 11:05:14 -0700
categories: banda
summary: A summary of how git deploy is used on this project
---

Git is often seen as a solution for version control, but it can also be be used to automatically and securely deploy code. For the Banda Theme, we're using this PHP deploy script: <https://github.com/markomarkovic/simple-php-git-deploy>

Here's the abstract three-step flow of the git deploy:

```
1. git push -> 2. Git push triggers the GitHub webhook -> 3. The webhook activates server-side deploy script
```

1. __`git push`__ 

   You `git push` to a branch you have setup to deploy to a server. In this case, we'll be pushing to a master branch that syncs with the live, production server, and a 'staging' branch that syncs with a development site.
2. __The push triggers a GitHub webhook.__

   Imagine the webhook and deploy scripts are people, and the webhook calls the deploy script on the phone:

   The webhook says: 
   
   >"Hey, it's me, GitHub. I just received a push to the master branch on the Banda Theme repo and have some new files to send you. Here's the secret access token: [insert your top secret access token here]."

   The overzealous server responds: 
   >"Cool! That access token is legit! Let me get those new files!!!"

3. __The server-side deploy script is activated.__ 

   The first time this happens, the deploy script creates a new directory to install the pushed repo's files into. The location of this directory is set in the deploy script's configuration file. In subsequent deploys, the deploy script compares the files in the directory with the new repo's files and updates the files that have changed.


 
Once the webhook and deploy script are setup, all you need to do is `git push` like usual and your remote site(s) will automatically update. Using git for version control and deployment is handy, but it's also secure. Setting up your server to only accept change requests from GitHub would allow you to block all other SSH and SFPT requests.
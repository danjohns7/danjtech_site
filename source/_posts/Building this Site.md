---
title: Building this Site
date: 2018-09-06 16:00:00
tags: tech, serverless, App Engine, cloudflare, cdn, hexo
---
I've been working in Linux and backend for around 10 years, but this has been my first venture into serverless technology. This blog is run through a CDN on serverless architecture with multiple layers of app security, for free.
The platform around this is Hexo, a Node.js-based blogging platform. All entries are stored as plaintext files, which keeps security simple.

First, the grit here: Cost, and how to run all this for free.
I've owned the domain danjtech.com for a few years now and repurpose it regularly for different projects. It's the only portion that has an associated cost. I believe it's managed through GoDaddy to the tune of $11 a year.

Everything else is free. The CDN and security are offered through CloudFlare for nothing, and the DNS passes through there with static caching. This minimizes cost on the backend significantly.

Google's new cloud platform also offers a significant amount of credit to new users, around $300 for the first year. The backend is run through App Engine.

Deployments are managed via the Google Cloud Shell in App Engine. From this origin the site is pushed out.

I'm going to document my steps here, as things worked in September 2018. Things may change and your milage may vary.

First, sign up for Google Cloud's free service, and launch App Engine. Create a new project based on Node.js, and launch the Cloud Shell.

The cloud shell already runs Node.js natively, so no configuration there is required.

For the first priority, get the Hexo CLI installed.

$ npm install -g hexo-cli

Next, create the directory you want your blog in, and get the initial blog created. For this, we're using /src/hexo.

$ mkdir /src/hexo 
$ hexo init /src/hexo 
$ cd /src/hexo 
$ npm init 

With that, the hard part is done; the code is present and ready to be deployed. The next steps are making changes to customize the site.

The most critical changes will be in _config.yml. Update the Site and URL sections with relevant data about the page.

To make site deployments, you'll also need to edit the Deployment section.

$deploy:
$  type: appengine
$  project: <appengine project name>

Add the needed plugin to deploy to App Engine.

$ npm i hexo-deployer-appengine

Blog posts make their way to /home/danjohns7/src/hexoblog/source/_posts. For documentation on writing, visit https://hexo.io/docs/writing. Files end with the .md extension.

Test your site in App Engine using the Web Preview in the top-right corner. The default port is 4000, so you'll need to change Google's port.

$ hexo server

If this works and testing is successful, generate and deploy your site.

$ hexo generate
$ hexo deploy

If you run into issues around Python as the runtime, edit node_modules/hexo-deployer-appengine/templates/pub/app.yaml from "python" to "python27"

_____________________________________

From here, the app should be deployed, testable, and working using a Google provided .appspot.com domain.

Next, you'll need to register your domain through Google using whatever provider you're working with via DNS. I was using CloudFlare, so I went through App Engine > Settings > Custom Domains and added a TXT record to verify my identity to Google.

This also involved some wait time as I created Google's requested DNS records, and waited for them to be verified. It took a few hours.

Once that's complete, that's it! A free, serverless blog.

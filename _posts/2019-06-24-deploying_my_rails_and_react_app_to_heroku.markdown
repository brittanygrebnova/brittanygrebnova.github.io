---
layout: post
title: "Deploying My Rails/React App to Heroku"
date: 2019-06-24 23:59:01 +0000
permalink: deploying_my_rails_and_react_app_to_heroku
---

Once I passed my React/Redux final project review, it was time to try deploying my app to Heroku. I encountered a number of errors but, with the help of the Flatiron community, I finally made it work! In this post, I’ll talk about some of the details that helped me get my app successfully deployed to Heroku.

Updating the Ruby Version:

I kept getting this error about the Ruby version:

```
An error occurred while installing ruby-2.3.3. This version of Ruby is not available on Heroku-18. The minimum supported version of Ruby on the Heroku-18 stack can be found at: https://devcenter.heroku.com/articles/ruby-support#supported-runtimes
```

This required me to use rvm to update my Ruby version to 2.6.1 locally and change the reference in my Gemfile. Once that was taken care of, another issue appeared.

Package.json in client and root:

The file that tells Heroku that your app uses Node.js. I had a package.json in my client folder (which contains my React/Redux code), but I was missing a package.json in my root directory. To remedy this, I ran the ‘npm init’ command from within the root directory and added the following code to the package.json that was created:

```
{
  "name": "spacious",
  "engines": {
    "node": "11.5.0"
  },
  "scripts": {
    "build": "cd client && npm install && npm run build && cd ..",
    "deploy": "cp -a client/build/. public/",
    "postinstall": "npm run build && npm run deploy && echo 'Client built!'"

}
```

You’ll notice in the scripts section, the build command includes ‘cd client’. The Heroku build kept crashing at that line. Actually, it looped over and over for so long that I had to kill the process myself. My problem was that I had my package.json inside client/src and not client. Oops!

The key here is having a package.json in the root directory of the project as well as in the client section.

Syntax Errors:

The next round of errors that I encountered were related to the capitalization of my file names. I had re-formatted them to keep with the first-word-lowercase-camel-case convention, then changed to all capitalized camel case. Github did not pick up the case change when I committed so there was a disparity between my text editor file names and file names that appeared in my Github repo. To fix this issue, I went error by error and corrected the capitalization as needed. Once all the file names matched, I ran ‘git push heroku master’ one more time and got…

```
remote: Verifying deploy... done.
```

Ahh...that feeling you get when you conquer all the error messages. I’m going to call it programmer’s high. It’s what keeps me eager to keep learning and figuring this stuff out.

I hope this helps at least one person who might be having the same issues deploying their app to Heroku. Thank you for reading!

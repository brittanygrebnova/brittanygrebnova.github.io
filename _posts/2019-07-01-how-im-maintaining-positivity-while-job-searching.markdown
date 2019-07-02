---
layout: post
title: "How I'm Maintaining Positivity While Job Searching"
date: 2019-07-01 12:57:01 +0000
permalink: how-im-maintaining-positivity-while-job-searching
---

Since graduating from the Flatiron School’s online self-paced curriculum, I’ve been swimming in a sea of job applications, coding tutorials, and LinkedIn searches. The job search process is one that requires a lot of time and energy. It’s not hard to get deflated and feel helpless, especially after weeks of applying and either not hearing back or being rejected. In this post, I’m going to share some of the steps I’m taking and choices I’m making in order to maintain positivity and balance in the face of rejection and uncertainty.

<strong>Short To-Do Lists</strong>

Getting through any kind of school, especially for moms like me, requires a ton of goal setting. In my year of learning to code online in a self-paced curriculum, I developed the habit of creating reasonable to-do lists with attainable short term goals. I learned to do what I know I can and leave the rest up to the universe. A concise list of 3-5 items that can be checked off by the end of the day feels a lot better than a laundry list of 20 abstract goals that drags from one week to the next.

<strong>Scarcity vs. Abundance Mindset</strong>

Last week I had 2 phone screenings that went great! Both interviewers told me that they wanted me to move forward to the next step, in-person technical interviews. I was thrilled...until it was 3 days later and all I heard were crickets. No emails to schedule a technical interview and no response to my follow-up messages. I felt myself slipping into a negative place. A scarcity mindset sounds something like “Man, there are SO few job opportunities and SO MANY applicants. HOW can I possibly compete?”. I’m learning to recognize those thoughts and adopt an abundance mindset instead. I think, and truly try to believe with all my heart, “You know what? There’s ONLY ONE ME out there and A LOT of companies who could greatly benefit from my unique experience and skill set.”

<strong>Gratitude & an Open Mind</strong>

It’s not hard to feel defeated after days, weeks, or months of rejections and silence. It feels like, sorry to use a cliche but, you’re searching for a needle in a haystack. This point touches on the previous about so many applicants and so few job opportunities, but to expand, I’m trying to be open to a wider range of positions that may put me a step closer to my target. And amongst those rejections and unanswered messages, I’m also reminding myself to be grateful for the seemingly negative experiences as they’re helping me build resilience and a better understanding of who, how, and where I want to be.

<strong>Being a Lifelong Learner</strong>

I truly believe that a person who’s willing to learn will always find opportunities. The career path of a developer requires a willingness to learn new skills and concepts in a climate of constant change. And since it’s impossible to know everything, it’s better to spend your time learning something that you like. There are a thousand new things to learn in the world of development, but I’ve chosen to branch off from what I already know and learn Node.js/Express. Here’s some code I’ve been working on…

<strong>Node.js App with Passport and OAuth for User Authentication</strong>

```
const passport = require("passport");
const GoogleStrategy = require("passport-google-oauth20").Strategy;
const mongoose = require("mongoose");
const keys = require("../config/keys");

// import the user model from mongoDB
const User = mongoose.model("users");

// passport takes the instance of user and saves the id
passport.serializeUser((user, done) => {
  done(null, user.id);
});

// mongoDB takes the id from passport and uses it to find the full user entry in the DB
passport.deserializeUser((id, done) => {
  User.findById(id).then(user => {
    done(null, user);
  });
});

// utilize passport's GoogleStrategy to get the user's google account
// info and set our mongoDB user's googleId to that user's google profile
// id through OAuth, then save the user
passport.use(
  new GoogleStrategy(

      clientID: keys.googleClientID,
      clientSecret: keys.googleClientSecret,
      callbackURL: "/auth/google/callback",
      proxy: true
    },
    async (accessToken, refreshToken, profile, done) => {
      const existingUser = await User.findOne({ googleId: profile.id });
      if (existingUser) {
        return done(null, existingUser);

      const user = await new User({ googleId: profile.id }).save();
      done(null, user);


);

```

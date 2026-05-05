---
title: "The Checklist for Deploying a Scary Change"
url: "https://artsy.github.io/blog/2023/09/13/deploying-a-scary-change/"
date: "Wed, 13 Sep 2023 00:00:00 +0000"
author: ""
feed_url: "https://artsy.github.io/feed"
---
<p>Lately, I’ve been getting involved with some sketchy stuff. You know what I’m
talking about–data migrations.</p>

<p>I’ve been rolling out changes that have a significant risk of breaking our
production environment for mission-critical services. It’s been exciting work
(keep your eyes out for more posts on the exact project, coming soon™️), but
I’ve definitely caused a couple incidents along the way.</p>

<p>After accidentally taking down a key service for a couple hours, I decided I
needed to have a better pre-deploy process for these changes. I did some
thinking and came up with a short checklist to run through before I press the
shiny green button.</p>

<!-- more -->

<p>Here’s the checklist I came up with:</p>

<ul class="task-list">
  <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />What is your plan if something goes wrong?
    <ul class="task-list">
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Run through ramifications of rolling back. If there’s a reason you’re
    worried about rolling back, then you’re not ready to deploy the change
    yet!</li>
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Figure out exactly what command(s) you will need to run to roll back. At
    Artsy, this is usually a
    <a href="https://github.com/artsy/hokusai/blob/main/docs/Command_Reference.md#how-to-do-a-rollback">one-liner using Hokusai</a>,
    our command-line Docker/Kubernetes CLI</li>
    </ul>
  </li>
  <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />How will you tell if something is going wrong after you deploy?
    <ul class="task-list">
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Error rate (DataDog)</li>
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Specific error reporting (Sentry)</li>
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Latency (DataDog)</li>
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Logs (Papertrail)</li>
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Functionality (does it still work? Are people using it successfully?
    Important for things where errors may not be bubbled up correctly or
    reported immediately)</li>
      <li class="task-list-item"><input class="task-list-item-checkbox" disabled="disabled" type="checkbox" />Sidekiq (are there lots of jobs queued to retry that are failing?)</li>
    </ul>
  </li>
</ul>

<p>With this checklist in hand, I’m deploying more confidently and causing fewer
incidents along the way.</p>

<p>Do you have something similar? Are there things you think this checklist should
include? Let me know in the comments!</p>

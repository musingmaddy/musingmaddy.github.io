---
title: Developing AI Apps Through Cursor
slug: developing-ai-apps-through-cursor
date: 2025-04-06T09:57:02+05:30
lastmod: 2025-04-06T09:57:02+05:30
description: Developing AI Apps Through Cursor descriptions
author: Maddy Meraki
avatar: /img/musingmaddy.webp
authorlink: https://musingmaddy.github.io
cover: /img/cover.webp
nolastmod: true
draft: false
showComments: true
tags:
  - AI
  - Coding
  - Cursor
  - App
categories:
  - Coding
---
First Cursor Project  
  
Yesterday, I created an app using Cursor. The idea was simple : Take in a plot and write a story using AI under 300 words. It was super easy to do through Cursor AI. I used Google's Gemini API for AI processing. In cursr I started off with Claude 3.5 then changed to Google Gemini Pro 2.5 model. Both are very good for this perticular project.

First, I went to Google AI studio and just gave this idea to Google's new Gemini Pro model and asked it what would be the best way to go. AI gave me the software specification for development: backend in Python and FastAPI, while frontend in Vue.js and Vite.  
  
I created two directories: frontend and backend. First, Cursor just breezed through the code—I just needed to accept each time. Then I ran it on localhost. I used Swagger UI to check the prompt, as I had not designed the frontend yet. It worked correctly. Then, using Vite + Vue.js, it wrote the frontend UI and it worked awesome.  
  
Next, I needed to upload it to a remote so that I could access it through the internet.  
  
First, I initialized Git and uploaded both frontend and backend into separate GitHub repositories. I hosted the frontend on GitHub Pages and the backend on Render. Initially, there were some issues and had to debug through prompting in Cursor. It tried a few things. But later I found out  all I had to do was to reinitialise Render. Now it's working perfectly. You can access it on a URL.

(Story Maker App)[https://mahadevan.github.io/story-maker-frontend/]
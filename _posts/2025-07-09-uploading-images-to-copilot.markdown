---
layout: post
title:  "Coding with AI - using images and Copilot agent"
date:   2025-07-09 12:13:41+0100
categories: ai
---
**TL;DR** - uploading screenshots to Copilot in agent mode of your wireframes saves some grunt work of creating the UI markup code. Give it a go!

I've been really enjoying trying out the new Copilot AI capabilities that are have been (rapidly) made available in the past few months.

I've been using Copilot in agent mode with Visual Studio since it was made available recently. If you aren't aware, this mode is designed to take prompts and commands from you and make real-time live code changes to your codebase in front of you, instead of just suggesting changes inside of the chat window (which is what we had with 'chat' mode)

A really neat trick I discovered last week is taking a screenshot of a wireframe (e.g. in Figma) and uploading the image as part an interaction with Copilot.

I had a screenshot of a wireframe of a table of information that we wanted to add to our UI. I specified that I wanted Copilot to produce the UI markup (I am using .NET's Blazor and therefore the markup is in razor syntax) and all the relevant model bindings, and to follow examples of a few similar code files. It took some nudging, but it was pretty successful.

I was then really pleased when I continued to iterate on the initial static markup to enhance the table to make it dynamic; I wanted the contents of the table to appear depending on an option a user has selected elsewhere in the app (this was already built at this point).

![Screenshot of how to upload an image to Copilot in Visual Studio](/_images/copilot_agent_mode_upload_screenshot.png)
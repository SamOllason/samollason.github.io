---
layout: post
title: "Thoughts on the GitHub Copilot app for software development"
date: 2026-11-24
categories: [AI, SDLC]
---

# TL;DR

Here are my thoughts on using the GitHub Copilot app for software development. I thought it was going to feel a bit clunky like VS and VS Code, but it felt very close to my experiences using Codex and Claude Code.

# The question - What was I trying to understand?

Last week I discovered that we now have access to the <a href="https://docs.github.com/en/copilot/concepts/copilot-surfaces/github-copilot-app" target="_blank">GitHub Copilot desktop app</a> to use at work in software development. To be honest, I wasn't exactly sure what it was. Although at Howden we are a Microsoft shop, I hadn't really come across this before. As a side note, I think this is just a reflection of how rapidly agent-assisted development is shifting at the moment, even for someone who is following this space closely. For example, at work in the past year I have used agent-assisted development in Visual Studio, VS Code and the GitHub Copilot CLI. Outside of work, for personal projects, I have been working my way through Cursor, Claude Code and Codex (with Codex being my favourite for ease of use and integration with ChatGPT).

Anyway, the GitHub Copilot app is yet another agent-assisted development tool to help create software more effectively.

For extra context, this was the first time I had been able to use an 'agent-first' app (i.e. not VS, VS Code or WebStorm) in a professional environment, and I was keen to experiment and see if this helped me produce better outcomes.

# What I initially thought (my hypothesis)

I thought that it would be very similar to the 'code-first, AI-second' approach taken by traditional IDEs such as VS Code/VS, and I expected it to feel clunky and far removed from the experience I am used to with Claude Code/Codex.

# What I discovered

I was pleasantly surprised by just how good the app was. It felt very similar to using Codex in terms of its slick user experience, the ability to connect easily to all the tools I needed, and the coherent and intuitive conversations.

I set is to run on auto-pilot (create branches, push code, create PRs etc all without question) and found that I spent most of my time reviewing the PR to check the unit tests, expand coverage and add comments to areas of the codebase that would confuse a future developer (e.g. where we have to deliberately implement an anti-pattern to accomodate a third-party API).

In particular, I liked
* How creating parallel 'sessions' was really straightforward. It automatically handled Git work trees.
* How transparently the tool created subagents and ran terminals, without cluttering the UI or distracting me.
* How easy it was to instruct the app to use Playwright to run the app in headless mode, verify the acceptance criteria of the work I was doing, take a screenshot for me and then automatically include that screenshot as evidence in the PR body (which it automatically created and uploaded for me).
* How easy it was for me to adjust and adapt the tool, all through written instructions. For example, I wanted it to create branch names in a certain pattern whenever I gave it a work item, and it added these to the repository instructions/app settings for me.

Further, I discovered that the conversations I had with other members of my team shifted slightly. I spent much of the week in calls, pairing, swarming and collaborating with them (as I did before with pre-AI-assisted code and with AI in traditional IDEs), but the nature of the conversations was different. This probably deserves another blog, but in essence I realised that we spent much more time focusing on how we wanted the app to behave and rigorously inspecting and iterating the unit tests to prove it was doing what we expected.

## Some things that need improving

Having built UIs for over 10 years, I am very particular about user experience, signals and intent. So I am very picky!

* The built-in 'browser' doesn't have developer tools or a history (meaning I have to type in localhost:22127 each time). I am used to having a modern browser to build front-end apps!
* The browser is also a bit buggy: sometimes half the screen appears extra magnified, and I have to resize the window to recover.
* One final step I take before sharing work for review with others (that is, other humans!) is to ask 'review this PR using an independent agent to spot errors, edge cases and issues'. Despite it finding several issues and quirks (which verifies that this is a useful step in the workflow!), I don't have full confidence that this was run by a different model, which is really what I would like—for example, getting a Claude model to check the work of a Microsoft model.
* I cannot find archived sessions, despite asking the app itself, searching sessions and even searching online myself. Let's hope I don't need them! 😅

# Open questions

* Just how far into the rest of the SDLC can this tool go? It is creating/editing work items on our board and creating and managing PRs for us. I can't quite get it to connect to the Datadog MCP (I've spent an hour on this and can't get anywhere), but I am now really keen on coordinating the SDLC from the app.

* Would I choose the GitHub Copilot desktop app over Claude Code/Codex? As mentioned above, I am under the constraints of being in a Microsoft environment, but at the moment I think I would still opt for Codex as my tool of choice for work.

---

Every word written by my, splle-checked by both me and AI 😉
---
layout: post
title:  "'Agentic TDD' and other Learnings from coding with CoPilot in agent mode"
date:   2025-07-24 12:13:41+0100
categories: ai
---

**TL;DR**
I've been using GitHub CoPilot for 'Agentic TDD' a lot recently in Visual Studio. Here is what I have been exploring and what I have learnt.

 1. User stories + TDD + CoPilot is a huge boost to speed while maintaining accuracy and understanding
 2. Analysis of your codebase and recommendations for best practices can boost learning and generate documentation at the same time.
 3. Easily increase unit test coverage of existing code (we increased by 5% in a month while adding more code)
 4. <a href="https://docs.github.com/en/copilot/how-tos/custom-instructions/adding-repository-custom-instructions-for-github-copilot" target="_blank">copilot-instructions.md</a> is a great place to evolve a useful and usable style guide that you can use to direct CoPilot.


---

# Overview

Agent mode in Visual Studio CoPilot came out about 6 weeks ago, and I've been playing around with a lot in day-to-day work to see how it can improve my workflow.

# 1. 'Agentic TDD'

This is a skill I have been actively working on and refining recently. It's a kind of meta-programming, in a way.

I've long been a fan of using TDD (Test Driven Development) as a development practice for many reasons. The obvious ones are that by the time you are finished, you have tests written (instead of moving onto the next feature) and self-documenting code. But another reason, with my product management hat on, is that it constantly forces you to think about the *problems* you are solving *while working on the solution* (e.g. "I want a method that formats the date of birth field on the screen ... so that users don't have to calculate this themselves"), which I think keeps re-focussing you on what you are trying to achieve.

**One of the challenges I have always found with TDD is how frustrating and long-winded it can be to set up the infrastructure and boilerplate needed for tests**, especially if you are creating tests for a new class or file that doesn't have any prior art. Anyone who has written unit tests in real production apps knows how fiddly and time-consuming it is to mock all of the dependencies needed for unit tests etc etc. We've all been there when you need 50+ lines of setup boilerplate ... all for a method assertion that is one line of code. However, CoPilot changes that! It's extremely good, though not perfect, at generating all of that setup and boilerplate.

This leans into the motto I've read about: **"outsource your tasks, not your thinking"** to CoPilot.

This has led me to change my workflow for most tasks to:

1. In the CoPilot chat window in my IDE (Visual Studio at the moment), share some context and User Story (context and Given/When/Then).
2. Ask CoPilot to generate a failing test for the first acceptance criteria.
3. Inspect the tests it generates and fix/tweak them. Often, I encourage CoPilot to generate extra scenarios, fix a few import errors, or subtly change what is being tested. Here is where humans, with experience and domain knowledge, can really add value. For instance, instead of just getting CoPilot to generate unit tests to calculate an insurance premium, nudge CoPilot to add realistic business scenarios based on cohorts/personas ("young drivers scenario where they will have higher premiums"), etc.
4. Ask CoPilot to fix the tests. Sometimes I literally just say "fix this" and it does it.
5. I have set up our CoPilot-instructions to ask CoPilot to build and run all unit tests after every change it makes. I inspect the results of the tests (usually they now pass).
6. Inspect CoPilot's solution: often I have to make a few tweaks, but not much; I suppose when you break down most work items into very small pieces, everything is usually a just a solved problem really, so CoPilot finds this easy.

**Another good habit alongside TDD that this encourages is writing down a proper user story**, which, again, aligns everyone in the team around what it is you are really doing, and why.

I've been surprised by good this approach above is; CoPilot has often added really neat UI layouts and implementations that I wasn't thinking of.

Also, I have added instructions about using TDD and my team's preferences into our CoPilot-instructions.md file (more on this below). I have also added some notes about style and coding preferences in here, which seem to be getting used (most of the time). More on this below.

---

## 2. Living style guide

Whenever CoPilot doesn't generate code or comments in a way that suits our team's style (or best practice), pI update our coPilot-instructions.md file (which sits in our application solution alongside other documentation) with notes about how things should be done. I have found that this is evolving into a useful and usable 'living' style guide for our project.

A few months ago I led an initiative at work to author the first draft of our engineer department cross-team C#/Blazor style guide. It lives in a shared Wiki and doesn't really get looked at (largely because it's separated from our workflow). The style guide section of the CoPilot-instructions file is starting to replacing this, and at a certain point I will probably break it out and share it with other teams.

A flavour of of our copilot-instructions (redacted and sensitive information removed):

```
Context

### Business context
* We work for Howden Insurance, a global insurance broking company.
* The work we do is regulated by the FCA and other UK/Global institutions.
* <some notes here about how our users use our apps, how they feel when using their apps, how we want them to feel, what they are trying to achieve, what their personas are>

### Code style
* Always consider accessibility.
* Use best practice and latest C#/Blazor features. Try to use the same style and patterns that exist already in the codebase, and if these two things conflict, use the best practice approach but make this clear to me in comments where you have deviated from the usual style.

### Testing
* We use BUnit and NUnit for unit tests.

### Security
* To mitigate against the risk of XSS attacks, please do not use `MarkupString` or any other method that means user input won't be properly validated and sanitized before being displayed in the UI.
* Consider OWASP top 10 security risks when adding new features
...
```

---

## 3. Increasing unit test coverage

Building on the above, the habit I am getting into is whenever I touch a new area of our codebase, **I review the unit test coverage and if it's lacking, then use CoPilot to generate some tests** before we add more (via TDD). This increases our coverage and increases my confidence that I haven't broken anything.

You are probably thinking, "you should be doing this anyway," and, yes, I completely agree. However, the difference now is that it takes CoPilot seconds to generate dozens of test cases for existing code that I can then tweak, review, and modify, instead of me having to write them all out by hand which, being realistic, isn't always feasible given deadlines and the need to add good quality to the new work being introduced. **This has been a 10x (or more) time saving for me** and has led to us increasing our unit test coverage 5% in a month (despite adding loads of new lines of code).

Interestingly, there are a few times where we have decided to refactor some older, existing code to make it more testable, which has resulted in better code!

---

## 4. Analysis of your codebase

When I first joined the team I am in now, I hadn't worked with Blazor, C# or .NET before. I wanted to understand the approach our app used for state management, and whether this was done consistently and followed best practices (especially compared to other projects I had worked on). It took a bit of nudging, lots of back and forth in the conversation with CoPilot (e.g. "please give some real examples"), but I got a great result. This now lives in our documentation for others to view, and it reinforces CoPilot to continue using these patterns (for consistency).

Similarly, I did a similar exercise for analysing our approach to security – what is our application's approach to auth? Does it follow best practices?

I started by writing down a numbered list of my understanding of how authentication/authorization works with our application in terms of the user flow and what methods are called etc. I then asked CoPilot to review it, correct it, and provide me with snippets and file locations of where the configuration is. It wasn't always right, but it got me 95% of the way.

**A massive benefit of using CoPilot to co-author these documents is speed of learning and being able to produce useful and relevant documentation to add to the solution for others as well. A double win!**

---

# Conclusion

I hope you found this useful. How are you using CoPilot? Feel free to message me on <a href="https://www.linkedin.com/in/samollason/" target="_blank" rel="noopener">LinkedIn</a> :)

---
layout: post
title: "Tic caT toe: What Dogs and Cats Taught Me About Developing with Codex"
date: 2026-07-07
categories: [AI, Azure, Foundry]
---

# TL;DR

I had fun using Codex for generating a simple pet-themed tic-tac-toe application and exploring how Codex subagents can review that goes and find bugs, ... despite a passing 'green' test suite!

# Introduction

The rate of change of tooling changes that can enhance developer productivity has increased massively over the years. Keeping up to date with what the tooling can do for my ways-of-working is just as important as keeping up to date with the changes in the underlying technology. I've used Copilot inside of VS Code, Claude Code, Cursor and I am currently having fun using Codex (note: the main reason I am not using Claude Code much at the moment is because I foudn that Claude is not nearly as useful for me compared to ChatGPT for every day tassks, and I wanted to keep costs down and just have a single OpenAI subscription of £20 per month!).

To explore some of the features of Codex I might not have used as much, I picked a small project: tic-tac-toe, with dogs and cats instead of noughts and crosses. 🐶 🐱

The game gave me something concrete to build, change and review. The real experiment was around the process: **how do I tell Codex about a project, give it a repeatable way to verify changes, and decide what evidence I want to see?**

I was inspired by [Will Larson's discussion of small changes](https://lethain.com/small-changes/) and [Anthropic's article on code review](https://claude.dev/blog/solving-code-review-at-anthropic/). Both raised a useful question for me: as agents can produce changes faster, what are some ways of making changes easier to check and understand?

---

## A small game, with a bigger purpose

The first version was a local two-player game. The rules lived separately from the code that renders the page, and it came with automated tests for wins, draws and invalid moves. This was basically a one-shot to get the project up and running, which is fine here as the actual code and project isn't what I wanted to explore.

I then asked Codex to add a completed-round counter. We agreed on the behaviour: a win or draw increases it once, starting a new round keeps the count, and reloading the page resets it.

That small change became a useful way to explore three different kinds of instructions:

```text
My prompt:     What do I want to change right now?
     ↓
AGENTS.md:     What should Codex know about this project?
     ↓
SKILL.md:      How should it perform a repeatable workflow?
     ↓
Tests + browser: What evidence shows the change works?
```

## Giving Codex project context with AGENTS.md

Our `AGENTS.md` describes the project, its conventions and how to check changes. It tells Codex that game rules live in `dist/game.js`, that the interface uses accessible native buttons, and that finished games should reject further moves.

It also says to run `node --test` after changing game behaviour and use the browser for interface changes. This is standing project guidance, which means I don't need to repeat the same request every time.

As a side note: watching Codex click through the browser (in-built into the IDE) was pretty cool - it felt much, much slicker than the classic Playwright etc. automated browser tooling I am used to. 

One distinction matters: this is an instruction to Codex, not a background process watching my files. If I edit the project myself, it won't automatically run tests just because I saved a file. A test watcher or CI pipeline would provide that separate trigger.

## A skill for checking changes

We created a project-local skill at `.agents/skills/check-game-change/SKILL.md`. Its job is to read the project guidance, run the tests, exercise the relevant browser scenario and report what it found back to me.

Note: the filename matters: `SKILL.md` identifies the entry point for a skill. A file called `REVIEW.md` could be useful guidance, but it isn't automatically a skill just because it has that name.

I can ask explicitly:

```text
Add a round counter, then use $check-game-change to verify it.
```

The skill is a reusable procedure, not the tests themselves. `node --test` executes the test suite; the skill tells Codex when and how to use it alongside other checks.

## What counts as evidence?

After implementing the counter, Codex ran six tests. It also used the built-in browser to play a win and a draw (watching that happen was fun!), checking the counter, restart and reload behaviour.

Initially, the answer said those checks passed, but I wanted to understand _how_ that was evidenced. The observations had appeared in the browser tool output, but screenshots hadn't been saved for that run (because I hadn't asked them to be).

So I asked for a more reviewable result:

```text
Use the built-in browser to test the current project. Start a server from
this project folder and tell me its URL. Play one win and one draw, check
the counter after each, then check restart and reload. For each step, report
the action, expected result, and observed result. Save screenshots of the
win and draw so I can review the evidence.
```

The browser run showed the counter go from zero to one after a win, stay at one after restarting, increase to two after a draw, and return to zero after reloading. It saved screenshots of both completed rounds. The same workflow also checked that a keyboard move worked and an occupied square was disabled.

![A screenshot of the appn where the dog won with one completed round](/_images/tic-cat-toe/win.png)

![A screenshot of the a draw with two completed round](/_images/tic-cat-toe/draw.png)

The two kinds of evidence answer slightly different questions. The automated tests exercise the game rules and can be rerun quickly. The browser check exercises the rendered interface and its connection to those rules. A screenshot shows a particular state; it doesn't, on its own, prove every transition that led there.

## Reviewing the review

I asked for a separate agent to review the staged, unstaged and untracked changes without modifying files. It reported no actionable findings.

That raised another question: **how could I influence what the reviewer pays attention to?**

Codex can use project-specific review guidance in `AGENTS.md`. We discussed rules for game invariants, but I asked whether the existing tests should already catch some of those cases. They should: occupied cells, terminal states and round counting are good candidates for deterministic checks.

We added review guidance focused on gaps around a change: a concrete regression the tests miss, keyboard and accessible state in interface changes, and the interface staying consistent with the underlying game state. The goal is to make review more useful without turning every passing test into a repeated review comment.

## Introducing a defect

To see the checks in action, we deliberately removed one condition from the game rules: the guard that prevents a move after someone has won. The change was small and reversible.

We first ran `node --test`. Four tests passed and two failed. The failures included the check for moves after a win and the completed-round counter: making another move after a win could increase the count from one to two.

Next, I asked Codex to use a separate review agent to see if it could spot the defect! The **fresh reviewer** was given the source and review instructions without being told the defect. It found the missing win check, and reproduced it with these moves:

```text
0, 3, 1, 4, 2  →  Dog wins; completed rounds: 1
5                →  The rules accept another move; completed rounds: 2
```

The reviewer independently ran the tests and saw the same two failures. We then restored the guard and ran the suite again: **all six tests passed**.

There are a couple of important limits to that result. The interface disables the squares after a win, so this experiment did not show that a player could click another square in the browser. The defect was in the game-rules function. Also, this project didn't yet have a committed Git baseline: the files were untracked, so the reviewer examined the working source rather than a normal before-and-after diff.

We didn't repeat the review without the new `AGENTS.md` rules, so I can't conclude that those rules caused the reviewer to find the problem, or that they improved its detection rate. The existing tests already caught this particular bug. This was one controlled example of tests and a fresh reviewer reaching the same result, not a measurement of how reliably either would catch other defects.

## A second defect: when there was a bug but the tests still passed!

The first experiment left an interesting question: **what might a reviewer catch when the tests are green?** 🤔

Introducing the bug 🐛: We made a local baseline commit, `13bf056`, with the existing six tests passing. This time we changed the interface's restart handler so that restarting an unfinished round reset the completed-round counter to zero. The six existing tests still passed.

A fresh reviewer was asked to review the uncommitted diff and wasn't told where the defect was. It identified that a second restart, or restarting after making a move, would lose the count. The existing tests exercised the `newGame` function, but they didn't exercise the restart handler that calls it.

The browser made the failure visible: I could complete a dog win and see a count of one, start another round, make a move, and restart again. The counter then showed zero.

We added a test in `app.test.js` that exercises the actual application event handlers with a lightweight simulated DOM. With the defect in place, it failed: expected `1`, actual `0`. Once the original restart handler was restored, all seven tests passed. A browser retest also retained the count after an interrupted round, and checked a win, a draw and a keyboard move.

This gave us a more useful comparison than the first experiment: the reviewer found a concrete gap in existing test coverage, and we turned the reproduced case into a repeatable test. It still isn't a controlled comparison of reviews with and without the new rules. I can't say the rules caused the finding, or infer how reliably a reviewer will catch other gaps from one example.

## What I'd take into a real workflow

I'd start with a small, understandable change and agree on what success looks like. Keep the commands and project-specific constraints close to the code. Let deterministic tests check the rules they can express reliably, and use browser checks to see how the rendered application behaves.

Then ask the reviewer for concrete findings rather than a general feeling of confidence. A separate review, screenshots and test output each give a different view; none is a substitute for deciding whether the change should exist or understanding what the checks leave out.

The interesting part of this mini dogs-versus-cats project is the opportunity to make the workflow itself visible: what I asked, what Codex did, what it observed, and where I still needed to make a decision.

---

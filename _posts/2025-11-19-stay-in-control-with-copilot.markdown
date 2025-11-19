---
layout: post
title:  "Mainting control and getting 10x from Vibe coding'
date:   2025-11-19 08:13:41+0100
categories: ai
---

**TL;DR**
My top tips to stay in control, maximise learning and get the best from months of Vibe Coding (using copilot in agent mode) are

Ive been using copilot to improve my productivity a lot in 2025. Im always exploring how it can serve me best. I've written about it before [here](https://samollason.github.io/ai/2025/07/24/using-copilot-agent-and-tdd.html) and lead sessions sharing my learnings across Howden's engineering function.

I'm convinced the best engineers are going to be those that can play the orchestrator mode (shepherding AI to do the 80&) and the heart surgeon role (locate and dive into a very specific line of code to solve a nuanced and tricky issue - the 20%).

Here are some of my top tips so far:

1. **Ctrl+V**: Pasting in screenshots to the chat window - 'A picture says 1000 words' and all that. I constantly share screenshots with copilot to give quick context to what I am trying to do. E.g. if I want it to debug a UI feature I will screenshot the browser, the open developer tools and ask it whats wrong. Just screengrab and paste it into the command. Another use case is when I don't understand why VS code is behaving a certain way - I just screenshot the area of the screen instead of trying to describe the issue.

2. `Add logs so we can debug this`. - sometimes I have seen copilot get stuck in loops when its trying to debug a particular issue. It suggests something, which doesnt work, and then suggests something else, which doesnt work ... and then goes back to option 1 again! This is particuarly acute when the bugs are due to race conditions, asynchronous behaviour and complex interactions in UIs. CoPilot can rapily add tonnes of Console.WriteLine() statements in seconds to help you spot whats going on, quickly (or, better yet, to help it understand whats going on! See notes on screeshots above).

3. `Are we using the right pattern/approach? What would be idiomatic for[languageX]` - Building off of 2 above, sometimes I have noticed copilto getting confused and spinning into loops by adddingmore and more code that doesnt solve the problem. As the human in conotrl, in these scenarios I ask copilot 'hang on a minute, are we trying to solve this using the wrong pattern/;. I saw this most recently with an issue relating to keeping differetn areas of our UI in sync. After asking copilot, it explained to me that the way we were using events wasnt best practice and wasnt idand instead it would be more idiomatic in Blazor.
note: this is where humans can add value and experience of senior engineers is valuable; I was able to refer to the way that React works with params ('props') and understand this issue that way.

4. `Give it permission for all git commands - except push`. Ive saved loads of time by giving co pilot the ability to run most git commands (add, merge, pull, commit etc) but I have explicitly asked it not to `push` on my command. This lets me review each micro change it makes to make sure I understand and agree.

5. `'quiz me!` - you are responsible for your code, so to make sure I understand whats being proposed I regularly ask copilot 'create a quiz with 3 questions about the core concepts we have covered so far'. This is particulalry useful when using copilot as a learning tool. When I am working on a side project to learn a new technology I lean on this heavilty to get the best combination of 'lets build a real thing quickly so I can see how it works' and 'I want to make sure I understand the theory'. This can also be a fun way of getting started on a project to 'warm up' for a session of coding.

---

# Conclusion

I hope you found this useful. How are you using CoPilot? Feel free to message me on <a href="https://www.linkedin.com/in/samollason/" target="_blank" rel="noopener">LinkedIn</a> :)

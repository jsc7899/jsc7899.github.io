## Prompting

### Writing Prompts

Prompting is a bit of art and science. There is certainly something to "prompt engineering" which refers to carefully crafting a prompt in a specific manner to get the best output, but there is also a lot to prompting that hasn't clearly been identified. The best way to improve your prompting skills that I've found is just to try a lot of different prompts. If you have a task you want the LLM to accomplish, try rewriting the prompt a few different ways and take note of which style gives the output you like best. Even better though, you can ask an LLM to help you write your prompt! You could ask for 3 different variations of the same prompt, or just tell it your basic idea and ask it to craft a prompt that would work best for an LLM. You can also ask for help choosing a model based on your prompt too!

### Context

Context is all the additional info the LLM has aside from the actual request in your prompt. There are a few ways to add context, including within your prompt, like adding links to documentation and modifying the system prompt. Context is key to getting good outputs from LLM. Without context, the LLM will resort to essentially statistical averages of its training data, so the more info and direction you can give it, the better it will be able to address your specific requests. Context also includes the entire input and output of the chat so far. It may also include other chats you've had in the past if you have a feature like ChatGPT's memory.

Something to be aware of thought is the "context window" or "context limit". Each LLM has a varying amount of context it can hold in its "working memory". Today there are LLMs like GPT-4.1 and Gemini 2.5 that have million token context windows, but smaller/faster models generally work with about a hundred thousand tokens or less.

One unique example of context is the documentation provided by the FastHTML and MonsterUI python libraries. They supply their entire documentation as downloadable markdown files that are easily parsed by an LLM. You do need to be conscious about the context window size and potential cost when using such large docs. On the flip side, inference is so cheap and only getting cheaper, so it's worth a shot to test it out - we're only talking a few bucks at worst.  

### Voice Prompts

The speech to text capabilities of recent models is quite impressive. Speaking directly to an app like ChatGPT is a very convenient way to prompt the model, especially if you've got a really long prompt or are having quick back and forth chats. I'll talk more about additional audio capabilities in a later section.

## Programming/Software Development

### Greenfield (new) projects

This is a one of the best uses cases for the current generation of LLMs. Just give it as much context related to what you want like the tools or frameworks it should use.

There are two different types of prompts I'll use depending on the type of project/research I'm doing:

A. **exploratory**: when I'm trying to get an idea of how something brand new to me works. This could be something like learning an entirely new topic. For example, learning a new programming language. When exploring ideas in a new language, you probably don't know about all the libraries and frameworks that could be relevant, so try to keep things broad and open ended. The whole point here is just to observe. You won't be using much of if any of the actual code produced here. What we're looking for is ideas.

Example:
> Create a todo list in rust

It's also a useful way to test the capabilities of models. By giving it such an open ended question, you can really see to what extent the model can infer and extrapolate. I've also noticed that with each new generation of model, they can create longer and more coherent outputs with even less descriptive prompts.

You can even run this with a few different models (or parameters if you're in an application that lets you tune those) and just see what it gives you. Using different models and params will let you see different approaches and one might just catch your eye or inspire you to dig deeper. And again, it also lets you see the unique capabilities of each of the different models.

B. **prototyping**: when Im ready build a deployable prototype or MVP, I take a slightly different approach. In this case, I've already done my research, I've got my ideas sorted, and I'm familiar with the details of the language and tooling. Here I can provide a lot of additional context because I can describe in more detail what I want. Some of this could also be written in an `AGENTS.md` file or similar so that you don't have to repeat yourself in each prompt.

Example:

```
Use python and the monsterui library to create a todo list app.
Features: 
- Each task should have 3 statuses: todo, in progress, and done. use the color red for todos, blue for in progress, and green for dones.
- Tasks can be linked to each other
- Tasks should be assignable to a user
- Tasks should have comments

Requirements:
- use a sqlite backend and the `sqlite3` python library
- **only** use the `monsterui` library for frontend work
- create a `requirements.txt` file with all the necessary depdenencies


Use the example image below for how the layout should look
<pasted image>

Use the monster ui docs below for working with the library
<pasted docs or links>
```

Remember that you've got pretty big context windows to work with now, so give it as much context as you can think of.

#### Vibe Coding vs AI-Assisted Engineering

"Vibe coding" is a term that's being brought up more and more frequently, especially in relation to starting new projects from scratch. I think there are a few different definitions depending on who you ask. I recommend reading some of [Simon Willison's posts](https://simonwillison.net/tags/vibe-coding/) if you're interested in the history of the phrase.

My own definition of vibe coding is writing close to 100% of the code with an LLM without any real review or manual intervention. You just start with a basic idea like the first example above and continue to build upon it solely through prompting. This is great for quick prototypes and smaller projects, but you'll quickly run into some limitations as the project grows. When you only use prompts, you begin to accumulate technical debt as the LLM just keeps barreling forward without an retrospective.

Now contrast that with "AI-assisted engineering" which I would define as a more rigorous method of LLM-based development that requires human involvement than vibe coding. I've had much better success with moving projects past the initial phase when I take the time to refactor and fix things manually before the LLM goes off the rails on a bad assumption or incorrect logic. Another part of AI-assisted engineering is integrating a more complete pipeline of linters, formatters, LSP diagnostics, tests, and CI/CD actions into your AI-assisted workflow. When the LLM has access to these additional tools, it can get more immediate feedback and automatically conform to your code style. It's a noticeably more positive experience when the LLM can fix LSP errors and failing tests before you even see the final output. We'll talk more about tools like this in the upcoming agents section.

### Mature Projects

There are some additional considerations when working with LLMs on mature projects: namely the fact that they're generally going to involve larger code bases which are too big for an LLM to fit all in the context window, and that there is a history of decision making that the LLM isn't privy too. This makes it so you can't as easily just ask for a new feature to be added without a bit of thought first.

There is still some low-hanging fruit that can get you quick and easy wins with an LLM like refactors and writing docs and comments. LLM's can power through certain types of refactors, even in large code bases, with great success. The key consideration here is again the context required. For example, changing a function signature does not necessarily require knowledge of the history of the project. Similarly for writing descriptive documentation like usage and requirements and writing informative comments. Be aware though that you may run into some trouble asking LLMs to document the "why" parts of your project since that's best done by the original decision makers in the project.LLMs are good at documenting the "how" parts though.

Building new features is where tests and other tools like your CI/CD pipeline have a much larger impact on your success. Without access to those tools, an LLM can easily get lost trying to integrate a new feature in an existing code base. But with them, you can be more confident that there won't be major regressions as long as your existing tests are good. LLMs are rarely perfect though even with additional tools, so be careful and review all code that goes into an existing project. You should also make sure that additional tests are created alongside and newly generated code. You can certainly have those tests generated for you too, but human review is still necessary at this stage of LLM-assisted development.

## Realtime Audio

Having a realtime audio conversation with AI is a new and exciting paradigm that I've only just begun exploring. Text to speech and speech to text have advanced significantly to the point where features like ChatGPT's advanced voice mode and nearly indistinguishable from real human speech. There are lots of use cases for such a feature (if you need ideas, ask an LLM!), and I'm still trying to figure out where it fits best into my own workflows. I'd like to think I'm a decent typist, at least decent enough to where there is minimal friction to typing lots of text back and forth with an LLM. However, there are plenty of times where my keyboard isn't handy - in these cases, having the option to speak is often much easier than say trying to type something out on a phone or tablet keyboard.

One thing I have tried recently is performing a mock interview with advanced voice mode while out mowing the lawn. That was a great experience because I was able to have an detailed conversation completely hands-free. It felt just like I was having a real phone interview, but even better since I was able to ask for feedback on each question I answered during the interview. I can definitely see myself using voice modes more when I'm on a walk or driving and want to have a conversation about something new I'm learning or an idea I'm looking for feedback on.

## Agents and Tools

Agents are one of latest and most fascinating topics in the AI world today. Things are still in the early stages, and I'm still deep into learning about and testing all the latest tools like Codex. I'll likely end up writing an separate post to talk about agents in depth soon.

The basic premise of agents are that they are LLMs that have access to tools. Tools can be anything from the ability to search the web to access to a set of GitHub APIs that allow an agent to review an issue or create a pull request. Giving tools to an LLM is a very powerful concept as it greatly increases their ability to interact with the world around them. It's even more exciting when you think about taking it one step further and consider tools that interact with the physical world - this is where the robots come in.

## Conclusion

I hope this gave you some ideas on how you can you AI yourself to be more productive, develop more software, and reach even higher than you thought possible on your own. If you'd like to share your own thoughts, please send me an email - I'd love to discuss! My next post will be about agents, so stay tuned!

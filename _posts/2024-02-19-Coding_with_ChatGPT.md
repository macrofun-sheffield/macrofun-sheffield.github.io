---
layout: page
title:  "Coding with ChatGPT"
teaser: "Using ChatGPT or other LMMs for coding?"
breadcrumb: true
tags:
    - coding
    - discussion
    - LMM
    - ChatGPT
---

# ChatGPT for coding

[Natalie Cooper](https://nhcooper123.github.io/) from the NHM gave us an excellent introduction to using ChatGPT for coding with a specific emphasis on how we can use it as a teaching tool to help students to learn coding. Find Natalie’s slides here: https://shorturl.at/kqvB5.

We started with a discussion on what LLMs are in general and ChatGPT in particular. LLMs are [large language models](https://en.wikipedia.org/wiki/Large_language_model) which basically predict which token (a word or part of a word) fits best next in a sequence, given the context provided. For example a LLM can give some probability of what comes next in the sentence “The cat eats the…” (the next word is more likely to be “mouse” rather than “rich”). Of course, the “large” part of LLM makes this example trivial and makes the results of using an LLM much more impressive than this example. ChatGPT is the LLM that has received the most attention in the last year but is by far not the only one (e.g. [LLaMA](https://en.wikipedia.org/wiki/LLaMA) can be better for coding; or [Microsoft Copilot](https://en.wikipedia.org/wiki/Microsoft_Copilot) (Bing Chat) that allows other inputs than text). There are two versions of ChatGPT (3.5 - free, and 4.0 - paid) with the latest being an improvement on the previous one (better results, less errors, etc.).

ChatGPT (as most LLMs) works by entering a prompt written in a human conversational language (e.g. English as you’d talk to a colleague rather than google style search terms). This prompt is a request that will help the LLM to predict what text comes next. The more specific the prompt, the better the results you get. As suggested by Rob, the following prompt:

```
Write the blurb for a short story about a computational evolutionary biologist called Thomas. He lives in Sheffield, a rainy city in the North of England and his primary research interest is innovation and elaboration on the tree of life. He has a luscious ponytail and beard and smokes a pipe which he is always having to relight. The story should be about Thomas accidentally uncovering an organised crime syndicate based in the Natural History Museum, London. The story should be written in the style of Raymond Chandler.
```

can give you a better results than:

```
Write the blurb about Thomas.
```

Though both are definitely worth trying, if only to have a grasp how prompts influence the results.

We then followed up with a fun exercise trying to reproduce a plot using either ggplot or ChatGPT. The exercise was quite fun and introduced how and when to actually useChatGPT: if you’re an advanced user of ggplot, ChatGPT can give you a good head start by providing the code structure, comments, etc, but you’ll achieve better and swifter results by coding it yourself in your favourite text editor or IDE. If you’re not familiar with the package or the language though, it can be very useful to get you going. It requires writing and rewriting the prompts many times but that gets you actually doing what we discussed is the most important part of coding: **the most important part of programming is about the logic of how the task should be done, rather than the exact details of the code you need to make that happen.**

The discussion then followed on what are the positive uses of ChatGPT (or any LLM) in general:

 * It’s great for starting something new: it removes the whole “blank page” stress, or it can give you a great starting place when working on a completely new project (see the ggplot example from [Natalie’s slides](https://shorturl.at/kqvB5)).

 * It’s great for helping you with repetitive or less cognitive tasks. For example, in coding, when writing a script quickly for someone, you can feed the script in ChatGPT and ask it to comment or explain it. This allows you to focus on the comments/explanation of parts of the script that are more crucial/complex and allows you to ignore the ones that are more obvious to you (e.g. all the “#Loading the packages”, “#Loading the data”, kind of comments).

 * It’s great for helping you with “bullshit” like admin. Because a decent amount of our work is [bullshit work](https://en.wikipedia.org/wiki/Bullshit_job) (think about parts of grant writing, reports that no one reads etc…) or work following social codes (writing polite emails, etc…) and that both these tasks are text based, LLMs are excellent at automating these tasks saving you a lot of time. We discussed some people using it to transform angry “no!” emails into polite and apologetic rejection ones.

However, following these positive points, our discussion also inevitably led to the more negative points of relying too much on AI:
    
 * First, the outputs of LLMs, although very often impressive, are only text predictions. In terms of coding for example, this can be problematic because the LLM is not at all following the syntax or coding logic by design. This can lead to some serious output problems, for example when the LLM is predicting a package name that does not exist, or a function that exists in a language but not in the one you’re interested in, etc. This means that these LLMs are **helpful tools but not tools that can do your work unsupervised**, i.e. you’ll have to thoroughly check the output which, in some cases doesn’t really save you that much time (following the workshop ggplot example, if you’re very familiar with ggplot, you’ll get the exercise done much faster than ChatGPT).

 * Second, so far, all of these AI tools (e.g. ChatGPT) are designed foremost for making profit, not helping you being a better researcher (though some open source and community led projects are ongoing - e.g. [BLOOM](https://en.wikipedia.org/wiki/BLOOM_(language_model))). Unless you specifically set the privacy settings in ChatGPT, all the information you type is recorded and saved for the usual internet money making schemes (selling data, targeted ads, profiling, etc…).

 * Third, the “hidden” negative impacts can be pretty daunting. This ranges from legal concerns (the LLM is using - “stealing”? - other people’s work without consent or crediting) to environmental ones (the carbon footprints of these tools can be incredibly high, especially for the training phase of the model) through to human right abuses (again, especially [during the training phases](https://time.com/6247678/openai-chatgpt-kenya-workers/)).

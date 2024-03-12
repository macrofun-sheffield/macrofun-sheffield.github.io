---
# author: Alain Danet
layout: page
title:  "Reproducible R"
teaser: "Reproducible computation at scale in R with the target package"
breadcrumb: true
tags:
    - coding
    - tutorial
    - reproducibility
    - R
---

These notes are designed to accompany a presentation and workshop given by Alain Danet as an introduction to the use of the ‘targets’ package for optimising large statistical computational pipelines in R. The corresponding slides are available here.
·         A problem in analysis is that we have bigger and bigger datasets – this means that if we’re running complicated models and creating plots, it can take a lot of computational time to run. Even analyses on small datasets can take up a lot of time if the models are particularly complex.
·         One of the key things that increases computational time is repetition. Rerunning an entire script every time you change a line, for example, can mean analyses take much longer than they need to. We often have the perception that it will be quick to rerun stuff, but in reality this often isn’t true!
·         It’s therefore convenient to have a tool that reduces computational time. The ‘targets’ package is one solution.
·         A computational pipeline has a number of interconnected steps – changing something in one step can mean that some, but not necessarily all, other steps also need to be updated and rerun. If you have a really complicated pipeline, though, working out manually what other steps have become outdated can be a daunting task, so you might just end up rerunning everything. This is unnecessary and can drastically increase computational time.
·         ‘targets’ offers a way to build a pipeline tool that allows you to keep track of which steps depend on other steps. If you update one step, targets automatically works out and tells you which other steps are now out of date and need to be rerun, Crucially, this allows you to rerun only the steps that you need, reducing computational time

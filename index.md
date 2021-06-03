--- 
title: "Analysing Flights with R - UNDER CONSTRUCTION"
author: "David Marsh"
date: "2021-06-03"
site: bookdown::bookdown_site
documentclass: book
bibliography:
- book.bib
- packages.bib
biblio-style: apalike
link-citations: yes
geometry: margin=3cm
description: This is a training and reference manual for Eurocontrol staff, on how
  to use RStudio to analyse our traffic data. - UNDER CONSTRUCTION -
---

# How to use this book {#howto}
-- UNDER CONSTRUCTION! ---

## Before you start

You need to have RStudio and R installed on your machine. These are available from IT Support as standard installations, or if you want to work on your own machine, both are open source: [R download here](https://cran.r-project.org) and [RStudio download here](https://rstudio.com/products/rstudio/download/).

We assume that you are familiar with air traffic data, so won't spend time explaining the concepts.

This book is available as [html here](https://david6marsh.github.io/flights_in_R/) and can be downloaded complete with data from here (TBD).

If you're working through the book as training, then it will help to install (TBD - training bundle)

If you want a more speedy introduction, then Enrico Spinielli has covered the [important steps here](https://github.com/euctrl-pru/portal/wiki/Intro-to-everything). Sebastien Thonnard and others have compiled an (internal) [wiki on the intranet](https://ost.eurocontrol.int/sites/STATFO/WikiPages/R_documentation.aspx).

There are also several useful 'cheat sheets', which we'll introduce as we go through.

Although this was written with Eurocontrol staff in mind, beyond this short section I think you'll find everything is open to anyone. The joys of open source! 




## Ways to Use the Material

The book aims to support you using it in a number of different ways:

* Training. Work through chapters in order, or pick out specific chapters if you need a refresher on a topic. Read the explanations, grab the code and run it, and test your understanding by answering questions [in square brackets in the text] and doing exercises.
* Reference. Use the book search (top left of this page) to find some how-to material directly.
* Snippets. The book builds up snippets of usable code (TBD - how complete?) for you to cut-and-paste.

But it is never going to replace the huge amount of excellent information, especially in [stackoverflow](https://stackoverflow.com/). A key principle is that _"Google is your friend"_: even if I prefer [DuckDuckGo](https://www.duckduckgo.com) for many purposes, a google search is often more productive for R. Whether it's for how to do something, or how to respond to an error, usually someone has already suffered, and some kind person has answered. Google it.

## Structure of the book

* Chapters \@ref(howto) and \@ref(start) should get you up and running in RStudio.
* Chapters \@ref(firstLook) to 5 start with how to look at data, using a data viewer and with some initial graphs. If you're mostly using existing code that creates simple charts, written by someone else, then these chapters should help you find your way around, and start to adapt the code for your own needs.
* Chapters \@ref(loopsfunctions) (and TBD) do more data loading and wrangling, covering the basics of loops and functions
* more...TBD
* Chapter \@ref(splitparse) covers pulling data out of strings (parsing) using 'regular expressions', with NOTAMs as the application.

## What's gone wrong?

Learning from mistakes is essential, so each chapter has a "what's gone wrong?" section near the end, discussing some typical potholes that we all fall into from time to time. Look there especially if you don't see the results from the code in the book that you were expecting.

We start the book with one classic pitfall: R is *case sensitive*, which can be quite a culture shock if you're brought up on other systems. This matters for filenames as well as code. When in doubt, check the case of what you've typed!


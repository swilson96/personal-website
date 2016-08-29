---
layout: page
title: Projects
permalink: /projects/
url: /projects/
---

* [blossom](#blossom)
* [#YouRock](#yourock)
* [Physio Assessment Tracker](#physio)
* [Sudoku Solver](#sudoku)
* [This Site](#thissite)

# <a name="blossom"></a>blossom
[http://swilson.co.uk/blossom/](http://swilson.co.uk/blossom/)

This site basically provides a UI to enter a graph and execute the weighted Edmonds blossom algorithm for finding maximal pairings. Several people can share an URL and edit the grid/graph at the same time, syncing with firebase.

Tech: React, Redux, Firebase. [Code.](https://github.com/swilson96/blossom)

The inspiration was to find an optimal pairing for a Softwire random coffee draw, a scheme to get people to get to know someone else in the company over a coffee. We wanted to choose a pairing of people (nodes) based on how well people already knew each other (weighted edges).

# <a name="yourock"></a>#YouRock
[http://www.yourocksite.com/](http://www.yourocksite.com/)

"Publicly praise your friends when they are awesome"

A web version of a popular internal tool at Softwire. We have an app that allows us to praise colleagues, and that triggers a popup on everyone's screen. It's encouraged an atmosphere of positive feedback and praising each other a lot more, and I wanted to bring that to the wider world. YouRock is one attempt, piggy-backing on twitter to provide the sign-up and sending praise. Softwire have some other ideas in the pipeline, so get in touch if you're interested.

Tech: Node, Express, MongoDB, Monk, Heroku

It's very much a work in progress at the moment, but feel free to get involved by following [@YouRockSite](http://twitter.com/YouRockSite) on twitter and tweeting with #YouRock.

# <a name="physio"></a>Physio Assessment Tracker and Visualiser
[http://www.physio-results.co.uk](http://www.physio-results.co.uk)

A private site to track all the details of a full physical MOT for athletes, as performed by the experts at [Physio&Therapy](http://www.physioandtherapy.co.uk/) in London. Problem areas are visualised and highlighted in traffic-light colours automatically.

![Physio Assessment Visualisation](/images/physio.png)

Tech: C# MVC.NET, SQL Server, jQuery, HTML Canvas

# <a name="sudoku"></a>Sudoku Solver
[http://sudoku.swilson.co.uk/](http://sudoku.swilson.co.uk/)

The first program I ever wrote for fun was a Sudoku solver. It lay unused for many years until recently when I remembered it existed. I'll admit it's not very original to write a sudoku solver, but since I had one lying around I thought I'd stick it on the web.

Tech: C# MVC.NET, Knockout.js, Heroku, Mono. The code is [on github](https://github.com/swilson96/sudoku-web-solver), but don't expect it to be pretty.

Heroku doesn't directly support .Net (and AppHarbor and Azure don't let me use my own domain for free), so I'm using this [Mono buildpack](https://github.com/friism/heroku-buildpack-mono), which appears to work for .Net 4.5 only if you _don't_ make the suggested changes to web.config.

# <a name="thissite"></a>This Site
[http://swilson.co.uk/](http://swilson.co.uk/)

Yes, it's a web project. I was originally going to use [Weebly](http://www.weebly.com/) (and [here is the result](http://swilson96.weebly.com/)). Weebly is very nice but it wouldn't let me use my own domain and it stuck a load of weebly advertising at the bottom. Weebly also had no support for code snippets. It may or not be feasable to fix that with a Weebly plugin using [SyntaxHighlighter](http://alexgorbatchev.com/SyntaxHighlighter/) or similar.

I was looking around for something else, even vaguely considering Wordpress, when I randomly came across Jekyll. It seems to be working for now. I'll have to put more effort in to maintain the style and add features, but oh well. The theme is based on Incorporated, by [Inc](https://sendtoinc.com), with a few adjustments from me like the addition of a linked-in icon.

I'm actually finding markdown a lot easier to use than the Weebly drag and drop interface.

Tech: Jekyll, Markdown, some fiddling with HTML and SCSS.

The code is even [on github](https://github.com/swilson96/swilson96.github.io) (the site is hosted on github pages).

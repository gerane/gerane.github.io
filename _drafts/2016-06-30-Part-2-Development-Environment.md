---
layout: page
title: Part 2 - Development Environment
teaser: Building a Release Pipeline
subheadline: Release Pipeline
date: "2016-06-30 8:00"
categories:
  - ReleasePipeline
tags:
  - powershell
  - VSCode
  - ISE
  - Atom
  - PowerShellStudio
  - Editors
  - Extensions
  - Modules
  - ReleasePipeline
image:
  thumb:
  homepage: 2016/06/GitBranches.PNG
header:
  image_fullwidth: 2016/06/GitBanner.png
---

People take their development environments seriously. Just put a group of Vim and Emacs people in a room together and watch the fireworks. You can bet there are some that would point out that I should have listed Emacs before Vim in the previous sentence and others that would have rubbed in the fact that I listed Vim first. When we spend so much time in an environment it starts to become personal. When we have a personal attachment like this, it is easy for us to take a simple differing opinion as an attack. It is similar to religion and politics in many ways.

Before I get any further, I want to make sure people understand my intensions of Part 2. I am in no way saying one editor is better than another. Most of the topics I will be discussing can be adopted to other editors. My goal is that I am able to give people ideas that improve their development environments. I advocate people finding what feels right for them and using that. There is no "best" text editor or IDE, but what is "best" for your own personal preferences. Experiment and see what other editors have to offer, and use what improves your productivity. The few exceptions to this for me are editors that are no longer supported and have been abandoned. As an example, if they take PowerShell seriously, I will strongly recommend someone using PowerGUI to move to a different solution.

My goal is to inspire people with ideas to help improve their own development environment. I am always looking to improve my own editor experience, and try to learn as many or educate myself to as many of them as I can.


As an example, I immediately started using [Atom](https://atom.io/) the first time I heard about the beta. I even got an awesome badge and thank you letter mailed to me by Github as thanks for contributing to the beta, which you can see below. Let me just say, more companies should do this sort of surprise thank you to their contributors, it made my week when I got it. I



A Development Environment is far more than just a text editor that we use to write code. If properly utilized an Editor can drastically improve productivity and the code the we write. Part 2 is dedicated to highlighting some of these Editor features and explaining how they can enhance a Release Pipeline. This is a topic I have always been very interested in, and I hope some of my excitement might rub off on others.

## Where Editors are heading
I am really excited about the options that are available for PowerShell right now. What is even more exciting is that I think the options are only going to get even better in the very near future. I first want to go over some very important changes and projects that are happening right now that should lead to many great things for the PowerShell community and other communities as well.

The first important topic is what is behind the PowerShell Extension for VSCode called [PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices). The PowerShell Editor Services (PSES) uses a new Open Source protocol that Microsoft is behind called *Language Server Protocol*. You can read more about Microsoft's partnership with Codenvy and Red Hat [here](http://www.zdnet.com/article/open-source-microsoft-protocol-aims-to-be-a-programming-standard/). The Language Server Protocol aims to be a standard way for Code Editors and Language integration. What does this mean for PowerShell? Well, it means that any Editor that implements the Language Server Protocol should be able to easily add support for other languages. This means that every editor could potentially have the same PowerShell experience in terms of Intellisense and all of the other features we have come to love. The other amazing benefit, is this opens up the possibility of cross platform PowerShell support. This will eventually allow Linux and MacOS users to get the same benefits in the editor as Windows users by utilizing a windows VM or external Windows device.

The next big things is called the [Monaco Editor](https://github.com/Microsoft/monaco-editor). This is the editor that powers VSCode, but it has been extracted out and made embeddable in webpages. You can see a demo [here](https://microsoft.github.io/monaco-editor/index.html). I am extremely excited to see where these projects go.

## Editor Options


## What's Next?
Part 2 will start bringing in many of the other tools covered in the series. You will start to get an understanding of how all of the pieces work together. After that we will start going into detail about how to configure and use each of the pieces of the Pipeline.

## Release Pipeline Series

- [Release Pipline Landing Page]({{ site.url }}/releasepipeline/Building-a-Release-Pipeline)
- [Part 1: Gitting Started]({{ site.url }}/releasepipeline/Part-1-Gitting-Started)
- [Part 2: Development Environment]({{ site.url }})

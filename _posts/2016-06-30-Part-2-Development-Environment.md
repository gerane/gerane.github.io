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

The line between Text Editors and Integrated Development Environments (IDE) has certainly blurred, and many light text editors now have full blown debugging environments and some of the other features that used to only be in full IDE type editors. I tend to categorize Editors as light, full or console because of this. Light Editors are ones like Sublime Text, Atom, Visual Studio Code, Notepad++, etc. These tend to be more lightweight with less emphasis on GUIs/panes and support a large number of languages. Full Editors are ones like Visual Studio, PowerShell Studio, Eclipse, etc. These often specialize in a single language and might offer more functionality for that specific language than others. The Console category are the editors that place an emphasis on the console or are ran inside of a console. I tend to put PowerShell ISE in this category along with editors like Vim, Emacs, Nano, etc. There are great options in all of these categories for PowerShell. 

## Goals  

Before I get started, I want to make sure people understand my intensions in this series in regards to Editors. I am in no way saying one editor is better than another. Most of the topics I will be discussing can be adopted to other editors. My goal is to give people ideas that improve their development environments. I advocate people finding what feels right for them and using that. There is no "best" text editor or IDE, but what is "best" for your own personal preferences. Experiment and see what other editors have to offer, and use what improves your productivity or feels right for you. I used the Full, Light, and Console categories becuase I feel they offer a good representation of some of the types of Editors that people prefer the feel of. Find the type that feels right for you and try to take the topics in this series and adopt them to your editor.

People take their development environments seriously. Just put a group of Vim and Emacs people in a room together and watch the fireworks. You can bet there are some that would point out that I should have listed Emacs before Vim in the previous sentence and others that would have rubbed in the fact that I listed Vim first. When we spend so much time in an environment it starts to become personal. When we have a personal attachment like this, it is easy for us to take a simple differing opinion as an attack. It is similar to religion and politics in many ways. amd  I want to encourage the PowerShell community to look past this, and to try to focus on improving the community as a whole. The more great editors the better if you ask me.

## Where Editors are Headed

I am really excited about the options that are available for PowerShell right now. What is even more exciting is that I think the options are only going to get even better in the near future. I first want to go over some very important changes and projects that are happening right now that should lead to great things for the PowerShell community and other communities as well.

The first important topic is what is behind the PowerShell Extension for Visual Studio Code (VSCode) called [PowerShell Editor Services (PSES)](https://github.com/PowerShell/PowerShellEditorServices). *PSES* uses a new Open Source protocol that Microsoft is behind called *Language Server Protocol (LPS)*. You can read more about Microsoft's partnership with Codenvy and Red Hat [here](http://www.zdnet.com/article/open-source-microsoft-protocol-aims-to-be-a-programming-standard/). *LPS* aims to be a standard way for Code Editors to integrate with Languages. What does this mean for PowerShell? Well, it means that any Editor that implements *LPS* should be able to easily add support for other languages and give users the same experience in any editor. This means that every editor could potentially have the same PowerShell experience in terms of Intellisense and all of the other features we have come to love. I previously mentioned that Full IDE Editors typically focused on a single language and were written with that language in mind, the *PSES* is attempting to  

The other amazing benefit, is this opens up the possibility of cross platform PowerShell support. This will eventually allow Linux and MacOS users to get the same benefits in the editor as Windows users by utilizing a Windows VM or external Windows device. 

The next interesting project is called [Monaco Editor](https://github.com/Microsoft/monaco-editor). This is the editor that powers *VSCode* under the hood, but it has been extracted out and made embeddable in webpages. You can see a demo [here](https://microsoft.github.io/monaco-editor/index.html). If you have noticed, the online OneDrive editor now has PowerShell intellisense. It is getting this by using the Monaco Editor. I am very interested to see where this project goes.

## Editor Options

I have given some brief examples of Editors above, and I wish I could go more into details on all of them. Due to the length, I am going to briefly discuss one editor from each of the categories. I will be using *VSCode* as my example for the rest of the series starting with Part 3. The other two editors I have chosen are *PowerShell Studio* and *PowerShell ISE*. When discussing *VSCode* I will try to offer references back to these and briefly mention how they can achieve the same or similar.

#### Full Editor

[PowerShell Studio (PSS)](https://www.sapien.com/software/powershell_studio) is a great full editor that is packed with features. If you like editors in this category, I highly recommend the trial of *PSS*. It really has too many features to mention here. The advanced function builder is one of my favorite features. You can see the Function Builder below.

![Function Builder](/images/2016/07/PSESfunctionbuilder.png)

You can achieve similar with digging down really deep into snippets, but that is not for everyone. It has a great debugger with some very cool features as well. Since *PSS* is a paid solution, one thing I can say is it has great support. I know this is very important to a lot of organizations and I felt it important to mention. The list of great features just goes on and on. You can see a screenshot of *PSS* below.

![PSS](/images/2016/07/PSES.png)

#### Console Editor

PowerShell ISE (ISE) is the obvious choice here. I highly recommend using [ISESteroids](http://www.powertheshell.com/isesteroids/) with *ISE* since it almost makes it a completely different editor with all of the features it adds. There are some tasks that just seem made for the *ISE*, and given the fact that it comes stock in Windows makes it a very popular choice. There are many who advocate Microsoft to drop development of *ISE* in favor of *VSCode*, but I am not totally on board with that. As I was explaining earlier, people have different preferences for editors, and some are going to love *ISE* while hate *VSCode*. Killing the *ISE* off just does not make sense to me. What other editors have tried to replicate is the *ISE's* integration with the terminal. In my own opinion, none of the other options are even in the same ballpark yet. I still feel *ISE* is the most productive environment in those instances where you need to actually figure out how to write a particular piece of code. You can see screenshot *ISE* with ISESteroids below.

![ISE](/images/2016/07/ISE.png)

#### Light Editor

*VSCode* is the light editor I have chosen and is also the Editor I will be focusing on when discussing integrations with the Pipeline. *VSCode* is improving at a rapid rate and has gained impressive amounts of adoption. I will be the first to admit there are areas where *VSCode* needs improvement, but I like the fact that I can help with that improvement process via Github. One advantage of a Light editor like *VSCode* is that it gives you a single editor to work with any type of file. *VSCode* also offers some great features like its debugger, Task Runners, Extensions, and Command Palette, but the features I am most excited about are the ones that are a part of the *PSES* and can potentially work in any editor that adpots it. You can see a screenshot of *VSCode* below.

![VSCode](/images/2016/07/vscode.png)


## Which Editor is right for you?

This is a question you need to answer for yourself. It can be hard to articulate why one editor feels right and another feels wrong, and the reasoning is going to be unqique for each person. 


## What's Next?

Part 2 will start bringing in many of the other tools covered in the series. You will start to get an understanding of how all of the pieces work together. After that we will start going into detail about how to configure and use each of the pieces of the Pipeline.

## Release Pipeline Series

- [Release Pipline Landing Page]({{ site.url }}/releasepipeline/Building-a-Release-Pipeline)
- [Part 1: Gitting Started]({{ site.url }}/releasepipeline/Part-1-Gitting-Started)
- [Part 2: Development Environment]({{ site.url }})


My goal is to inspire people with ideas to help improve their own development environment. I am always looking to improve my own editor experience, and try to learn as many or educate myself to as many of them as I can.


As an example, I immediately started using [Atom](https://atom.io/) the first time I heard about the beta. I even got an awesome badge and thank you letter mailed to me by Github as thanks for contributing to the beta, which you can see below. Let me just say, more companies should do this sort of surprise thank you to their contributors, it made my week when I got it. I



A Development Environment is far more than just a text editor that we use to write code. If properly utilized an Editor can drastically improve productivity and the code the we write. Part 2 is dedicated to highlighting some of these Editor features and explaining how they can enhance a Release Pipeline. This is a topic I have always been very interested in, and I hope some of my excitement might rub off on others.
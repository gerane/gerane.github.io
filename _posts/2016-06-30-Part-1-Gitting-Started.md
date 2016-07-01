---
layout: page
title: Part 1 - Gitting Started
teaser: Building a Release Pipeline
subheadline: Release Pipeline
date: "2016-06-30 8:0"
categories:
  - ReleasePipeline
tags:
  - powershell
  - VSCode
  - Git
  - Github
  - ReleasePipeline
image:
  thumb:
  homepage: 2016/06/GitBranches.PNG
header:
  image_fullwidth: 2016/06/GitBanner.png
---

There is only one place this series can start. **Source Control**. You may sometimes hear it referred to as Version Control. Regardless of what you call it, it is an absolute requirement and is the glue that holds everything else together. Because of this, Part 1 is completely devoted to Source Control.

When deciding on what source control to use you will find multiple types, but I highly recommend Git. If you want to collaborate you really want to use Git in conjunction with Github. It really is the only option you should consider if wanting work with the community.

## Git Clients

### Github Desktop
There are several good Git Clients to choose from. If you are just learning, it might be best to start with one that feels comfortable to you. A good place to start is the [Official Github Client](https://desktop.github.com/). It has a straight forward interface and is simple and easy to use. It has an easy setup process for signing into Github and entering in your Two Factor Authentication.

![Github Client Image](/images/2016/06/Github.png)

### SourceTree
The client I got started with was [SourceTree](https://www.sourcetreeapp.com/). It has a lot of features and gets the job done. Sourcetree has a busier interface, but that might not be an issue. Some may prefer this over the flatter metro sort of style of the Github Desktop client. It may not be quite as easy to use if you are just getting started.

![SourceTree Image](/images/2016/06/SourceTree.png)

### GitKraken
The client that I am using now is [GitKraken](https://www.gitkraken.com/) and it is by far my favorite. I try to use the commandline interface for git as much as I can, but sometimes I just need a good client when working with something more complicated. It might be a little harder to use if you are just getting started, but it just outperforms the other clients I have used. It has a great merge and diff tool built in for resolving conflicts that is very nice. It really starts to outperform the other clients when you start getting more into collaborating and using branching. Below is an image of GitKraken in a white theme.

![GitKraken](/images/2016/06/GitKrakenLight.png)

The other feature that I love about GitKraken was just added in a recent release of v1.4. It now has a **Command Palette** built into the client. If you aren't aware of what one is, I will be covering them in one of the upcoming posts in this series. A Command Palette is a way to launch commands from anywhere in the client. On Windows, the typical keybinding for a Command Palette is <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>. You can see an image of the GitKraken Command Palette below.

![Command Palette](/images/2016/06/CommandPalette.png)

Typically a Command Palette gives you easy access to a way to enter client, extension, or plugin based commands or to do searches for files etc. I have been a huge fan of Command Palettes and have been wanting to see more adoption of them outside of Editors, and it is great to see more Applications adopting them. I still feel Windows should have a form of Command Palette built in.

I will be focusing on GitKraken in my examples for this series. It helps highlight how Git fits into the Pipeline.

## Don't ignore the Gitignore
When testing functions and code in your project, you might have some test data, test scripts or other resources that you might not actually want distributed into your Repository. This is where the `.gitignore` comes into play. This let's you keep this type of clutter in your project, but not have to worry about it showing up on github. An example usage in the **Posh-Teamviewer** module is a script that has preloaded data in it that allows me to easily test functions as I am writing them.

```
.vscode
.gitignore
Debug<%=$PLASTER_PARAM_ModuleName%>.ps1

# Windows image file caches
Thumbs.db
ehthumbs.db

# OSX
.DS_Store
.AppleDouble
.LSOverride
.Spotlight-V100
.Trashes
._*
.DocumentRevisions-V100
.fseventsd
.TemporaryItems
.VolumeIcon.icns

# Office Temp Files
~$*

# Powershell Studio
*.psbuild
*.psproj
*.psprojs
*.MainForm.psf
*.TempPoint.psd1
*.TempPoint.psm1
*.TempPoint.ps1
*.pss
```

Here is an example `gitignore` that is similar to the ones I usually start a project with. There are some standard rules you might want to add if working on different platforms. There are also some of the [PowerShell Studio](https://www.sapien.com/software/powershell_studio) Project files excluded as well. This example also has an odd entry `Debug<%=$PLASTER_PARAM_ModuleName%>.ps1` which will be covered in Part 2.

## Branching is Scary!
It certainly was scary to me when I was getting started with it. For those unfamiliar, Branching allows you to have multiple different versions of your code without overwriting each other. You can have your main branch, or **"master"** Branch, that has your code that is what you would use or release. You can then create a second branch for working on new features, bugs, or anything that is currently being developed. This lets you keep your current known good state intact. A tool like GitKraken makes branching much easier to use and understand, especially when learning. You can see what branching looks like in GitKraken below.

![Image of GitKraken Branching](/images/2016/06/GitBranches.PNG)

Don't let Branching intimidate you. There are a number of different branching models out there or rules people say never break, but ignore it. What is important is to learn the basics of how branching works and how it is beneficial. Once you have the basics, then you can start worrying about other people's preferences. You will also find that projects usually have their own preferences and will outline how they want you to contribute in a **contributing.md** file in the root folder of the project. You can also ask the project members how they would like you to branch or handle your pull requests.

My advice is to start using branching in your own personal scripts and projects. Make some backup copies of your repository just to be safe and give yourself some comfort and just start using it for everyday small stuff and get used to it with your own projects. Like anything else, the more you use it and practice it the easier it gets.

Branching is also utilized later in this Series when git starts integrating with the other tools and processes I will be covering.

## Pull Requests
Now that you have started using branching, you should start practicing Pull Requests on your own projects too. You will see the benefits when you start integrating in with Continuous Integration and Deployment tools. It is also important to understand that a Pull Request is not final. While the Pull Request is still open, you can take feedback or suggestions and add those changes in. When you add additional commits to an open Pull Request, the Pull Request will detect those changes and update them in. It creates a great opportunity for collaborating with the others from the project.

![Pull Reuquest](/images/2016/06/PullRequest.png)

Above you can see back and forth collaboration on a Pull Request. If it helps you can always ask or open an issue about your potential pull request to get feedback.

## Merging
{:refdef: style="text-align: center;"}
![Merge](/images/2016/06/GitMerge.jpg)
{: refdef}

One area that GitKraken is really helpful is merging. It has a great built in tool to help you break down any conflicts and pick and choose what to merge. Inevitably you are going to encounter a situation where normal git merging isn't successful and it will require a you to make the change yourself. I recommend practicing using merge tools with your own small projects, that way when a real issue comes around you know what to do. You don't want to encounter your first big merge issue on something important or in production.

![GitKraken Conflict](/images/2016/06/conflict.PNG)

You can see a conflict in GitKraken in the example above. It allows you to select an entire **A** or **B** commit change, or you can get more granular and pick and chose certain lines from each. You can see an example of this below.

![GitKraken A and B Merge](/images/2016/06/AandB.PNG)

I felt it was important to go over a few of the aspects of Git that will be used in this series. Being able to trigger automated tests and scripts when a project has a new commit or pull request is a very nice feature of the Pipeline. Git is tied in to pretty much every aspect of the Pipeline.

## What's Next?
Part 2 will start bringing in many of the other tools covered in the series. You will start to get an understanding of how all of the pieces work together. After that we will start going into detail about how to configure and use each of the pieces of the Pipeline.

## Release Pipeline Series

- [Release Pipline Landing Page]({{ site.url }}/ReleasePipeline/Building-a-Release-Pipeline)
- [Part 1: Gitting Started]({{ site.url }}/ReleasePipeline/Part-1-Gitting-Started)

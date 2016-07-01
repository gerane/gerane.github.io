---
layout: page
title: Building a Release Pipeline
teaser:
subheadline: PowerShell
date: "2016-06-30 7:59"
categories:
  - ReleasePipeline
tags:
  - powershell
  - VSCode
  - Git
  - Github
  - ReleasePipeline
  - PSake
  - Plaster
  - PlatyPS
  - EditorCommands
  - AppVeyor
  - Slack
image:
  thumb:
  homepage: 2016/06/GitBranches.PNG
header:
  image_fullwidth: 2016/06/GitBanner.png
---

This series covers a [Release Pipeline](https://aka.ms/thereleasepipelinemodel) for community projects similar to what is outlined in the whitepaper by [Michael Greene](https://twitter.com/migreene) and [Steven Murawski](https://twitter.com/StevenMurawski). The series will explain the processes and tools used to create and deploy a module I recently released called [Posh-Teamviewer](https://github.com/gerane/Posh-Teamviewer) and another module I am working on called [VSCodeExtensions](https://github.com/gerane/VSCodeExtensions). The Posh-Teamviewer module is not particularly exciting, but the process used to build it is much more interesting. I wanted to document the process and share it with the community. I am going to be going over how each of the processes and tools integrate and work together, and how you can get started using them. Instead of going in depth on how these tools and processes work, I am focusing on the basics and how to use them together in a Release Pipeline. This pipeline is flexible and not limited to Modules. It can be adapted to just about anything.

![Release Pipeline](/images/2016/06/ReleasePipeline.PNG)

The above image is an example Community Project Release Pipeline from a slide by Michael Greene. We will be covering a pipeline very similar to this one. The Pipeline covers project creation, version control, building the project, productivity tools, writing the documentation, automating a documentation website, automating Tests and Deployment and automating Publishing to the PSGallery.

### Topics Covered
- Version Control and Git clients
- Plaster
- Visual Studio Code, Code Extensions, Editor Commands and Integrations
- PlatyPS
- Pester
- PSScriptAnalyzer
- PSake
- PSDeploy
- AppVeyor
- ReadTheDocs
- Slack

In each of the categories discussed, I will cover some of the options available and then get more detailed on the option I used. You don't need to use exactly what I did here, these are easily customizable. I will try to mention other options to some of the choices I used. My goal is to give a good example from start to finish of how a Pipeline integrates together and how to get started. You should pick and choose the tools that makes sense for your project and environment.

## Brief Overview of the Pipeline
To give you a good understanding of what is going to be covered, the following is a brief summary of the pipeline.

- [Plaster](https://github.com/PowerShell/Plaster) is used to create the initial scaffolding of the module. It gets us started with our initial structure.
- [Visual Studio Code](https://code.visualstudio.com/) is used with [Editor Commands](http://brandonpadgett.com/powershell/Getting-Started-With-Editor-Commands/), Command Palette and [Task Runners](https://code.visualstudio.com/Docs/editor/tasks) to automate processes like Tests, Builds, Git and several other tasks.
- [Pester](https://github.com/pester/Pester) is utilized to write unit tests and validation tests.
- [PlatyPS](https://github.com/PowerShell/platyPS) is used to write the Modules help in Markdown. This markdown can then be used to reference the help in the Github Repository as well as being converted to external MAML help for the module.
- [ReadTheDocs](https://readthedocs.org/) can take the markdown for PlatyPS and turn it into a documentation webpage for your module. Whenever you commit new code it regenerates your new Documentation site. This allows you to write help once and use it for Module Help, Repo Help and Documentation Webpage.
- [PSake](https://github.com/psake/psake) automates your build processes in Visual Studio Code and AppVeyor.
- [PSDeploy](https://github.com/RamblingCookieMonster/PSDeploy) is used in conjunction with PSake to easily configure and Deploy portions of the project. It automates the deployments of PlatyPS help, local filesystem module directory and publishing to PSGallery.
- [AppVeyor](https://www.appveyor.com/) runs the automated build and deployment process. It is trigger when a new commit is made to the Repository. It can be told to automate publishing the module to the PSGallery if all tests pass. It also automates running your Tests every time someone commits code into the repository and gives the status of the tests in the comments on things like Pull Requests.
- [Slack](https://slack.com/) works in conjunction with AppVeyor. When a build runs it sends the status of the build into a slack channel.


## Release Pipeline Series

- [Part 1: Gitting Started]({{ site.url }}/ReleasePipeline/Part-1-Gitting-Started)

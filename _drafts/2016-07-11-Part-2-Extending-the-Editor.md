---
layout: page
title: Part 3 - Extending the Editor
teaser: Building a Release Pipeline
subheadline: Release Pipeline
date: "2016-07-11 8:00"
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
  thumb: 2016/07/pspipelinethumb.png
  homepage: 2016/07/vscode.png
header:
  image_fullwidth: 2016/06/part2banner.png
---

- Install Extensions - PowerShell
- launch.json - Sets up the Debugger
- Tasks.json - Task Runners
- Settings.json

## Visual Studio Code Setup


## Install PowerShell Extension
![Install PowerShell Extension](/images/2016/07/installpowershell.gif)

## Setup the Debugger
![Launch.json](/images/2016/07/launchjson.gif)

## Command Palette
The **Command Palette** is a box at the top of the Editor that you can use to execute Editor specific commands. It is a very convenient way to work with your environment. It also gives us a great way to start Tests, Builds, or other commands. The **Command Palette** can be launched by one of the following two keybindings:

* <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>p</kbd>
* <kbd>f1</kbd>


## Source Control
I won't go much into the Source Control inside of VSCode. It is a more simple way to interact with git that is convenient. It has options to work with git using the **Command Palette** as well as the Git Menu that can be accessed via <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>g</kbd> or using the GUI like in the gif below.

![Git Menu](/images/2016/07/git.gif)

## Task Runners
Command Palette > Tasks: Configure Task Runner
[Task documentation](https://code.visualstudio.com/docs/editor/tasks)

## Integrated Terminal
<kbd>ctrl</kbd> + <kbd>\`</kbd>
Multiple Terminals

## User Settings
Set integrated terminal to 64bit PowerShell [here](http://brandonpadgett.com/powershell/Set-Default-VSCode-PS-64Bit/)

```json
// Place your settings in this file to overwrite the default settings
{
    // Sets the logging verbosity level for the PowerShell Editor Services host executable.  Possible values are 'Verbose', 'Normal', 'Warning', and 'Error'
	"powershell.developer.editorServicesLogLevel": "Verbose",    

    // Sets the integrated terminal to 64Bit PowerShell
    "terminal.integrated.shell.windows": "C:\\WINDOWS\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe",
       
    // Sets ps1xml File Association to Xml.
    "files.associations": {
        "*.ps1xml": "xml"
    },

    // Enables Tabs
    "workbench.editor.showTabs": true    
}
```

## Keybindings

These keyboard shortcuts are stored in the Keybindings.json. This can be accessed via File>Preferences>Keyboard Shortcuts
- <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>t</kbd>: I also set a Test Task Runner that runs any Pester tests in the open project.
- <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>r</kbd>: This runs selected text in the Integrated terminal. f8 still runs selected in the Powershell Extension, which will eventually be merged.
- <kbd>alt</kbd> + <kbd>p</kbd>: You can set the Editor Commands menu as a Keybinding in Code. I like alt+p. Editor Commands will be covered in Part 4, but since we are covering Keybindings I went ahead and added that here.

```json
// Place your key bindings in this file to overwrite the defaults
[
    { "key": "ctrl+shift+t",          "command": "workbench.action.tasks.test" },
    { "key": "ctrl+shift+r",          "command": "workbench.action.terminal.runSelectedText" },
    { "key": "alt+p",                 "command": "PowerShell.ShowAdditionalCommands",
                                         "when": "editorLangId == 'powershell'" } 
]
```

Addional Terminal Keybindings
- **workbench.action.terminal.focus** - Focus the terminal. This is like toggle but focuses the terminal instead of hides it if its visible.
- **workbench.action.terminal.focusNext** - Focuses the next terminal instance.
- **workbench.action.terminal.focusPrevious** - Focuses the previous terminal instance.
- **workbench.action.terminal.kill** - Remove the current terminal instance.



[](http://brandonpadgett.com/powershell/Getting-Started-With-Editor-Commands/)


## What's Next?

Part 3 we will start discussing how to extend **VSCode** and use it within our pipeline. The command palette, task runners, extensions, integrated terminal, snippets, and editor commands are examples of some of the topics that will be covered. After we discuss those we will start getting into using community modules to start bringing everything together.

## Release Pipeline Series

- [Release Pipline Landing Page]({{ site.url }}/releasepipeline/Building-a-Release-Pipeline)
- [Part 1: Gitting Started]({{ site.url }}/releasepipeline/Part-1-Gitting-Started)
- [Part 2: Development Environment]({{ site.url }}/releasepipeline/Part-2-Development-Environment)
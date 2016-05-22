---
layout: page
title: Getting Started with Editor Commands
subheadline: PowerShell
date: "2016-05-22 11:07"
teaser: An Introduction to PowerShellEditorServices and it's new features.
categories:
  - powershell
tags:
  - powershell
  - VSCode
  - PowerShellEditorServices
  - EditorCommands
  - Editorcmds
image:
  thumb: 2016/05/thumb.gif
  homepage: 2016/05/banner.gif
header:
  image_fullwidth: 2016/05/banner.gif
---

# PowerShell Editor Services? Wha?

If you have not heard of PowerShell Editor Services (PSES), that's ok. You may be using it and not even realize it. PSES is an Open Source project that can be found on Github here [PowerShellEditorServices](https://github.com/PowerShell/PowerShellEditorServices) and is a service that provides a consistent PowerShell experience across multiple Editors. Currently it works for [Visual Studio Code](https://code.visualstudio.com/) in the [PowerShell Extension](https://github.com/PowerShell/vscode-powershell) and [Sublime Text](https://www.sublimetext.com/) is working on adding the functionality as well. Later this summer PowerShell ISE Preview will offer the ability to use PSES in a new experimental mode. The end goal is to have PSES used by default in ISE. PSES gives standard hooks that any editor can implement so that all editors can have an identical PowerShell editing experience.

Here are some of the features the Github Readme lists for PSES:

* The Language Service provides common editor features for the PowerShell language:
    * Code navigation actions (find references, go to definition)
    * Statement completions (IntelliSense)
    * Real-time semantic analysis of scripts using PowerShell Script Analyzer
    * Basic script evaluation
* The Debugging Service simplifies interaction with the PowerShell debugger (breakpoints, variables, call stack, etc)
* The Console Service provides a simplified interactive console interface which implements a rich PSHost implementation:
    * Interactive command execution support, including basic use of native console applications
    * Choice prompt support
    * Input prompt support
    * Get-Credential support (coming soon)
* The Extension Service provides a generalized extensibility model that allows you to write new functionality for any host editor that uses PowerShell Editor Services

## Requirements

There are a few things you will need to get started.

1. First you will need to have one of the latest builds of Visual Studio Code. This can be downloaded at the link provided above.<br><br>
2. Next you will need to have the PowerShell Extension installed. To install this open code and do the following:
    * Launch the **Command Palette** by one of the two keybindings:
        * <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>p</kbd>
        * <kbd>f1</kbd>
    * The **Command Palette** is a box at the top of the Editor that you can use to execute Editor specific commands.
    * In **Figure 1** you can see a picture of the **Command Palette**.
    * Now that you have the **Command Palette** open, type `ext install` then hit <kbd>Enter</kbd>. Then wait for the list to load and type `Powershell` and hit <kbd>Enter</kbd>.
    * After Code restarts the PowerShell Extension is installed.
    * You can see the installation process in **Figure 2**


![Command Palette](/images/2016/05/CommandPalette.png)

**Figure 1** Command Palette


![Install PowerShell Extension](/images/2016/05/PSExt.gif)

**Figure 2** Installing PowerShell Extension.


3. Setup the Debugger for PowerShell.
    * This can be done by hitting <kbd>f5</kbd> and Selecting *PowerShell*
    * This will create a directory named `.vscode` in the root of your project folder and place a launch.json config file with the PowerShell configuration. You can see how this is done below.


![Debugger](/images/2016/05/Debugger.gif)

**Figure 3** Setting up the Debugger


# New Features

The PSES provides an API that can be extended with PowerShell. There is a new `$psEditor` variable that can be accessed via scripts or modules to extend the editor. This is similar to the `$psISE` variable in PowerShell ISE. There was also a new profile added for VSCode similar to the **Microsoft.PowerShell_profile.ps1** and **Microsoft.PowerShellISE_profile.ps1** profiles for consoles and ISE. It is called **Microsoft.VSCode_profile.ps1** and goes in your `$env:USERPROFILE\Documents\WindowsPowerShell\` directory. The other big addition is Editor Commands, which are custom commands that can be registered to extend the editor.

You can find some additional information for these new features at the [PowerShellEditorServices Documentation](https://powershell.github.io/PowerShellEditorServices/guide/extensions) page.

## VS Code Profile

Users can now have a profile specific to VS Code. This is significant for some of the other new changes discussed later. The profile does not exist by default, so now is a good time to go ahead and create your profile and add anything to it that you might find beneficial. The profile now allows users to load modules and other scripts and functions automatically which allows PSES to provide IntelliSense for those commands. This makes writing scripts and debugging much easier. Prior to this change it was easiest to create a little helper script that loaded everything you needed and ran the scripts you were debugging.


## $psEditor Variable
This is one of my favorite additions to PSES and VS Code. The ability to extend functions, scripts and modules to extend into any editor that supports PSES has so much potential. Module creators can build support directly into their modules, and you won't even have to do anything special to get it working. All the module author has to do is add a little bit of code to their psm1.

{% highlight powershell %}
if ($psEditor) {
    Register-EditorCommand `
        -Name "MyModule.MyEditorCommand" `
        -DisplayName "My editor command" `
        -Function Invoke-MyEditorCommand `
        -SuppressOutput
}
{% endhighlight %}

All that needs to be done is check for the `$psEditor` variable and then launch the `Register-EditorCommand` command that registers their commands in the Editor. This allows you to add modules into your VSCode profile to have these modules with Editor commands to load by default. Users can also create their own editor commands and put those directly into their VSCode profiles.

`$psEditor` has limited functionality in this first release, but is being actively developed so expect features to be added pretty regularly. This variable gives you access to the details of the current workspace of the editor. You can access and alter cursor position, text selection, Open Files and other workspace related activities. I will have a few examples of this later.

## Editor commands

This is the feature that I am most excited about. You can create commands to alter parts of the editor or you can create commands that extend your own modules. I am going to show a few examples of how you can extend your own modules with these Editor Commands. One thing that I have done to make working with these editor commands easier, is I have added a KeyBinding in my Keybindings.json file that launches the ShowAdditionalCommands prompt. You can edit this file by **File=>Preferences=>Keyboard Shortcuts**. I used <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>p</kbd> because I knew it was available and I wanted to use "P" because it was easy for me to remember. You might not have to worry about this in a future release of the Extension. We are looking for feedback on how this should be handle and would love people to give their feedback on the [PowerShell Extensions Issue](https://github.com/PowerShell/vscode-powershell/issues/196) for it.

{% highlight json %}
[
    { "key": "ctrl+shift+alt+p",      "command": "PowerShell.ShowAdditionalCommands",
                                         "when": "editorTextFocus && editorLangId == 'powershell'" }
]
{% endhighlight %}

This is up to you, but just check and make sure the keybinding isn't used by another command. Now when I hit this keybinding my list of Editor Commands drops down for easy access. This KeyBinding is only available if the Editor text has focus and the editor language is set to PowerShell. You might potentially want to change this if using a module like the one in the next example and would be interested to have the altered text to work on any file type, but right now this doesn't work properly and there is an [issue open](https://github.com/PowerShell/vscode-powershell/issues/195) to improve this.


### Example 1

I chose a fun module by [Trevor Sullivan](https://twitter.com/pcgeek86) called [Emojis](https://twitter.com/pcgeek86). **Emojis** has a command called **Get-Emoji** that takes the name of an emoji and outputs the emoji. For this example, I am going to build a command that takes the text you have highlighted and converts it into the associated emoji.

I started off by adding the Module Import and initial function that will be my editor command to my VSCode profile.

{% highlight powershell %}
Import-Module Emojis

function Invoke-EmojiSelection
{
    [Cmdletbinding()]
    param
    (
        [Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context
    )
    $EmojiText = $context.CurrentFile.GetText($context.SelectedRange)
    $Emoji = Get-Emoji -Name $EmojiText
    if ($Emoji)
    {
        $context.CurrentFile.InsertText($Emoji, $context.SelectedRange)
    }
}

Register-EditorCommand `
    -Name "Emojis.Invoke-EmojiSelection" `
    -DisplayName "Replace selected text with emoji" `
    -Function Invoke-EmojiSelection `
    -SuppressOutput
{% endhighlight %}

In order for PSES to hook into an Editor command, it passes the function the variable `$context` so we first set this parameter.

In this function I am doing the following:

* The `$context` variable VSCode passes the Function used to get the currently selected text in the editor.
* Then that text is being Passed to `Get-Emoji` and storing the output as `$Emoji`.
* If it found a matching Emoji, it is then replaces the currently selected text for the Emoji.
* The `Register-EditorCommand` is then used to register the command when VSCode starts.

You can see this in action in **Figure 4**


![Convert Selection to Emoji](/images/2016/05/selection.gif)

**Figure 4** Convert Selection to Emoji.

### Example 2

In my second example I am adding a second Emoji command that works a little differently. Currently there is a little bit of an issue with timeout and selecting a command. This should be addressed and there is an [issue open](https://github.com/PowerShell/PowerShellEditorServices/issues/242) looking for feedback on how this should be handled if anyone wants to give their feedback. It could be a little longer for big lists. This is what I added for my second example.

{% highlight powershell %}
function Invoke-EmojiList
{
    [Cmdletbinding()]
    param
    (
        [Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context
    )
    $Names = (Get-Command -name Get-Emoji).Parameters['Name'].Attributes.ValidValues
    $caption = "Please select an Emoji"
    $message = "Inserts Emoji at Cursor Position"
    [int]$defaultChoice = 0
    $Choices = [System.Management.Automation.Host.ChoiceDescription[]] @($Names)

    $Name = $host.ui.PromptForChoice($caption,$message, $choices,$defaultChoice)
    $Emoji = Get-Emoji -name $Names[$Name]
    $context.CurrentFile.InsertText($Emoji, $context.CursorPosition)
}

Register-EditorCommand `
    -Name "Emojis.EmojiFromList" `
    -DisplayName "Inserts an Emoji from List" `
    -Function Invoke-EmojiList `
    -SuppressOutput
{% endhighlight %}

In this example I am doing the following:

* `Get-Command -Name Get-Emoji` is used to select all of the Parameter Values for the Emoji Names.
* `$host.ui.PromptForChoice` is used to create a prompt to give the user a list of emojis.
* When the user selects an item from the list it is passed to the `Get-Emoji` command.
* The Emoji is then inserted where the current Cursor position is using the `$context` variable.

Here is an example of it in **Figure 5**


![Insert Emoji from List](/images/2016/05/list.gif)

**Figure 5** Insert Emoji from List.

### Example 3

The next example is using a new Project called [Plaster](https://github.com/PowerShell/Plaster). This project is very early and is not an official release, but it is a perfect example of a great use of Editor Commands. Here is a quote from the project just to make sure this warning is mentioned here.

> NOTE: This project is at a very early phase of development. We have not officially launched this project yet but we're opening up the repo for early feedback from the PowerShell community. Please try it out and let us know what you think!

Plaster is a template generator similar to Yeoman. You can create a template scaffolding of a project like a Module, Pester Test, DSC Resource, function or whatever project meets your needs, and it will walk you through the details needed to build that project. In this example I am creating a module from a template and Plaster walks me through the ModuleName, Version, Author's Name, License Type, Pester/Psake/Git Support, and Editor of Choice. It then goes through all of those different files and folders and sets all of those variables throughout the project. I wont go much more into Plaster, as it deserves its own blog post which I am currently planning/working on.

The following is the function and editor command.

{% highlight powershell %}
Import-Module Plaster

function Invoke-PlasterTemplate
{
    [CmdletBinding()]
    param
    (   
        [Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context,

        [Parameter(Mandatory=$true)]                
        [string]$Name = (Read-Host 'Please type Toolkit Name')
    )

    Process
    {  
        $TemplatePath = "C:\Test\Templates\ModuleTemplate.zip"
        $Destination = "C:\Test\$Name"

        Invoke-Plaster -TemplatePath $TemplatePath -DestinationPath $Destination -ModuleName $Name        
    }
}

Register-EditorCommand `
    -Name "Plaster.PlasterTemplate" `
    -DisplayName "Start Plaster Template" `
    -Function Invoke-PlasterTemplate `
    -SuppressOutput
{% endhighlight %}

This example uses a second parameter that uses Read-Host to get the Modules Name. It then just passes some Path variables to the `Invoke-Plaster` command and let's the Template handle the rest of the prompts. You can see the results below.


![Plaster Editor Command](/images/2016/05/Plaster.gif)

**Figure 6** Plaster Editor Command.

## Contribute!

This project is new and needs people to give feedback. Don't be afraid to submit issues, enhancements, ideas, suggestions, examples, code or anything like that to the Github Issues pages for the [Visual Studio Code PowerShell Extension](https://github.com/PowerShell/vscode-powershell) or the [PowerShellEditorServives](https://github.com/PowerShell/PowerShellEditorServices) Page. I have had nothing but great experiences with the community and want to encourage even those who have never contributed to an Open Source project to give feedback. These projects or even the [Plaster](https://github.com/PowerShell/Plaster) one in my Example would be an awesome place to start because you can be sure that you will have a great experience. If you need help learning Git or Github and how to submit feedback or issues, the people in the community would be happy to help and walk you through it. The great thing about these Editor Commands, is they are written in pure PowerShell, so if you know PowerShell, you can write your own Editor Extensions.

I have added the example profile I used in this post to a gist for anyone interested in playing around with it.

{% gist gerane/c87a640337a9739da746a18e1b62682f %}


### Related Posts
{: .t60 }

{% include list-posts.html tag='powershell' %}

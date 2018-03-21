---
layout: page
title: Using VSCode to Give Better Presentations
teaser: Take advantage of VSCode features and ditch ISE
subheadline: VSCode PowerShell
date: "2018-03-21 8:00"
categories:
  - VSCode
tags:
  - powershell
  - VSCode
  - EditorCommands
  - Presentations
  - ISE
image:
  thumb: 2018/3/thumbnail.png
  homepage: 2018/3/Demo.gif
# header:
#   image_fullwidth: 2018/3/Demo.gif
---


Many of us in the community want people to migrate away from PowerShell ISE and start using VSCode as their primary editor. There is one problem, many still use ISE for their Demos and Preentations. We can get a similar workflow of the usual **F8** demos, but using VSCode Editor Commands.

I think back to when I was new to PowerShell. I remember seeing a presentation by [Dave Wyatt](https://twitter.com/MSH_Dave), and he was using [ISESteroids](http://www.powertheshell.com/isesteroids/). It blew me away and I had to have it. I immediately went out and got ISESteroids. Seeing someone like Dave use some of it's advanced features had a big impact on me. We need to set this example with VSCode if we want to spread adoption.

![Demo]({{ site.urlimg }}/2018/3/Demo.gif)

## VSCode Unique Features

I say these features are unique to VSCode, but that is not entirely true. What is running under the hood is a project called [PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices). I have been a huge fan of this project since it first released, and I believe in what it is accomplishing. It is a language service that can be implemented by any editor to bring ISE like features. This means any editor can bring rich PowerShell support that will be the same across any platform or editor. When you look at it from this perspective, it makes since they Microsoft moved focus away from ISE to VSCode + PowerShell Editor Services.

One of my favorite features of the PowerShell Extension is **Editor Commands**. If I had seen **Editor Commands** when watching a Demo, I would have dropped what I was doing and gone and downloaded VSCode to try them out, similar to how I reacted to ISESteroids. This has to be one of the most useful features that is being underutilized in the [VSCode PowerShell Extension](https://github.com/PowerShell/vscode-powershell). When coupled with projects like [Plaster Templates](https://github.com/PowerShell/Plaster) or pipeline tasks, it has become one of my most used tools that I am constantly using throughout the day.

![Plaster Example]({{ site.urlimg }}/2018/3/Plaster.gif)

Editor Commands also can be embedded in modules. You can add a simple piece of code to check for the $PSEditor variable and then dot source your Editor Commands if they exist. You could also put these in your VSCode PowerShell Profile, which is named **Microsoft.VSCode_profile.ps1** and goes in the same directory as your other Profiles. This also could be used to make Demos and Presentations distributable via PSGallery.

### Using Editor Commands

One limitation of VSCode, is you can't have Editor Commands show up in the top level of the [Command Palette](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette). They have to be accessed from the PowerShell Extensions Command **Show Additional Commands from PowerShell Modules**. I set the Keyboard Shortcut **Show Additional Commands from PowerShell Modules** to **Alt+P**. This actually works better for me, because I can access the commands quicker without having to do as much filtering. You can see the Built in PowerShell Commands by typing PowerShell in the Command Palette (**ctrl+shift+p**), as seen in the image below.

![PowerShell Commands]({{ site.urlimg }}/2018/3/PowerShellCommands.png)

To set the keybinding in VSCode, go to *File > Preferences > Keyboard Shortcuts* and the Keybinding can be seen in the Screenshot below.

![PowerShell Commands]({{ site.urlimg }}/2018/3/keybinding.png)

### Register-EditorCommand

**Register-EditorCommand** is the function that Loads our commands into the PowerShell Editor Services. You will notice that this command has a parameter called **Function**, and you can provide it a function to execute, but it may not work as you would expect. The PowerShell Editor Services passes the function a context variable, so your function would need to be built with this in mind. I have an issue open on this if anyone wants more details, [Different method of passing $Context to Editor Commands](https://github.com/PowerShell/PowerShellEditorServices/issues/249).

This is what PowerShell Editor Services is doing under the hood for a function Editor Command:

```powershell
Register-EditorCommand `
    -Name 'ModuleName.CommandName' `
    -DisplayName 'This is the main text shown to the user' `
    -SuppressOutput `
    -Function Test-YourFunction


Invoke-Command -ScriptBlock param($context) Test-YourFunction $context -ArgumentList System.Object[]
```

The **Function** Parameter used to make a lot more sense. Editor Commands used to work a lot differently. If you called a function and you missed a mandatory parameter, it would send the prompt to the command palette for you to type in. This meant you could point an editor command at a function, and it would just automatically handle the prompts for you. This functionality was broken when the integrated console was implemented. Now you have to create your own prompts using some commands from Patrick's [EditorServicesCommandSuite](https://github.com/SeeminglyScience/EditorServicesCommandSuite). The problem is his helper functions are private, so you will need to extract them to use them in your own Editor Commands. I will have an example of how to use these in this demo, but here are the helper commands that allow you to create prompts.

```powershell
using namespace Microsoft.PowerShell.EditorServices.Protocol.MessageProtocol
using namespace Microsoft.PowerShell.EditorServices.Protocol.Messages
using namespace Microsoft.PowerShell.EditorServices


function ReadInputPrompt
{
    param([string]$Prompt)
    end
    {
        $result = $psEditor.Components.Get([IMessageSender]).SendRequest([ShowInputPromptRequest]::Type,[ShowInputPromptRequest]@{
            Name  = $Prompt
            Label = $Prompt
        },$true).Result

        if (-not $result.PromptCanceled)
        {
            $result.ResponseText
        }
    }
}
function ReadChoicePrompt
{
    param([string]$Prompt, [System.Management.Automation.Host.ChoiceDescription[]]$Choices)
    end
    {
        $choiceIndex = 0
        $convertedChoices = $Choices.ForEach{
            $newLabel = '{0} - {1}' -f ($choiceIndex + 1), $_.Label
            [ChoiceDetails]::new($newLabel, $_.HelpMessage)
            $choiceIndex++
        } -as [ChoiceDetails[]]

        $result = $psEditor.Components.Get([IMessageSender]).SendRequest([ShowChoicePromptRequest]::Type,[ShowChoicePromptRequest]@{
                Caption        = $Prompt
                Message        = $Prompt
                Choices        = $convertedChoices
                DefaultChoices = 0
            },$true).Result

        if (-not $result.PromptCanceled)
        {
            $result.ResponseText | Select-String '^(\d+) - ' | ForEach-Object { $_.Matches.Groups[1].Value - 1 }
        }
    }
}
```

You can still create functions that account for the **Context** variable, and have Editor Commands prompt you for missing Mandatory parameters, but they are sent to the terminal instead of the Command Palette. What I typically use is just the **ScriptBlock** parameter to create my code. This lets me have a wrapper around whatever function or code I am running so I can pass parameters or create prompts to pass parameter values. Below is a basic example of using **ScriptBlock**.


```powershell
Register-EditorCommand `
    -Name 'ModuleName.CommandName' `
    -DisplayName 'This is the main text shown to the user' `
    -SuppressOutput `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

       # Your code here
    }
```

The final parameter is **SuppressOutput**, which does what you would expect, suppresses any output from displaying in the terminal. The other thing to note about the **Name** and **DisplayName** parameters, these impact the sorting. You can see in this example, that my Demo commands are not sorted the way you would expect. The order is not based on the **DisplayName**, but the **Name**. The reasoning behind this is to allow commands to be named in the format **Module.Command** so that commands that are imported from a module stay sorted together.

![Command Order]({{ site.urlimg }}/2018/3/order.png)


## Example Module

I have created an example of how VSCode can be used for a Demo. I have this Example in my Github Repo [VSCodePresentations](https://github.com/gerane/VSCodePresentations/). I will go over how this example works.

### Demo 1

This example opens a PowerPoint in your Demo Folder. We often have a PowerPoint Presentation in our Demos, so why not open it with style.

```powershell
Register-EditorCommand `
    -Name 'Demo.OpenPowerPoint' `
    -DisplayName 'Demo 1: Open PowerPoint' `
    -SuppressOutput `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        $Pptx = Get-ChildItem -Path $PSScriptRoot\..\*.pptx
        Invoke-Item -Path $Pptx
    }
```

![PowerPoint]({{ site.urlimg }}/2018/3/PowerPoint.gif)

### Demo 2

This is a simple example of opening a file in the Editor. I open the example psm1 module file to show an example of adding support to your modules. You can see I am using **Open-EditorFile**, which is a command from the PowerShell VSCode Extension.

```powershell
Register-EditorCommand `
    -Name 'Demo.OpenPsm1' `
    -DisplayName 'Demo 2: Open Demo Module Psm1' `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        Open-EditorFile "$PSScriptRoot\..\*.psm1"
    }
```

### Demo 3

This Demo opens the **Helpers.ps1** file to show the helper commands for taking input or prompting for choice. When [David Wilson](https://twitter.com/daviwil) released the integrated console support, it broke Editor Commands ability to tie into the Command Palette. Luckily, [Patrick Meinecke](https://twitter.com/SeeminglyScienc) came up with a work around a while back and got it working (You are awesome man!).

```powershell
Register-EditorCommand `
    -Name 'Demo.OpenHelpers' `
    -DisplayName 'Demo 3: Open Helpers Script' `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        Open-EditorFile "$PSScriptRoot\Helpers.ps1"
    }
```

### Demo 4

This is just a quick example of using the **Open-EditorFile** to open a file in the Editor.

```powershell
Register-EditorCommand `
    -Name 'Demo.OpenPowerPointScript' `
    -DisplayName 'Demo 4: Open PowerPoint Script' `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        Open-EditorFile "$PSScriptRoot\Demo1.OpenPowerPoint.ps1"
    }
```

### Demo 5

This shows how you can use the Helper Commands to prompt for choice. It gives you a selection of your PowerShell Profiles and will open the one you select using the **$PSEditor.Workspace.OpenFile()** Method.

```powershell
Register-EditorCommand `
    -Name 'Demo.OpenProfile' `
    -DisplayName 'Demo 5: Open Profile' `
    -SuppressOutput `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        $Current = Split-Path -Path $profile -Leaf
        $List = @($Current, 'Microsoft.VSCode_profile.ps1', 'Microsoft.PowerShell_profile.ps1', 'Microsoft.PowerShellISE_profile.ps1', 'Profile.ps1') | Select-Object -Unique
        $Choices = [System.Management.Automation.Host.ChoiceDescription[]] @($List)
        $Selection = ReadChoicePrompt -Prompt "Pl`ease Select a Profile" -Choices $Choices
        $Name = $List[$Selection]

        $ProfileDir = Split-Path $Profile -Parent
        $ProfileName = Join-Path -Path $ProfileDir -ChildPath $Name

        If (!(Test-Path -Path $ProfileName)) { New-Item -Path $ProfileName -ItemType File }

        $psEditor.Workspace.OpenFile($ProfileName)
    }
```

### Demo 6

This is another simple example showing how you can run commands and have the output go to the terminal. Notice how **SuppressOutput** is not present.

```powershell
Register-EditorCommand `
    -Name 'Demo.GetProcessOutput' `
    -DisplayName 'Demo 6: Get-Process Output to Console' `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        Get-Process Code*
    }
```

## Making the Process Better

It may take just a little more work to get a Demo up a working this way, but it actually isn't as much as you would think. Just take the bits of code you would normally hit **F8** on, and place this in wrappers. These Editor Commands give us more ways to be creative, and it just looks better. I have created to template Editor Commands to help quickly create Editor Commands for normal use or even for Demos. This should make the process even easier. Here are the Commands:

### EditorCommand.NewEditorCommandToClipboard

This command does the following:

* Prompts for **Name**
* Prompts for **DisplayName**
* Prompts for **SupressOutput** (Yes or No) Choice
* Copies Editor Command text to the Clipboard

```powershell
Register-EditorCommand `
    -Name 'EditorCommand.NewEditorCommandToClipboard' `
    -DisplayName 'Create Editor Command and copy to Clipboard' `
    -SuppressOutput `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        $Name = ReadInputPrompt 'Please Type the Name of the Editor Command. Ex: Module.Command'
        $DisplayName = ReadInputPrompt 'Please Type the DisplayName of the Editor Command'

        $List = @('Yes', 'No')
        $Choices = [System.Management.Automation.Host.ChoiceDescription[]] @($List)
        $Selection = ReadChoicePrompt -Prompt "Do you want to Suppress Output?" -Choices $Choices
        $SuppressOutput = $List[$Selection]

        if ($SuppressOutput -eq 'Yes')
        {
            $Command = @"
Register-EditorCommand ``
    -Name "$Name" ``
    -DisplayName "$DisplayName" ``
    -SuppressOutput ``
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]`$context)
        $Selection
        $SuppressOutput
    }
"@
        }
        else
        {
            $Command = @"
Register-EditorCommand ``
    -Name "$Name" ``
    -DisplayName "$DisplayName" ``
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]`$context)
        $Selection
        $SuppressOutput
    }
"@
        }

        $Command | Set-Clipboard
    }
```

### EditorCommand.NewEditorCommandToFile

This command does the following:

* Prompts for **Name**
* Prompts for **DisplayName**
* Prompts for **SupressOutput** (Yes or No) Choice
* Creates a file name **Name.ps1** in the current Project under the **EditorCommands** directory, which can be easily altered.

```powershell
Register-EditorCommand `
    -Name 'EditorCommand.NewEditorCommandToFile' `
    -DisplayName 'New Editor Command File' `
    -SuppressOutput `
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]$context)

        $Name = ReadInputPrompt 'Please Type the Name of the Editor Command. Ex: Module.Command'
        $DisplayName = ReadInputPrompt 'Please Type the DisplayName of the Editor Command'

        $List = @('Yes', 'No')
        $Choices = [System.Management.Automation.Host.ChoiceDescription[]] @($List)
        $Selection = ReadChoicePrompt -Prompt "Do you want to Suppress Output?" -Choices $Choices
        $SuppressOutput = $List[$Selection]

        if ($SuppressOutput -eq 'Yes')
        {
            $Command = @"
Register-EditorCommand ``
    -Name "$Name" ``
    -DisplayName "$DisplayName" ``
    -SuppressOutput ``
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]`$context)

        # Enter Code Here
    }
"@
        }
        else
        {
            $Command = @"
Register-EditorCommand ``
    -Name "$Name" ``
    -DisplayName "$DisplayName" ``
    -ScriptBlock {
        param([Microsoft.PowerShell.EditorServices.Extensions.EditorContext]`$context)

        # Enter Code Here
    }
"@
        }

        $Command | Out-File -FilePath "$($psEditor.Workspace.path)\EditorCommands\$($Name).ps1"
    }
```

I have added these example Editor Command to the Demo Module I linked to earlier. These commands also serve as more examples of how you can easily integrate into VSCode.

I have also been throwing around the idea of making a Plaster Template for VSCode Presentations. There are several ways you could do this, and I would need to play around with it some. I would love some feedback on this, or maybe this could be something the community could work on together.


### Related Posts
{: .t60 }

{% include list-posts.html tag='VSCode' %}
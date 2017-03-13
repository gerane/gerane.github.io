---
layout: page
title: Using VSCode Background Tasks to Automatically Run Pester Tests
teaser: Background Tasks that trigger Pester Tests when a file is modified
subheadline: VSCode Tips
date: "2017-03-08 8:00"
categories:
  - VSCode
tags:
  - powershell
  - VSCode
  - Tasks
  - Testing
  - Pester
image:
  thumb: 2017/3/ProblemThumb.png
  homepage: 2017/3/WatchHomepage.gif
header:
  image_fullwidth: 2017/3/ProblemBanner.png
---


While working on a couple future posts for my [Release Pipeline](2016-06-30-Building-a-Release-Pipeline.md) Series, I stumbled onto a very interesting feature in the VSCode Task system. These tasks are called **Watcher Tasks** or **Background Tasks**. The [documentation](https://code.visualstudio.com/docs/editor/tasks#_background-watching-tasks) has an example of a TypeScript watcher task that uses the `tsc --watch` command to auto compile the project when a modified file is detected.

These Tasks can also implement Problem Matchers like a normal task. Problem Matchers use regex patterns that find error, warnings, and other information in the Task Output. The output that matches these patterns is then added as entries in the Problems Pane. Below you can see a picture of the Problems Pane with a few Problems the *PowerShellEditorServices* found.

![Problems Pane]({{ site.urlimg }}/2017/3/ProblemMatcherLocation.png)

The Problems Pane can be found as a tab in the Integrated Terminal, and can be opened with the <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>m</kbd> Keybinding. Problem Matcher regex patterns can be used to find the file, line number, and column information that allows the user to jump directly to them by clicking on the Matched Problem. Below you can see an example of how this works.

![Problem Matcher]({{ site.urlimg }}/2017/3/TestProblemMatcher.gif)

PowerShell can also monitor for file system changes using the [FileSystemWatcher Class](https://msdn.microsoft.com/en-us/library/system.io.filesystemwatcher%28v=vs.110%29.aspx?f=255&MSPPError=-2147217396), so I had to try this out. I have used **FileSystemWatcher** in a few projects in the past, but they were tied into those projects pretty tight. I decided to use Steven Murawski's [PowerShellGuard](https://www.powershellgallery.com/packages/PowerShellGuard/0.6.1) Module since it was already in a Module format and easier to quickly integrate into a task. **PowerShellGuard** watches for modified files and executes configured commands when something is modified. I would like to look around to see if there are any other projects out there like this one, so if anyone knows of any let me know.

## Using Tasks

You have several options to start Tasks in VSCode. I will quickly cover a few of these options.

### Tasks via Task Commands

You can find commands related to Tasks by typing **Tasks** into the **Command Palette**. You can access the **Command Palette** using the following KeyBindings:

- <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>p</kbd>
- <kbd>F1</kbd>

![TasksKeyBinding]({{ site.urlimg }}/2017/3/TasksKeyBinding.gif)

### "Tasks: Run Tasks" Command

Tasks can also be accessed from the **Tasks: Run Tasks** Command. This can be accessed from the **Quick Open** window using <kbd>ctrl</kbd> + <kbd>p</kbd> and then typing **task** followed by <kbd>Space</kbd> which gives you a list of all of your tasks. The **Tasks: Run Tasks** Command can also be accessed directly with the following KeyBinding:

<kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>t</kbd>

![TaskQuickOpen]({{ site.urlimg }}/2017/3/TaskQuickOpen.gif)

### KeyBindings for Default Task Properties

You can specify a Task as your default **Build** or **Test** Task. This is done by adding one of the following properties to a Task, `"isBuildCommand": true` or `"isTestCommand": true`. This ties the task to the **Tasks: Run Build Task** or **Tasks: Run Test Task** Commands. You can see an example of what this looks like in the **tasks.json** in the **Watch.Project** Task I will be covering shortly. The **Tasks: Run Build Task** has the following KeyBinding:

<kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>B</kbd>

The **Tasks: Run Test Task** does not have an assigned KeyBinding by default, but it can be set in the **keybindings.json**. I like to assign it to the following KeyBinding:

<kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>t</kbd>

To do this, add the following to the **keybindings.json**:

```json
{ "key": "ctrl+shift+t",          "command": "workbench.action.tasks.test" }
```

## Configuring Tasks

The Schema for the **tasks.json** can be found in the [Tasks Appendix](https://code.visualstudio.com/docs/editor/tasks_appendix) and the Task Documentation can be found [here](https://code.visualstudio.com/docs/editor/tasks). Background tasks are similar to normal tasks, but have a few additional properties that need to be configured. You can find an example Module and **tasks.json** in this Repository: [VSCodeWatcherTask](https://github.com/gerane/VSCodeWatcherTask). I have included 4 example tasks, which each have slightly different use cases depending on your testing style or project size. I will go over each task to help explain how to set these up and how they work.

### **Watch.Project**

![WatchProject]({{ site.urlimg }}/2017/3/WatchProject.gif)

The **Watch.Project** task watches for any ps1 changes in the project and then runs all Tests in the **Tests** folder when something is modified. It isn't dependant on any folder naming as it is just monitoring the **WorkspaceRoot** recursively any then runs anything in the **WorkspaceRoot\Tests**. You may need to modify the Tests path if yours is different or you could also narrow down the project directory if needed. It has had `"isTestCommand": true` set so it is a default task that can be ran using the **Tasks: Run Test Task** command. The PowerShell triggered by **PowerShellGuard** included a `Write-Host "Invoking Watch.Project"` to let the task know when the tests have started running, and then it looks for the Pester Summary `Passed: X Failed: X Skipped: X Pending: X Inconclusive: X` so it knows when it has finsihed. When the Tests are triggered, all Problems previously found with the property `"owner": "Watch.Project"` will be cleared out and repopulated. That way if any problems were resolved they will be removed.

In smaller projects, I have enjoyed using the **Watch.Project** task. Since it reruns all of the tests for every modified file, it helps ensure I don't overlook a breaking change. However, if Tests take a long time to run, you may need to look into running single tests.

It also should be noted, a while ago Pester added a new option called **IncludeVSCodeMarker** to add better support for the PowerShell Extension in VSCode. However, Pester v4.0.2 actually broke this functionality. You can track the issue [here](https://github.com/pester/Pester/issues/725). A fix has been merged and should make it into the v4.0.3 release. Until then I forced using v3.4.3 for these Tasks.

```json
{
    "taskName": "Watch.Project",
    "isTestCommand": true,
    "suppressTaskName": true,
    "args": [
        "Write-Host 'Watching Project';",
        "Import-Module -Name Pester -RequiredVersion 3.4.3 -Force;",
        "Import-Module -Name PowerShellGuard;",
        "New-Guard -Path \"${workspaceRoot}\" -PathFilter \"*.ps1\" -MonitorSubdirectories -TestPath \"${workspaceRoot}\\Tests\" -TestCommand {Write-Host \"Invoking Watch.Project\"; Invoke-Pester -PesterOption @{IncludeVSCodeMarker=$true}} -Wait;",
    ],
    "isBackground": true,
    "problemMatcher": [
        {
            "owner": "Watch.Project",
            "fileLocation": "absolute",
            "pattern": [
                {
                    "regexp": "^\\s*(\\[-\\]\\s*.*?)(\\d+)ms\\s*$",
                    "message": 1
                },
                {
                    "regexp": "^\\s+at\\s+[^,]+,\\s*(.*?):\\s+line\\s+(\\d+)$",
                    "file": 1,
                    "line": 2
                }
            ],
            "watching": {
                "activeOnStart": true,
                "beginsPattern": "^Invoking Watch\\.Project$",
                "endsPattern": "^Passed:\\s(\\d+)\\sFailed:\\s(\\d+)\\sSkipped:\\s(\\d+)\\sPending:\\s(\\d+)\\sInconclusive:\\s(\\d+)\\s$"
            }
        }
    ]
}
```

### **Watch.Tests**

![WatchTests]({{ site.urlimg }}/2017/3/WatchTests.gif)

The **Watch.Tests** task watches all tests in the **WorkspaceRoot\Tests** and the runs all tests in this directory if a test is modified. Similar to **Watch.Project**, you may need to modify the TestPath if you keep you tests in a different location. The `New-Guard` command in this Task leaves off the `-Wait` parameter and instead uses the `Wait-Guard` command. This is needed because we are running several `New-Guard` commands for each Test in the **Tests** directory. This allows us to set up all of are Guards before we start waiting for changes. This Task also uses a similar `Write-Host "Invoking Watch.Tests"` and `Passed: X Failed: X Skipped: X Pending: X Inconclusive: X` to specify when the Tests begin and end.

```json
{
    "taskName": "Watch.Tests",
    "suppressTaskName": true,
    "args": [
        "Write-Host 'Watching Tests';",
        "Import-Module -Name Pester -RequiredVersion 3.4.3 -Force;",
        "Import-Module PowerShellGuard;",
        "(gci \"${workspaceRoot}\\Tests\\*.ps1\").Foreach{ New-Guard -Path $_.FullName -TestPath $_.FullName -TestCommand {Write-Host \"Invoking Watch.Tests\"; Invoke-Pester -PesterOption @{IncludeVSCodeMarker=$true}} -ErrorAction SilentlyContinue};",
        "Wait-Guard"
    ],
    "isBackground": true,
    "problemMatcher": [
        {
            "owner": "Watch.Tests",
            "fileLocation": "relative",
            "pattern": [
                {
                    "regexp": "^\\s*(\\[-\\]\\s*.*?)(\\d+)ms\\s*$",
                    "message": 1
                },
                {
                    "regexp": "^\\s+at\\s+[^,]+,\\s*(.*?):\\s+line\\s+(\\d+)$",
                    "file": 1,
                    "line": 2
                }
            ],
            "watching": {
                "activeOnStart": true,
                "beginsPattern": "^Invoking Watch\\.Tests$",
                "endsPattern": "^Passed:\\s\\d+\\sFailed:\\s\\d+\\sSkipped:\\s\\d+\\sPending:\\s\\d+\\sInconclusive:\\s\\d+\\s$"
            }
        }
    ]
}
```


### **Watch.Project.Single**

![WatchProjectSingle]({{ site.urlimg }}/2017/3/WatchProjectSingle.gif)

The **Watch.Project.Single** Task might require a little alteration depending on your Project layout. It watched all ps1 files in **WorkspaceRoot\WorkspaceRootFolderName** and **WorkspaceRoot\Tests**. This follows the structure outlined in this article: [Building a PowerShell Module](https://ramblingcookiemonster.github.io/Building-A-PowerShell-Module/). In the example Module, it watches **VSCodeWatcherTask\VSCodeWatcherTask** and **VSCodeWatcherTask\Tests** directories. This one also uses the `Wait-Guard` command and the same type Patterns to match the begin and end of the tests running.

There is currently a downside to running the **Watch.Project.Single** and **Watch.Tests.Single** Tasks. Since all matched problems are attached to the same `owner` property, when a single Test runs it still clears out all Problems associated to that `owner`. This may not be a problem if you are only working on one function or test at a time. You can see this behavior in the example gif above. I have filed a [feature request](https://github.com/Microsoft/vscode/issues/22242) to improve this behavior. My suggestion is to let you dynamically set the `owner` property using the pattern matcher.

```json
{
    "taskName": "Watch.Project.Single",
    "suppressTaskName": true,
    "args": [
        "Write-Host 'Watching Single Project Files';",
        "Import-Module -Name Pester -RequiredVersion 3.4.3 -Force;",
        "Import-Module -Name PowerShellGuard;",
        "(gci \"${workspaceRoot}\\${workspaceRootFolderName}\\*.ps1\" -recurse).Foreach{ New-Guard -Path $_.FullName -TestPath \"${workspaceRoot}\\Tests\\$($_.basename).Tests.ps1\" -TestCommand {Write-Host \"Invoking Watch.Project.Single\"; Invoke-Pester -PesterOption @{IncludeVSCodeMarker=$true}} -ErrorAction SilentlyContinue};",
        "(gci \"${workspaceRoot}\\Tests\\*.ps1\").Foreach{ New-Guard -Path $_.FullName -TestPath $_.FullName -TestCommand {Write-Host \"Invoking Watch.Project.Single\"; Invoke-Pester -PesterOption @{IncludeVSCodeMarker=$true}} -ErrorAction SilentlyContinue};",
        "Wait-Guard"
    ],
    "isBackground": true,
    "problemMatcher": [
        {
            "owner": "Watch.Project.Single",
            "fileLocation": "absolute",
            "pattern": [
                {
                    "regexp": "^\\s*(\\[-\\]\\s*.*?)(\\d+)ms\\s*$",
                    "message": 1
                },
                {
                    "regexp": "^\\s+at\\s+[^,]+,\\s*(.*?):\\s+line\\s+(\\d+)$",
                    "file": 1,
                    "line": 2
                }
            ],
            "watching": {
                "activeOnStart": true,
                "beginsPattern": "^Invoking Watch\\.Project\\.Single$",
                "endsPattern": "^Passed:\\s\\d+\\sFailed:\\s\\d+\\sSkipped:\\s\\d+\\sPending:\\s\\d+\\sInconclusive:\\s\\d+\\s$"
            }
        }
    ]
}
```

### **Watch.Tests.Single**

![WatchTestsSingle]({{ site.urlimg }}/2017/3/WatchTestsSingle.gif)

The **Watch.Tests.Single** task is similar to the **Watch.Project.Single** Task, but only watches the Test files in the **WorkspaceRoot\Tests** directory. It suffers from the same problem matcher issue as the **Watch.Project.Single** Task.

```json
{
    "taskName": "Watch.Tests.Single",
    "suppressTaskName": true,
    "args": [
        "Write-Host 'Watching Tests';",
        "Import-Module -Name Pester -RequiredVersion 3.4.3 -Force;",
        "Import-Module PowerShellGuard;",
        "(gci \"${workspaceRoot}\\Tests\\*.ps1\").Foreach{ New-Guard -Path $_.FullName -TestPath $_.FullName -TestCommand {Write-Host \"Invoking Watch.Tests.Single\"; Invoke-Pester -PesterOption @{IncludeVSCodeMarker=$true}} -ErrorAction SilentlyContinue};",
        "Wait-Guard"
    ],
    "isBackground": true,
    "problemMatcher": [
        {
            "owner": "Watch.Tests.Single",
            "fileLocation": "relative",
            "pattern": [
                {
                    "regexp": "^\\s*(\\[-\\]\\s*.*?)(\\d+)ms\\s*$",
                    "message": 1
                },
                {
                    "regexp": "^\\s+at\\s+[^,]+,\\s*(.*?):\\s+line\\s+(\\d+)$",
                    "file": 1,
                    "line": 2
                }
            ],
            "watching": {
                "activeOnStart": true,
                "beginsPattern": "^Invoking Watch\\.Tests\\.Single$",
                "endsPattern": "^Passed:\\s\\d+\\sFailed:\\s\\d+\\sSkipped:\\s\\d+\\sPending:\\s\\d+\\sInconclusive:\\s\\d+\\s$"
            }
        }
    ]
}
```

## Bonus: Jekyll Background Task Example

I use [Github Pages](https://pages.github.com/) and [Jekyll](https://jekyllrb.com/), and when I am writing my blog posts in markdown in VSCode, I normally have a terminal open that is running a local instance of *Jekyll* for Testing. I use the command `jekyll serve --config _config.yml,_config_dev.yml`, which lets me preview my blog via localhost as I am working on it. I just save a file, wait a few seconds, and then reload my web browser and the changes are live. I have this set as my **Build Task** so I can just use <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>B</kbd> and *Jekyll* builds my blog locally and starts monitoring the file system in the background.

This is just a rough example, and I know the Problem Matchers need a lot of work. I may just remove the Problem Matchers since any errors I would likely encounter are normally visible when looking at the site. I kept them intact to show another example of using different `fileLocation` property values. The *Jekyll* errors can have both **relative** and **absolute** paths, so I had to make patterns to match both. Below you can see the json for this task.

```json
{
	"version": "0.1.0",
	"command": "jekyll",
	"isShellCommand": true,
	"showOutput": "always",
    "args": [
        "serve", "--config","_config.yml,_config_dev.yml","--verbose"
    ],
    "tasks": [
        {
            "taskName": "Build",
            "isBuildCommand": true,
            "suppressTaskName": true,
            "args": [],
            "isBackground": true,
            "problemMatcher": [
                {
                    "owner": "Jekyll",
                    "fileLocation": "absolute",
                    "pattern": [
                        {
                            "regexp": "^\\s*Liquid\\s(Warning|Error):.*\\(line\\s(\\d+)\\):\\s(.*)\\sin\\s(\\w:.*)$",
                            "severity": 1,
                            "line": 2,
                            "message": 3,
                            "file": 4
                        }
                    ],
                    "watching": {
                        "activeOnStart": true,
                        "beginsPattern": "^\\s+Generating\\.\\.\\.|\\s*Regenerating:.*$",
                        "endsPattern": "^.*\\.\\.\\.done.*$"
                    }
                },
                {

                    "owner": "Jekyll",
                    "fileLocation": "relative",
                    "pattern": [
                        {
                            "regexp": "^\\s*Liquid\\s(Warning|Error):.*\\(line\\s(\\d+)\\):\\s(.*)\\sin\\s(\\w+\/.*|\/_\\w+\/.*|\\w+\\.\\w+)$",
                            "severity": 1,
                            "line": 2,
                            "message": 3,
                            "file": 4
                        }
                    ],
                    "watching": {
                        "activeOnStart": true,
                        "beginsPattern": "^\\s+Generating\\.\\.\\.|\\s*Regenerating:.*$",
                        "endsPattern": "^.*\\.\\.\\.done.*$"
                    }
                }
            ]
        }
    ]
}
```

I have added my *Jekyll* **tasks.json** to this [VSCodeJekyllWatcherTask](https://github.com/gerane/VSCodeJekyllWatcherTask) Repository for those interested.

## Conclusion

I wanted to include the *Jekyll* example to help show some of the other ways these can be used. These Background tasks aren't just limited to Builds, Tests or File System changes. I have a few other use cases I am thinking about. One is monitoring for Commits or commits with certain messages, and then have the task trigger different build tasks.

After seeing the [Live Unit Testing](https://blogs.msdn.microsoft.com/visualstudio/2017/03/09/live-unit-testing-in-visual-studio-2017-enterprise/) feature in Visual Studio 2017, I would love to expand the integration between VSCode, Pester and the PowerShell Extension to achieve similar results for PowerShell. Where I think this could get interesting is adding a watcher for Code Coverage that integrates with the PowerShell Extension\PowerShellEditorServices to highlight lines green or red if they have Code Coverage or not. You can also do something like the [Gitlens Extension](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) that uses the VSCode [CodeLens](https://code.visualstudio.com/blogs/2017/02/12/code-lens-roundup) feature. Basic Code Coverage could be done now with a problem matcher that would underline uncovered lines with a red squiggly, but it would be interesting to see what could be done with the PowerShell Editor Services. Code Coverage is something I need to put more time into learning, so if I start working on that I might work on adding a Code Coverage into the Task as well.

These background tasks have a lot of potential. I would love to hear what other uses people come up with. Right now you can only have a single background task at a time, but they [have plans to add support for running multiple](https://github.com/Microsoft/vscode/issues/18842).In the [January Release Notes](https://code.visualstudio.com/updates/v1_9#_task-support), they list this change:

> The property isWatching is deprecated in favor of isBackground to support more scenarios in the future.

Tasks used to use the `isWatching` property, but this change to `isBackground` implies they want these Background Tasks to be seen as more than just watcher tasks. I am wondering if this might lead to more **Live Unit Testing** like features in the future. They also added the ability to use the terminal for the Task Output by adding this setting to the **tasks.json**: `"_runner": "terminal"`. They definitely are still improving the Task engine, and I am interested to see what else they have planned. I'll have to do some digging to see what else I can find on Github. Please contact me here or on [Twitter](https://twitter.com/BrandonPadgett) if you know anymore details.

## Full Config

Here is a gist of the full example **tasks.json**

{% gist gerane/0e1c2b5a63276bff6269e3db9192ca05 %}

### Related Posts
{: .t60 }

{% include list-posts.html tag='VSCode' %}
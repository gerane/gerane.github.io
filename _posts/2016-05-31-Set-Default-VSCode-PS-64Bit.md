---
layout: page
title: Set Default for VS Code's New Integrated Terminal to 64 Bit PowerShell
subheadline: PowerShell
date: "2016-05-31 2:27"
teaser:
categories:
  - powershell
tags:
  - powershell
  - VSCode
image:
  thumb:
  homepage: 2016/05/settings2.png
header:
  image_fullwidth: 2016/05/settings2.png
---
![New Terminal](/images/2016/05/term.png)

The Latest Insider's build `(v1.2.0-insider)` of [Visual Studio Code](https://code.visualstudio.com/insiders) added an Integrated Terminal. It is still early and isn't perfect, but this is a step in the right direction. By default, VS Code has cmd.exe set to it's default Terminal on Windows. Even worse, it is the 32 Bit version of cmd.exe on 64 Bit machines. This can be very problematic and cause some major headaches in the long run, so I hope they change this in a future release. One quick example is PSReadline will not work in a 32bit console.

Right now, there is no integration between the PowerShell Extension and the Integrated terminal. I know this is something [David Wilson](https://twitter.com/daviwil), who is over the [PowerShell Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell), is trying to make happen. This means the new [VSCode profile](http://brandonpadgett.com/powershell/Getting-Started-With-Editor-Commands/) that was added in the latest PowerShell Extension Update will currently only apply to the PowerShell session that the Extension uses, which runs in 64 Bit by default. This might be a little confusing to users in it's current state.

The terminal can be accessed by using the keyboard shortcut <kbd>ctrl</kbd> + <kbd>\`</kbd>. To set the 64 Bit version of PowerShell as the default Editor we are going to use Sysnative path for PowerShell. We can do this in the User Settings that can be accessed from the menu by **File > Preferences > User Settings**. If you want to have different settings for different workspaces you can use **File > Preferences > Workspace Settings**. This will create a new settings.json file in that projects **.vscode** directory. For the below example I am using Workspace settings. We want to add the following setting override to the **Settings.json**:

```json
"terminal.integrated.shell.windows": "C:\\WINDOWS\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
```

You can see the process of adding the new default below:

![Set Default Terminal](/images/2016/05/64bitPS.gif)

Your **Settings.json** should look similar to the below example if you don't have any other overridden settings.

```json
// Place your settings in this file to overwrite default and user settings.
{
    "terminal.integrated.shell.windows": "C:\\WINDOWS\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
}
```

I hope others find this helpful and hopefully this won't be an issue as the terminal matures.


### Related Posts
{: .t60 }

{% include list-posts.html tag='powershell' %}

---
layout: page
title:  "Having some fun with Dynamic Parameters and Rick Rolls"
subheadline:  "Powershell"
teaser: "PowerShell, PowerShell Gallery, NPM, Aprils Fools, and Rick Rolls"
categories:
    - powershell
tags:
    - powershell
    - Security
    - PSGallery
image:
    thumb:
    homepage:
---
A little while ago I found out that intellisense didn't quite function the way I thought it did. It starts processing results much earlier than I had initially thought. The recent issues NPM has encountered, which you can read about here [kik, Left Pad and NPM][2556ad65], got me thinking about the [PowerShell Gallery][83a06856]. Many in the PowerShell Community come from an IT background and not a Developer background, and I think many are less likely to realize the potential risks a Repository like the PSGallery might impose. Instead of doing the typical April Fools prank, I wanted to do something fun that also served as a way to spread awareness of potential risks.

## PowerShell HTML5 Demo

I was amazed when I first saw [Lee Holmes][6a5d6173] original April Fools joke where he used a fake Powershell and HTLM5 demo to Rick Roll people. You can find his original post here: [Powershell and HTML5][5c4b4685]. What he did in that had a lasting impression on me, and I learned a lot when I went through the code he wrote. I have actually used a few of the things that I learned from that Rick Roll in my day to day work. As a salute to Lee, I am using his Rick Roll one liner in my example.

## Dynamic Parameters

I love using dynamic params in my modules and scripts. If you aren't familiar, a Dynamic Param lets you populate the autocomplete list dynamically. This works similar to the built in cmdlets. An example is when you type **Get-Process -Name** and a list of currently running Processes populates into your autocomplete. Ed Wilson has a few great [Ed Wilson][774eaec8] blog posts on these. A great one can be found here: [Use Dynamic Parameters to Populate List of Printer Names][e1883aa4]

I had a few instances where I was wanting to be able to use a Dynamic Param, but also select more than one. I started testing a few ideas just to see how it worked out. One idea I had was to use **Out-Gridview** where the options in the grid were dynamic, and then pass back my selections as an array. This sort of worked... but had an interesting side effect I wasn't expecting. When the script or module was loaded or in memory, intellisense is evaluating Dynamic Params way earlier than what I was expecting.

Below is an example function called **Get-RickRoll** that I will be referencing.


{% gist gerane/68b9d796ce3f9881568242e241a69bee %}


Intellisense starts processing Dynamic Params as soon as it thinks you might be typing the function's name. When the function **Get-RickRoll** is loaded, intellisense starts processing the Dynamic Param when "**Get-** is typing into the ISE or console. You will see slightly different behavior in a console vs ISE. The ISE will auto trigger when the intellisense drop down appears like the example below:


![ISE Example](/images/2016/04/isedropdown.png)


In a console it triggers when tab completing or triggering intellisense with other methods such as PSReadline ctrl + spacebar. In my testing I also had it where it was triggering anytime I typed **"-"**, but I forgot the exact way I was triggering that behavior.

## The PowerShell Gallery

When I first encountered this in my testing, I quickly started thinking about the potential security issues this could impose. I wanted to make sure that there wasn't a way that autoload could be an issue with this. However, if you install a module, it already had a chance to do something malicious on your machine.

After a little testing, I realized that if I didn't manually Import the module, it won't start processing the Dynamic Param until you actually type the Dynamic Param's name. This was how I initially thought intellisense worked. I thought the Dynamic Param processing would occur as I typed the param name and not the function name. After autoload has loaded the module the first time, it will start triggering after **"Get-"** again.

This process highlighted a major weakness in how I was downloading and validating modules. If I was interested in a module, I would install it and then inspect it. The PSGallery has the option to **Save-Module**, which is what I should have been using. Even if we trust the person who Published that module, we still should inspect the module. You need to also consider the possibility that Publishers of modules could be compromised or even bribed, and have their modules altered to be malicious. This is also a potential risk when running **Update-Module**. The PSGallery does scan for malicious code, but it is not going to catch everything. The best defense against these types of problems is to verify Modules before installing or updating them. This is especially important if using the PSGallery in production. I would suggest setting up your own private repository and inspecting modules as you add them to the private repo.

I strongly recommend reading through and understanding the modules you use anyway. I have learned so much while attempting to understand how others are accomplishing something in a module or script. I always find out some new way to accomplish a task or new way to approach a problem. Just other coding styles can be very interesting to me and I have often changed things in my own coding style because I liked something they did better. If this is something you aren't already doing, I highly recommend it.

Luckily, one of the biggest problems that NPM suffered is not an issue in the PSGallery. You are not allowed to unpublish Modules in the PSGallery. This is a big deal because if a Publisher decided to unpublish a popular module, and someone immediately created a new malicious module with the same name, it could have disastrous results. There is also the dependency issue that hit NPM, but I don't think this is as big of a problem for PowerShell. Authors not being able to remove a Module should mitigate most of that issue.

## The Rick Roll

I'll admit, before I thought about the security concerns of this, my brain immediately thought of Lee's Rick Roll April Fools joke and put the two together. In the **Get-RickRoll** example above, instead of something that builds your typical array list, like **Get-Childitem** or similar, I have replaced it with Lee's one liner Rick Roll. Now when this function or module is loaded, anytime someone types **"Get-"** this happens


![Rick Roll](/images/2016/04/AprilFools.gif)


To process the Dynamic Param it executes the code in the Dynamic Param section. You could also potentially do some weird stuff with parameter validation or default parameter values. I haven't gone very far into looking into those yet, but I do plan on playing with **Out-Gridview** in default param values at some point.

Hopefully others might learn something from this like I did. I also have an example Module that I have put up on Github if anyone is interested in playing with Module loading. [Github - AprilFools][39917991]

  [2556ad65]: http://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm "kik, Left Pad and NPM"
  [83a06856]: https://www.powershellgallery.com/ "PowerShell Gallery"
  [6a5d6173]: https://twitter.com/Lee_Holmes "Lee Holmes"
  [5c4b4685]: http://www.leeholmes.com/blog/2011/04/01/powershell-and-html5/ "Powershell and HTML5"
  [774eaec8]: https://twitter.com/ScriptingGuys "Ed Wilson - The Scripting Guy"
  [e1883aa4]: https://blogs.technet.microsoft.com/heyscriptingguy/2014/03/21/use-dynamic-parameters-to-populate-list-of-printer-names/ "Use Dynamic Parameters to Populate List of Printer Names"
  [39917991]: https://github.com/gerane/AprilFools "Github - AprilFools"


### Related Posts
{: .t60 }

{% include list-posts.html tag='powershell' %}

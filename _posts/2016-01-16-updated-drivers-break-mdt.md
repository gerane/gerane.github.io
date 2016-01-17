---
layout: page
title: Powershell&#58; Find and Extract Drivers that break Task Sequences in Windows 10.
date: "2016-01-16 21:01"
subheadline: Powershell
teaser: Windows Updates can break Windows 10 Deployments.
categories:
    - powershell
tags:
    - mdt
    - powershell
    - scripts
    - windows 10
    - windows update
    - task sequence
image:
    thumb:
    homepage:
---

Windows 10 is a sore subject for me. As a Sysadmin, it has been a very frustrating to say the least. The guides you find on Technet or other blogs make it seem so easy, but they usually only cover the most general a basic situations. I keep finding myself wondering if they have ever actually tried to get it to work outside of a Lab or VM. My deployments work great in a VM, but start to fall apart once targeting real hardware. I am going to try to stay away from the topics of deploying configurations Windows 10, because I feel those deserve their own blog posts.

## The Problem

Shortly after the release of 1511 I started noticing the Task Sequences I was testing were being killed on several of the Models I was testing on. After the Task Sequence connected to the Deployment Share, the NIC was getting disabled briefly. This would immediately kill the Task Sequence with the, "No physical adapters present" error.

## Failed Solution #1: Updating Drivers

I was pretty positive this was being caused by Windows Update, and looking at my logs I confirmed this. Updating my driver cabs had no impact. I then moved to importing a newer driver from NIC manufacturer that matched the Windows Update version number. Windows Update still updated the NIC even though the drivers were the same version.

## Failed Solution #2: Disabling Driver Updates for Windows Updates

I decided to test disabling Driver updates in Windows Updates. In Windows 10 1511, Microsoft changed the GUI where this change is made. I am cautious when it comes to these sorts of settings in Windows 10. I have encountered issue after issue with Windows 10 RTM and the 1511 update where settings get removed, settings stop working, new settings get added quietly, GPOs stop working, and just headache after headache. The top image is the new 1511 GUI setting and the bottom is the RTM.

![1511 Settings](\images\2016\01\1511.PNG)

![RTM Settings](\images\2016\01\rtm.PNG)

The change in the wording was a red flag for me. When I see changes like this, I have learned that there is a good chance that the Registry keys or Group Policy settings may no longer work, work as you would expect or may stop working in an upcoming update. There were also changes in 1511 that have broken Group Policy and other settings that were working in RTM for me. I then decided to do some tests and found that it appeared the Group Policy settings were working and decided to give those a try and set the local group policy.

When booting the test machine up for the first time, I got greeted by a nice Driver related BSOD. The base image was identical except for that single setting change, so either the capture was bad or Windows 10 does not like having Windows Update disabled during first boot. After testing, it works if applied after first boot and you can see the Event logs stating an Update had been blocked by Group Policy settings. At this point I decided to take another approach because I didn't want to sit through another capture. I really wanted to do more research on this GPO before moving forward with it. If anyone has any information or advice on disabling Driver updates in Windows 10 you can reply here or find me on Twitter: [@brandonpadgett][0aeb2f66]

  [0aeb2f66]: https://twitter.com/BrandonPadgett "Twitter"

## The Solution

I knew the versions were the same, so I decided to see if the hashes matched. I was pretty sure they wouldn't, and I was right. I will have to read into this more. I am assuming Windows Update is updating based on hashes. That was when I decided to extract the exact driver Windows Update was using. I hadn't played with *Export-WindowsDriver* so I decided I wanted to write something using that. There is likely a more efficient method that extracts only the specific drivers instead of all of them making the process quicker, but I figured having all of them might be beneficial for troubleshooting.

The script does the following:

* Parses Event Logs for the Events indicating a Driver was Updated by Windows Update.
* Gathers the Driver Name.
* Exports all Windows Drivers to a folder.
* Collects all Drivers Updated by Windows Update and copies them to an UpdatedDrivers folder.
* Logs the Event Logs it found to a log file
* Logs the Driver Details for the Updated Drivers

I added a param to delete all of the Drivers that weren't updated, but I have mostly kept them around until I knew I wouldn't need them. The script is rough and i'll likely add some path/logpath params to it if I find I am using it often enough. After importing the exact NIC driver that Windows Update was using, it no longer attempted to update the NIC and the problem ceased.

## Disabling Driver Updates

I need to do more reading about Driver updates through Windows Updates in Windows 10. The way Microsoft is handling Windows 10 has me worried about disabling or enabling a setting like that. I know I am going to have some issues to sort out if keeping Drivers Updates through Windows Updates enabled. If I were to run the OEM's driver update utility, it sees the NIC driver installed by Windows Update as out of date and wants to install the original one I had in my Task Sequence. Even though the Windows Update version is a later version, it still sees it as needing to be updated. I still think Disabling the Driver Updates may be the best long term solution if I can get it working and if it doesn't impact anything in Windows 10 that I am unaware of. Regardless, I find this script useful for troubleshooting and thought I'd share.

Here is a link to the script on my github and a gist is below.

## Github Repo
[Github Repo][ee56304e]

  [ee56304e]: https://github.com/gerane/PowerShell/blob/master/scripts/Get-UpdatedDrivers.ps1 "Get-UpdatedDrivers.ps1"

## Get-UpdatedDrivers.ps1 Gist

{% gist gerane/3183353ba4713de06c66 %}


## Related Posts
{: .t60 }

{% include list-posts.html tag='powershell' %}

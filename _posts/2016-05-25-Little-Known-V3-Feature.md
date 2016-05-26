---
layout: page
title: The Little Known PowerShell V3 Feature that Everyone should be using
subheadline: PowerShell
date: "2016-05-25 2:27"
teaser:
categories:
  - powershell
tags:
  - powershell
  - Validation
image:
  thumb:
  homepage:
---

PowerShell added a lot of great features in Version 3. PSCustomObjects, Passing Variables to Remote Sessions, and Member Enumeration are a few of the great features that come to mind. However, I have rarely, if ever, seen one of the best features from Version 3 mentioned. We are now in Version 5, and I want to try to spread awareness about this and get people using it.

Everyone knows about Validation Attributes for Advanced Function Parameters. Using these has been a best practice since I started writing PowerShell. However, what I have not seen mentioned as a Best Practice or rarely if ever seen used by others is a little feature added in PowerShell Version 3. The feature allows Validation Attributes on **ANY** Variable. You can see here in a blog post about the new features being released for Version 3: [New V3 Language Features](https://blogs.msdn.microsoft.com/powershell/2012/06/13/new-v3-language-features/).

Below you can see what the Variable Validations looks like in a PowerShell Console. Each line is being ran via <kbd>f8</kbd> in the PowerShell ISE.

![Variable Validation](/images/2016/05/VarValidation.gif)

Below is the Console output from above for each Validation.

## ValidateRange

{% highlight powershell %}
PS> [ValidateRange(1,10)][int]$ValidateRange = 1
PS> $ValidateRange = 3
PS> $ValidateRange = 11
The variable cannot be validated because the value 11 is not a valid value for the x variable.
At line:1 char:1
+ $ValidateRange = 11
+ ~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ValidationMetadataException
    + FullyQualifiedErrorId : ValidateSetFailure
{% endhighlight %}


## ValidateSet
{% highlight powershell %}
PS> [ValidateSet('Test1','Test2','Test3')][string]$ValidateSet = 'Test1'
PS> $ValidateSet = 'Test2'
PS> $ValidateSet = 'Test4'
The variable cannot be validated because the value Test4 is not a valid value for the x variable.
At line:1 char:1
+ $ValidateSet = 'Test4'
+ ~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ValidationMetadataException
    + FullyQualifiedErrorId : ValidateSetFailure
{% endhighlight %}


## ValidateNotNullOrEmpty
{% highlight powershell %}
PS> [ValidateNotNullOrEmpty()][string]$ValidateNotNullOrEmpty = 'Test1'
PS> $ValidateNotNullOrEmpty = 'Test'
PS> $ValidateNotNullOrEmpty = ''
The variable cannot be validated because the value  is not a valid value for the x variable.
At line:1 char:1
+ $ValidateNotNullOrEmpty = ''
+ ~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ValidationMetadataException
    + FullyQualifiedErrorId : ValidateSetFailure
{% endhighlight %}


## ValidateCount
{% highlight powershell %}
PS> [ValidateCount(1,10)][array]$ValidateCount = @(1..9)
PS> $ValidateCount += 'test'
PS> $ValidateCount += 'test2'
The variable cannot be validated because the value System.Object[] is not a valid value for the ValidateCount variable.
At line:1 char:1
+ $ValidateCount += 'test2'
+ ~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ValidationMetadataException
    + FullyQualifiedErrorId : ValidateSetFailure
{% endhighlight %}


## ValidateLength
{% highlight powershell %}
PS> [ValidateCount(1,10)][string]$ValidateLength = 'Test1'
PS> $ValidateLength = 'Testing2'
PS> $ValidateLength = 'TestOverTen'
The variable cannot be validated because the value TestOverTen is not a valid value for the ValidateLength variable.
At line:1 char:1
+ $ValidateLength = 'TestOverTen'
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ValidationMetadataException
    + FullyQualifiedErrorId : ValidateSetFailure
{% endhighlight %}


## ValidatePattern
{% highlight powershell %}
PS> [ValidatePattern('^\d{3}[-.]?\d{3}[-.]?\d{4}$')][string]$ValidatePattern = '800-123-4567'
PS> $ValidatePattern = '000-000-0000'
PS> $ValidatePattern = '000-000-00000'
The variable cannot be validated because the value 000-000-00000 is not a valid value for the ValidatePattern variable.
At line:1 char:1
+ $ValidatePattern = '000-000-00000'
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ValidationMetadataException
    + FullyQualifiedErrorId : ValidateSetFailure
{% endhighlight %}


## ValidateScript
{% highlight powershell %}
PS> [ValidateScript({Test-Path $_})][string]$ValidateScript = 'C:\Windows\System32\cmd.exe'
PS> $ValidateScript = 'C:\Windows\System32\powercfg.exe'
PS> $ValidateScript = 'C:\Windows\System32\DoesNotExist.exe'
The variable cannot be validated because the value C:\Windows\System32\DoesNotExist.exe is not a valid value for the ValidateScript variable.
At line:1 char:1
+ $ValidateScript = 'C:\Windows\System32\DoesNotExist.exe'
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ValidationMetadataException
    + FullyQualifiedErrorId : ValidateSetFailure
{% endhighlight %}


## Conclusions
I am going to start using Variable Validations whenever possible, and I encourage others to start doing so as well. If you find this useful, please try to spread the word.


### Related Posts
{: .t60 }

{% include list-posts.html tag='powershell' %}

---
layout: page
title: Creating a Custom Theme for ISESteroids
subheadline: Powershell
teaser: A Detailed look at ISESteroids Themes and their XML format. Each XML value is explained for easy reference.
categories:
  - powershell
tags:
  - powershell
  - ise
  - theme
image:
  thumb: 2015/05/PoshBlogThumb.PNG
  homepage: 2015/05/ISESteroids.PNG
header:
  image_fullwidth: 2015/05/ISESteroids.PNG
date: '2015-05-25 07:49'
---

I prefer creating custom themes for my text editors. I am inside a text editor for large percentages of my day, so even small adjustments can vastly improve my user experience.

When exploring the ISESteroids Theme Manager I had issues finding the values I was looking for. I quickly realized that going to the actual xml file and editing the values that was the way to go. I quickly found that trial and error was needed for finding the correct values to change. This process was actually quite time consuming, so I decided to document the process and share so others may have an easier time than I did.

### Getting Started

I have good news and bad news. To make things easier, I used a theme that was almost entirely green and made all changes in red. This gave a nice contrast and lets the changes really stand out. However, it may make your eyes bleed.

### Menu Bar Settings


You will see a Monochrone setting in several places throughout this XML. This setting acts as a switch between a gradient and a solid color. If *MenuBarMonochrome* is set to *true*, only the first color setting will be used. The following are the settigs for the Menu Bar:

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<MenuBarMonochrome>true</MenuBarMonochrome>

<!-- For Solid Color Background with Monochrome = True -->
<MenuBarColor1>#FF2B8007</MenuBarColor1>

<!-- For Gradient Background Colors if Monochrome = False -->
<MenuBarColor2>#FFE6F0FA</MenuBarColor2>
<MenuBarColor3>#FFE6F0FA</MenuBarColor3>
<MenuBarColor4>#FFDCE6F4</MenuBarColor4>
<MenuBarColor5>#FFE1ECF9</MenuBarColor5>
<MenuBarColor6>#FFF8FAFC</MenuBarColor6>
{% endhighlight %}

#### Solid Menu Bar Color

When *MenuBarMonochrome* is set to *true* the *MenuBarColor1* will determine the color. The image below shows the changes once applied.

![Menu Bar Monochrome]({{ site.urlimg }}/2015/05/MenuBarColor1.PNG)

#### Gradient Colored Menu Bar

If *MenuBarMonochrome* is set to *false* the other colors are applied. If *MenuBarMonochrome* was set to *true* and you just changed *MenuBarColor3* you can see the portion of the gradient that it changes. You can then change one at a time until you get the desired gradient.

![Menu Bar Color 3]({{ site.urlimg }}/2015/05/MenuBarColor3.PNG)

After a few colors are changed you can start to see the gradient take shape.

![Menu Bar Gradient]({{ site.urlimg }}/2015/05/MenuBarColorGradient.PNG)

### Tab Settings

Tabs have a few settings that can be tweaked. One of the first I went hunting for was the square tab option. Here are the settings for Tabs.

For Active Tabs:
{% highlight xml %}
<!-- For Square Tabs -->
<ScriptSquareTabs>true</ScriptSquareTabs>

<!-- For Solid Color or Gradient Color Background -->
<ScriptSelectedMonochrome>false</ScriptSelectedMonochrome>

<!-- For Solid Color Background if Monochrome = True -->
<ScriptSelectedColor1>#FF3BF800</ScriptSelectedColor1>

<!-- For Gradient Background Colors if Monochrome = False -->
<ScriptSelectedColor2>#FFFFFFFF</ScriptSelectedColor2>
<ScriptSelectedColor3>#FFDFE9FC</ScriptSelectedColor3>
<ScriptSelectedColor4>#FFE1EEFC</ScriptSelectedColor4>
{% endhighlight %}

For Inactive Tabs:
{% highlight xml %}
<!-- For Solid Background Color or Gradient Color Background -->
<ScriptUnselectedMonochrome>true</ScriptUnselectedMonochrome>

<!-- For Solid Color Background if Monochrome = True -->
<ScriptUnselectedColor1>#FF0F7102</ScriptUnselectedColor1>

<!-- For Gradient Colors if Monochrome = False -->
<ScriptUnselectedColor2>#FFEBEBEB</ScriptUnselectedColor2>
<ScriptUnselectedColor3>#FFDDDDDD</ScriptUnselectedColor3>
<ScriptUnselectedColor4>#FFCDCDCD</ScriptUnselectedColor4>
{% endhighlight %}

#### Tab Borders

Tabs have a *ScriptSquareTabs* setting that if set to *true* makes the tab square. This can be seen in the following image:

![Square Tab]({{ site.urlimg }}/2015/05/SquareTab.PNG)

#### Active Tab Colors

The Tabs also use a similar Monochrome setting for single color or gradient called *ScriptSelectedMonochrome*. This works the same as the other Elements that use Monochrome.

#### Solid Tab Color

You can also see the solid Tab color in the Square Tab image above. Only the *ScriptSelectedColor1* value applies if *ScriptSelectedMonochrome* is set to *true*. 

#### Gradient Tab Color

If *ScriptSelectedMonochrome* is set to *false* the tab will be a gradient. In the example in the image below, *ScriptSelectedColor2* is blue, *ScriptSelectedColor3* is yellow and *ScriptSelectedColor4* is pink. You can see how it makes a crazy Tab color combination in the image below.

![Gradient Tab]({{ site.urlimg }}/2015/05/ScriptSelectedColorGradient.PNG)

#### Tab Background Color

The colors for the Tab Background function similar to the other color settings. Here you can find the settings that apply:

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<EditorTabControlMonochrome>false</EditorTabControlMonochrome>

<!-- If Monochrome = True this is the Background Color -->
<!-- If Monochrome = False this is the first Gradient Color -->
<EditorTabControlColor1>#FF122C09</EditorTabControlColor1>

<!-- Used for the second Gradient Color if Monochrome = False -->
<EditorTabControlColor2>#FF000000</EditorTabControlColor2>

<!-- Border Trim Color -->
<EditorTabControlBackground>#FF273800</EditorTabControlBackground>
{% endhighlight %}

You can the Background with a gradient in the following image:

![Gradient Tab Background]({{ site.urlimg }}/2015/05/EditorTabControl.PNG)

The *EditorTabControlBackground* Setting had a somewhat confusing name. It actually seemed to control the border color of the Editor. You can see the border in the following image:

![Tab Background Border Color]({{ site.urlimg }}/2015/05/EditorTabControlBackground.PNG)

### Toolbar Settings

The Toolbars are the sets of buttons in the Menu Bar. They have several options that can be customized. Here is a list of the Toolbar Options:

{% highlight xml %}
<!-- Color of the Button Groups Background -->
<ToolbarGroupBackground>#27EEF5FD</ToolbarGroupBackground>

<!-- This is the Group Background Color for the Run/Debug Group  -->
<ToolbarGroupBackgroundForRunCode>#90B6FD43</ToolbarGroupBackgroundForRunCode>
<ToolbarGroupBackgroundForDebugger>#00FFB98B</ToolbarGroupBackgroundForDebugger>

<!-- Used for the Arrow in the Group Overflow -->
<ToolbarOverflowButtonBackground>#FF007D11</ToolbarOverflowButtonBackground>

<!-- This is the Button Group Background Color for the Run/Debug Area -->
<ToolbarOverflowButtonBackgroundForRunCode>#FFFF0000</ToolbarOverflowButtonBackgroundForRunCode>
<ToolbarOverflowButtonBackgroundForDebugger>#FFEEF5FD</ToolbarOverflowButtonBackgroundForDebugger>

<!-- This is the Border Color -->
<RunspaceBarBackground>#FF275321</RunspaceBarBackground>
{% endhighlight %}

The Button Groups by default have a very light transparent color. Here is an image of this:

![Group Background Color]({{ site.urlimg }}/2015/05/ToolbarGroupBackground.PNG)

This sort of takes the default color and lightens it up slightly. You can set *ToolbarGroupBackground* to a full color for a bigger impact. You can see Red in the following image:

![Group Background Color Red]({{ site.urlimg }}/2015/05/ToolbarGroupBackgroundSolid.PNG)

You will notice that there are some Group Button Backgrounds that did not change. The Run Code/Debugger section has its own set of Settings. They work the same way as the other group colors. 

There is also the Overflow Buttons that can be colored. These function the same way as the Groups, but target the Overflow Buttons. You can see them set to Red in the image below:

![Group Overflow Background Color]({{ site.urlimg }}/2015/05/ToolBarOverflowBackground.PNG)

There are also settings for the Code/Debugger section that function the same way. Then the last setting is the *RunspaceBarBackground* that controls the Border Color. You can see what it looks like set to Red in the image below: 

![Group Border Color]({{ site.urlimg }}/2015/05/RunSpacebarBackground.PNG)

### Function Bar Settings

This section has Settings for the Search and Function Bar Area under the Menu Bar at the top. Here is a list of the Settings:

{% highlight xml %}
<!-- Function/Search Area Background Color -->
<FunctionComboCollapsedFill>#FFFFFFFF</FunctionComboCollapsedFill>

<!-- Function/Search Area Text Color -->
<FunctionComboForegroundStatic>#FFD3D3D3</FunctionComboForegroundStatic>

<!-- Function/Search Area Dropdown Background Color -->
<FunctionComboExpandedFill>#FFFFFFFF</FunctionComboExpandedFill>

<!-- Function/Search Area Dropdown Text Color -->
<FunctionComboForeground>#FF000000</FunctionComboForeground>
{% endhighlight %}

The Function/Search Bar is colored by *FunctionComboCollapsedFill* while the dropdown for your functions is colored by *FunctionComboExpandedFill*. You can also adjust the Text colors in this section as well. Here is an example of the *FunctionComboCollapsedFill* being set to Red: 

![Function Background]({{ site.urlimg }}/2015/05/FunctionComboCollapseFill.PNG)

And here is a *FunctionComboExpandedFill* being set to Red: 

![Function Background]({{ site.urlimg }}/2015/05/FunctionComboExpandedFill.PNG)

### Script Restore Settings

These settings control a Script Expander Menu that is normally hidden. This menu is only visible when the Script Pane is hidden. The settings for this section work the same as the other menu settings that use Monochrome. Here are the settings: 

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<ScriptExpanderMonochrome>false</ScriptExpanderMonochrome>

<!-- For Solid Color Background if Monochrome = True -->
<ScriptExpanderColor1>#FFE6F0FA</ScriptExpanderColor1>

<!-- For Gradient Colors if Monochrome = False -->
<ScriptExpanderColor2>#FFE6F0FA</ScriptExpanderColor2>
<ScriptExpanderColor3>#FFE6F0FA</ScriptExpanderColor3>
<ScriptExpanderColor4>#FFDCE6F4</ScriptExpanderColor4>
<ScriptExpanderColor5>#FFE1ECF9</ScriptExpanderColor5>
<ScriptExpanderColor6>#FFF8FAFC</ScriptExpanderColor6>
{% endhighlight %}

Here is an image of the Script Expander with a Red Background: 

![Function Background]({{ site.urlimg }}/2015/05/ScriptExpander.PNG)

When the Script Pane is hidden, the Script Expander Menu will be in the location of the Tab Bar. 

### Panel Divider Settings

These settings control the colors of the dividers between scripting, terminal, and other panels. They have the same coloring scheme as the other monochrome settings. There are two different sets of settings that control Vertical and Horizontal Splitters. Here are the settings: 

For Vertical Dividers:

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<VerticalSplitterMonochrome>true</VerticalSplitterMonochrome>

<!-- For Solid Color Background if Monochrome = True -->
<VerticalSplitterBorder>#FF808080</VerticalSplitterBorder>

<!-- For Gradient Colors if Monochrome = False -->
<VerticalSplitterColor1>#FFFF0000</VerticalSplitterColor1>
<VerticalSplitterColor2>#FFA2CBF3</VerticalSplitterColor2>
<VerticalSplitterColor3>#FFB7CEF5</VerticalSplitterColor3>
<VerticalSplitterColor4>#FFC6D2DE</VerticalSplitterColor4>
{% endhighlight %}

For Horizontal Dividers:

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<HorizontalSplitterMonochrome>true</HorizontalSplitterMonochrome>

<!-- For Solid Color Background if Monochrome = True -->
<HorizontalSplitterBorder>#FF808080</HorizontalSplitterBorder>

<!-- For Gradient Colors if Monochrome = False -->
<HorizontalSplitterColor1>#FFff0000</HorizontalSplitterColor1>
<HorizontalSplitterColor2>#FFA2CBF3</HorizontalSplitterColor2>
<HorizontalSplitterColor3>#FFB7CEF5</HorizontalSplitterColor3>
<HorizontalSplitterColor4>#FFC6D2DE</HorizontalSplitterColor4>
{% endhighlight %}

The picture below shows how the dividers look when Monochrome is set to True and *VerticalSplitterColor1* and *HorizontalSplitterColor1* are set to Red.

![Panel dividers](/images/2015/05/images\2015\05\splitter.PNG)

### Status Bar Settings

The Status Bar is the Bar at the very bottom of the ISE. You can customize the coloring of the Bar and its text. There are also options to customize the hyperlink colors. Here are are settings for the Status Bar:

{% highlight xml %}
<!-- Sets the Default Color of the Status Bar -->
<StatusBarBackgroundInactive>#FF003A12</StatusBarBackgroundInactive>
<!-- The Color of the Status Bar when Powershell is Actively Running -->
<StatusBarBackgroundActive>#FFFF0000</StatusBarBackgroundActive>

<!-- Sets the Color of the Status Bar Text when Powershell is Actively Running -->
<StatusBarForegroundColorActive>#FFFFFFFF</StatusBarForegroundColorActive>
<!-- Set the Default Color of the Status Text -->
<StatusBarForegroundColorInactive>#FFFFFFFF</StatusBarForegroundColorInactive>

<!-- Sets the Default hyperlink Color -->
<StatusBarHyperlinkDefaultColor>#FFF5DEB3</StatusBarHyperlinkDefaultColor>
<!-- Sets the Hover hyperlink Color -->
<StatusBarHyperlinkHoverColor>#FFFAFAD2</StatusBarHyperlinkHoverColor>
<!-- Sets the Underline Preference -->
<StatusBarHyperlinkUnderlineAlways>false</StatusBarHyperlinkUnderlineAlways>
<!-- Sets the Hover Underline Preference -->
<StatusBarHyperlinkUnderlineOnHover>false</StatusBarHyperlinkUnderlineOnHover>
{% endhighlight %}

Here is an image of the Status Bar with *StatusBarBackgroundInactive* color set to Red, *StatusBarForegroundColorInactive* set to White, and *StatusBarHyperlinkDefaultColor* set to a tan shade of yellow:

![Status Bar Background](/images/2015/05/StatusBarBackgroundInactive.PNG)

### Snippet Settings

This controls the name color of the snippet. In the image below, the color of the word BlockComment is controlled by this setting.

<SnippetHeader>#FFADD8E6</SnippetHeader>
<SnippetDescription>#FFADD8E6</SnippetDescription>
<SnippetBorder>#FFFF0000</SnippetBorder>

<SnippetFontFamily>Segoe UI</SnippetFontFamily>
<SnippetFontSize>8</SnippetFontSize>

This controls the color of the border of the Snippet as seen below

Snippet font size and type





<!-- This is the selection color -->
<ActiveSelection>#FFFF0000</ActiveSelection>
<!-- This is color when above selection is not focused -->
<InactiveSelection>#FFBFCDDB</InactiveSelection>



![finishedtheme](/images/2015/05/finishedtheme.PNG)


{% highlight xml %}
<DebuggerMarginUnsavedScript>#FFFF0000</DebuggerMarginUnsavedScript>
This is the color when saved
<DebuggerMarginSavedScript>#FF083101</DebuggerMarginSavedScript>
This controls the colors when then the debugger is active
<DebuggerMarginDebuggerActive>#FFFF6464</DebuggerMarginDebuggerActive>
<DebuggerMarginDebuggerActiveBackground>#FF000000</DebuggerMarginDebuggerActiveBackground> 
{% endhighlight %}

This is the border around the arrow in the margin
<CurrentLineAdornmentBorder>#FFFF0000</CurrentLineAdornmentBorder>
This is the color inside of the arrow
<CurrentLineAdornmentFill>#FF0BA514</CurrentLineAdornmentFill>

These set the colors of the squiggle lines under issues in your code. 
<SquiggleSyntaxError>#FFFF0000</SquiggleSyntaxError>
<SquiggleMinorWarning>#FF008000</SquiggleMinorWarning>
<SquiggleCriticalWarning>#FF0000FF</SquiggleCriticalWarning>
<SquiggleIncompatibility>#FFFEFF00</SquiggleIncompatibility>

These are the gutter colors for file changes.
<TrackChangesBeforeSave>#FFFF0000</TrackChangesBeforeSave>
<TrackChangesAfterSave>#FF2E8B57</TrackChangesAfterSave>
<TrackRevertedChanges>#FF0000FF</TrackRevertedChanges>


This setting changes the margin size of the line numbers. The above image shows a setting of 10, while the smaller image below shows 20
<LineNumberMarginSize>10</LineNumberMarginSize>
This sets the color of the line numbers. In this image red is selected.
<LineNumberMarginForegroundColor>#FFFF0000</LineNumberMarginForegroundColor>
This sets the color of the line numbers for selected lines.
<LineNumberMarginSelectedForegroundColor>#B4FFA500</LineNumberMarginSelectedForegroundColor>
<LineNumberMarginBookmarkColor>#B400DC00</LineNumberMarginBookmarkColor>
This controls the number color of lines with functions
<LineNumberMarginFunctionForegroundColor>#FF008000</LineNumberMarginFunctionForegroundColor>
Color of the number of the currently selected line
<LineNumberMarginCurrentForegroundColor>#AC28DF02</LineNumberMarginCurrentForegroundColor>
Controls the line number font family
<LineNumberMarginFontFamily>OCR A</LineNumberMarginFontFamily>

These control the Size and font family of the function references.
FunctionReferenceSize>10</FunctionReferenceSize>
<FunctionReferenceFontFamily>DIN</FunctionReferenceFontFamily>



These control the colors for function references. The image shows an active reference. If the function had no references the color in the inactive setting would be applied
<FunctionReferenceInactive>#FFD3D3D3</FunctionReferenceInactive>
<FunctionReferenceActive>#FFFF0000</FunctionReferenceActive>
<FunctionReferenceHoverBackground>#64DCE4EB</FunctionReferenceHoverBackground>



This controls the color of a collapsed block of text as seen below
<CollapsedTextColor>#FFFF0000</CollapsedTextColor>
This is the color of the bar in the margin when hovering over text that can be collapsed
<CollapseBarColor>#FFF5DEB3</CollapseBarColor>
This is the background of the highlighted expandable text
<CollapseSelectionBackground>#23FFFFFF</CollapseSelectionBackground>
This is the small square that shows there is collpased text
<CollapseSquareBackground>#FFADD8E6</CollapseSquareBackground>






I wont go in depth on these settings. These you can edit via Tools => Options
These are your syntax highlighting sections
<DefaultTextColor>#FF00FF00</DefaultTextColor>
<DefaultTokenColor>#FFF5DEB3</DefaultTokenColor>
<PowerShellScriptToken>
  <Token Name="Attribute" Color="#FF009F00" />
  <Token Name="Command" Color="#FF00BF00" />
  <Token Name="CommandArgument" Color="#FF00DF00" />
  <Token Name="CommandParameter" Color="#FF00FF00" />
  <Token Name="Comment" Color="#FF007F00" />
  <Token Name="GroupEnd" Color="#FF009F00" />
  <Token Name="GroupStart" Color="#FF00BF00" />
  <Token Name="Keyword" Color="#FF00DF00" />
  <Token Name="LineContinuation" Color="#FF00FF00" />
  <Token Name="LoopLabel" Color="#FF00FF00" />
  <Token Name="Member" Color="#FF00DF00" />
  <Token Name="NewLine" Color="#FF00BF00" />
  <Token Name="Number" Color="#FF009F00" />
  <Token Name="Operator" Color="#FF007F00" />
  <Token Name="Position" Color="#FF009F00" />
  <Token Name="StatementSeparator" Color="#FF00BF00" />
  <Token Name="String" Color="#FF00DF00" />
  <Token Name="Type" Color="#FF00FF00" />
  <Token Name="Unknown" Color="#FF00BF00" />
  <Token Name="Variable" Color="#FF00BF00" />
</PowerShellScriptToken>
<PowerShellConsoleToken>
  <Token Name="Attribute" Color="#FF00FF00" />
  <Token Name="Command" Color="#FF00DF00" />
  <Token Name="CommandArgument" Color="#FF00BF00" />
  <Token Name="CommandParameter" Color="#FF009F00" />
  <Token Name="Comment" Color="#FF007F00" />
  <Token Name="GroupEnd" Color="#FF009F00" />
  <Token Name="GroupStart" Color="#FF00BF00" />
  <Token Name="Keyword" Color="#FF00DF00" />
  <Token Name="LineContinuation" Color="#FF00FF00" />
  <Token Name="LoopLabel" Color="#FF00DF00" />
  <Token Name="Member" Color="#FF00BF00" />
  <Token Name="NewLine" Color="#FF009F00" />
  <Token Name="Number" Color="#FF009F00" />
  <Token Name="Operator" Color="#FF009F00" />
  <Token Name="Position" Color="#FF00BF00" />
  <Token Name="StatementSeparator" Color="#FF00DF00" />
  <Token Name="String" Color="#FF00FF00" />
  <Token Name="Type" Color="#FF009F00" />
  <Token Name="Unknown" Color="#FF00FF00" />
  <Token Name="Variable" Color="#FF00BF00" />
</PowerShellConsoleToken>
<XMLToken>
  <Token Name="Comment" Color="#FF006400" />
  <Token Name="CommentDelimiter" Color="#FF008000" />
  <Token Name="ElementName" Color="#FF8B0000" />
  <Token Name="MarkupExtension" Color="#FFFF8C00" />
  <Token Name="Attribute" Color="#FFFF0000" />
  <Token Name="Quote" Color="#FF0000FF" />
  <Token Name="QuotedString" Color="#FF00008B" />
  <Token Name="Tag" Color="#FF00008B" />
  <Token Name="Text" Color="#FF000000" />
  <Token Name="CharacterData" Color="#FF808080" />
</XMLToken>

The color of the background of selected matching brackets
<BraceMatchingFill>#FFF7C96C</BraceMatchingFill>
The outline color of selected matching brackets
<BraceMatchingBorder>#FFFFA500</BraceMatchingBorder>

Color of border of selected lines as in image
<ASTDashLineColor>#FFFFE4B5</ASTDashLineColor>

These are the colors of siblings. You can see how the sibling of the selected is colored in the image.
<ASTSiblingBorder>#FFF7C93C</ASTSiblingBorder>
<ASTSiblingFill>#C8F0F0E6</ASTSiblingFill>

Sets the colorsfor the selected item
<ASTScopeBorder>#64FFE4B5</ASTScopeBorder>
<ASTScopeFill>#3CFFB533</ASTScopeFill>

Sets the colorsof a selected Pipeline
<ASTPipelineBorder>#50C8C814</ASTPipelineBorder>
<ASTPipelineFill>#00F0F0E6</ASTPipelineFill>

<SnippetInsertionBorder>#FF038567</SnippetInsertionBorder>
<SnippetInsertionFill>#C847E7C1</SnippetInsertionFill>
<ConsoleAdminWarningColor1>#32FF9600</ConsoleAdminWarningColor1>
<ConsoleAdminWarningColor2>#0047E7C1</ConsoleAdminWarningColor2>
<ConsoleAdminWarningStripeOffset>5</ConsoleAdminWarningStripeOffset>

The Palettes in the color picker are represented here.
<PaletteColor1>#FFFF0000</PaletteColor1>
<PaletteColor2>#FF00FF00</PaletteColor2>
<PaletteColor3>#FF0000FF</PaletteColor3>
<PaletteColor4>#FFFFFF00</PaletteColor4>
<PaletteColor5>#FFFF00FF</PaletteColor5>
<PaletteColor6>#FFFFFF00</PaletteColor6>
<PaletteColor7>#FF00FFFF</PaletteColor7>
<PaletteColor8>#FFFF00FF</PaletteColor8>
<PaletteColor9>#FF00FFFF</PaletteColor9>
<PaletteColor10>#FF000000</PaletteColor10>
<PaletteColor11>#FF3C3C3C</PaletteColor11>
<PaletteColor12>#FF6E6E6E</PaletteColor12>
<PaletteColor13>#FFC8C8C8</PaletteColor13>
<PaletteColor14>#FFFFFFFF</PaletteColor14>
</ColorOptions>



### Related Posts
{: .t60 }

{% include list-posts.html tag='powershell' %}

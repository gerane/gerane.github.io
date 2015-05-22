---
layout: page
title:  "A Detailed look at ISESteroids Theme Values"
subheadline:  ""
teaser: "Creating a Custom Theme for ISESteroids"
categories:
    - powershell
tags:
    - powershell
    - ise
    - theme
---
I prefer creating custom themes for my text editors. I am inside a text editor for large percentages of my day, so even small adjustments can vastly improve my user experience.

When exploring the ISESteroids Theme Manager I had issues finding the values I was looking for. I quickly realized that going to the actual xml file and editing the values that was the way to go. I quickly found that trial and error was needed for finding the correct values to change. This process was actually quite time consuming, so I decided to document the process and share so others may have an easier time than I did.

### Getting Started

I have good news and bad news. To make things easier, I used a theme that was almost entirely green and made all changes in red. This gave a nice contrast and lets the changes really stand out. However, it may make your eyes bleed.

### Menu Bar Settings


You will see a Monochrone setting in several places throughout this XML. This setting acts as a switch between a gradient and a solid color. If *MenuBarMonochrome* is set to *true*, only the first color setting will be used. The following are the settigs for the Menu Bar:

{% highlight xml %}
<MenuBarMonochrome>true</MenuBarMonochrome>
<MenuBarColor1>#FF2B8007</MenuBarColor1>
<MenuBarColor2>#FFE6F0FA</MenuBarColor2>
<MenuBarColor3>#FFE6F0FA</MenuBarColor3>
<MenuBarColor4>#FFDCE6F4</MenuBarColor4>
<MenuBarColor5>#FFE1ECF9</MenuBarColor5>
<MenuBarColor6>#FFF8FAFC</MenuBarColor6>
{% endhighlight %}

#### Solid Menu Bar Color

When *MenuBarMonochrome* is set to *true* the *MenuBarColor1* will determine the color. The image below shows the changes once applied.

![Menu Bar Monochrome]({{ site.url }}/images/isesteroids/MenuBarColor1.png)

#### Gradient Colored Menu Bar

If *MenuBarMonochrome* is set to *false* the other colors are applied. If *MenuBarMonochrome* was set to *true* and you just changed *MenuBarColor3* you can see the portion of the gradient that it changes. You can then change one at a time until you get the desired gradient.

![Menu Bar Color 3]({{ site.url }}/images/isesteroids/MenuBarColor3.png)

After a few colors are changed you can start to see the gradient take shape.

![Menu Bar Gradient]({{ site.url }}/images/isesteroids/MenuBarColorGradient.png)

### Tab Settings

Tabs have a few settings that can be tweaked. One of the first I went hunting for was the square tab option. Here are the settings for Tabs.

For Active Tabs:
{% highlight xml %}
<ScriptSquareTabs>true</ScriptSquareTabs>
<ScriptSelectedMonochrome>false</ScriptSelectedMonochrome>
<ScriptSelectedColor1>#FF3BF800</ScriptSelectedColor1>
<ScriptSelectedColor1>#FF3BF800</ScriptSelectedColor1>
<ScriptSelectedColor2>#FFFFFFFF</ScriptSelectedColor2>
<ScriptSelectedColor3>#FFDFE9FC</ScriptSelectedColor3>
<ScriptSelectedColor4>#FFE1EEFC</ScriptSelectedColor4>
{% endhighlight %}

For Inactive Tabs:
{% highlight xml %}
<ScriptUnselectedMonochrome>true</ScriptUnselectedMonochrome>
<ScriptUnselectedColor1>#FF0F7102</ScriptUnselectedColor1>
<ScriptUnselectedColor2>#FFEBEBEB</ScriptUnselectedColor2>
<ScriptUnselectedColor3>#FFDDDDDD</ScriptUnselectedColor3>
<ScriptUnselectedColor4>#FFCDCDCD</ScriptUnselectedColor4>
{% endhighlight %}

#### Tab Borders

Tabs have a *ScriptSquareTabs* setting that if set to *true* makes the tab square. This can be seen in the following image:

![Square Tab]({{ site.url }}/images/isesteroids/SquareTab.png)

#### Active Tab Colors

The Tabs also use a similar Monochrome setting for single color or gradient called *ScriptSelectedMonochrome*. This works the same as the other Elements that use Monochrome.

#### Solid Tab Color

You can also see the solid Tab color in the Square Tab image above. Only the *ScriptSelectedColor1* value applies if *ScriptSelectedMonochrome* is set to *true*. The Settings and Values used would be:

For Active Tabs:
  
  * **ScriptSelectedMonochrome** = **True**
  * **ScriptSelectedColor1** = #**ColorCode**
  
For Inactive Tabs:
  
  * **ScriptUnselectedMonochrome** = **True**
  * **ScriptUnselectedColor1** = #**ColorCode**  

#### Gradient Tab Color

If *ScriptSelectedMonochrome* is set to *false* the tab will be a gradient. The *ScriptSelectedColor1* value is not used if monochrome is set to *false*. Only the *ScriptSelectedColor2*, *ScriptSelectedColor3* and *ScriptSelectedColor4* will be used. In this example,  *ScriptSelectedColor2* is blue, *ScriptSelectedColor3* is yellow and *ScriptSelectedColor4* is pink. You can see how it makes a crazy Tab color combination in the image below.

![Gradient Tab]({{ site.url }}/images/isesteroids/ScriptSelectedColorGradient.png)

The Settings and Values used for Gradient Tab Colors are:

  * **ScriptSelectedMonochrome** = **False**
  * **ScriptSelectedColor2** = #**ColorCode**
  * **ScriptSelectedColor3** = #**ColorCode**
  * **ScriptSelectedColor4** = #**ColorCode**


Unselected Tabs follow the same 

This is the selection color
<ActiveSelection>#FFFF0000</ActiveSelection>
This is color when above selection is not focused
<InactiveSelection>#FFBFCDDB</InactiveSelection>









This changes the color of the dropdown overflow arrow on thie toolbar. You will notice that some are controlled by other settings. It is controlled by the next setting
<ToolbarOverflowButtonBackground>#FF007D11</ToolbarOverflowButtonBackgrou

These next few settings setting controls the last overflow buttons on in the previous image for Run Code Group.
<ToolbarOverflowButtonBackgroundForRunCode>#FFFF0000</ToolbarOverflowButtonBackgroundForRunCode>
<ToolbarOverflowButtonBackgroundForDebugger>#FFEEF5FD</ToolbarOverflowButtonBackgroundForDebugger>

By default, these group backgrounds are heavily transparent. This gives the groups just a slighty lighter background color
<ToolbarGroupBackground>#27EEF5FD</ToolbarGroupBackground>
<ToolbarGroupBackgroundForRunCode>#90B6FD43</ToolbarGroupBackgroundForRunCode>
<ToolbarGroupBackgroundForDebugger>#00FFB98B</ToolbarGroupBackgroundForDebugger>

This colors the outline of the top menu bars
<RunspaceBarBackground>#FF275321</RunspaceBarBackground>

This is the menu that is hidden unless you maximize the console
monochrome and gradient works the same as before
<ScriptExpanderMonochrome>false</ScriptExpanderMonochrome>
<ScriptExpanderColor1>#FFE6F0FA</ScriptExpanderColor1>
<ScriptExpanderColor2>#FFE6F0FA</ScriptExpanderColor2>
<ScriptExpanderColor3>#FFE6F0FA</ScriptExpanderColor3>
<ScriptExpanderColor4>#FFDCE6F4</ScriptExpanderColor4>
<ScriptExpanderColor5>#FFE1ECF9</ScriptExpanderColor5>
<ScriptExpanderColor6>#FFF8FAFC</ScriptExpanderColor6>

This functions the same, and controls the backgroun behind the tabs
<EditorTabControlMonochrome>false</EditorTabControlMonochrome>
<EditorTabControlColor1>#FF122C09</EditorTabControlColor1>
<EditorTabControlColor2>#FF000000</EditorTabControlColor2>
This controls the color of the trim around the border
<EditorTabControlBackground>#FF273800</EditorTabControlBackground>

This controls the margin color on the left when a script has not been saved
<DebuggerMarginUnsavedScript>#FFFF0000</DebuggerMarginUnsavedScript>
This is the color when saved
<DebuggerMarginSavedScript>#FF083101</DebuggerMarginSavedScript>
This controls the colors when then the debugger is active
<DebuggerMarginDebuggerActive>#FFFF6464</DebuggerMarginDebuggerActive>
<DebuggerMarginDebuggerActiveBackground>#FF000000</DebuggerMarginDebuggerActiveBackground>

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

Snippet font size and type
<SnippetFontFamily>Segoe UI</SnippetFontFamily>
<SnippetFontSize>8</SnippetFontSize>

These control the colors for function references. The image shows an active reference. If the function had no references the color in the inactive setting would be applied
<FunctionReferenceInactive>#FFD3D3D3</FunctionReferenceInactive>
<FunctionReferenceActive>#FFFF0000</FunctionReferenceActive>
<FunctionReferenceHoverBackground>#64DCE4EB</FunctionReferenceHoverBackground>

This controls the name color of the snippet. In the image below, the color of the word BlockComment is controlled by this setting.
<SnippetHeader>#FFADD8E6</SnippetHeader>
<SnippetDescription>#FFADD8E6</SnippetDescription>
This controls the color of the border of the Snippet as seen below
<SnippetBorder>#FFFF0000</SnippetBorder>

Sets the colors of the background and text of the status bar when active and inactive
<StatusBarBackgroundInactive>#FF003A12</StatusBarBackgroundInactive>
<StatusBarBackgroundActive>#FFFF0000</StatusBarBackgroundActive>
<StatusBarForegroundColorActive>#FFFFFFFF</StatusBarForegroundColorActive>
<StatusBarForegroundColorInactive>#FFFFFFFF</StatusBarForegroundColorInactive>

This controls the colors of hyperlinksin the Status Bar
<StatusBarHyperlinkDefaultColor>#FFF5DEB3</StatusBarHyperlinkDefaultColor>
<StatusBarHyperlinkHoverColor>#FFFAFAD2</StatusBarHyperlinkHoverColor>
<StatusBarHyperlinkUnderlineAlways>false</StatusBarHyperlinkUnderlineAlways>
<StatusBarHyperlinkUnderlineOnHover>false</StatusBarHyperlinkUnderlineOnHover>

This controls the color of a collapsed block of text as seen below
<CollapsedTextColor>#FFFF0000</CollapsedTextColor>
This is the color of the bar in the margin when hovering over text that can be collapsed
<CollapseBarColor>#FFF5DEB3</CollapseBarColor>
This is the background of the highlighted expandable text
<CollapseSelectionBackground>#23FFFFFF</CollapseSelectionBackground>
This is the small square that shows there is collpased text
<CollapseSquareBackground>#FFADD8E6</CollapseSquareBackground>

This sets the color of the bar where search and other items are located
<FunctionComboCollapsedFill>#FFFFFFFF</FunctionComboCollapsedFill>
This is the color where the function bar is
<FunctionComboExpandedFill>#FFFFFFFF</FunctionComboExpandedFill>
This is the text color of the function bar
<FunctionComboForeground>#FF000000</FunctionComboForeground>
This is the color of words like "search" in the search bar
<FunctionComboForegroundStatic>#FFD3D3D3</FunctionComboForegroundStatic>


These splitters control the borders that seperate the different sections.
They work similar the the monochrome setting above.
<VerticalSplitterMonochrome>true</VerticalSplitterMonochrome>
<VerticalSplitterBorder>#FF808080</VerticalSplitterBorder>
<VerticalSplitterColor1>#FFFF0000</VerticalSplitterColor1>
<VerticalSplitterColor2>#FFA2CBF3</VerticalSplitterColor2>
<VerticalSplitterColor3>#FFB7CEF5</VerticalSplitterColor3>
<VerticalSplitterColor4>#FFC6D2DE</VerticalSplitterColor4>

<HorizontalSplitterMonochrome>true</HorizontalSplitterMonochrome>
<HorizontalSplitterBorder>#FF808080</HorizontalSplitterBorder>
<HorizontalSplitterColor1>#FFff0000</HorizontalSplitterColor1>
<HorizontalSplitterColor2>#FFA2CBF3</HorizontalSplitterColor2>
<HorizontalSplitterColor3>#FFB7CEF5</HorizontalSplitterColor3>
<HorizontalSplitterColor4>#FFC6D2DE</HorizontalSplitterColor4>

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



<!-- ### All Header-Styles 
{: .t60 }

{% include list-posts.html tag='header' %} -->

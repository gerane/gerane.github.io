---
layout: page
title: Creating a Custom Theme for ISESteroids
subheadline: PowerShell
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
date: "2015-05-29 17:39"
---

I am a big fan of the Powershell ISE, but I have always felt it was lacking. I have never used it exclusively, but it has a set place in my toolset. Recently I was watching a talk by [Dave Wyatt][f14190f1] and saw him using [ISESteroids][06461585]. I was immediately interested and had to try it out. After playing with it I noticed it had additional theming support. I have always been unimpressed with the default ISE theming, so this excited me.

I prefer creating custom themes for my text editors. I am inside a text editor for the vast majority of my day, so even small adjustments can vastly improve my user experience. ISESteroids has really changed the way I look at the ISE. I normally have issues using dark themes with editors like Powershell ISE. I feel dark themes are easier on the eyes, but much of this is ruined when using a dark theme with a white editor. ISESteroids gives me enough control over the interface theme that a dark syntax theme is doable.

  [06461585]: http://www.powertheshell.com/isesteroids/ "ISESteroids"
  [f14190f1]: https://twitter.com/msh_dave "Dave Wyatt"

When exploring the ISESteroids Theme Manager I had issues finding the values I was looking for. If you enable UI Customization it allows you to right click an element and customize the color. This is great in theory, but I could figure out where to click to change a large percentage of the values. I quickly realized that going to the actual xml file and manually editing the values was the way to go. I quickly found that trial and error was needed for finding the correct values to change. This process was actually quite time consuming, so I decided to document the process and share my results so others may have an easier time than I did. It also will make it easier to port some of my other themes over. I will be adding any themes I create to my [Github][f5a331ee].

  [f5a331ee]: https://github.com/gerane "Github"

## Github Gist

If you just need a quick reference I have created a Theme Template Gist/Repo with comments documenting the theme xml. I go into greater detail with screenshots in this post if further documentation is needed. Feel free to Fork and/or Share any themes or theme ideas.

[Click here to go directly to the Gist at the end of this post.](#Gist)

## Getting Started

I have good news and bad news. To make things easier, I used a theme that was almost entirely green and made all changes in red. This gave a nice contrast and lets the changes really stand out. However, it may make your eyes bleed. If you aren't sure where to find your Theme xml files, you can find a link to your theme folder in the theme dropdown. You can make changes to your theme and reload it to see the changes. If you do not have a color picker handy, you can just use the built in color picker in ISESteroids.

The default color picker has a nice feature that lets you steal colors from other applications or windows. You can move the color picker window over an application with colors you want. When you have the color picker focused you can move your mouse pointer over a color you want to steal and press <kbd>ctrl</kbd>. This will steal the color under the cursor and insert into the color picker. This is an easy way to get colors from other applications.

## Console Settings

The Console Window has several customizable settings that are different from the Script Panes. You can Share the Font Settings between the Console and the Script Pane by setting the *ShareFontForConsole* setting to True. The following settings allow you to set all of the Console Pane and Script Pane Font Settings if you want them to have different values.

{% highlight xml %}
<!-- Shares Console and Script Pane Settings -->
<ShareFontForConsole>false</ShareFontForConsole>

<!-- Script Pane Font Settings -->
<ScriptPaneFontFamily>ubuntumono-r.ttf#Ubuntu Mono</ScriptPaneFontFamily>
<ScriptPaneFontSize>12</ScriptPaneFontSize>

<!-- Console Pane Font Setings -->
<ConsolePaneFontFamily>sourcecodepro-regular.ttf#Source Code Pro</ConsolePaneFontFamily>
<ConsolePaneFontSize>10</ConsolePaneFontSize>

<!-- Console Pane Background and Text Colors -->
<ConsolePaneForegroundColor>#FFEEEEEC</ConsolePaneForegroundColor>
<ConsolePaneBackgroundColor>#FF212121</ConsolePaneBackgroundColor>

<!-- Script Pane Background Color -->
<ScriptPaneBackgroundColor>#FF212121</ScriptPaneBackgroundColor>
{% endhighlight %}

The Text Error Colors can be set with the following Settings:

{% highlight xml %}
<!-- Sets the Console Text and Text Background Colors for Error Messages -->
<ConsolePaneErrorForegroundColor>#FFEF2929</ConsolePaneErrorForegroundColor>
<ConsolePaneErrorBackgroundColor>#00FFFFFF</ConsolePaneErrorBackgroundColor>

<!-- Sets the Console Text and Text Background Colors for Warning Messages -->
<ConsolePaneWarningForegroundColor>#FFC4A000</ConsolePaneWarningForegroundColor>
<ConsolePaneWarningBackgroundColor>#00FFFFFF</ConsolePaneWarningBackgroundColor>

<!-- Sets the Console Text and Text Background Colors for Verbose Messages -->
<ConsolePaneVerboseForegroundColor>#FF06989A</ConsolePaneVerboseForegroundColor>
<ConsolePaneVerboseBackgroundColor>#00FFFFFF</ConsolePaneVerboseBackgroundColor>
{% endhighlight %}

This image shows Verbose text with *ConsolePaneVerboseForegroundColor* set to Blue and *ConsolePaneVerboseBackgroundColor* Set to Transparent.

![Verbose Text]({{site.urlimg}}/2015/05/Verbose.PNG)

## Menu Bar Settings

The Menu Header can have normal text or can be set to all caps. To set all caps you can set *CapitalizeMainMenuHeaders* to True.

{% highlight xml %}
<!-- This Sets All Caps for Menu Header -->
<CapitalizeMainMenuHeaders>false</CapitalizeMainMenuHeaders>
{% endhighlight %}

The following image shows the Menu Header with *CapitalizeMainMenuHeaders* set to True.

![Cap Headers]({{site.urlimg}}/2015/05/CapHeaders.PNG)

#### Menu Bar Colors

You will see a Monochrone setting in several places throughout this XML. This setting acts as a switch between a gradient and a solid color. If *MenuBarMonochrome* is set to *true*, only the first color setting will be used. The following are the settings for the Menu Bar:

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

![Menu Bar Monochrome]({{site.urlimg}}/2015/05/MenuBarColor1.PNG)

#### Gradient Colored Menu Bar

If *MenuBarMonochrome* is set to *false* the other colors are applied. If *MenuBarMonochrome* was set to *true* and you just changed *MenuBarColor3* you can see the portion of the gradient that it changes. You can then change one at a time until you get the desired gradient.

![Menu Bar Color 3]({{site.urlimg}}/2015/05/MenuBarColor3.PNG)

After a few colors are changed you can start to see the gradient take shape.

![Menu Bar Gradient]({{site.urlimg}}/2015/05/MenuBarColorGradient.PNG)

## Tab Settings

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

![Square Tab]({{site.urlimg}}/2015/05/SquareTab.PNG)

#### Active Tab Colors

The Tabs also use a similar Monochrome setting for single color or gradient called *ScriptSelectedMonochrome*. This works the same as the other Elements that use Monochrome.

#### Solid Tab Color

You can also see the solid Tab color in the Square Tab image above. Only the *ScriptSelectedColor1* value applies if *ScriptSelectedMonochrome* is set to *true*.

#### Gradient Tab Color

If *ScriptSelectedMonochrome* is set to *false* the tab will be a gradient. In the example in the image below, *ScriptSelectedColor2* is blue, *ScriptSelectedColor3* is yellow and *ScriptSelectedColor4* is pink. You can see how it makes a crazy Tab color combination.

![Gradient Tab]({{site.urlimg}}/2015/05/ScriptSelectedColorGradient.PNG)

#### Tab Background Color

The colors for the Tab Background function similar to the other color settings. Here you can find the settings that apply:

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<EditorTabControlMonochrome>false</EditorTabControlMonochrome>

<!-- For Solid Color Background if Monochrome = True -->
<EditorTabControlColor1>#FF122C09</EditorTabControlColor1>

<!-- For Gradient Colors if Monochrome = False -->
<EditorTabControlColor2>#FF000000</EditorTabControlColor2>

<!-- Border Trim Color -->
<EditorTabControlBackground>#FF273800</EditorTabControlBackground>
{% endhighlight %}

You can the Background with a gradient in the following image:

![Gradient Tab Background]({{site.urlimg}}/2015/05/EditorTabControl.PNG)

The *EditorTabControlBackground* Setting had a somewhat confusing name. It actually seemed to control the border color of the Editor. You can see the border in the following image:

![Tab Background Border Color]({{site.urlimg}}/2015/05/EditorTabControlBackground.PNG)

## Toolbar Settings

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

#### Button Groups

The Button Groups by default have a very light transparent color. This will lighten the default background color and make the buttons easier to see. Here is an image of this:

![Group Background Color]({{site.urlimg}}/2015/05/ToolbarGroupBackground.PNG)

 You can set *ToolbarGroupBackground* to a full color for a bigger impact. You can see Red in the following image:

![Group Background Color Red]({{site.urlimg}}/2015/05/ToolbarGroupBackgroundSolid.PNG)

You will notice that there are some Group Button Backgrounds that did not change. The Run Code/Debugger section has its own set of Settings. They work the same way as the other group colors.

#### Overflow Buttons

There are also Overflow Buttons that can be colored. These function the same way as the Button Groups above, but target the Overflow Buttons. You can see them set to Red in the image below:

![Group Overflow Background Color]({{site.urlimg}}/2015/05/ToolBarOverflowBackground.PNG)

There are also settings for the Code/Debugger section that function the same way. Then the last setting is the *RunspaceBarBackground* that controls the Border Color. You can see what it looks like set to Red in the image below:

![Group Border Color]({{site.urlimg}}/2015/05/RunSpacebarBackground.PNG)

## Function Bar Settings

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

The Function/Search Bar is colored by *FunctionComboCollapsedFill* and the dropdown for your functions is colored by *FunctionComboExpandedFill*. You can also adjust the Text colors in this section as well. Here is an example of the *FunctionComboCollapsedFill* being set to Red:

![Function Background]({{site.urlimg}}/2015/05/FunctionComboCollapseFill.PNG)

And here is a *FunctionComboExpandedFill* being set to Red:

![Function Background]({{site.urlimg}}/2015/05/FunctionComboExpandedFill.PNG)

## Script Restore Settings

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

![Function Background]({{site.urlimg}}/2015/05/ScriptExpander.PNG)

When the Script Pane is hidden, the Script Expander Menu will be in the location of the Tab Bar.

## Panel Divider Settings

These settings control the colors of the dividers between scripting, terminal, and other panels. They have the same coloring scheme as the other monochrome settings. There are two different sets of settings that control Vertical and Horizontal Splitters. Here are the settings:

For Vertical Dividers:

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<VerticalSplitterMonochrome>true</VerticalSplitterMonochrome>

<!-- Border Color -->
<VerticalSplitterBorder>#FF808080</VerticalSplitterBorder>

<!-- For Solid Color Background if Monochrome = True -->
<VerticalSplitterColor1>#FFFF0000</VerticalSplitterColor1>

<!-- For Gradient Colors if Monochrome = False -->
<VerticalSplitterColor2>#FFA2CBF3</VerticalSplitterColor2>
<VerticalSplitterColor3>#FFB7CEF5</VerticalSplitterColor3>
<VerticalSplitterColor4>#FFC6D2DE</VerticalSplitterColor4>
{% endhighlight %}

For Horizontal Dividers:

{% highlight xml %}
<!-- For Solid Color or Gradient Color Background -->
<HorizontalSplitterMonochrome>true</HorizontalSplitterMonochrome>

<!-- Border Color -->
<HorizontalSplitterBorder>#FF808080</HorizontalSplitterBorder>

<!-- For Solid Color Background if Monochrome = True -->
<HorizontalSplitterColor1>#FFff0000</HorizontalSplitterColor1>

<!-- For Gradient Colors if Monochrome = False -->
<HorizontalSplitterColor2>#FFA2CBF3</HorizontalSplitterColor2>
<HorizontalSplitterColor3>#FFB7CEF5</HorizontalSplitterColor3>
<HorizontalSplitterColor4>#FFC6D2DE</HorizontalSplitterColor4>
{% endhighlight %}

The picture below shows how the dividers look when Monochrome is set to True and *VerticalSplitterColor1* and *HorizontalSplitterColor1* are set to Red.

![Panel dividers](/images/2015/05/Splitter.PNG)

## Status Bar Settings

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

## Snippet Settings

This controls the name color of a snippet. In the image below, the color of the word BlockComment is controlled by this setting.

{% highlight xml %}
<!-- Snippet Color settings when a Snippet has been added -->
<SnippetHeader>#FFADD8E6</SnippetHeader>
<SnippetDescription>#FFADD8E6</SnippetDescription>
<SnippetBorder>#FFFF0000</SnippetBorder>

<!-- Snippet Font Settings -->
<SnippetFontFamily>Segoe UI</SnippetFontFamily>
<SnippetFontSize>8</SnippetFontSize>

<!-- Color of Snippets when Typed -->
<SnippetInsertionBorder>#FF038567</SnippetInsertionBorder>
<SnippetInsertionFill>#C847E7C1</SnippetInsertionFill>
{% endhighlight %}

The *SnippetHeader*, *SnippetDescription*, and *SnippetBorder* control the color settings for the Snippet Box that opens after inserting a Snippet. In the image you can see the Snippet box with a Red Border and Light Blue Header.

![Snippet Border](\images\2015\05\SnippetBorder.PNG)

When a Snippet text has been typed into the ISE they will be colored to let you know a Snippet can be auto-completed. The *SnippetInsertionBorder* and *SnippetInsertionFill* Settings control these colors. In the image below, you can see the text "func1" has been highlighted. A Red background and blue border were used in this example.

![Insert Snippet](\images\2015\05\SnippetInsert.PNG)

## Gutter Settings

The Gutter can be different colors based on different criteria. If a script is saved or unsaved it can have different colors, but it can also have different colors based on debugging. There are also Gutter settings for colors when there are changes made to the current script. We can also customize the color of the current line Arrow.

{% highlight xml %}
<!-- Script Saved State Gutter Colors -->
<DebuggerMarginUnsavedScript>#FFFF0000</DebuggerMarginUnsavedScript>
<DebuggerMarginSavedScript>#FF083101</DebuggerMarginSavedScript>

<!-- Script Debugger State Colors -->
<DebuggerMarginDebuggerActive>#FFFF6464</DebuggerMarginDebuggerActive>
<DebuggerMarginDebuggerActiveBackground>#FF000000</DebuggerMarginDebuggerActiveBackground>

<!-- Gutter Change Colors -->
<TrackChangesBeforeSave>#FFFF0000</TrackChangesBeforeSave>
<TrackChangesAfterSave>#FF2E8B57</TrackChangesAfterSave>
<TrackRevertedChanges>#FF0000FF</TrackRevertedChanges>

<!-- Current Line Arrow Colors -->
<CurrentLineAdornmentBorder>#FFFF0000</CurrentLineAdornmentBorder>
<CurrentLineAdornmentFill>#FF0BA514</CurrentLineAdornmentFill>
{% endhighlight %}

#### Debugger Margin Colors
Here we can see the Gutter color when *DebuggerMarginSavedScript* is Colored Red

![Debugger Margin Color Active](\images\2015\05\DebuggerMarginActive.PNG)

When the Debugger is active, the Gutter gets a striped pattern. In the next image we see the Gutter with *DebuggerMarginDebuggerActive* colored Red with the Debugger Active.

![Debugger Margin Unsaved](\images\2015\05\DebuggerMarginUnsaved.PNG)

#### File Change Colors

When a file is edited the ISE tracks those changes in the gutter using different colors for changes made before a save, after a save, and any changes reverted. The following image shows the Gutter Colors when something has been added, but not yet saved. In the example the *TrackChangesBeforeSave* has been set to the color Red.

![Changes Before Save](\images\2015\05\ChangesBeforeSave.PNG)

#### Current Line Arrow

The Current Line Arrow can be colored with two different settings. The first determines the background color and the second determines the outline color. This can be seen in the following image.

![Current Line Adornment Border](\images\2015\05\CurrentLineAdornmentBorder.PNG)

## Line Number Settings

Line Numbers have several settings that can change their color depending on certain criteria. These are the settings that control the Line Numbers:

{% highlight xml %}
<!-- Line Number Margin Size -->
<LineNumberMarginSize>10</LineNumberMarginSize>

<!-- Line Number Color -->
<LineNumberMarginForegroundColor>#FFFF0000</LineNumberMarginForegroundColor>

<!-- Selected Line Number Color -->
<LineNumberMarginSelectedForegroundColor>#B4FFA500</LineNumberMarginSelectedForegroundColor>

<!-- Bookmark Color -->
<LineNumberMarginBookmarkColor>#B400DC00</LineNumberMarginBookmarkColor>

<!-- Line Number Function Color -->
<LineNumberMarginFunctionForegroundColor>#FF008000</LineNumberMarginFunctionForegroundColor>

<!-- Current Line Number Color -->
<LineNumberMarginCurrentForegroundColor>#AC28DF02</LineNumberMarginCurrentForegroundColor>

<!-- Line Number Font Type -->
<LineNumberMarginFontFamily>OCR A</LineNumberMarginFontFamily>
{% endhighlight %}

In the previous section, we saw the Track Changes image. In this image the *LineNumberMarginSize* was set to 10. In the Below image the *LineNumberMarginSize* is set to 20. You can see that it has made the margin size much smaller.

![Line Number Margin Size](\images\2015\05\LineNumberMarginSize.PNG)

Functions can be set to always have a unique line number color. In the following image the *LineNumberMarginFuncForegroundColor* is set to Red. This sets the line number to Red if the Line is the start of the Function.

![Line Number Function Foreground](\images\2015\05\LineNumberMarginFuncForegroundColor.PNG)

Here you can see where multiple lines have been selected. The *LineNumberMarginCurrentForegroundColor* has been set to Red. All lines in the selection have had their line numbers set to the color Red.

![Line Number Selected Color](\images\2015\05\LineNumberMarginSelectedForegroundColor.PNG)

The below image shows the *LineNumberMarginForegroundColor* set to Red. This sets the Default Line Number Color.

![Line Number Foreground Color](\images\2015\05\MarginForegroundColor.PNG)

## Function Reference Settings

Function References will show up just above Functions in your scripts. Any time those functions are referenced it will give links to the places referencing them. These settings control the Font and Color settings of those References:

{% highlight xml %}
<!-- Function Reference Font Settings-->
<FunctionReferenceSize>10</FunctionReferenceSize>
<FunctionReferenceFontFamily>DIN</FunctionReferenceFontFamily>

<!-- Function Reference Color Settings -->
<FunctionReferenceInactive>#FFD3D3D3</FunctionReferenceInactive>
<FunctionReferenceActive>#FFFF0000</FunctionReferenceActive>
<FunctionReferenceHoverBackground>#64DCE4EB</FunctionReferenceHoverBackground>
{% endhighlight %}

The following image shows an active Reference with 8 References. The *FunctionReferenceActive* settings is set to the color Red.

![Reference Actice](\images\2015\05\FunctionReferenceActive.PNG)

## Collapsed Test Settings

There are several elements of Collapsed Text that can be customized. The following settings control these elements:

{% highlight xml %}
<!-- Collapsed Text Color -->
<CollapsedTextColor>#FFFF0000</CollapsedTextColor>

<!-- Bar in Margin when Hovering -->
<CollapseBarColor>#FFF5DEB3</CollapseBarColor>

<!-- Background of highlighted expandable text -->
<CollapseSelectionBackground>#23FFFFFF</CollapseSelectionBackground>

<!-- Margin Square Color -->
<CollapseSquareBackground>#FFADD8E6</CollapseSquareBackground>
{% endhighlight %}

The *CollapsedTextColor* Setting controls the color of a collapsed block of text as seen below.

![Collapsed Text](\images\2015\05\CollapsedText.PNG)

The following image shows the *CollapseBarSelection* and *CollapseSelectionBackground* Settings. The Brown bar in the margin is the *CollapseBarSelection*. The *CollapseSelectionBackground* is set to Red and sets the color of the background when hovering over text that can be collapsed.

![Collapsed Selection](\images\2015\05\CollapseBarSelection.PNG)

The next image shows the the Square in the margin colored Red. This is set by the *CollapseSquareBackground* Setting.

![Collapsed Square](\images\2015\05\CollpaseSquareBackground.PNG)

## Text Warning Settings

These Settings set the colors of the squiggle lines under issues in your code.

{% highlight xml %}
<!-- Squiggle Colors -->
<SquiggleSyntaxError>#FFFF0000</SquiggleSyntaxError>
<SquiggleMinorWarning>#FF008000</SquiggleMinorWarning>
<SquiggleCriticalWarning>#FF0000FF</SquiggleCriticalWarning>
<SquiggleIncompatibility>#FFFEFF00</SquiggleIncompatibility>
{% endhighlight %}

The foillowing image shows Red colored squiggles caused by a Syntax Error.

![Squiggles](\images\2015\05\Squiggle.PNG)

## Brace Settings

These settings set the background and border of Matching Brackets.

{% highlight xml %}
<!-- Bracket Colors -->
<BraceMatchingFill>#FFF7C96C</BraceMatchingFill>
<BraceMatchingBorder>#FFFFA500</BraceMatchingBorder>
{% endhighlight %}

The following image shows the *BraceMatchingFill* Setting set to the color Red.

![Brace Matching](\images\2015\05\BraceMatchingFill.PNG)

## Selection Settings

The selection colors can be a little confusing at first. There are several settings that control different aspects of Selection Colors. The following are the settings that control these.

{% highlight xml %}
<!-- This is the selection color -->
<ActiveSelection>#FFFF0000</ActiveSelection>

<!-- This is the color when above selection is not focused -->
<InactiveSelection>#FFBFCDDB</InactiveSelection>

<!-- Border Color of Selected Lines -->
<ASTDashLineColor>#FFFFE4B5</ASTDashLineColor>

<!-- Sibling Color Settings -->
<ASTSiblingBorder>#FFF7C93C</ASTSiblingBorder>
<ASTSiblingFill>#C8F0F0E6</ASTSiblingFill>

<!-- Scope Border Settings -->
<ASTScopeBorder>#64FFE4B5</ASTScopeBorder>
<ASTScopeFill>#3CFFB533</ASTScopeFill>

<!-- Pipeline Color Settings -->
<ASTPipelineBorder>#50C8C814</ASTPipelineBorder>
<ASTPipelineFill>#00F0F0E6</ASTPipelineFill>
{% endhighlight %}

#### Highlighted Text

The Actively Selected text is the text you have highlighted. In the following image you can see this set to a transparent shade of red.

![Active Selection](\images\2015\05\activeselection.PNG)

#### Text Section Border

The *ASTDashLineColor* sets the border color of the section of lines or text you have selected. The following image shows this set to Red.

![Dash Line](\images\2015\05\DashLineColor.PNG)

#### Sibling Colors

These are the colors of siblings. Siblings are other instances of the currently selected item. In the image below you can see how multiple instances of $ArchivedOutLogFile have been colored.

![Sibling Color](\images\2015\05\SiblingFillColor.PNG)

#### Scope Colors

The following image shows the *ASTScopeFill* setting set to Red. It is the current Scope of where you cursor is located. In this image you can see it colors the entire block of quoted text.

![Scope Border](\images\2015\05\ScopeBorder.PNG)

#### Pipeline Colors

The *ASTPipelineFill* and *ASTPipelineBorder* set the colors of a selected Pipeline. In the image you can see these set to Red.

![Pipeline Colors](\images\2015\05\PipelineBorderFill.PNG)

## Console Admin Warning Settings

If you right click the left margin in the console, you can turn on *Show Admin Warning*. If this is enabled and the ISE is launched with Admin privileges, there will be a Bar in the left margin letting you know the ISE has been ran as Admin. This is an added visual feature to help notify ensure the user has ran ISE as admin. If ISE is ran as Admin you will also notice the ISE icon is red. Here are the settings:

{% highlight xml %}
<!-- Console Admin Warning Settings -->
<ConsoleAdminWarningColor1>#32FF9600</ConsoleAdminWarningColor1>
<ConsoleAdminWarningColor2>#0047E7C1</ConsoleAdminWarningColor2>
<ConsoleAdminWarningStripeOffset>5</ConsoleAdminWarningStripeOffset>
{% endhighlight %}

The below image shows the Admin Console Warning in the left margin.

![Admin Warnings](/images/2015/05/consoleadmin.PNG)

## Color Picker Palette Settings

The Color Picker has a section of easy to select colors called Palettes. The Palettes in the color picker can be set with the following settings:

{% highlight xml %}
<!-- Color Picker Palette Colors -->
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
{% endhighlight %}

The following image shows the palettes on the right side.

![Palettes](\images\2015\05\palettes.PNG)

## Syntax Settings

I wont go in depth on these settings. These you can edit via Tools => Options in the ISE. This is the same editor as in the Default ISE.

{% highlight xml %}
<!-- Text Color Settings -->
<DefaultTextColor>#FF00FF00</DefaultTextColor>
<DefaultTokenColor>#FFF5DEB3</DefaultTokenColor>

<!-- Script Pane Syntax Settings -->
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

<!-- Console Pane Syntax Settings -->
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

<!-- XML Syntax Settings -->
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
{% endhighlight %}

## Finished Theme

This is still a work in progress, and I am still making minor tweaks as I go. This theme is based on a Monokai theme and customized in a few areas for personal preference.

![finishedtheme](/images/2015/05/finishedtheme.PNG)

## <a name="Gist"></a>Gist

{% gist gerane/6206dd9755fd365c3887 %}

## Related Posts
{: .t60 }

{% include list-posts.html tag='powershell' %}

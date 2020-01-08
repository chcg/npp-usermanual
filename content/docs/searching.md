---
title: Searching
weight: 40
---

There are multiple methods to search (and replace) text in files. You can also mark search results with a bookmark on their lines, or highlight the textual results themselves.  Generating a count of matches is also possible.

There are three main built-in search mechanisms: the standard (dialog-based) Find / Replace / Find In Files / Mark, the dialog-free Next / Previous search-navigation, and the Incremental Search.

All keyboard shortcuts mentioned below are the default values, but are configurable in the [Shortcut Mapper](../preferences/#shortcut-mapper).  You can see the active shortcut for any menu item in the menu entry, or in the Shortcut Mapper.

## Dialog-based Searching

The most powerful set of searching features is found in the standard dialog-based Find / Replace / Find In Files / Mark dialog.  This dialog is generically known as the "Find" dialog or window.  The dialog has one tab for each of the aforementioned searching-related features.

The **Find** tab (accessible using **Search > Find** or the keyboard shortcut Ctrl+F) gives access to searching and counting.  The **Replace** tab (**Search > Replace** or Ctrl+H) is similar, but allows you to also replace the matched text after it's found.  The **Find in Files** tab (**Search > Find in Files** or Ctrl+Shift+F) allows you to search and replace in multiple files with one action.  The **Mark** tab (**Search > Mark...**) allows you to apply red-marking (a red background to matched text; see (**Preferences > Style Configurator > Global Styles > Find Mark style**](../preferences/#global-styles)) to certain sections of text, and to add "bookmarks" to the lines that matched text is found upon.

*Note:*  Although a keyboard command can open and/or move input focus to one of the four tabs of the "Find" window, once this input focus is achieved, there is no possibility to switch to another of the tabs via the keyboard; the mouse must be used, or the window closed (via the *Escape* key) and the alternate tab's keyboard shortcut (or menu command) then invoked.

*Note:*  Use of some "Find" family features can cause the window to close.  Some users dislike this and wish for the "Find" window to always remain open.  This may be achieved by editing `config.xml` and looking for the `dlgAlwaysVisible=` parameter.  It defaults to `dlgAlwaysVisible="no"` but may be set to `dlgAlwaysVisible="yes"` to keep the "Find" window open and always available for user interaction.

*Note:*  Search option choices made by the user are remembered across invocations of Notepad++.

### Find / Replace

All the dialog-based have certain features in common, though some are disabled under certain circumstances.

* **Find what** edit box with dropdown history: this is the text you are searching for
* **Replace with** edit box with dropdown history: this is the text that will replace what was matched

* **☐ In selection**: If you have a region of text selected, and **In selection** is enabled, it will only **Count**, **Replace All**, or **Mark All** within that selection of text, rather than in the whole document (other buttons, such as **Find Next**, will continue to work on the whole document)
* **☐ Backward direction**: normally, searches go forward (down the page); with this option, they will go backward (up the page)
* **☐ Match whole word only**: if enabled, searches will only match if the result is a whole word (so "it" will not be found inside "hitch")
* **☐ Match case**: if enabled, searches must match in case (so a search for "it" will not find "It" or "IT").  The regular expression `i` flag will override this checkbox, where `(?i)` will make the search case insensitive, and `(?-i)` will make the search case sensitive.
* **☐ Wrap Around**: if enabled, when the search reaches the end of the document, it will wrap around to the beginning and continue searching

* **Search Mode**: this determines how the text in the **Find what** and **Replace with** will be treated
    * **☐ Normal**: all text is treated literally.
    * **☐ Extended (\n, \r, \t, \0, \x...)**: use certain "wildcards", as described in [Extended Search Mode (below)](#extended-search-mode)
    * **☐ Regular Expression**: uses the Boost regular expression engine to perform very power search and replace actions, as explained in [Regular Expressions (below)](#regular-expressions)
        * **☐ . matches newline**: in regular expressions, with this disabled, the regular expression `.` matches any character except the line-ending characters (carriage-return and/or linefeed); with this enabled, `.` also matches the line-ending characters.  As an alternative to using this checkbox, begin the **Find what** box text with `(?-s)` to obtain the unchecked behavior of **. matches newline**, or with `(?s)` to get its checked behavior.

* **☐ Transparency**: these settings affect the dialog box.  Normally, the dialog box is opaque (can't see the text beneath), but with these settings, it can be made semi-transparent (can partially see the text beneath)
    * **☐ On losing focus**: if this is chosen, the dialog will be opaque when you are actively in the dialog box, but if you click in the Notepad++ window, the dialog will become semi-transparent
    * **☐ Always**: if this is chosen, the dialog will be semi-transparent, even when you are actively in the dialog box
    * Slider Bar: sliding it right makes the dialog more opaque; sliding it left makes it more transparent.
        * Be careful when sliding it to the extreme left: you might not be able to see the dialog box anymore
        * By (temporarily) setting it to **Always**, you can see how transparent the dialog will be while moving the slider, which can help prevent making it too transparent to see.

The various action buttons available include:

* **Find Next**: Finds the next matching text
    * **☐** This checkbox changes the single **Find Next** button into **<<** and **>>** buttons, which are "Search backward" and "Search forward" (hovering over this checkbox with the mouse will, after a slight pause in movement, pop up a tooltip indicating "2 find buttons mode")
* **Count**: Counts how many matches are in the entire document, or possibly "In Selection", and shows that count in the message section at the bottom of the dialog box
* **Find All in All Opened Documents**: Lists all the search-results in a new **Find result** window; searches through all the file buffers currently open in Notepad++
* **Find All in Current Document**: Lists all the search-results in a new **Find result** window; only searches the active document buffer
* **Close**: Closes the search dialog

* **Replace**: Replaces the currently-selected match.  (If no match is currently selected, it behaves like **Find Next** and just highlights the next match in the specified direction)
* **Replace All**: Replaces all matches from the active location onward (following the **Backward direction** and **Wrap around** settings as appropriate).
    * NOTE: for regular expressions, this will be equivalent to running the regular expression multiple times, which is _not_ the same as running with the /g global flag enabled that is available in the regular expression engines of some programming-languages.
* **Replace All in All Opened Documents**: same as **Replace All**, but goes through all the documents open in Notepad++, not just the active document.

The above actions may be initiated via mouse by pressing the appropriate button, or via special `Alt` key combinations.  With focus on one of the Find dialog windows, press and release the `Alt` key.  This will cause Notepad++ to underline a single character in the text of *most* of the buttons.  Pressing Alt and one of the underlined characters will be the same as pressing the same button with the mouse, i.e., the chosen action will be initiated.  The Alt technique works for controls other than buttons as well, e.g., a checkbox control can be ticked/unticked via pressing its Alt key command.

**Find Next** has a special way of being invoked by keyboard control.  Pressing Enter when the Find dialog has input focus will initiate the **Find Next** command in the direction indicated by **Backward direction**.  Pressing Shift+Enter when the Find dialog has input focus will run the **Find Next** in the ***opposite*** direction as that indicated by **Backward direction**.  Hovering over the **Find Next** button with the mouse will, after a slight delay, pop up a tool tip indicating *Use Shift+Enter to search in the opposite direction* as a reminder of this capability.

When a find-family function is invoked via the Search menu, toolbar, or keyboard combination, the word at the caret (or the selected text, if any) is automatically copied into the **Find what** edit box.  This behavior cannot be disabled; it always happens.  To avoid this in a limited way, use the mouse ONLY to move input focus from an editing tab buffer to an already-open Find dialog, or make sure your caret is not "touching" a word and that there is no active selection when invoking the find-family command.  Aside:  This auto-copy feature can be exploited to get multi-line data into the **Find what** edit box, something that it is not possible to do via only typing into the box.  Simply select the multi-line text that you want to search for, and then call up the Find dialog via one of its functions.  The selected text will appear as usual in the **Find what** box.  The line-ending character(s) won't be visible, but they will be there and will be matched when a search/replace action is initiated.

A valid **Find what** edit box entry length ranges from 1 to 2046 characters.  A valid **Replace with** edit box entry length ranges from 0 to 2046 characters.  Any text entered/pasted into these boxes beyond the 2046th character is simply ignored when an action is invoked.  Note that a replacement operation with a zero-length **Replace with** box entry is effectively a deletion of the matched text.

Selecting **Search Mode** of **Regular expression** will cause the **Match whole word only** option to become deselected and disabled.  A possible workaround to allow doing this type of searches is to add `\b` to the beginning and end of your regular expression **Find what** text.

The **Find what** and **Replace with** edit boxes have a dropdown arrow which allows the user to repeat searches conducted previously.  For a given run of Notepad++, the search history can grow rather large; when Notepad++ is exited, it only saves the last 10 items in the history by default; number of search/replace terms retained may be changed by editing the `config.xml` configuration file.  The **Find in Files** tab's **Filters** and **Directory** text boxes have this "history" feature as well.  This history can be activated by clicking on the down-arrow with the mouse, or by using the down (and/or up) keys -- be careful though, sometimes when editing a control with the history feature, a user will accidentally hit an up or down arrow key when they really mean to press left or right arrow; this unfortunately wipes out the search/replace expression (as those are edited most often) that was being worked on and replaces it with some different text from the history buffer.

The **In selection** option will automatically be chosen by Notepad++ if a Find dialog window is opened when more than 1024 characters occur in the active selection.  The selected text will also be placed in the **Find what** box.  Running a **Count** or **Replace All** action without making other changes to the search parameters will result in *Count: 1 match* or *Replace All: 1 occurrence was replaced*, respectively, which is likely not what was intended.  The proper resolution for this situation is to change the **Find what** text if the intention is to search within-selection, or deselect **In selection** if the intention is to search for a fairly long block of text.

The status bar area of the Find dialog keeps the user informed of what occurred during an action.  For example, it might say *Mark: 1 match* or *Find: Invalid regular expression*.  Colors are used in the status bar for emphasis:  red for some sort of error; green or blue for various success or general information.

**Important remark**: When the regular expression search mode is invoked, the red alert error message "Find: Invalid regular expression" appears **ONLY** when you hit the **Find Next** button. All other possible actions lead to simply notify you that no result occurs, whereas, in fact, your search regular expression is just malformed. So, always do a **Find Next** search first, to test the validity of your regular expression input.

Notepad++ uses a flashing of the Find dialog window and the main Notepad++ window itself (when the Find dialog is not open) to indicate that search text has not been found (or possibly that a **Wrap around** in the search has occurred).  In general, if a search results in no matches, and the Find dialog window is open, that window will flash briefly as a failure indication.  If the Find dialog window is NOT open, and a failed search is initiated (e.g. via **Find Next** on the **Search** menu), the main Notepad++ window will flash briefly, again, as an indicator of the lack of success.  With the Find dialog window closed, but with **Wrap around** previously activated, a search that causes a wrap at an end of the file to occur will also cause the Notepad++ main window to flash.  In addition, the system error bell will sound if a **Find Next** or **Replace** action results in the **Find what** text not being encountered; the bell cannot be disabled.

If a search action is invoked by keyboard command with the Find dialog window open and input focus in the editing window, an unsuccessful search will result in input focus being changed to the Find window.  Presumably, the user would want to conduct a different search at this point?

*Disclaimer:*  It is Notepad++'s design intention to fulfill some basic searching/replacing capability.  As such, Notepad++ searching is not infinitely flexible and capable of meeting all needs.  For such power needs, please turn to external tools, some of which integrate well with Notepad++.  Integrating well means that after such tools produce results, they can tell Notepad++ which files to open and which line and column numbers to move the caret to, in order to work with matched results.  Examples of such power file/text searching tools might be:  "GrepWin", "PowerGREP", "FileSeek", "Everything" and many others.

#### "Find result" Window

After running one or more **Find All in ...** commands, a new **Find result** window appears, and within it is placed a **Find result** tab.  The **Find result** window may be opened and/or given input focus by using the menu command **Search > Search Results Window** or the F7 keyboard shortcut.  *Note:*  That menu command will seem to not do anything if there haven't been any **Find All in ...** commands run since Notepad++ was opened.

*Definition:*  **Find All in ...** commands include:

|*Which **Find All in ...** command*| *Find window owner tab*|
|-----------------------------------|------------------------|
|Find All in All Opened Documents   | **Find**               |
|Find All in Current Document       | **Find**               |
|Find All                           | **Find in Files**      |

The **Find result** window by default appears docked at the bottom of the Notepad++ main window.  Like other such windows, it can be moved or even be a free-floating window.

From **Find All in ...** searches, three types of sections are added to the Find result window.  First is a line describing what was searched for, how many total matches (known as "hits") occurred (this is also shown in the title bar for the window, for the most recently-occurring search), and how many files had matches.  Second is a line that shows the filename with the matches and the count of matches for that file (this type will be repeated if the search found multiple files with matches).  Last comes the details about the matches found, including line number and the line contents with the matched text emphasized.  The default emphasis is red text on a yellow background, but this may be changed in the Style Configurator's "Search result" Language area.

When Notepad++ populates the **Find result** window, it does so using one line for each match found by the search.  Note that this can and often does end with the same source file line being repeated multiple times in the output.  An example of this would be if you are searching for "the" in the line of text that reads "Now is the time for all good men to come to the aid of their country"; the **Find result** window would list the line twice, once with the word "the" called out in red text with a yellow background, and a second time with "the" in "their" similarly emphasized.

When the **Find result** window has input focus, the currently active line has a different background color, much like how the main editor window does by default.  Unlike the main editor window, where the current-line background feature may be turned off, the **Find result** window always has a background highlight for its active line.

Use the up and down arrows to navigate within the **Find result** window when it has input focus.  Double-clicking with the mouse or hitting ENTER when input focus is on a specific match will move the editor window to that match and cause its text to be selected.

Other ways to navigate back to an editor window via the **Find result** window matches include the **Search** menu items **Next Search Result** (keyboard: F4) and **Previous Search Result** (keyboard: Shift+F4).  These can be invoked regardless of where input focus is in Notepad++.

The *Delete* key can be used to delete individual results, file matches or whole search matches in the **Find result** window, depending on which type of line is active when the key is pressed.  As the result history is hierarchical, that is, tree-like, pressing *Delete* when in a higher-level element of the tree removes that whole branch.  Thus:

|*Pressing Delete when **Find result** active line starts with...*| *What is removed*                                                                              |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------|
|the text: "Search"                                               | that "Search" line, all pathname lines under it, and all "Line" lines under the pathname lines |
|a pathname                                                       | that pathname line and all "Line" lines under it                                                |
|the text: "Line"                                                 | only that line                                                                                 |

Multiple searches will be listed under separate headers, which are "foldable", so you can hide or unhide results from previous searches.  When you run a new search, previous searches are folded closed.

If the source file lines are judged by Notepad++ to be too long when they are copied to be placed in the **Find result** window, they will be truncated and **...** will be added at the end.  In this case, matched text occurring in the line after the **...** position will not be emphasized.  However, using a method to return to the editor window (e.g. pressing Enter) will result in the correct selection of matching text there.  The length limit is 1024 characters; this includes the match line number information and other formatting.

If a search is conducted such that a match which spans two or more lines occurs, only the contents of the *FIRST* line of that match will be copied into the **Find result** window.  However, using a method to return to the editor window (e.g. pressing Enter) will result in the correct selection of multi-line matching text there.

##### *RightClick* commands in the client area of a **Find result** window's tab

###### Copying text from the **Find result** window

There are two ways to copy text from the **Find result** window:  Make sure input focus is in the **Find result** window by selecting some text and use the keyboard's *Ctrl+c* or the mouse's *RightClick* to invoke the context menu and select **Copy** from that.  Somewhat surprisingly, these two copy mechanisms produce vastly different results.

When *Ctrl+c* is used, it is very straightforward:  Whatever text is selected is copied, simply and exactly.  If nothing is selected when *Ctrl+c* is pressed (and the **Find result** window has input focus of course), nothing is copied to the clipboard.

When the mouse's *RightClick* > **Copy** is used, however, it is more complicated.  Basically, this type of copy can be thought of as a "Copy Special".  It copies the text of ENTIRE LINES from the results, WITHOUT any search information (called "metadata") being included in what is copied.

Here's a more detailed description of what happens for *RightClick* > **Copy**:

First, if the user makes a selection of text in the **Find result** window and copies it this way, only the lines of text touched (even partially) by the selection is part of the copy.  All other text with information about the search (pathname, line number, etc.) is *not* copied, even if part of the selection.  Secondly, if there is no active selection when the *RightClick* > **Copy** is invoked, results depend upon what exactly is under the cursor during the *RightClick*:

| *RightClick* item     | What gets copied when *RightClick* > **Copy** is run |
|-----------------------|-----------------------------------------------------------------------------------------------------|
|a line with line # info|the entire line of the *RightClick* but without line number text                                     |
|a pathname header line |all the lines for that single file without pathname or line number text                              |
|a "search" header line |all the lines for that search (1 or more files) without search header, pathname or line number text  |

*Tip*:  It is possible to select and copy a rectangular selection of data from the **Find result** window.  This is done using the usual Shift+Alt+arrow keys or by holding Alt+LeftClick and dragging with the mouse.  This is really only practical when using the *Ctrl+c* method of copying; *RightClick* > **Copy** only copies entire lines, and this copy will only copy the single full line at the top/bottom of the column block.

###### Other commands

There are some commands that don't need a lot of explanation; these are:

* **Collapse all**
* **Uncollapse all**
* **Select all**
* **Clear all**
* **Open all**

The **Find result** window/tab accumulates results from every **Find All in ...** search the user does; the results from old searches remain until the user removes them.  Individual results can be deleted with the *Delete* key, or all previous results can be deleted by invoking **Clear all**.  Stale results can be removed to reduce visual clutter, or when it is desired that a follow-on action should not be affected by old results.  An example of this would be the **Open all** command which opens *all* files listed in the **Find result** tab that have previously had hits.  If the search history in **Find result** is really long, it may not be desirable to open all files listed there, so using **Clear all** before doing some new searches (with the intent to **Open all** afterwards) may be the thing to do.

The **Select all** command is self-explanatory:  *ALL* text in the **Find result** tab will be selected

The contents of the **Find result** tab are in the form of a tree.  When Notepad++ adds to the result history, it does so in an uncollapsed way, that is, the user can see all of the information from the recently-added search.  However, before adding new results, Notepad++ will collapse all previous result data; perhaps it is deciding that the most-recent search is the most important?

The user can collapse/uncollapse "branches" of this tree.  To collapse, click with the mouse on the little box symbol with an interior `-`, found to the left of each line.  After doing so, that part of the tree will be collapsed (removed from view) and the first line of the branch (remaining visible) will then show a `+` in the box symbol.  To uncollapse an individual item that has previously been collapsed (either by the user or by Notepad++'s automatic mechanism), simply click the box symbol with the `+`.  That branch will then be expanded and again shown.

The **Collapse all** and **Uncollapse all** commands perform the corresponding actions on *ALL* elements of the entire result history in the **Find result** window at once.  Perhaps a better name for **Uncollapse all** would have been the more-conventional "Expand all"?

###### Searching in previously-found results (secondary searching)

Perhaps you have done a search and your results are in a tab in the **Find result** window.  Now you'd like to conduct a search but with a scope of only the files that have previous matches.  Or maybe you want to look only in the *lines* matched by previous searches, not only the matched files, tightening the search criteria even more.  Can you do this sort of second-level searching with Notepad++?  Yes, by *RightClick*ing the **Find result** window client area and selecting **Find in this found results...**.

Selecting **Find in this found results...** will cause a window to pop up, and this window looks much like the standard **Find** window, but is stripped down a bit.  Once you input your search parameters and choose **Find All**, a *new* **Find result** tab will open (in the **Find result** window) with the results of the "refined" search.

The popup window has a parameter not available in the searches described earlier:  **☐ Search only in found lines**.  Checking this box limits the search to lines that appear in matched files in the parent **Find result** window.  Unchecking the box will cause the new search to examine previously matched files in their entirety.  When a search has been limited to previously-found lines, its results will indicate this by using this type of output:  `Search "___" (__ hits in __ files - Line Filter Mode: only display the filtered results)` as opposed to the normally seen:  `Search "___" (__ hits in __ files)`

*Tip:*  Use the *RightClick* option **Clear all** to limit the scope of these types of searches (before invoking the secondary search!) -- remember: a **Find in this found results...** search will look in files matched by ALL previous searches whose results are still present in the parent **Find result** tab.

*Tip:*  Since the newly opened **Find result** window also has a *RightClick* menu, you may do another **Find in this found results...** based upon the new results, focusing your search for some bit of data even more.  This type of refinement may be repeated as much as desired.  [Note that the title bar of the window does *not* show the hit count of the currently active tab, but rather shows the hit count of the *first* **Find result** tab of the window.]

*Note:*  The commands that switch input focus to the **Find result** window always activate the *first* **Find result** tab, not any additional **Find result** tabs that may have been created.

*Note:*  The contents of the **Find result** window are discarded upon Notepad++ shutdown.  If there is data of importance there it should be copied, using one of the methods above, and saved in a more-permanent location.

### Find in Files

Find in Files allows both finding and replacing. You can choose an extension filter (**Filters:**), the containing folder (**Directory:**), and whether to also process hidden files or subfolders. 

The **Filters** list is a space-separated list of wildcard expressions that cmd.exe can understand, like `*.doc foo.*`.   If you have a blank filter, it is implied to be `*.*`.  As of Notepad++ v7.8.2, you can also exclude certain file patterns by prefixing the filter with a `!`; for example, **Filters:  `!*.bin *.*`** will exclude files matching `*.bin` from the search results, but include any other filename; if you have at least one exclusion in your filter, you need to have at least one inclusion in your filter, otherwise it excludes files from the 0 matched inclusion files, resulting in no files matched, which probably isn't what you want.  Please also note that the PathMatchSpec() Windows API is being used, as its behavior departs from cmd.exe wildcard parsing sometimes.

The **Directory** is the containing folder for where to search.  It has three options that affect it's behavior: 
* **☐ Follow current doc** ⇒ if enabled, it will default to searching the folder that contains the current active document (this sets the `fifFolderFollowsDoc` in `config.xml`).
* **☐ In all sub-folders** ⇒ if enabled, it will recursively search sub-folders of the given folder.
* **☐ In hidden folders** ⇒ if enabled, it will search hidden sub-folders as well as normally-visible sub-folders.

### Highlighting and bookmarking

The Mark tab from the Find/Replace dialog will perform searches similar to the Find tab, in the current document or selection:

* When **Bookmark line** is checked, a bookmark is dropped on each line where an individual hit occurs.  In the case where an individual hit spans multiple lines, each line in the span will receive the bookmark.

* Otherwise, the matched pattern is highlighted according to the Settings -&gt; Styler Configurator -&gt; Global Styles , Find Mark Style setting.  (See also [Style Configurator](../preferences/#style-configurator).)

In either case, the Mark All button will perform the marking.

To control whether highlighting or bookmarks accumulate over successive searches, use the **Clear all marks** button to remove marks, or check **Purge for each search** for this action to be performed automatically on each search.  When the **Clear all marks** button is pressed, any marked text will have the marking background coloring removed; additionally, any bookmarks previously set will be removed if the **Bookmark line** checkbox is checked.

Highlighting is also available in Incremental search, and the style setting is Settings -&gt; Styler Configurator -&gt; Global Styles , Incremental Highlighting instead.

## Dialog-free search/mark actions

### Searching

The following commands, available through the Search menu or keyboard shortcuts, perform a search without invoking a dialog, because they reuse the previous pattern or find it in the current document:

* Find Next / Find Previous repeat the current search, either down or up.

* Go to Next Found / Go to Previous found jump to a place recorded on the search result window. You can use  Search  -&gt;  Found Results Window to toggle the visibility of this window, which allows a more visual navigation.

* Find (volatile) Next / Find (volatile) Previous attempt to find the word the caret is in, or the selected text, down or up. Note that this does not interfere with ordinary search - it is really volatile.

* Select and Find Next / Select and Find Previous select the word the caret is in if no text is selected, and then find the next/previous occurrence of selected text. This will also set this word as the current search target, so that Find Next or Find Previous will lose its previous target and look for the selected word again.  However, other search parameters, e.g. Match case or Wrap around, will remain in force as last set.

Please note that "selected" refers to the contents of the active stream selection. This also applies to the selected part of the caret line when a rectangular area is selected.

### Highlighting

Use the Mark All or Unmark All submenus of the Search menu to mark all occurrences of the word the caret is in. The settings for each of the 5 available styles are Settings -&gt; Styler Configurator -&gt; Global Styles , Mark style #. You can also cause all occurrences of the word at the caret to get dynamically highlighted if you enable Smart Highlighting; the mark style then is Settings -&gt; Styler Configurator -&gt; Global Styles , Smart Highlighting. You may choose there whether the matching should be sensitive to case.
You enable smart highlighting through Settings -&gt; Preferences -&gt; MISC -&gt; Smart highlighting, Enable smart highlighting. By default, the highlighting is case insensitive, which may be a problem sometimes. Then just toggle Settings -&gt; Preferences -&gt; MISC -&gt; Highlighting is case sensitive on.  (See also [Style Configurator](../preferences/#style-configurator) and [Preferences](../preferences/#preferences).)


## Finding characters in a specific range

While regular expressions provide for specifying a range (or multiple ranges) of characters using the  [start....end] pattern, this is sometimes awkward when the start or end character isn't easily typed in. Notepad++ provides a specific dialog, available using Find -&gt; Find Characters in range....

A custom range of characters can be asked for, as well as either half of the 0..255 range: ASCII covers the lower half, non-ASCII covers the upper part. Note that entries should be in decimal notation, and that values above 255 are not handled in a useful way.

Search may proceed up or down, and optionally wraps around. Hit Find to perform it. Close dialog using either the dedicated button, the close button on the title bar or the Esc key.

## Incremental Search

Incremental search is similar to the searching capabilities found in your favorite web browser (like Firefox or Chrome).  You can launch it from the **Search > Incremental Search** menu, or the keyboard shortcut (Ctrl+Alt+I).

This command will show a small region at the bottom of the Notepad++, which has a few simple features.

* The **✗** allows you to close out of Incremental Search.
* The **Find** box is where you type your literal search term.
* The **<** and **>** buttons navigate backward and forward through the search results (wrapping around when it reaches the end or start of the document).
* If the **☐ Highlight all** checkbox is not checked, it will only highlight the current match; if it is checked, all matches will be highlighted.
* If the **☐ Match case** checkbox is checked, the results will only match if case is exactly the same, otherwise case doesn't matter.
* To the right of those checkboxes, a message about the results will occur: either the number of matches, a message that indicates that you've wrapped around to the top or bottom of the document, or "Phrase not found" if there are no matches.  When there are no matches, the **Find** box also changes color.

## Comparison between "Select and Find Next" and "Find Next (Volatile)"

This section is aimed to clear the confusion about these 2 similar commands.

Both commands "Select and Find Next" (Ctrl+F3) and "Find Next (Volatile)" (Ctrl+Alt+F3) does the same thing (almost): select the current word (on which caret is) then jump to the next occurrence.

However, there is a slight difference between these 2 commands: "Select and Find Next" remembers the searched word, "Find Next (Volatile)" does not.

Here's an example:

If you do "Select and Find Next" command for word word1 then you can always use "Find Next" command (F3) or "Find Previous" command (Shift+F3) to search word1, even the caret is on word2.

If your caret is on word word2, "Find Next (Volatile)" will search the next word2. Now if you move your caret on word word3 and do "Find Next (Volatile)", it will search next word3, and word2 is forgotten.

## Extended Search Mode

In extended mode, these escape sequences (a backslash followed by a single character and optional material) have special meaning, and will not be interpreted literally.

* `\n`:  the Line Feed control character LF (ASCII 0x0A)
* `\r`:  The Carriage Return control character CR (ASCII 0x0D)
* `\t`:  the TAB control character (ASCII 0x09)
* `\0`:  the NUL control character (ASCII 0x00)
* `\\`:  the literal backalash character (ASCII 0x05C)
* `\b`:  the binary representation of a byte, made of 8 digits which are either 1's or 0's. †
* `\o`:  the octal representation of a byte, made of 3 digits in the 0-7 range
* `\d`:  the decimal representation of a byte, made of 3 digits in the 0-9 range
* `\x`:  the hexadecimal representation of a byte, made of 2 digits in the 0-9, A-F/a-f range.
* `\u`:  The hexadecimal representation of a two byte character, made of 4 digits in the 0-9, A-F/a-f range. In Unicode builds, finds a Unicode character (for instance, `\u2020` matches the `†` char, in an UTF-8 encoded file). In ANSI builds, finds characters requiring two bytes, like in the Shift-JIS encoding. †

†NOTE: While some of these Extended Search Mode escape sequences look like regular expression escape sequences, they are not identical.  Ones marked with † are different from or not available in regular expressions.

## Regular Expressions

Notepad++ regular expressions use the Boost regular expression library v1.70, which is based on PCRE (Perl Compatible Regular Expression) syntax, only departing from it in very minor ways. Complete documentation on the precise implementation is to be found on the Boost pages for [search syntax](https://www.boost.org/doc/libs/1_70_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html) and [replacement syntax](https://www.boost.org/doc/libs/1_70_0/libs/regex/doc/html/boost_regex/format/boost_format_syntax.html)

The Notepad++ Community has a [FAQ on other resources for regular expressions](https://notepad-plus-plus.org/community/topic/15765/faq-desk-where-to-find-regex-documentation).

### Regex Special Characters

In a regular expression (shortened into regex throughout), special characters interpreted are:

#### Single-character matches

* `.` or `\C` ⇒ Matches any character. If you check the box which says **. matches newline**, the dot match any character, including newline sequences.  With the option unchecked, then `.` will only match characters within a line, and not the newline sequences (`\r` or `\n`).

* `\X` ⇒ Matches a single non-combining character followed by any number of combining characters. This is useful if you have a Unicode encoded text with accents as separate, combining characters.  For example, the letter `ǭ̳̚`, with four combining characters after the `o`, can be found either with the regex `(?-i)o\x{0304}\x{0328}\x{031a}\x{0333}` or with the shorter regex `\X`.

* `\☒`  ⇒ This allows you to search for a literal character ☒, which would otherwise have a special meaning as a regex meta-character, rather than treating it as a regex meta-character. For example, `\[` would be interpreted as literal `[` and not as the start of a character set. Adding the backslash (this is called _escaping_) can work the other way round, too, as it makes special a character that otherwise isn't: for instance, `\d` stands for "a digit", while `d` is just an ordinary letter.  (Please note: `☒` here was chosen as a placeholder for the character you want to escape.)


##### Non ASCII characters

* `\xℕℕ` ⇒ Specify a single character with code ℕℕ, where each ℕ is a hexadecimal digit. What this stands for depends on the text encoding. For instance, `\xE9` may match an `é` or a `θ` depending on the code page in an ANSI encoded document.

* `\x{ℕℕℕℕ}` ⇒ Like above, but matches a full 16-bit Unicode character. If the document is ANSI encoded, this construct is invalid.

* `\0ℕℕℕ` ⇒ A single byte character whose code in octal is ℕℕℕ, where each ℕ is an octal digit.  (That's the number `0`, not the letter `o` or `O`.)  For example, `\0101` looks for the letter `A`, as 101 in octal is 65 in decimal.

*  `[[.`_col_`.]]` ⇒ The character the _col_ "[collating sequence](https://www.boost.org/doc/libs/1_70_0/libs/regex/doc/html/boost_regex/syntax/collating_names.html)" stands for. For instance, in Spanish, `ch` is a single letter, though it is written using two characters. That letter would be represented as `[[.ch.]]`. This trick also works with symbolic names of control characters, like `[[.BEL.]]` for the character of code 0x07. See also the discussion on character ranges.



##### Control characters

* `\a` ⇒ The BEL control character 0x07 (alarm).

* `\b` ⇒ The BS control character 0x08 (backspace). This is only allowed inside a character class definition. Otherwise, this means "a word boundary".

* `\e` ⇒ The ESC control character 0x1B.

* `\f` ⇒ The FF control character 0x0C (form feed).

* `\n` ⇒ The LF control character 0x0A (line feed). This is the regular end of line under Unix systems.

* `\r` ⇒ The CR control character 0x0D (carriage return). This is part of the DOS/Windows end of line sequence CR-LF, and was the EOL character on Mac 9 and earlier. OSX and later versions use `\n`.

* `\R` ⇒ Any newline sequence.  Specifically, the atomic group `(?>\r\n|\n|\x0B|\f|\r|\x85|\x{2028}|\x{2029})`.

* `\t` ⇒ The TAB control character 0x09 (tab, or hard tab, horizontal tab).

* `\c☒` ⇒ The control character obtained from character ☒ by stripping all but its 6 lowest order bits. For instance, `\c1`, `\cA` and `\ca` all stand for the SOH control character 0x01.  You can think of this as "\c means ctrl", so `\cA` is the character you would get from hitting Ctrl+A in a terminal.

#### Ranges or kinds of characters

##### Character Classes

* `[`_set_`]`  ⇒ This indicates a _set_ of characters, for example, `[abc]` means any of the literal characters `a`, `b` or `c`. You can also use ranges by doing a hyphen between characters, for example `[a-z]` for any character from `a` to `z`.  You can use a collating sequence in character ranges, like in `[[.ch.]-[.ll.]]` (these are collating sequence in Spanish).

* `[^`_set_`]`  ⇒ The complement of the characters in the _set_. For example, `[^A-Za-z]` means any character except an alphabetic character.  Care should be taken with a complement list, as regular expressions are always multi-line, and hence `[^ABC]*` will match until the first `A`, `B` or `C` (or `a`, `b` or `c` if match case is off), including any newline characters. To confine the search to a single line, include the newline characters in the exception list, e.g. `[^ABC\r\n]`.

* `[:`_name_`:]` ⇒ The whole character class named _name_, which must be part of a character class.  For many, there is also a single-letter short class name.

    | short | full name      | description | equivalent character class |
    |:-----:|:--------------:|:------------|----------------------------|
    |       | alnum          | letters and digits | |
    |       | alpha          | letters | |
    | h     | blank          | spacing which is not a line terminator | `[\t\x20\xA0]`|
    |       | cntrl          | control characters | `[\x00-\x1F\x7F\x81\x8D\x8F\x90\x9D]` |
    | d     | digit          | digits | |
    |       | graph          | graphical character, so essentially any character except for control chars, `\0x7F`, `\x80` | |
    | l     | lower          | lowercase letters | |
    |       | print          | printable characters | `[\s[:graph:]]` |
    |       | punct          | punctuation characters | `[!"#$%&'()*+,\-./:;<=>?@\[\\\]^_`{|}~]` <!-- ` -->|
    | s     | space          | whitespace (word or line separator) | `[\t\n\x0B\f\r\x20\x85\xA0\x{2028}\x{2029}]` |
    | u     | upper          | uppercase letters |  |
    |       | unicode        | any character with code point above 255 | `[\x{0100}-\x{FFFF}]` |
    | w     | word           | word characters | `[_\d\l\u]` |
    |       | xdigit         | hexadecimal digits | `[0-9A-Fa-f]` |

    Note that letters include any unicode letters (ASCII letters, accented letters, and letters from a variety of other writing systems); digits include ASCII numeric digits, and anything else in Unicode that's classified as a digit (like superscript numbers ¹²³...).

    Note that those character class names may be written in upper or lower case without changing the results.  So `[:alnum:]` is the same as `[:ALNUM:]` or the mixed-case `[:AlNuM:]`.

##### Character Properties

These properties behave similar to named character classes, but cannot be contained inside a character class.

*  `\p☒` or `\p{`_name_`}` ⇒ Same as `[[:☒:]]` or `[[:`_name_`:]]`, where ☒ stands for one of the short names from the table above, and _name_ stands for one of the full names from above. For instance, `\pd` and `\p{digit}` both stand for a digit, just like the escape sequence `\d` does.

*  `P☒` or `\P{`_name_`}` ⇒ Same as `[^[:☒:]]` or `[^[:`_name_`:]]` (not belonging to the class _name_).

##### Character escape sequences

These single-letter escape sequences are each equivalent to a class from above.  The lower-case escape sequence means it matches that class; the upper-case escape sequence means it matches the negative of that class.  (Unlike the properties, these can be used both inside or outside of a character class.)

| Description]     | Escape Sequence | Positive Class | Negative Escape Sequence | Negative Class |
|:-----------------|:----------------|:---------------|:-------------------------|:---------------|
| digits           | `\d`            | `[[:digit:]]`  | `\D`                     | `[^[:digit:]]` |
| lowercase        | `\l`            | `[[:lower:]]`  | `\L`                     | `[^[:lower:]]` |
| space chars      | `\s`            | `[[:space:]]`  | `\S`                     | `[^[:space:]]` |
| uppercase        | `\u`            | `[[:upper:]]`  | `\U`                     | `[^[:upper:]]` |
| word chars       | `\w`            | `[[:word:]]`   | `\W`                     | `[^[:word:]]`  |
| horizontal space | `\h`            | `[[:blank:]]`  | `\H`                     | `[^[:blank:]]` |
| vertical space   | `\v`            | see below      | `\V`                     |                |

> Vertical space: This encompasses the LF, VT, FF, CR , NEL control characters and the LS and PS format characters : 0x000A (line feed), 0x000B (vertical tabulation), 0x000C (form feed), 0x000D (carriage return), 0x0085 (next line), 0x2028 (line separator) and 0x2029 (paragraph separator).  There isn't a named class which matches.

##### Equivalence Classes

* `[[=`_char_`=]]` ⇒ All characters that differ from _char_ by case, accent or similar alteration only. For example `[[=a=]]` matches any of the characters: `a`, `À`, `Á`, `Â`, `Ã`, `Ä`, `Å`, `A`, `à`, `á`, `â`, `ã`, `ä` and `å`.


#### Multiplying operators

* `+`  ⇒ This matches 1 or more instances of the previous character, as many as it can. For example, `Sa+m` matches `Sam`, `Saam`, `Saaam`, and so on.  `[aeiou]+` matches consecutive strings of vowels.

* `*`  ⇒ This matches 0 or more instances of the previous character, as many as it can. For example, `Sa*m` matches `Sm`, `Sam`, `Saam`, and so on.

* `?` ⇒ Zero or one of the last character. Thus `Sa?m` matches `Sm` and `Sam`, but not `Saam`.

* `*?` ⇒ Zero or more of the previous group, but minimally: the shortest matching string, rather than the longest string as with the "greedy" operator. Thus, `m.*?o` applied to the text `margin-bottom: 0;` will match `margin-bo`, whereas `m.*o` will match `margin-botto`.

* `+?` ⇒ One or more of the previous group, but minimally.

* `{ℕ}` ⇒ Matches ℕ copies of the element it applies to (where ℕ is any decimal number).

* `{ℕ,}` ⇒ Matches ℕ or more copies of the element it applies to.

* `{ℕ,ℙ}` ⇒ Matches ℕ to ℙ copies of the element it applies to, as much it can (where ℙ ≥ ℕ).

* `{ℕ,}?` or `{ℕ,ℙ}?` ⇒ Like the above, but mimimally.

* `*+` or `?+` or `++` or `{ℕ,}+` or `{ℕ,ℙ}+` ⇒ These so called "possessive" variants of greedy repeat marks do not backtrack. This allows failures to be reported much earlier, which can boost performance significantly. But they will eliminate matches that would require backtracking to be found. As an example:

    When regex `“.*”` is run against the text `“abc”x` :

        “  matches “
        .* matches abc”x
        ”  cannot match $ ( End of line ) => Backtracking

        “  matches “
        .* matches abc”
        ”  cannot match letter x => Backtracking

        “  matches “
        .* matches abc
        ”  matches ” => 1 overall match “abc”

    When regex `“.*+”`, with a possessive quantifier, is run against the text `“abc”x` :

        “   matches “
        .*+ matches abc”x ( catches all remaining characters )
        ” cannot match $ ( End of line )

    Notice there is no match at all for the possive version, because the possessive repeat factor prevents from backtracking to a possible solution


#### Anchors
Anchors match a zero-length position in the line, rather than a particular character.


* `^` ⇒ This matches the start of a line (except when used inside a set, see above).

* `$`  ⇒ This matches the end of a line.

* `\<`  ⇒ This matches the start of a word using Scintilla's definitions of words.

* `\>`  ⇒ This matches the end of a word using Scintilla's definition of words.

* `\b` ⇒ Matches either the start or end of a word.

* `\B` ⇒ Not a word boundary. It represents any location between two word characters or between two non-word characters.

* `\A` or `\'` ⇒ The start of the matching string.

* `\z` or `` \` `` <!-- ` --> ⇒ The end of the matching string.

* `\Z` ⇒ Matches like `\z` with an optional sequence of newlines before it. This is equivalent to `(?=\v*\z)`, which departs from the traditional Perl meaning for this escape.

* `\G` ⇒ This "Continuation Escape" matches the end of the previous match.  In **Find All** or **Replace All** circumstances, this will allow you to anchor your next match at the end of the previous match.  If it is the first match of a **Find All** or **Replace All**, and any time you use a single **Find Next** or **Replace**, the "end of previous match" is defined to be the start of the search area -- the beginning of the document, or the current cursor position, or the start of the highlighted text.



#### Groups

* `(`_subset_`)` ⇒ Parentheses mark a _subset_ of the regular expression. The string matched by the contents of the parentheses (indicated by _subset_ in this example) can be re-used as a backreference or as part of a replace operation; see [Substitutions](#substitutions), below. Groups may be nested.

* `(?<name>`_subset_`)` or `(?'name'`_subset_`)` or `(?(name)`_subset_`)` ⇒ Names the value matched by _subset_ as group _name_.  Please note that group names are case-sensitive.

* `\gℕ`, `\g{ℕ}`, `\g<ℕ>`, `\g'ℕ'`, `\kℕ`, `\k{ℕ}`, `\k<ℕ>` or `\k'ℕ'` ⇒ The ℕ-th subexpression, aka parenthesized group. Using a form other than `\gℕ*` and `\kℕ` has some small benefits, like ℕ being more than 9, or disambiguating when ℕ might be followed by digits. When ℕ is negative, groups are counted backwards, so that `\g{-1}` is the last matched group, and `\g{-2}` is the next-to-last matched group.

    Please, note the difference between absolute and relative backreferences. For instance, an exact four-letters word palindrome can be matched with :

    * the regex `(?-i)\b(\w)(\w)\g{2}\g{1}\b`, when using absolute coordinates

    * the regex `(?-i)\b(\w)(\w)\g{-1}\g{-2}\b`, when using relative coordinates


* `\g{name}`, `\g<name>`, `\g'name'`, `\k{name}`, `\k<name>` or `\k'name'` ⇒ The string matching the subexpression named _name_.

* `\ℕ` ⇒ _Backreference:_ `\1` matches an additional occurrence of a text matched by an earlier part of the regex. Example: This regular expression:  `([Cc][Aa][Ss][Ee]).*\1` would match a line such as `Case matches Case` but not `Case doesn't match cASE`.  A regex can have multiple subgroups, so `\2`, `\3`, etc can be used to match others (numbers advance left to right with the opening parenthesis of the group). So `\ℕ` is a synonym for `\gℕ`, but only for `\1` through `\9`: `\10` means "the contents of the first group `\1` followed by the literal character `0`, _not_ "the contents of the 10th group".


#### Readability enhancements

* `(?:`_subset_`)` ⇒ A grouping construct for the _subset_ expression that doesn't count as a subexpression (doesn't get numbered or named), but just groups things for easier reading of the regex, or for using a quantified amount of that group, with a quantifier located right after that grouping construct.

* `(?#`_comment_`)` ⇒ Comments. The whole group is for humans only and will be ignored in matching text.

Using the x flag modifier (see section below) is also a good way to improve readability in complex regular expressions.


#### Search modifiers
The following constructs control how matches condition other matches, or otherwise alter the way search is performed.

* `\Q` ⇒ Starts verbatim mode (Perl calls it "quoted"). In this mode, all characters are treated as-is, the only exception being the `\E` end verbatim mode sequence.

* `\E` ⇒ Ends verbatim mode. Thus, `\Q\*+\Ea+` matches `\*+aaaa`.

* `(?enable-disable)` or `(?enable-disable:subpattern)` ⇒ There are four flags, described below, which can be applied to a regex or subgroup.  The _enable_ term can be made up of 0-4 of the flags described below; the _disable_ term can be made up of 0-4 of the flags described below. Any flags in _enable_ will be enabled (turned on); any flags in _disable_ will be disabled (turned off).  (Remember, it does not make sense to include the same flag in both the _enable_ and _disable_ terms.)  If there are no _disable_ flags, the `-` is not necessary; if there are no _enable_ flags, then the `-` will come immediately after the `?`: `(?-...)`.  If there is a subpattern, then the flags only apply for the contents of the subpattern; without a subpattern, there is no `:` separator, and the flags apply for the remainder of the current regex, or until the next flags are set.

    * `i` ⇒ case insensitive (default: set by **☐ Match case** dialog option)
    * `m` ⇒ ^ and $ match embedded newlines (default: on)
    * `s` ⇒ dot matches newline (default: as per **☐ . matches newline** dialog option)
    * `x` ⇒ Ignore non-escaped whitespace in regex (default: off).  Any whitespace that you need to match must be escaped

    Examples:

    * `blah(?i-s)foobar` ⇒ enables case insensitivity and disables dot-matches-newline for the rest of the regular expression: thus expression `blah` is run under the default rules (set by the dialog), whereas expression `foobar` will be case-insensitive and dot will not match newline.
    * `(?i-s:subpattern)` ⇒ enables case insensitivity and disables dot-matches-newline, but just for the `subpattern`
    * `(?-i)caseSensitive(?i)cAsE inSenSitive` ⇒ disables case insensitivity (makes it case-sensitive) for the portion of the regex indicated by `caseSensitive`, and re-enables case-insensitive matching for the rest of the regex
    * `(?m:justHere)` ⇒ `^` and `$` will match on embedded newlines, but just for the contents of this subgroup `justHere`
    * `(?x)` ⇒ Allow extra whitespace in the expression for the remainder of the regex

* `(?|expression)` ⇒ If an alternation expression has parenthetical subexpressions in some of its alternatives, you may want the subexpression counter not to be altered by what is in the other branches of the alternation. This construct will just do that.

    For example, you get the following subexpression counter values:

~~~
#      before  ---------------branch-reset----------- after
/ (?x) ( a )  (?| x ( y ) z | (p (q) r) | (t) u (v) ) ( z )
#      1            2         2  3        2     3     4
~~~


    Without the branch reset, `(y)` would be group #3, and `(p(q)r)` would be group #4, and `(t)` would be group #5. With the branch reset, they both report as group #2.

### Control flow
Normally, a regular expression parses from left to right linearly. But you may need to change this behavior.


* `|` ⇒ The alternation operator, which allows matching either of a number of options.  For example, `one|two|three` will match either of `one`, `two` or `three`. Matches are attempted from left to right. Use `(?:)` to match an empty string in such a construct.

*  `(?ℕ)` ⇒ Refers to ℕth subexpression. If ℕ is negative, it will use the ℕth subexpression from the end.

    Please, note the difference between subexpressions and back-references. For instance, using a similar structure to the one, when searching for a four-letters word being a palindrome, this time, both regexes just find a four-letters word, because each subexpression, signed or not, refers to the regex itself, enclosed in each group and NOT to the present value of each group!

    * the regex `(?-i)\b(\w)(\w)(?2)(?1)\b` find a four-letter word, when using absolute coordinates

    * the regex `(?-i)\b(\w)(\w)(?-1)(?-2)\b` find a four-letter word, when using relative coordinates

    Actually, these two regexes could be simplified to `(?-i)\b(\w)(\w)\w\w\b`, assuming that group 1 and 2 are still needed in replacement

*  `(?0)` or `(?R)` ⇒ Backtrack to start of pattern.

*  `(&name)` or `(?P>name)` ⇒ Backtrack to subexpression named _name_.

    * If a non-signed subexpression is located OUTSIDE the parentheses of the group to which it refers, it is called a subroutine call

    * If a non-signed subexpression is located INSIDE the parentheses of the group to which it refers, it is called a recursive call

*  `(?(assertion)YesPattern|NoPattern)` ⇒ If the _assertion_ is true, then _YesPattern_ will be used for matching the text; if the _assertion_ is false, then _NoPattern_ will be used for matching the text.

    _YesPattern_ and _NoPattern_ are any valid regex patterns.

    The _assertion_ will always be inside parentheses; this is emphasized by including the parentheses in the list of supported _assertion_ syntax, below:

    * `(ℕ)` ⇒ true if ℕth unnamed group was previously defined

    * `(?<name>)` or `(?'name')` ⇒ true if group called _name_ was previously defined

    *  `(?=lookahead)` ⇒ true if the _lookahead_ expression matches

    *  `(?!lookahead)` ⇒ true if the _lookahead_ expression does not match

    *  `(?<=lookbehind)` ⇒ true if the _lookbehind_ expression matches

    *  `(?<!lookbehind)` ⇒ true if the _lookbehind_ expression does not match

    *  `(?(R))` ⇒ true if inside a recursion

    *  `(?(Rℕ)` ⇒ true if in a recursion to subexpression numbered ℕ

    *  `(?(R&name))` ⇒ true if in a recursion to named subexpression _name_

    Note: These are all still _inside_ the conditional expression.

    Do not confuse the assertions that control a conditional expression (here) with the assertions that are part of the pattern matching (the [Assertions](#assertions) section, below).  Here, if the assertion is used to decide which expression is used; below, the assertion decides whether the pattern is matching or not.

    Note: PCRE doesn't treat recursion expressions like Perl does:

    > In PCRE (like Python, but unlike Perl), a recursive subpattern call  is
always treated as an atomic group. That is, once it has matched some of
the subject string, it is never re-entered, even if it contains untried
alternatives  and  there  is a subsequent matching failure.

*  `\K` ⇒ Resets matched text at this point. For instance, matching `foo\Kbar` will not match `bar`. It will match `foobar`, but will pretend that only `bar` matches. Useful when you wish to replace only the tail of a matched subject and groups are clumsy to formulate.

    It is also useful if you would need a look-behind assertion which would contain a non-fixed length pattern (see further on). As variable-length lookbehind is not allowed in Boost's regular expressions, you can use the `\K` syntax, instead. For instance, the non-allowed syntax `(?-i)(?<=\d+)abc` can be replaced with the correct syntax `(?-i)\d+\Kabc` which matches the exact string `abc` only if preceded by, at least, one digit.

#### Assertions
These special groups consume no characters. Their successful matching counts, but when they are done, matching starts over where it left.

* `(?=pattern)` ⇒ positive lookahead: If _pattern_ matches, backtrack to start of _pattern_. This allows using logical AND for combining regexes.

    * The expression `(?=.*[[:lower:]])(?=.*[[:upper:]]).{6,}` tries finding a lowercase letter anywhere. On success it backtracks and searches for an uppercase letter. On yet another success, it checks whether the subject has at least 6 characters.

    * `q(?=u)i` doesn't match `quit`, because the assertion `(?=u)` matches the `u` but does not consume the `u`, as matching `u` consumes zero characters, so then trying to match `i` in the pattern fails, because it is still comparing against the `u` in the text being searched.

* `(?!pattern)` ⇒ negative lookahead: Matches if lookahead _pattern_ didn't match.

* `(?<=pattern)` ⇒ positive lookbehind: This assertion matches if _pattern_ matches before the current token.

* `(?<!pattern)` ⇒ negative lookbehind: This assertion matches if _pattern_ does not match before the current token.

    * NOTE: In the lookbehind assertions, _pattern_ has to be of fixed length, so that the regex engine knows where to test the assertion.  Use `\K` (above) for the equivalent of variable-length lookbehind.

*  `(?>pattern)` ⇒ Match _pattern_ independently of surrounding patterns, and don't backtrack into it. Failure to match will cause the whole subject not to match.



### Substitutions

In substitutions (the contents of the **Replace with** entry), there are additional escape sequences:

*  `\l` ⇒ Causes next character to output in lowercase

*  `\L` ⇒ Causes next characters to be output in lowercase, until a `\E` is found.

*  `\u` ⇒ Causes next character to output in uppercase

*  `\U` ⇒ Causes next characters to be output in uppercase, until a `\E` is found.

*  `\E` ⇒ Puts an end to forced case mode initiated by `\L` or `\U`.

*  `$&`, `$MATCH`, `${^MATCH}`, `$0`, `${0}` ⇒ The whole matched text.

*  `` $` `` <!-- ` -->, `$PREMATCH`, `${^PREMATCH}` ⇒ The text between the previous and current match, or the text before the match if this is the first one.

*  `$'`, `$POSTMATCH`, `${^POSTMATCH}` ⇒ Everything that follows current match.

*  `$^N`, `$LAST_SUBMATCH_RESULT`, `${^LAST_SUBMATCH_RESULT}` ⇒ Returns what the last matching subexpression matched.

*  `$+`, `$LAST_PAREN_MATCH`, `${^LAST_PAREN_MATCH}` ⇒ Returns what matched the last subexpression in the pattern, if that subexpression is currently matched by the regex engine.

*  `$$` or `\$` ⇒ Returns literal `$` character.

*  `$ℕ`, `${ℕ}`, **\\ℕ** ⇒ Returns what matched the ℕth subexpression, where ℕ is a positive integer (1 or larger).

*  `$+{name}` ⇒ Returns what matched subexpression named _name_.


### Zero length matches

In normal or extended mode, there would be no point in looking for text of length 0; however, in regular expression mode, this can often happen. For example, to add something at the beginning of a line, you'll search for "^" and replace with whatever is to be added.

Notepad++ would select the match, but there is no sensible way to select a stretch zero character long. When this happens, a tooltip very similar to function call tips is displayed instead, with a caret pointing upwards to the empty match.


### Examples

These examples are meant to help better show what the complex regex syntax will accomplish.  Many of these examples, written by Georg Dembowski, have been in previous versions of the documentation for years; they have been updated to match with the modern Notepad++ v7.7 regular expression syntax.

**IMPORTANT**

*  You have to check the box "regular expression" in search &amp; replace dialog

*  When copying the strings out of here, pay close attention not to have additional spaces before or after them! Otherwise, the tested regex will not match anything!

#### Example 0
How to replace/delete full lines according to a regex pattern?
Let's say you wish to delete all the lines in a file that contain the word "unused", without leaving blank lines in their stead. This means you need to locate the line, remove it all, and additionally remove its terminating newline.

So, you'd want to do this:

* Find: `^.*?unused.*?$\R`
* Replace with: nothing, not even a space

The regular expression **appears** to always work  is to be read like this:

*  assert the start of a line

*  match some characters, stopping as early as required for the expression to match

*  the string you search in the file, "unused"

*  more characters, again stopping at the earliest necessary for the expression to match

*  assert line ends

*  A newline character or sequence


Remember that `.*` gobbles everything to the end of line if **☐ . matches newline** is off, and everything to the end of file if the option is on!

Well, why is **appears** above in bold letters? Because this expression assumes each line ends with an end of line sequence. This is almost always true, and may fail for the last line in the file. It won't match and won't be deleted.

But the remedy is fairly simple: we translate in regex parlance that the newline should match if it is there. So the correct expression actually is:

* Find: `^.*?unused.*?$\R?`

This is because `?` makes it match 0 or 1 `\R`.

#### Example 1
You use a MediaWiki (like Wikipedia) and want to make all headings one level higher, so a H2 becomes a H1 etc.

*  Search `^=(=)`

*  Replace with `\1`

*  Click **Replace all**

You do this to find all headings2...9 (two equal sign characters are required) which begin at line beginning (^) and to replace the two equal sign characters by only the last of the two, so eliminating one and having one remaining.


*  Search `=(=)$`

*  Replace with `\1`

*  Click **Replace all**

You do this to find all headings2...9 (two equal sign characters are required) which end at line ending ($) and to replace the two equal sign characters by only the last of the two, so eliminating one and having one remaining.

`== title ==` became `= title =`

You're done :-)


#### Example 2
You have a document with a lot of dates, which are in date format `dd.mm.yy` and you'd like to transform them to sortable format `yy-mm-dd`. Don't be afraid by the length of the search term – it's long, but consisting of pretty easy and short parts.

Do the following:


*  Search `([^0-9.])([0123][0-9])\.([01][0-9])\.([0-9][0-9])([^0-9.])` or

   Search `(\s)([0123][0-9])\.([01][0-9])\.([0-9][0-9])(\s)`

*  Replace with `\1\4-\3-\2\5`

*  Click **Replace all**


You do this to fetch:

*  the day, whose first number can only be 0, 1, 2 or 3

*  the month, whose first number can only be 0 or 1

*  but only if the separator is a literal dot and not any standard character ( `\.` versus `.` )

*  but only if no numbers are surrounding the date, as then it might be an IP address instead of a date


and to write all of this in the opposite order, except for the surroundings. Pay attention: Whatever SEARCH matches will be deleted and only replaced by the stuff in the REPLACE field, thus it is mandatory to have the surroundings in the REPLACE field as well!

Outcome:

*  `31.12.97` became `97-12-31`

*  `14.08.05` became `05-08-14`

*  the IP address `14.13.14.14` did not change


You're done :-)


#### Example 3
You have printed in windows a file list using `dir /a-d /b/s /-p > filelist.txt` to the file filelist.txt and want to make local URLs out of them.

*  Open `filelist.txt` with Notepad++

*  Search `\\`

*  Replace with `/`

*  Click **Replace all**.

    This changes the Windows path separator char `\` into the URL path separator char `/`


* Search `\x20`

* Replace `%20`

* Click **Replace all** to change any space character into the `%20` syntax

    According on your requirements, you can similarly change any possible symbol `! # $ % & ' ( ) + , - ; = @ [ ] ^ { } ~` with the appropriate `%ℕℕ` expression

*  Search `^(?=.)$`

*  Replace with `file:///`

*  Click **Replace all**

    This adds file:/// to the beginning of all non-empty lines

After this sequence, `C:\!\Test A.csv` became `file:///C:/!/Test%20A.csv`.

You're done :-)


#### Example 4

Let’s suppose you need a comma delimited table from the table, below :

~~~
[Data]
AS AF AFG 004 Afghanistan
EU AX ALA 248 Åland Islands
EU AL ALB 008 Albania, People's Socialist Republic of
AF DZ DZA 012 Algeria, People's Democratic Republic of
OC AS ASM 016 American Samoa
EU AD AND 020 Andorra, Principality of
AF AO AGO 024 Angola, Republic of
NA AI AIA 660 Anguilla
AN AQ ATA 010 Antarctica (the territory South of 60 deg S)
NA AG ATG 028 Antigua and Barbuda
SA AR ARG 032 Argentina, Argentine Republic
AS AM ARM 051 Armenia
NA AW ABW 533 Aruba
OC AU AUS 036 Australia, Commonwealth of
~~~

Then use the following regex S/R :

*  Search for: `(?-i)[\u\d]\K\x20(?=[\u\d])`

*  Replace with: `,`

*  Hit **Replace All**


~~~
[Final Data]
AS,AF,AFG,004,Afghanistan
EU,AX,ALA,248,Åland Islands
EU,AL,ALB,008,Albania, People's Socialist Republic of
AF,DZ,DZA,012,Algeria, People's Democratic Republic of
OC,AS,ASM,016,American Samoa
EU,AD,AND,020,Andorra, Principality of
AF,AO,AGO,024,Angola, Republic of
NA,AI,AIA,660,Anguilla
AN,AQ,ATA,010,Antarctica (the territory South of 60 deg S)
NA,AG,ATG,028,Antigua and Barbuda
SA,AR,ARG,032,Argentina, Argentine Republic
AS,AM,ARM,051,Armenia
NA,AW,ABW,533,Aruba
OC,AU,AUS,036,Australia, Commonwealth of
~~~

You’re done :-)


#### Example 5
How to recognize a balanced expression, in mathematics or in programming?

First, let's give some example data:

~~~
[Sample Test Start]

((((ab(((cd((()))))ef))))))
0000  000  00100000  00000•
1234  567  89098765  43210


((ab((((cd(((ef(()))))gh))))ijkl))))
00  0000  000  1110000  0000    00••
12  3456  789  0109876  5432    10


((((((ab(cd(ef((()))))gh)ijkl)))mn)))))
000000  0  0  01110000  0    000  00•••
123456  7  8  90109876  5    432  10


((01ab(cd(ef23gh(ij45kl)mn)op((qr67st)uv\wx)34)yz))128956)abc
12    3  4      5      4  3  45      4     3  2  10      •

[[@ab[cd{ef@gh{ij@kl}mn]op((qr@st}uv@x]34yz])12@56)[cdedf]
                          12                1     0

((12ab(cd{ef34gh{ij56kl}mn}123}op((qr78stu)v\wx34)yz)12905126]
12    3                          45       4      3  2
••

[[@ab[cd{ef@gh{ij@kl}mn}123]op((qr@stu)v@x34)yz]12@5@6]
                              12      1     0
[Sample Test End]
~~~

For instance, let’s try to build a regular expression that finds the largest range of text with well balanced parentheses!

First, some typographic conventions :

* Let Sp be a starting parenthesis. So, its regex syntax is the escaped form `\(`, or simply `(` if inside a character class

* Let Ep be an ending parenthesis. So, its regex syntax is the escaped form `\)`, or simply `)` if inside a character class

* Let Ac be any single allowed character, including EOF character(s), different from EP and SP. So, its regex syntax is the negative class character `[^EpSp]`, i.e., the negative class character `[^()]`

* Let R0 be a recursion to the whole matched pattern. By convention, in PCRE, its regex syntax is `(?0)` or `(?R)`

* Let R1 be a recursion to the group 1 pattern. By convention, in PCRE, its regex syntax is `(?1)`

* Now , let Bb be a well-balanced block, containing an Ep....Sp construction, itself possibly composed with, both, Ac characters and an other Bb, at any level greater than level 0

    This Bb block can be represented by the symbolic regex, below ( Blank chars are ignored, for readability ) :

        Sp ( Ac+ | R0 )* Ep

    This syntax may be improved as `Bb = Sp (?: Ac++ | R0 )* Ep`, using, both :

* A non-capturing group, surrounding the two alternatives

* A possessive quantifier relative to the Ac character, to be similar to the atomic state of recursions, in PCRE.

    It is important to point out that, if you would use the greedy form `Ac+`, instead of `Ac++`, the last match would be, wrongly, all the file contents, even against a very short text! Again, the advantage of not allowing backtracking reduces combinations and avoids the catastrophic backtracking process :-)

Now, more precisely, between the Sp and Ep parentheses, you may meet :

* Nothing, hence the star quantifier, after the non-capturing group

* A non-null range of allowed chars, so the atomic group Ac++

* An other well-balanced Bb construction which can be verified, in turn, by the recursion feature R0

On the other hand, any subject text scanned can be defined, either, as :

* A combination of successive syntaxes  Ac*  Bb  Ac*  Bb  Ac*  Bb, ended with a last Ac*. So, in the symbolic regex syntax, this can be written as (?: Ac* Bb)+ Ac*

* A non-null range of allowed chars, when the subject text does NOT contain any Ep and Sp parenthesis, so the Ac+ symbolic syntax, only ( By extension, a text without parentheses is, obviously, a well balanced parentheses text... as it contains no parenthesis ! )

This implies that the general symbolic regex is `(?: Ac* Bb )+ Ac* | Ac+`

Now, by substituting the above value of the well-balanced Bb construction, in our final expression, we get our final symbolic regex expression :

    (?: Ac* ( Sp (?: Ac++ | R1 )* Ep ) )+ Ac* | Ac+
            \ ---------------------- /
                     Group 1

Note, however, that we just had to add two parentheses to define a new group #1 , which embeds the Bb construction,. Indeed, during the recursion process, it must refer, specifically, to that group #1 and NOT recurse to the whole regex pattern. Hence, the R1 notation, instead of the R0 notation!

Finally, we can get something more legible if we use the free-spacing mode to identify the components of our regex and rewrite this expression with the correct regex syntax:

    (?x) (?: [^()]*  (  \(  (?:  [^()]++  |  (?1)  )*  \)  )  )+  [^()]*  |  [^()]+

Note that, with the free-spacing mode, you may, as well, insert comments and split the regex on several lines, leading, for instance, to the following text:

    (?x)                  #  FREE-SPACING mode
    (?:                   #  Start of the FIRST NON-CAPTURING group
        [^()]*            #      Any range, possibly NUL, of ALLOWED characters
        (                 #      Start of CAPTURING group #1
            \(            #          STARTING parenthesis
            (?:           #          Start of the SECOND NON-CAPTURING group
                [^()]++   #              Any NON-NULL ATOMIC range of ALLOWED characters,
                |         #              OR
                (?1)      #              A RECURSION, using the regex pattern of group #1
            )*            #          End of the SECOND NON-CAPTURING group, repeated 0 or MORE times
            \)            #          ENDING parenthesis
        )                 #      End of the CAPTURING group 1
    )+                    #  End of the FIRST NON-CAPTURING group, repeated 1 or MORE times
    [^()]*                #  Any range, possibly NUL, of ALLOWED characters
    |                     #  OR
    [^()]+                #  Any NON-NULL range of ALLOWED characters,

If we reduce the syntax of this recursive regular expression to its minimum, we get :

    (?:[^()]*(\((?:[^()]++|(?1))*\)))+[^()]*|[^()]+

But it is about as hard to decrypt as a badly indented piece of code without a comment and with unpromising, unclear identifiers.


## Searching actions when recorded as macros

The Find family of actions can be recorded in a macro to make them easy to name and later replay via the **Macro** menu or an assigned keyboard shortcut.  Somewhat unfortunately, **Find what** and **Replace with** text is hardwired into the macro when it is created, and isn't something the user can change when the macro runs, but often this isn't a significant limitation.

Note, however, that Find-related actions are recorded a bit differently than other Notepad++ actions, so we'll discuss them a bit more in-depth here.  Typically, Notepad++ will record a step in a macro every time a user does something in the Notepad++ user interface.  The Find family of actions is more "coordinated" where macro recording is concerned.

The macro recorder only records when an actual Find family action (e.g. **Replace**, **Find All in Current Document**, etc.) occurs.  Thus you can tweak a future action's parameters (e.g. **Match case**, **Wrap around**, etc.) all you'd like, and all of that fiddling doesn't get remembered.  At the point where you perform an action, then a snapshot is taken of all of the parameters and the action, and this is logged in the macro memory as a proper macro sequence.

While the user can simply record and use Find family macros, one can also edit those macros later to change or add to their functionality, so it is helpful to know the details of the macro sequences that were previously recorded.  While the details of how macros in general are recorded and stored in *shortcuts.xml* is discussed elsewhere, here are the details of what happens when Notepad++ saves a recorded Find family macro:

First comes a **1700** message which carries out some initialization of the Find engine:

`<Action type="3" message="1700" wParam="0" lParam="0" sParam="" />`

Next is a **1601** message with the **Find what** text in the **sParam** field; in this example we search for "it":

`<Action type="3" message="1601" wParam="0" lParam="0" sParam="it" />`

Following that is a **1625** message with the **Search mode** in **lParam**, with possible values of 0=**Normal** / 1=**Extended** / 2=**Regular expression**; let's show **Regular expression** in this example:

`<Action type="3" message="1625" wParam="0" lParam="2" sParam="" />`

After that, if a type of replacement operation is being performed, is a **1602** message with **sParam** holding the **Replace with** text; here we'll make that "IT":

`<Action type="3" message="1602" wParam="0" lParam="0" sParam="IT" />`

Moving on, next, if performing a **Find All** (really a Find-in-Files) or a **Replace in Files**, is a **1653** message containing the base **Directory** for the search in **sParam**:

`<Action type="3" message="1653" wParam="0" lParam="0" sParam="C:\Program Files\Notepad++\" />`

Also when doing a **Find All** or a **Replace in Files**, will be a **1652** message containing the **Filters** for the search in **sParam**:

`<Action type="3" message="1652" wParam="0" lParam="0" sParam="*.*" />`

Next will be a **1702** message that contains a bit-weighted number in **lParam** that represents the "checkbox" option parameters for the action (more on this later, for now we will just use 515 in the example, and present the bit-weight table):

`<Action type="3" message="1702" wParam="0" lParam="515" sParam="" />`

| 1702-Bit-Weight |Binary-Bit-Weight  | Meaning (equivalent option ticked) |
|----------------:|------------------:|:-----------------------------------|
| 1               | 0000000001        | Match whole word only              |
| 2               | 0000000010        | Match case                         |
| 4               | 0000000100        | Purge for each search              |
| 16              | 0000010000        | Bookmark line                      |
| 32              | 0000100000        | In all sub-folders                 |
| 64              | 0001000000        | In hidden folders                  |
| 128             | 0010000000        | In selection                       |
| 256             | 0100000000        | Wrap around                        |
| 512             | 1000000000        | Backward direction (*)             |

*: **Backward direction** ticked means 512 is _not_ included; unticked means 512 _is_ included.

> Let's see how the example value 515 used above is decoded:

> lParam="515" (decimal) = 203 (hex) = 10 0000 0011 (binary) = 512 + 2 + 1 = (***not*** Backward direction + Match case + Match whole word only).  Thus, this would represent a forward-from-caret-towards-end-of-file search of exact case specified, with the additional qualifier that the match text must be bracketed by non-word characters.


Finally appears a **1701** message which encodes the Find family action to perform in **lParam**, which, when executed will conduct the action using all of the information encoded in the prior messages; let's do a **Replace in Files**, which has an integer value of 1660, for purposes of an example:

`<Action type="3" message="1701" wParam="0" lParam="1660" sParam="" />`

| 1701-Value | Meaning (equivalent button press)   |
|-----------:|-------------------------------------|
| 1          | Find Next                           |
| 1608       | Replace                             |
| 1609       | Replace All                         |
| 1614       | Count                               |
| 1615       | Mark All                            |
| 1633       | Clear all marks                     |
| 1635       | Replace All in All Opened Documents |
| 1636       | Find All in All Opened Documents    |
| 1641       | Find All in Current Document        |
| 1656       | Find All (in Files)                 |
| 1660       | Replace in Files                    |


Here is a complete example (that could occur in *shortcuts.xml*) and how it is interpreted:

    <Macro name='Book Mark lines NOT containing ABC' Ctrl="no" Alt="no" Shift="no" Key="0">
        <Action type="3" message="1700" wParam="0" lParam="0" sParam="" />
        <Action type="3" message="1601" wParam="0" lParam="0" sParam="^(?-s)(?!.*ABC).*" />
        <Action type="3" message="1625" wParam="0" lParam="2" sParam="" />
        <Action type="3" message="1702" wParam="0" lParam="786" sParam="" />
        <Action type="3" message="1701" wParam="0" lParam="1615" sParam="" />
    </Macro>

First we have our initializing 1700 message.

Following that in the 1601 message's sParam field is a regular expression that will match lines that do not contain "ABC": `^(?-s)(?!.*ABC).*`

The search type for "Regular expression" appears next as lParam="2" in the 1625 message.

Skipping the 1702 message for the moment, the 1701 message has lParam="1615" which, from the 1701 table, means "Mark All".

Finally, let's consider the 1702 message.  Its pertinent part is lParam="786".  The best way to break that down into its component parts is to convert the number to binary and then determine how the one-bits in the binary contribute to the meaning.  786 in binary is 1100010010 (= 512 + 256 + 16 + 2), which breaks down as follows, and then reading the 1702 table from earlier we get the contributors to functionality:

* `1000000000` = 512 - Backward direction _disabled_ (thus forward direction from caret toward bottom end of file)

* `0100000000` = 256 - Wrap around

* `0000010000` = 16 - Bookmark line

* `0000000010` = 2 - Match case

Note that in this example we seem to have conflicting search parameters:  We have a direction encoded, as well as a Wrap around, which nullifies the need for a direction.  This is not a problem, as the Wrap around option will take precedence, just like in a non-macro'd interactive searching operation.
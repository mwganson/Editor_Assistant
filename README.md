# Editor_Assistant
FreeCAD macro adds functionality to FreeCAD's built-in python editor

## Installation
Install via the Addon manager (soon).  Until then copy the .FCMacro file to your Macro folder.  The macro will create a new file called Editor_Assistant_Templates.txt in the same folder.  This file holds the templates.  It will need to be removed manually when you uninstall the macro.

## Toolbar icon
Download the toolbar icon: <img src="Editor_Assistant_Icon.svg" alt="icon"><a href="Editor_Assistant_Icon.svg"> Link</a><br/>

## Screenshot
<img src="Editor_Assistant_scr1.png" alt="screenshot">

## Usage
By defaul the macro opens in a 3rd take in the combo view.  You will have Model, Task, and Editor assistant tabs.  If you prefer having a floating dockable, hold Alt key down while executing the macro.  

The title bar will show the current version: "Editor Assistant v1.09", for example.  The dialog works with the concept of a current editor, which will be one of the text documents currently open.  These can be python scripts or "Text document" objects.  Basically, any view in the MDI area that has a QPlainTextEdit as a child shows up in the macro's editor list at the top of the dialog.  The edior list will need to be refreshed from time to time when you open/close a document, using the "Refresh"  button.

The widgets in the dialog are presented in a series of horizontal lines (using QHBoxLayouts).  Here I will discuss briefly each line and the widgets contained on each.

## Top line
The Top Line has the Main menu button and the Toast button.
### Main menu button
This button is at the top left of the dialog and has the main editor assistant icon.  Click the button to bring up the main menu.  The main menu will be discussed in more detail below.
### Toast button
At the top of the dialog, above the Editor List there is a button reserved for "toast" messages.  These are transient messages that disappear automatically after a few seconds.  They can be error messages or just information.  The button serves as the label for the messages.  Click the button to see a log of previous messages in the Report view.  The color of the message indicates the type of message.  Types are: message: black, information: blue, error: red, warning: yellow.  For some types the background color of the toast button is temporarily changed to provide better contrast.
## Editor line
### Editor list
This is the QListWidget at the top of the dialog.  It will list the names of the documents currently registered with the dialog.  Select an item in the list to make make that item the current editor.  This also sets the focus on that editor.  All the actions in the dialog will work on that current editor even if you select a different editor in FreeCAD's MDI widget.  It is recommended to select the editor here so the dialog is in sync with the correct editor.
## Refresh line
### Refresh button
This button is labeled "Refresh".  Click it if you have opened a new document or closed a document, so the Editor List can be updated.  Updates also happen when other functions are used and when documents are opened, but not when closed.
### Goto menu button
This button is next to the refresh button and currently has a down arrow icon.  Click this button to bring up a menu of Goto options.  This is a very powerful and useful feature that can save you a lot of time.  The Goto menu is also incorporated into the main menu as a Goto submenu.  The Goto menu will be discussed in more detail below.
#### Go to line numbers
This is a QLineEdit that can hold either a single number (the line number you wish to go to) or a list of numbers that will appear in a selection menu when press return/enter while the widget has keyboard focus.  The goto line option(s) appear as menu items in the Goto menu.
## Undo line
### Undo button
Important: Both undo and redo will replace all the text in the current editor with what it was when the undo/redo was stored in memory.  This overwrite any changes you made by, for example, typing directly into the editor.  As an example, suppose you rename some variable using replace all, from "myvar" to "myvariable".  This is staged in the undo queue as "Undo replace myvar".  Then you type in code for a new function definition.  After typing that in you decide you want to undo renaming myvar to myvarible because it's too much typing, so you click Undo rename myvar.  Guess what just happened to the new function definition?  It's a gone pecan (but is staged in Redo).

Click to undo an operation.  Undo only works on things done in the dialog.  There is a maximum number of events that can be undone.  Currently, this is 100, but is subject to change.  This is set in the source code as UNDO_QUEUE_MAX_SIZE = 100, at the top of the source file.  This is to prevent potential out of memory errors.  The queue is based on the current editor.  As you switch editors (via the editor list) the Undo button label will change, and will sometimes become disabled if there are no undo events in the queue for that editor.
### Purge undo/redo queues
Clicking this button purges the undo/redo queues.  This cannot be undone.
### Redo button
When an operation is undone, the undone is popped and sent to the redo queue.
## Find line
### Find edit
This is a QLineEdit into which you may enter text to search for in the current editor.  As characters are typed you will see a new toast showing how many times the entered text can be found in the current editor.  (The Match case checkbox figures into this analysis, but not the Whole words checkbox.)  You may press Enter/Return in this box to instigate a search or use one of the Find buttons.  For both the Find and Replace line edit widgets there is a context menu option to paste the currently selected text from the current editor into them.  This can also be done by double clicking the line edit.
The text in the Find edit can also be used to fill in replacement text when using templates, discussed in more detail below.
### Find next
Click this button to find the next instance of the text in the Find edit.  If a match is found the text will be selected in the current editor and the cursor will move to that line.  If it is not found, then nothing happens.  Ctrl+Click to search from the beginning of the document.  Alt+Click to find from selection.  This uses the text currently selected in the current editor as if it were in the Find edit widget.
### Find previous
Like Find next except it searches backward.  Ctrl+Click to begin the search from the end of the document.  Alt+Click to find from selection.  This uses the text currently selected in the current editor as if it were in the Find edit widget.
## Replace line
### Replace edit
This is a QLineEdit containing replacement text (replacing the text in the Find Edit) when the Replace or Replace All buttons are used.  The text in the Replace edit can also be used as replacement text when using templates, discussed in more detail below.
### Replace
Replace the currently selected text in the current editor with the text in the Replace Edit, and then toggle the Find next button.  Note: there need not be a previous find success.  Whatever is currently selected gets replaced.  You can use Undo to undo this operation.
### Replace all
Replace all occurrences of the text in the Find edit with the text in the Replace edit in the entire document.  Or Ctrl+Click to only replace the occurrences in the currently selected text.  If Replace edit is empty, then the occurrences are simply deleted.  This respects the Match case checkbox state.  It is incompatible with the Whole word checkbox checked state, and is therefore disabled when that checkbox is checked.  This can be undone using the Undo button.
## Match line
### Unindent
Moves the selected text in the current editor to the left by 4 spaces if there are 4 spaces in front of every selected line.  If not every selected line has 4 leading spaces the operation will not be done.
### Indent
Moves the selected text in the current editor to the right by 4 spaces.
### Match case
This is a QCheckBox.  If checked searches are case-sensitive.  Replace and Replace all both respect this checkbox state.
### Whole words
This is a QCheckBox.  If checked, then searches are for the entire word.  For example, "en" will not find "end" as a match if Whole words is checked.
### Loop
This is a QCheckBox.  If checked, then when there is a search that finds no result the cursor is set back to the top of the document.  Or if it is a backwards search then the cursor goes back to the end of the document.
## Snaps line
### Take snapshot
Takes a snapshot of the current editor, including it's full text and position of cursor if there is text selected.  Snapshots can be very useful as temporary, in-memory backups of the file currently open.  They can be used to restore the content back to the previous state, including the cursor position.  They can be used to create diffs to show the changes that have been made since the snapshot was taken.  (When a file is first opened an automatic snapshot is taken.)
There is a special menu devoted to snapshots and diffs, the Snaps menu, discussed in more detail below.
Snapshots can be taken, popped, restored, discarded, saved, loaded, edited, and diffed.  Popping a snapshot means to restore it to the current editor and then discard it.  Restoring is like popping except for 2 differences: the snapshot is not discarded, and snapshots may be restored in any order (not only the most recent).  Discarding is like popping except without restoring.  You may save the snapshot as a file with the .py or .FCMacro extension, or any other extension you prefer.  You can also save all the snapshots into a single file, stored in JSON format.  Snapshots may be loaded from the JSON file.  Editing a snapshot involved editing the "reason", which is a very brief description of the snapshot.  Diffing creates an html file showing the differences between the 2 compared things, typically the current editor with one of its snapshots.
### Discard latest snapshot
Discards most recent snapshot (without restoring it).  Use pop to restore and discard or restore from the menu if you don't want to discard.
### Snaps menu
This opens the Snaps menu, discussed in more detail below.
### Pop snap
This pops the latest snapshot in the queue, restoring it to the current document (overwriting its contents) and then discarding the snapshot.
## Console line
### To console
This line edit can be very useful for directly controlling the current QPlainTextEdit widget, referenced as variable "editor" from this widget.  For example, enter editor.selectAll() to select all the text in the current document.  Refer to Qt documentation for functions available to QPlainTextEdit widgets.
Another variable available is "dlg", which refers to the Editor assistant dialog itself.  This is mainly something I wanted as a debugging aid while writing this macro, but can have its uses.



## Changelog
### 1.55 (2002.03.30)
Template editor dialog improvements
* on deleting select the next item
* on renaming, offer current name as default
* on renaming, select same item
* on new item, don't allow empty name
* move apply button to standard dialog button box
* change label of execute to clipboard so all buttons line up vertically
### 1.54 (2022.03.30)
* make template editor dialog non-modal
### 1.53 (2022.03.30)
* Ctrl+Replace all button = replace only in selection (not in whole document)
### 1.52 (2022.03.30)
* Home/End buttons, default = not shown, enable in main menu -> settings -> layout -> GotoLine
* Add Home/End menu items to Goto menu
### 1.51c (2022.03.30)
* allow "\n" to be used in input dialogs when using templates
### 1.51b (2022.03.30)
* add "current" as optional value for "goto"
* update help documentation in template editor
### 1.51 (2022.03.30)
* add "goto" key token to templates
### 1.50 (2022.03.30)
* instead of saving templates in user parameters, create a new file called:
"Editor_Assistant_Templates.txt" in same folder as macro
* add execute to clipboard button in template editor
* add execute button in editor
* preformat help text
* preformat test text
* add apply button in editor
### 1.49 (2022.03.29)
* add template function
### 1.48 (2022.03.28)
* Add .FCMacro, .py, .txt to snapshot save as file type
### 1.47 (2022.03.28)
* add qSearch() to search qt for python online documentation
### 1.46 (2022.03.28)
* add search() to help menu: 3 ways to search 1) from find edit text, 2) from selected text, 3) ask for text in a new dialog
### 1.45 (2022.03.28)
* add search() function to To console: line edit (opens new browser tab to github repo search results page)
### 1.44 (2022.03.28)
* add menu() alias for dlg.makeReferenceMenu() in To console:
* better support module names as strings in help() and menu()
### 1.42 (2022.03.28)
* allow for multiple line statements in To console line edit
* allow for help("help string") in To console line edit
### 1.41 (2022.03.27)
* add help menu
### 1.40b (2022.03.26)
* bugfix --error when creating new empty document
### 1.40 (2022.03.26)
* put line numbers first for find results in goto menu
* put some context characters ahead of find results in goto menu
### 1.39b (2022.03.26)
* remove a debug message
### 1.39 (2022.03.26)
* Automatically take a snapshot of each file when it is first opened.
* add goto menu to main menu
* add snaps menu to main menu
* internally rename snaps menu from context menu to normal menu since it's no longer a context menu
* disconnect mdi area subWindowActivated signal on closing
### 1.38 (2022.03.26)
* move diffs to mdi area for a more seamless integration
* add low priority toast option -- such toasts do not overwrite existing toasts
### 1.37 (2022.03.26)
* add view mode to main menu<br/>
  supported modes: tabbed, tiled, cascade<br/>
### 1.36 (2022.03.25)
* refresh editor list when a new text file is opened
### 1.35b (2022.03.25)
* fix typo in window title
### 1.35 (2022.03.25)
* change default from floating dockable widget to embedded as a 3rd tab in combo view.
* fix bug where warning toasts were shown as normal messages
* add Close to main menu
### 1.34 (2022.03.25)
* add main menu button
* add settings -> layout menu to enable/disable individual lines of widgets
### 1.33 (2022.03.25)
* optimize diff performance
### 1.32 (2022.03.24)
* show log of all toasts when clicking toast button instead of just the last toast
* add restore last snap to restore any menu
* allow to diff current editor to any other open editor
* allow to diff current editor against clipboard text
* extend showDiff() to be able to make diffs of arbitrary text strings
* add labels to diff screen
### 1.31 (2022.03.24)
* add find selection results to goto menu
### 1.30 (2022.03.24)
* trim some labels -- rely on icon and tooltips
* after To console command check if text changed for creating undo
* on To console command open python console if not already open, set keyboard focus to it
### 1.29 (2022.03.24)
* add dlg variable to To console command
* add Loop checkbox
* Ctrl+Find to search from beginning
* Ctrl+Find previous to search from end
* Alt+Find / Find previous to search using selected text
### 1.28 (2022.03.24)
* fix failure to set current editor (causes some blinking)
### 1.27 (2022.03.23)
* add restore any functionality
* rename some buttons so pop, restore, discard always mean the same things
### 1.26 (2022.03.23)
* add reason editor
* check for tabs and warn if any are found
* remove some tabs from Gimp-generated XPM icon string
### 1.25 (2022.03.23)
* improve diff viewer
### 1.24 (2022.03.23)
* make diffs into html and show in a web browser view
### 1.23 (2022.03.22)
* improve tooltips for snaps menu button and goto menu button
* allow to pop individual snaps out of order
* add load/save to/from JSON files (all snaps for all editors)
* add snap diff functionality (diff text goes to new Text document object) 
### 1.22c (2022.03.22)
* improve cursor centering
* check macro hasn't been closed before attempting to clear toast
* warn that find results do not respect whole words checkbox
### 1.22b (2022.03.22)
* make Find results menu respect Match case checkbox
### 1.22 (2022.03.22)
* add find results to goto menu
### 1.21 (2022.03.22)
* add bookmark functionality
* put submenus in goto menu for line numbers, class/defs, and bookmarks
### 1.20 (2022.03.22)
* rearrange some icons
* add goto menu
* allow for multiple line numbers (comma separated list) in goto line edit
### 1.19 (2022.03.21)
* change from task panel to dockable widget (Hold Alt key while executing macro for old behavior.)
* rework some icons  
### 1.18b (2022.03.21)
* add error checking for when there are no open editors
### 1.18 (2022.03.21)
* add snapshot feature
* add text cursor positioning to snapshots and undo/redo
### 1.17 (2022.03.20)
* new icons for indent and unindent
* double click Find/Replace edits to replace existing content with selected text in current editor
### 1.16 (2022.03.20)
* add redo button
* add undo queue clear button
* rearrange widgets, putting goto on same line as refresh button
* add placeholder text to goto line edit
### 1.15 (2022.03.20)
* Use a button for toast messages instead of a label.  That way it can be clicked and the last toast reshown.
* Use message types instead of colors direction.  message types: warning, error, message, information
* Set background color for some message types to get a nice contrast.
* Set a maximum length for undo queue and pop the oldest when the max is reached.  This is to prevent out of memory issues potentially.
* UNDO_QUEUE_MAX_SIZE = 100 (subject to change)
### 1.14b (2022.03.19)
* avoid infinite loop if replace all is clicked without anything in the Find edit
### 1.14 (2022.03.19)
* implement Undo button
### 1.13 (2022.03.19)
* handle backslashed characters in Find edit, such as \t, \n
### 1.12b (2022.03.19)
* fix type in Replace all handler.
### 1.12 (2022.03.19)
* Disable Replace all if Whole words is checked.
### 1.11 (2022.03.19)
* Make Replace all respect Match case checkbox (but still not Whole words)
### 1.10 (2022.03.19)
* add icons to the dialog
### 1.09c (2022.03.19)
* new icon
### 1.09b (2022.03.19)
* remove icon, to be replaced
### 1.09 (2022.03.19)
* embed xpm icon


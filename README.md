# Editor_Assistant
FreeCAD macro adds functionality to FreeCAD's built-in python editor

## Installation
Install via the Addon manager (soon).

## Toolbar icon
Download the toolbar icon: <img src="Editor_Assistant_Icon.svg" alt="icon"><a href="Editor_Assistant_Icon.svg"> Link</a><br/>

## Screenshot
<img src="Editor_Assistant_scr1.png" alt="screenshot">

## Usage
As of v1.19 the macro now opens as a dockable widget rather than as a task panel dialog.  If you prefer having the task panel dialog, hold Alt key down while executing the macro.  This will enable the macro to remain open while other task panels are being used.  IMO, the best place to put it is to drag/drop on top of the combo view, which sets it as a tabbed view sibling with the combo view.

The title bar will show the current version: "Editor Assistant v1.09", for example.  The dialog works with the concept of a current editor, which will be one of the text documents currently open.  These can be python scripts or "Text document" objects.  At the top of the dialog is the editor list, showing the documents currently open.  (This will need to be refreshed if you open/close a document, using the "Refresh editor list" button.)
### Editor List
This is the QListWidget at the top of the dialog.  It will list the names of the documents currently registered with the dialog.  Select an item in the list to make make that item the current editor.  This also sets the focus on that editor.  All the actions in the dialog will work on that current editor even if you select a different editor in FreeCAD's MDI widget.  It is recommended to select the editor here so the dialog is in sync with the correct editor.
### Toast
At the top of the page, above the Editor List there is a button reserved for "toast" messages.  These are transient messages that disappear automatically after a few seconds.  They can be error messages or just information.  The button serves as the label for the messages.  Click the button to see again the most recent message.  Only the most recent message is stored for viewing again.  The color of the message indicates the type of message.  Types are: message: black, information: blue, error: red, warning: yellow.  For some types the background color of the toast button is temporarily changed to provide better contrast.
### Refresh button
This button is labeled "Refresh editors".  Click it if you have opened a new document or closed a document, so the Editor List can be updated.  (Updates also happen when other functions are used.)
### Goto menu button
Click this button to bring up a menu of Goto options.  This is a very powerful and useful feature that can save you a lot of time.
#### Go to line
These depend on whether you have entered a list of line numbers in the Goto line edit.  Each number in the list gets a menu item.  Click the menu item to go to that line in the current editor.
#### Class/Def lines
In this menu you can go to any line in the source that begins with class or def.  An attempt is made to arrange it so all of the class functions are together, but it's a very simple algorithm.  Each time a line is found with "class" in it the curClass variable is updated. This variable is shown with each "def" line encountered up until the next "class" is found.  It is not guaranteed that all the defs will actually belong to that class.
#### Bookmarks
You can enter a comment in the source code to create a bookmark for that line.  Then you can go to that bookmark from the Goto menu.  The marker is 2 pound symbols in a row followed by a colon, then a description.  Example:

<pre>##: menu text goes here</pre>

You must include a description of at least one character or else the bookmark will not show up in the menu.  The comment does not have to be at the beginning of the line.

#### Find results
If there is text in the Find edit, then on every line where this text appears there will be a menu item in the find results menu, allowing to go to that line.  This function respects the Match case checkbox, but not the Whole words checkbox.

### Goto line edit
Enter a line number and press enter to go that line in the current editor.  Watch the toast area for any messages.  You may enter multiple numbers separated by commas.  Then when you press enter you get a menu item for each of the line numbers.  This is also accessible from the Goto menu button.  Each open editor gets its own list of line numbers, so when you switch editors this line edit gets cleared and when you switch back to this editor, the contents are put back in it from before.

### Find Edit
This is a QLineEdit into which you may enter text to search for in the current editor.  As characters are typed you will see a new toast showing how many times the entered text can be found in the current editor.  (The Match case checkbox figures into this analysis, but not the Whole words checkbox.)  You may press Enter/Return in this box to instigate a search or use one of the Find buttons.  For both the Find and Replace line edit widgets there is a context menu option to paste the currently selected text from the current editor into them.  This can also be done by double clicking the line edit.
### Replace Edit
This is a QLineEdit containing replacement text (replacing the text in the Find Edit) when the Replace or Replace All buttons are used.
### Match case
This is a QCheckBox.  If checked searches are case-sensitive.  (Replace All is always case-sensitive.)
### Whole words
This is a QCheckBox.  If checked, then searches are for the entire word.  For example, "en" will not find "end" as a match if Match word is checked.
### Find next
Click this button to find the next instance of the text in the Find Edit.  If a match is found the text will be selected in the current editor and the cursor will move to that line.  If it is not found, then nothing happens.
### Find previous
Like Find next except it searches backward.
### Replace
Replace the currently selected text in the current editor with the text in the Replace Edit, and then toggle the Find next button.  Note: there need not be a previous find success.  Whatever is currently selected gets replaced.  There is no Undo for this at this time.  Close without saving and reopen your document if you make a mistake.  Use with caution!
### Replace all
Replace all occurrences of the text in the Find Edit with the text in the Replace Edit.  If Replace Edit is empty, then the occurrences are simply deleted.  This now (as of v1.11) respects the Match case checkbox state.  It is incompatible with the Whole word checkbox checked state, and is therefore disabled when that checkbox is checked.  There is no Undo for this.  Use with caution!
### Snapshot
Takes a snapshot of the current editor, including it's full text and position of cursor if there is text selected.  This button also has a context menu with additional features.  Snapshots are kept in a stack, only the head (most recent) can be accessed with the Restore option.  When a snapshot is restored it is popped from the queue, making the next snapshot available.
### Restore
Restores a snapshot to the current editor if there is a snapshot in memory for the current editor, replacing existing text with that from the snapshot.  This also pops the snapshot from memory.
### Undo button
Important: Both undo and redo will replace all the text in the current editor with what it was when the undo/redo was stored in memory.  This overwrite any changes you made by, for example, typing directly into the editor.  As an example, suppose you rename some variable using replace all, from "myvar" to "myvariable".  This is staged in the undo queue as "Undo replace myvar".  Then you type in code for a new function definition.  After typin that in you decide you want to undo renaming myvar to myvarible because it's too much typing, so you click Undo rename myvar.  Guess what just happened to the new function definition?  It's a gone pecan (but is staged in Redo).

Click to undo an operation.  Undo only works on things done in the dialog.  There is a maximum number of events that can be undone.  Currently, this is 100, but is subject to change.  This is set in the source code as UNDO_QUEUE_MAX_SIZE = 100, at the top of the source file.  This is to prevent potential out of memory errors.  The queue is based on the current editor.  As you switch editors (via the editor list) the Undo button label will change, and will sometimes become disabled if there are no undo events in the queue for that editor.
### Redo button
When an operation is undone, the undone is popped and sent to the redo queue.  
### Indent
Moves the selected text in the current editor to the right by 4 spaces.
### Unindent
Moves the selected text in the current editor to the left by 4 spaces if there are 4 spaces in front of every selected line.  If not every line has 4 leading spaces the operation will not be done.
### To console
Enter a python command here and press enter.  The variable "editor" refers to the current editor.  Once this is done you can use "editor" as a variable in the python console to directly access the QPlainTextEdit of the current editor.  Refer to PySide documentation for attributes available.  You can also enter "help(editor)" in the console (without the quotes) to see a brief summary of available attributes.


## Changelog
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


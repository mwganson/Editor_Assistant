# Editor_Assistant
FreeCAD macro adds functionality to FreeCAD's built-in python editor

## Installation
Install via the Addon manager (soon).

## Toolbar icon
Download the toolbar icon: <img src="Editor_Assistant_Icon.svg" alt="icon"><a href="Editor_Assistant_Icon.svg">Link</a><br/>

## Screenshot
<img src="Editor_Assistant_scr1.png" alt="screenshot">

## Usage
Execute the macro to bring up the new task dialog.  If another task dialog is currently active you will get an error message and the macro will exit.  The title bar will show the current version: "Editor Assistant v1.09", for example.  The dialog works with the concept of a current editor, which will be one of the text documents currently open.  These can be python scripts or "Text document" objects.  At the top of the dialog is the editor list, showing the documents currently open.  (This will need to be refreshed if you open/close a document, using the "Refresh editor list" button.)
### Editor List
This is the QListWidget at the top of the dialog.  It will list the names of the documents currently registered with the dialog.  Select an item in the list to make make that item the current editor.  This also sets the focus on that editor.  All the actions in the dialog will work on that current editor even if you select a different editor in FreeCAD's MDI widget.  It is recommended to select the editor here so the dialog is in sync with the correct editor.
### Toast
At the top of the page, above the Editor List there is a button reserved for "toast" messages.  These are transient messages that disappear automatically after a few seconds.  They can be error messages or just information.  The button serves as the label for the messages.  Click the button to see again the most recent message.  Only the most recent message is stored for viewing again.  The color of the message indicates the type of message.  Types are: message: black, information: blue, error: red, warning: yellow.  For some types the background color of the toast button is temporarily changed to provide better contrast.
### Refresh button
This button is labeled "Refresh editor list".  Click it if you have opened a new document or closed a document, so the Editor List can be updated.  (Updates also happen when other functions are used.)
### Find Edit
This is a QLineEdit into which you may enter text to search for in the current editor.  As characters are typed you will see a new toast showing how many times the entered text can be found in the current editor.  (The Match case checkbox figures into this analysis, but not the Whole words checkbox.)  You may press Enter/Return in this box to instigate a search or use one of the Find buttons.
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
### Goto line
Enter a line number and press enter to go that line in the current editor.  Watch the toast area for any messages.
### Undo button
Click to undo an operation.  Undo only works on things done in the dialog.  There is a maximum number of events that can be undone.  Currently, this is 100, but is subject to change.  This is set in the source code as UNDO_QUEUE_MAX_SIZE = 100, at the top of the source file.  This is to prevent potential out of memory errors.  The queue is based on the current editor.  As you switch editors (via the editor list) the Undo button label will change, and will sometimes become disabled if there are no undo events in the queue for that editor.
### Indent
Moves the selected text in the current editor to the right by 4 spaces.
### Unindent
Moves the selected text in the current editor to the left by 4 spaces if there are 4 spaces in front of every selected line.  If not every line has 4 leading spaces the operation will not be done.
### To console
Enter a python command here and press enter.  The variable "editor" refers to the current editor.  Once this is done you can use "editor" as a variable in the python console to directly access the QPlainTextEdit of the current editor.  Refer to PySide documentation for attributes available.  You can also enter "help(editor)" in the console (without the quotes) to see a brief summary of available attributes.


## Changelog
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


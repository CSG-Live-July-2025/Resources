# VSCode Keyboard Shortcuts Guide (macOS & Windows with WSL)

Below are useful shortcuts categorized by levels, for both macOS and Windows (using WSL). For Windows, replace `Command` with `Ctrl` and `Option` with `Alt`.

---

## **Level 1: Basic Editing and Navigation**

| Shortcut               | Description                                                              | macOS                     | Windows (WSL)            |
| ---------------------- | ------------------------------------------------------------------------ | ------------------------- | ------------------------- |
| Copy                   | Copy highlighted text (copies the whole line if nothing highlighted)     | `Command + C`             | `Ctrl + C`               |
| Cut                    | Cut highlighted text (cuts the whole line if nothing highlighted)        | `Command + X`             | `Ctrl + X`               |
| Paste                  | Paste copied text                                                        | `Command + V`             | `Ctrl + V`               |
| Paste Without Formatting | Paste text without formatting                                        | `Shift + Option + Command + V` | `Ctrl + Shift + V`   |
| Undo                   | Undo the last action                                                    | `Command + Z`             | `Ctrl + Z`               |
| Redo                   | Redo the last undone action                                             | `Command + Shift + Z`     | `Ctrl + Y`               |
| New File               | Create a new file                                                       | `Command + N`             | `Ctrl + N`               |
| Close File             | Close the current tab                                                   | `Command + W`             | `Ctrl + W`               |
| Save File              | Save the current file                                                   | `Command + S`             | `Ctrl + S`               |
| Save All               | Save all open files                                                     | `Command + Shift + S`     | `Ctrl + Shift + S`       |
| Find                   | Find text in the current file                                           | `Command + F`             | `Ctrl + F`               |
| Find Next              | Go to the next occurrence of the found text                             | `Command + G`             | `F3`                     |
| Find Previous          | Go to the previous occurrence of the found text                         | `Shift + Command + G`     | `Shift + F3`             |
| Find in All Files      | Find text across all files                                              | `Shift + Command + F`     | `Ctrl + Shift + F`       |
| Toggle Sidebar         | Show/hide the sidebar                                                   | `Command + B`             | `Ctrl + B`               |
| Quick Open File        | Search and open files by name                                           | `Command + P`             | `Ctrl + P`               |
| Command Palette        | Open Command Palette to run commands                                    | `Shift + Command + P`     | `Ctrl + Shift + P`       |

---

## **Level 2: Code Editing Enhancements**

| Shortcut                | Description                                                      | macOS                    | Windows (WSL)            |
| ----------------------- | ---------------------------------------------------------------- | ------------------------ | ------------------------ |
| Multi-Cursor Select     | Select word (repeat to select other occurrences)                 | `Command + D`            | `Ctrl + D`               |
| Undo Multi-Cursor Select | Undo a multi-cursor selection                                  | `Command + U`            | `Ctrl + U`               |
| Toggle Line Comment     | Comment or uncomment the selected lines                          | `Command + /`            | `Ctrl + /`               |
| Toggle Block Comment    | Toggle a block comment                                          | `Shift + Option + A`     | `Ctrl + Shift + A`       |
| Indent Lines            | Indent the selected lines                                       | `Command + ]`            | `Ctrl + ]`               |
| Outdent Lines           | Outdent the selected lines                                      | `Command + [`            | `Ctrl + [`               |
| New Line Below          | Start a new line below regardless of cursor position             | `Command + Enter`        | `Ctrl + Enter`           |
| New Line Above          | Start a new line above regardless of cursor position             | `Shift + Command + Enter`| `Ctrl + Shift + Enter`   |
| Move Line Up/Down       | Move the current line up or down                                | `Option + Up/Down Arrow` | `Alt + Up/Down Arrow`    |
| Copy Line Up/Down       | Copy the current line up or down                                | `Shift + Option + Up/Down Arrow` | `Shift + Alt + Up/Down Arrow` |
| Delete Line             | Delete the current line                                         | `Command + Shift + K`    | `Ctrl + Shift + K`       |
| Go to Definition        | Navigate to the definition of the symbol under the cursor       | `Command + Click` or `F12` | `Ctrl + Click` or `F12`|
| Peek Definition         | View definition inline without leaving the current file         | `Option + F12`           | `Alt + F12`              |
| Go to Symbol in File    | Search and go to a symbol within the current file               | `Command + Shift + O`    | `Ctrl + Shift + O`       |
| Show References         | Display references to the symbol under the cursor               | `Shift + F12`            | `Shift + F12`            |

---

## **Level 3: Advanced Navigation and Management**

| Shortcut                               | Description                                                       | macOS                          | Windows (WSL)                 |
| -------------------------------------- | ----------------------------------------------------------------- | ------------------------------ | ----------------------------- |
| Select All Occurrences                 | Select all instances of the current selection                     | `Command + Shift + L`          | `Ctrl + Shift + L`            |
| Insert Cursor at All Occurrences       | Place cursor at all occurrences of the current word               | `Command + F2`                 | `Ctrl + F2`                   |
| Split Editor                           | Split the editor into two columns                                 | `Command + \`                  | `Ctrl + \`                    |
| Zen Mode                               | Enter Zen mode (distraction-free)                                 | `Command + K`, then `Z`        | `Ctrl + K`, then `Z`          |
| Fold All                               | Fold all code regions                                             | `Command + K`, then `Command + 0` | `Ctrl + K`, then `Ctrl + 0` |
| Unfold All                             | Unfold all code regions                                           | `Command + K`, then `Command + J` | `Ctrl + K`, then `Ctrl + J` |
| Next Tab                               | Switch to the next tab                                            | `Command + Shift + ]`          | `Ctrl + Page Down`            |
| Previous Tab                           | Switch to the previous tab                                        | `Command + Shift + [`          | `Ctrl + Page Up`              |
| Close All Editors                      | Close all open editors                                            | `Command + K`, then `W`        | `Ctrl + K`, then `W`          |
| Focus Left/Right Editor                | Focus on the left/right editor pane                               | `Command + K`, then `Command + Left/Right Arrow` | `Ctrl + K`, then `Ctrl + Left/Right Arrow` |

---

## **Additional Useful Shortcuts**

| Shortcut                             | Description                                                          | macOS                       | Windows (WSL)               |
| ------------------------------------ | -------------------------------------------------------------------- | --------------------------- | --------------------------- |
| Run Code (Code Runner extension)     | Execute code in the active editor                                    | `Command + Option + N`      | `Ctrl + Alt + N`            |
| Open Integrated Terminal             | Open or toggle the integrated terminal                               | `` Control + ` ``           | `` Ctrl + ` ``              |
| Open External Terminal               | Open the external system terminal                                    | `Command + Shift + C`       | `Ctrl + Shift + C`          |
| Format Document                      | Format the entire document                                           | `Command + Option + F`      | `Shift + Alt + F`           |
| Format Selection                     | Format the selected code block                                       | `Command + K`, then `Command + F` | `Ctrl + K`, then `Ctrl + F` |
| Open Markdown Preview                | Preview the current Markdown file                                    | `Command + Shift + V`       | `Ctrl + Shift + V`          |
| Open Problems Panel                  | Show the Problems panel for errors and warnings                      | `Command + Shift + M`       | `Ctrl + Shift + M`          |
| Next Error                           | Go to the next error or warning                                     | `F8`                        | `F8`                        |
| Previous Error                       | Go to the previous error or warning                                 | `Shift + F8`                | `Shift + F8`                |
| Change Language Mode                 | Switch the language mode for the current file                        | `Command + K`, then `M`     | `Ctrl + K`, then `M`        |

---

## **Customizing Shortcuts**

- **View and Modify Keyboard Shortcuts:**
  - **macOS**: `Command + K`, then `S`
  - **Windows (WSL)**: `Ctrl + K`, then `S`

Open Keyboard Shortcuts settings to view and customize key bindings based on your preferences.

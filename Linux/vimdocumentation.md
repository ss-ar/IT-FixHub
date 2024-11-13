# Learning Vim

## Vim Installation

- **Debian**:  
  `sudo apt install vim-nox`

- **CentOS**:  
  `sudo dnf install vim-enhanced`

## Opening and Closing Vim

- **Opening Vim**:  
  Run the following command in your terminal:  
  `vim`

- **Closing Vim**:  
  `:q` + Enter

## Vim Modes

### Normal Mode
This is the default mode when Vim is opened. In this mode, you cannot type or edit text directly. You need to switch to **Insert Mode** to make changes.

### Insert Mode
To enable Insert Mode, press `i` after opening Vim. Now, you can edit or write text.

- **Alternative ways to enter Insert Mode**:  
  Press `A` to insert text at the end of the current line.

### Exiting Insert Mode
Press `Esc` to return to Normal Mode.

## Saving and Exiting in Vim

- **Exit without saving**:  
  `:q!`

  > Note: The colon (`:`) puts you into **Command Mode**.

- **Save changes**:  
  `:w`

- **Save to a new file**:  
  `:w newfile.txt`

- **Save and exit**:  
  `:wq`

## Undoing Changes
While in **Normal Mode**, you can undo changes by pressing `u`.

## Moving the Cursor

- `0` - Move to the beginning of the line.
- `$` - Move to the end of the line.

## Deleting Text in Vim

- `x` - Delete a character.
- `dd` - Delete an entire line.  
  > Note: The deleted line is stored in the paste buffer, so you can use `p` to paste it at the cursor position.

## Navigating in Normal Mode

- `h` - Move left.
- `j` - Move down.
- `k` - Move up.
- `l` - Move right.

## Reading Files into Vim
To read contents from another file into your current buffer:  
`:r /path/to/file`

## Running Linux Commands in Vim
You can execute Linux commands without leaving Vim:

- Example:  
  `:! sudo apt update`  
  `:! ls`

## Buffers in Vim
A buffer in Vim is like a holding area for text. A buffer becomes a file when saved. Until then, it's temporary.

### Editing Another Buffer
To open a new file while working on another buffer:  
`:e filename`

### Switching Between Buffers
- `:bn` - Switch to the next buffer.
- `:bp` - Switch to the previous buffer.

> Note: Ensure you save changes (`:w`) before switching buffers.

### Creating a New Buffer
To create a new empty buffer:  
`:enew`

If no changes are made in the new buffer, it will be discarded when you switch back. If you edit the new buffer, save it:  
`:w newname.txt`

### Closing or Deleting a Buffer
To close a buffer:  
`:bd`

### Adding a Buffer Without Switching to It
You can add a buffer in the background without switching to it:  
`:badd filename`

## Vim Visual Mode
Press `v` while in **Normal Mode** to enter **Visual Mode**.

### Copying and Pasting in Visual Mode

1. Place the cursor at the starting point of the text you want to copy.
2. Press `v` to enter **Visual Mode**.
3. Use the arrow keys (`h`, `j`, `k`, `l`) or the arrow keys to select the text.
4. Press `y` to copy the text.
5. Navigate to the desired location and press `p` to paste.

> Note: Pressing `p` multiple times pastes the text repeatedly.

### Sorting Text in Visual Mode
Once text is selected, press `:` to access **Command Mode** for sorting or other operations.

### Sorting Options
- `:sort` - Sort alphabetically.
- `:sort i` - Ignore case while sorting.
- `:sort n` - Sort numerically.
- `:sort r` - Sort in reverse order.
- `:sort u` - Remove duplicate lines.

## Finding and Replacing Text
You can perform a search and replace operation with:  
`:%s/search_term/replacement/g`

## Moving to the Beginning and End of a File
- `gg` - Move to the top of the file.
- `G` - Move to the bottom of the file.

## Splitting Windows

### Horizontal Split
To open another file in a horizontally split window:  
`:split filename`

You can also use the shorthand:  
`:sp filename`

### Vertical Split
To split the window vertically:  
`:vsplit filename`

You can also use the shorthand:  
`:vs filename`

### Moving Between Split Windows
Press `Ctrl + ww` to switch between windows.

### Opening the Same File in Multiple Windows
Vim allows you to open the same file in different windows. Changes made in any window will reflect in all windows.

### Opening Multiple Files in Split Mode

- **Horizontal Split**:  
  `vim -o file1 file2`

- **Vertical Split**:  
  `vim -O file1 file2`

## Line Numbers in Vim

- **Enable line numbers**:  
  `:set number`

- **Disable line numbers**:  
  `:set nonumber`

> Note: This change is temporary and will be lost when Vim is closed.

## Opening Vim at a Specific Line
To open Vim and place the cursor at a specific line number:  
`vim +20 filename`

## Making Permanent Configurations in `.vimrc`
The `.vimrc` file stores your Vim configurations and is loaded every time Vim is opened. If the file doesn’t exist, you can create it in your home directory.

### Creating `.vimrc`
Open a new buffer for your `.vimrc` file:  
`vim ~/.vimrc`

### Sample `.vimrc` Configuration
```vim
" Enable line numbers
set number


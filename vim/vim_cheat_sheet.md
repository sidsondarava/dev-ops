# Comprehensive Vim Command Cheat Sheet 🧭

A practical, field-tested reference guide covering Vim from survival basics to advanced modal navigation, buffer management, macros, and configuration.

---

## 📑 Table of Contents
1. [The Modal Philosophy](#1-the-modal-philosophy)
2. [Starting & Exiting Vim](#2-starting--exiting-vim)
3. [Cursor Navigation (Normal Mode)](#3-cursor-navigation-normal-mode)
4. [Entering Insert & Replace Modes](#4-entering-insert--replace-modes)
5. [Editing, Deleting & Changing Text](#5-editing-deleting--changing-text)
6. [Copying (Yanking) & Pasting (Putting)](#6-copying-yanking--pasting-putting)
7. [Undo, Redo & Repeat Actions](#7-undo-redo--repeat-actions)
8. [Visual Mode (Text Selection)](#8-visual-mode-text-selection)
9. [Searching & Pattern Matching](#9-searching--pattern-matching)
10. [Search & Replace (Substitutions)](#10-search--replace-substitutions)
11. [Working with Multiple Files (Buffers, Windows & Tabs)](#11-working-with-multiple-files-buffers-windows--tabs)
12. [Registers & Macros (Automation)](#12-registers--macros-automation)
13. [Folds, Marks & Jumps](#13-folds-marks--jumps)
14. [Useful Ex Commands & Shell Integration](#14-useful-ex-commands--shell-integration)
15. [Essential `.vimrc` Configuration Starter](#15-essential-vimrc-configuration-starter)

---

## 1. The Modal Philosophy

Unlike conventional modeless editors, Vim behaves like an editor with distinct operational gears:

```text
               ┌───────────────┐
  ┌───────────>│  NORMAL MODE  │<──────────────┐
  │            │ (Navigation/  │               │
  │            │   Commands)   │               │
  │            └───────┬───────┘               │
  │                    │                       │
[Esc]           [i, a, o, s, c]              [Esc]
  │                    │                       │
  │                    v                       │
┌─┴────────────┐       │               ┌───────┴──────┐
│ COMMAND MODE │       └──────────────>│ INSERT MODE  │
│ (: commands) │                       │ (Text Entry) │
└──────────────┘                       └──────────────┘
       ^                                       ^
       │                 [v, V, Ctrl-v]        │
       └───────────────────────┬───────────────┘
                               v
                       ┌───────────────┐
                       │  VISUAL MODE  │
                       │  (Selection)  │
                       └───────────────┘
```

- **Normal Mode**: The default command deck for navigating, deleting, copying, and manipulating text.
- **Insert Mode**: Typing plain text directly into the buffer.
- **Visual Mode**: Highlighting continuous text blocks, full lines, or vertical columns.
- **Command-line (Ex) Mode**: Performing file saves, regex search/replace, and environment settings.

> 💡 **Golden Rule**: Whenever you are unsure of your current state, press `Esc` twice to return safely to **Normal Mode**.

---

## 2. Starting & Exiting Vim

### Launching from the Shell

```bash
# Open an existing file or start a new empty file
vim filename.txt

# Open directly at line number 42
vim +42 main.cpp

# Open at the first occurrence of a pattern
vim +/search_term config.yaml

# Open in read-only mode (view only)
vim -R /var/log/syslog

# Open multiple files in vertical split windows
vim -O file1.cpp file2.cpp

# Run interactive built-in tutorial (takes ~25 minutes)
vimtutor
```

### Exiting Vim (Command-line Mode)

Press `Esc` first to ensure you are in Normal Mode, then type:

| Command | Action |
| :--- | :--- |
| `:w` | Write (save) changes to disk |
| `:w filename` | Save buffer under a new filename |
| `:q` | Quit (fails if unsaved changes exist) |
| `:q!` | Force quit **without** saving modifications |
| `:wq` or `:x` | Write changes and quit |
| `ZZ` | Equivalent to `:x` (write and exit) from Normal Mode |
| `ZQ` | Equivalent to `:q!` (quit discard changes) from Normal Mode |
| `:w !sudo tee %` | Force write a protected root-owned file without restarting Vim |

---

## 3. Cursor Navigation (Normal Mode)

Keep your fingers stationed comfortably on the keyboard home row.

### Basic Directional Movement

```text
             ▲
             k (Up)
             │
◄── h (Left) ┼ d (Down) ──► l (Right)
             │
             ▼
```

### Word & Sentence Jumps

| Key | Movement Description |
| :--- | :--- |
| `w` | Forward to the start of the next alphanumeric word |
| `W` | Forward to the start of the next space-delimited WORD |
| `b` | Backward to the start of the previous word |
| `B` | Backward to the start of the previous space-delimited WORD |
| `e` | Forward to the end of the current or next word |
| `E` | Forward to the end of the next space-delimited WORD |
| `ge` | Backward to the end of the previous word |

### Line & Screen Positioning

| Key | Description |
| :--- | :--- |
| `0` | Move to the absolute first character of the line |
| `^` | Move to the first non-blank character of the current line |
| `$` | Move to the end of the current line |
| `g_` | Move to the last non-blank character of the current line |
| `gg` | Jump to the very first line of the document |
| `G` | Jump to the very last line of the document |
| `50G` or `:50` | Jump directly to line number 50 |
| `H` | Jump to the High position (top visible line on screen) |
| `M` | Jump to the Middle visible line on screen |
| `L` | Jump to the Low position (bottom visible line on screen) |
| `%` | Jump to matching parenthesis `()`, bracket `[]`, or brace `{}` |

### Scrolling

| Key | Action |
| :--- | :--- |
| `Ctrl + f` | Scroll down one full screen |
| `Ctrl + b` | Scroll up one full screen |
| `Ctrl + d` | Scroll down half a screen |
| `Ctrl + u` | Scroll up half a screen |
| `zz` | Center current cursor line in the middle of the viewport |
| `zt` | Scroll screen so cursor line is at the top |
| `zb` | Scroll screen so cursor line is at the bottom |

---

## 4. Entering Insert & Replace Modes

Transitioning from **Normal Mode** into typing modes:

| Key | Insertion Position |
| :--- | :--- |
| `i` | Insert immediately **before** the cursor |
| `I` | Insert at the beginning of the line (first non-whitespace character) |
| `a` | Append immediately **after** the cursor |
| `A` | Append at the absolute **end** of the current line |
| `o` | Open a new line **below** the cursor and enter Insert mode |
| `O` | Open a new line **above** the cursor and enter Insert mode |
| `s` | Substitute single character under cursor and enter Insert mode |
| `S` | Delete entire current line and enter Insert mode |
| `R` | Enter **Replace Mode** (overwrites text character-by-character) |

---

## 5. Editing, Deleting & Changing Text

Vim commands follow a clean grammatical syntax:  
`[Count] + [Operator] + [Motion / Text Object]`

### Common Operators
- `d` = Delete (cut)
- `c` = Change (delete and switch immediately to Insert mode)
- `y` = Yank (copy)

### Deletion & Change Commands

| Command | Action |
| :--- | :--- |
| `x` | Delete character under cursor |
| `X` | Delete character to the left of cursor |
| `dw` | Delete from cursor to start of next word |
| `de` | Delete from cursor to end of current word |
| `d$` or `D` | Delete from cursor to the end of the line |
| `d0` | Delete from cursor to the start of the line |
| `dd` | Delete (cut) entire current line |
| `3dd` | Delete 3 consecutive lines downward |
| `cw` | Change current word from cursor onward |
| `cc` | Change (replace) the entire line |
| `C` | Change from cursor to line end (`c$`) |
| `r<char>` | Replace single character under cursor with `<char>` (stays in Normal mode) |
| `~` | Toggle case of character under cursor |

### Text Objects (The Superpower)
Use `i` (inner) or `a` (around/all) with an operator (`d`, `c`, `y`):

```text
ci"   -> Change text inside quotes:      "hello world"  =>  ""
ca(   -> Change around parentheses:      (int a, int b) =>  deletes braces too
diw   -> Delete inner word (cursor anywhere inside the word)
daw   -> Delete word including trailing whitespace
ci{   -> Change block inside curly braces {}
dit   -> Change inside HTML/XML tags
```

---

## 6. Copying (Yanking) & Pasting (Putting)

In Vim terminology: **Yank** = Copy, **Put** = Paste.

| Command | Action |
| :--- | :--- |
| `yy` or `Y` | Yank (copy) the entire current line |
| `3yy` | Yank 3 lines downward |
| `yw` | Yank from cursor to the start of the next word |
| `y$` | Yank to the end of the line |
| `yi"` | Yank inside double quotes |
| `p` | Paste clipboard/register contents **after** cursor or below line |
| `P` | Paste clipboard/register contents **before** cursor or above line |
| `"ap` | Paste text specifically stored in named register `a` |
| `"+p` | Paste from the operating system system clipboard (if compiled with clipboard support) |

---

## 7. Undo, Redo & Repeat Actions

| Command | Action |
| :--- | :--- |
| `u` | Undo last change |
| `Ctrl + r` | Redo last undone change |
| `.` (dot) | Repeat the last editing change (invaluable for iterative refactoring) |
| `U` | Undo all recent changes on the current line |

---

## 8. Visual Mode (Text Selection)

Visual mode lets you highlight text before executing an operator (`d`, `y`, `c`, `>`, etc.).

| Trigger | Mode Type |
| :--- | :--- |
| `v` | Character-wise Visual Mode |
| `V` | Line-wise Visual Mode |
| `Ctrl + v` | Block-wise (Column) Visual Mode |
| `gv` | Re-select the last visual selection |
| `o` | Move cursor to the opposite end of the active visual block |

### Column Editing with Visual Block Mode (`Ctrl + v`)
To prepend comments or indents across 10 lines at once:
1. Position cursor at the beginning of the top line.
2. Press `Ctrl + v` to enter Visual Block mode.
3. Move down (`j` or `10j`) to highlight the vertical column.
4. Press `Shift + i` (`I`) to enter block insertion mode.
5. Type your comment prefix (e.g., `// ` or `# `).
6. Press `Esc`. Vim will instantly propagate the edit across all selected lines.

---

## 9. Searching & Pattern Matching

### Search Commands

| Key | Action |
| :--- | :--- |
| `/pattern` | Search forward for `pattern` |
| `?pattern` | Search backward for `pattern` |
| `n` | Repeat search in the same direction |
| `N` | Repeat search in the opposite direction |
| `*` | Search forward for the word currently under the cursor |
| `#` | Search backward for the word currently under the cursor |
| `:noh` or `:nohlsearch` | Clear highlighting of active search matches |

### In-Line Navigation

| Key | Action |
| :--- | :--- |
| `fx` | Find and jump forward to the next occurrence of character `x` on current line |
| `Fx` | Find and jump backward to the previous occurrence of character `x` |
| `tx` | Move forward **till** just before character `x` |
| `Tx` | Move backward **till** just after character `x` |
| `;` | Repeat last `f`, `F`, `t`, or `T` forward |
| `,` | Repeat last `f`, `F`, `t`, or `T` backward |

---

## 10. Search & Replace (Substitutions)

Syntax: `:[range]s/{pattern}/{replacement}/[flags]`

### Substitution Examples

```vim
" Replace first occurrence of 'foo' with 'bar' on current line
:s/foo/bar/

" Replace all occurrences of 'foo' with 'bar' on current line
:s/foo/bar/g

" Replace all occurrences in the entire file
:%s/foo/bar/g

" Replace all occurrences with interactive confirmation prompt [y/n/a/q]
:%s/foo/bar/gc

" Replace occurrences between line 10 and line 50
:10,50s/foo/bar/g

" Case-insensitive replacement
:%s/foo/bar/gi

" Delete all lines containing 'DEBUG'
:g/DEBUG/d

" Delete all empty or whitespace-only lines
:g/^\s*$/d
```

---

## 11. Working with Multiple Files

### Buffers (In-Memory Files)

| Command | Action |
| :--- | :--- |
| `:ls` or `:buffers` | List all open in-memory buffers |
| `:b <num>` or `:b <name>`| Switch to buffer number or matching name (e.g., `:b main.cpp`) |
| `:bn` | Jump to next buffer |
| `:bp` | Jump to previous buffer |
| `:bd` | Delete current buffer (close file without quitting Vim) |

### Split Windows

```vim
" Split current window horizontally
:split or :sp [filename]

" Split current window vertically
:vsplit or :vsp [filename]
```

#### Window Navigation
- `Ctrl + w`, followed by `h` / `j` / `k` / `l` : Move to Left / Down / Up / Right window
- `Ctrl + w`, followed by `c` : Close active split window
- `Ctrl + w`, followed by `o` : Close all windows except current active window
- `Ctrl + w`, followed by `=` : Equalize sizes of all open split windows
- `Ctrl + w`, followed by `+` / `-` : Increase / Decrease window height

### Tabs

| Command | Action |
| :--- | :--- |
| `:tabnew [file]` | Open a new tab |
| `gt` | Go to next tab |
| `gT` | Go to previous tab |
| `:tabclose` | Close active tab |

---

## 12. Registers & Macros (Automation)

### Registers (Vim Clipboards)
Vim maintains multiple clipboard buckets called registers:
- `""` : Default unnamed register (receives any `d`, `x`, `y`).
- `"0` : Yank register (holds only text copied via `y`, unaffected by subsequent deletes).
- `"a` through `"z` : 26 named registers to store text deliberately.
- `"+` : System clipboard (access via `"+y` to copy to OS, `"+p` to paste into Vim).

### Macros (Recording Sequences)
Automate repetitive repetitive editing tasks:

```text
1. Press 'q' followed by a register letter (e.g., 'qa') to begin recording into register 'a'.
   (The status line shows "recording @a")
2. Perform your keyboard edits.
3. Press 'q' in Normal mode to stop recording.
4. Execute macro once:            @a
5. Repeat macro 10 times:        10@a
6. Re-run the last executed macro: @@
```

---

## 13. Folds, Marks & Jumps

### Code Folding

| Command | Action |
| :--- | :--- |
| `zf` | Create fold over visual selection |
| `zo` | Open fold under cursor |
| `zc` | Close fold under cursor |
| `za` | Toggle fold open/closed |
| `zR` | Open all folds throughout the buffer |
| `zM` | Close all folds throughout the buffer |

### Bookmarks (Marks) & Jumps

| Command | Action |
| :--- | :--- |
| `ma` | Set local mark `a` at current cursor position |
| `'a` | Jump to line of mark `a` |
| `` `a `` | Jump to exact line and column of mark `a` |
| `mA` | Set global mark `A` (accessible across different files/buffers) |
| `Ctrl + o` | Jump back to older cursor location in the jump list |
| `Ctrl + i` | Jump forward to newer cursor location |
| `''` | Jump back to the position before the previous jump |

---

## 14. Useful Ex Commands & Shell Integration

Execute terminal commands and manipulate text without leaving Vim:

```vim
" Execute an external shell command and view output
:!ls -la
:!make

" Read external shell output into current buffer below cursor
:r !git branch --show-current
:r !date

" Format entire file with external tool (e.g., clang-format or jq)
:%!jq .

" Open an embedded terminal inside a split window (Vim 8.1+)
:terminal
```

---

## 15. Essential `.vimrc` Configuration Starter

Create a file at `~/.vimrc` to make Vim behave ergonomically by default:

```vim
" ~/.vimrc - Clean, Modern Vim Setup

" General Defaults
set nocompatible              " Be iMproved, don't emulate strict vi
filetype plugin indent on     " Enable filetype detection, plugins, and indentation
syntax on                     " Enable syntax highlighting

" Interface & Line Numbers
set number                    " Show absolute line numbers
set relativenumber            " Show relative line numbers for quick jumping
set cursorline                " Highlight current line
set showcmd                   " Show incomplete commands in bottom bar
set showmode                  " Display current mode (-- INSERT --)
set scrolloff=5               " Keep 5 lines above/below cursor when scrolling

" Indentation & Tabs (2 or 4 spaces)
set tabstop=4                 " Number of visual spaces per TAB
set softtabstop=4             " Number of spaces in tab when editing
set shiftwidth=4              " Indent size for >> and <<
set expandtab                 " Convert tabs to spaces
set autoindent
set smartindent

" Searching
set incsearch                 " Show search matches as you type
set hlsearch                  " Highlight all search matches
set ignorecase                " Case-insensitive searching
set smartcase                 " Case-sensitive if uppercase letters are typed

" Splits & Windows
set splitbelow                " Horizontal splits open below
set splitright                " Vertical splits open to the right

" Performance & Encoding
set encoding=utf-8            " Standard UTF-8 encoding
set backspace=indent,eol,start" Allow backspacing over everything in insert mode
set hidden                    " Allow switching buffers without saving first

" Quick Shortcut: Clear search highlights with space bar
nnoremap <Space> :nohlsearch<CR>
```

---

*Keep this reference close until the muscle memory takes over!*
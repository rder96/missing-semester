# Editors (Vim)
- default mode: normal 
  - (i) insert
  - (R) replace
  - selection
    - (v) visual
    - (shift-v) line
    - (ctrl-v) block
  - (:) command (vim command shell)
    - to quit the editor(close current window) - :q/quit
      - to quit all opened windows - :qa
    - to save(write) - :w
    - doc - :help :w different from :help w (command vs just w movement)
    - new split window(panes) - :sp
    - new tab - :tabnew
    - :ls show open buffers

1. movements (nouns - refer to chunks of text)
   - hjkl (left, down, up, right)
   - w (next word), b (beginning of word), e (end of word)
   - 0 (beginning of line), ^ (first non-blank character), $ (end of line)
   - H (top of screen), M (middle of screen), L (bottom of screen)
   - Scroll: Ctrl-u (up), Ctrl-d (down)
   - gg (beginning of file), G (end of file)
   - Find: f{character}, t{character}, F{character}, T{character}
     find/to forward/backward {character} on the current line
   - Search: /regex, n / N for navigating matches

2. Editing mode commands (at normal mode) (verbs - because verbs act on nouns)
   - from normal mode, press o -> insert mode + newline below
   - press O -> insert mode + newline above
   - u -> undo
   - ^R -> redo
   - y + movement commands -> copy
     - yy -> copy the line 
     - yw -> copy the word
   - p -> paste
   - d + movement commands 
     - dw -> delete a word
     - de -> delete end of word
     - dd -> delete line
   - c + movement commands (change) -> delete + insert mode
     - ce (change to end of word)
     - cc (delete line) + insert mode
   - x -> delete character
   - r + arg -> replace with arg
   - ~ flips the case of a char

3. visual modes 
- (shift-v) line of text visual 
- (ctrl-v) rectangular block visual

4. Counts (number + commands)
- 7dw -> delete 7 words
- 3hjkl -> move 3 lines left/down/up/right
- c2w/2cw -> change 2 words

5. modifiers (useful in .md)
- a (around/including)
  - da( (delete inside and parenthsis/around parenthesis)
- i (inside)
  - [something] - ci[ (change inside brackets)
  - (something) - di( (delete inside parenthesis)
- to move back-forth matching parenthesis etc -> %

6. others
- / to search
- $ end of line
- . repeat preious editting command made
- x in normal mode, delete char; in visual mode, delete selection
- enable vim mode for different programs - bash/python/etc.
- set of open files, called "buffers" - each window shows a single buffer. A buffer may be open in multiple windows/ within same tab (handy to view different parts of a file at the same time)
- vim plugins
- macros

# Exercises

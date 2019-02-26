---
layout: post
title: "Personal dev-settings for Linux"
category: dev
tags: dev_env
---

What I'm going to explain is my own dev-settings for Linux, especially for Ubuntu.
Over several times to re-install Ubuntu, I realized the need to write down all the things that I'd done.

## 1. Things about terminal
### 1-1. Terminator
You can install terminator at the terminal. Type the command below.<br>
```
sudo apt-get install terminator
```
After installing the app, you have to install [terminator themes](https://github.com/EliverLara/terminator-themes).<br>
We'll use `onedark` theme. If you installed the themes, Let's move on.<br><br><br>
The next step is for configuration of the app. Make or edit the file: `~/.config/terminator/config` and then paste the commands to it.
```vim
[global_config]
  enabled_plugins = TerminatorThemes, LaunchpadCodeURLHandler, APTURLHandler, LaunchpadBugURLHandler
  handle_size = 1
  window_state = maximise
[keybindings]
  close_term = <Primary><Shift>w
  new_tab = <Primary><Shift>t
  switch_to_tab_1 = <Primary>1
  switch_to_tab_10 = <Primary>0
  switch_to_tab_2 = <Primary>2
  switch_to_tab_3 = <Primary>3
  switch_to_tab_4 = <Primary>4
  switch_to_tab_5 = <Primary>5
  switch_to_tab_6 = <Primary>6
  switch_to_tab_7 = <Primary>7
  switch_to_tab_8 = <Primary>8
  switch_to_tab_9 = <Primary>9
[layouts]
  [[default]]
    [[[child1]]]
      parent = window0
      profile = default
      type = Terminal
    [[[window0]]]
      parent = ""
      type = Window
[plugins]
[profiles]
  [[default]]
    background_color = "#282c34"
    background_darkness = 1.0
    cursor_color = "#676c76"
    font = Ubuntu Mono 14
    foreground_color = "#abb2bf"
    palette = "#000000:#e06c75:#98c379:#d19a66:#61afef:#c678dd:#56b6c2:#abb2bf:#5c6370:#e06c75:#98c379:#d19a66:#61afef:#c678dd:#56b6c2:#fffefe"
    scrollbar_position = hidden
    show_titlebar = False
```
The next step is to eliminate border of the app. Make a file:` ~/.config/gtk-3.0/gtk.css` to contain the commands below.
```vim
@deifine-color border #282c34;

.terminator-terminal-window notebook header, .terminator-terminal-window stack {
    border: 0;
}

.terminator-terminal-window notebook {
    border: 0;
	padding: 0;
}

.terminator-terminal-window decoration {
    border: 1px solid shade(@border, 1);
	background: shade(@border, 1);
}
```
Finally, open the app <em>settings</em> and go to the keyboard menu. `Ctrl+Alt+T` launches terminal as default. Disable it, and make a new shortcut as same as the key combination before. The command should be `terminator`. Then the shortcut will launch the app.
</details><br>

### 1-2. Zsh & Oh-my-zsh
```
sudo apt-get install zsh zsh-completions
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" # via curl
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)" # via wget
```
After the installation, commit
```
chsh -s /usr/bin/zsh
```
to set zsh as the default shell. Then, you can set the zsh configuration via `~/.zshrc` file. The code below is an example.
```vim
export PATH=$HOME/bin:/usr/local/bin:$PATH
export ZSH="/home/blankymunn/.oh-my-zsh"

ZSH_THEME="robbyrussell"
plugins=(
git
zsh-syntax-highlighting
zsh-autosuggestions
)

source $ZSH/oh-my-zsh.sh
export PATH="/home/blankymunn/anaconda3/bin:$PATH"

export EDITOR=/usr/local/bin/nvim

alias vi='nvim'
alias vim='nvim'
alias vimdiff="nvim -d"
export PWS='/home/blankymunn/Desktop/computer-vision/python'
export CV='/home/blankymunn/Desktop/computer-vision'
export GIT='/home/blankymunn/Desktop/computer-vision/git'
source /home/blankymunn/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
# Install Ruby Gems to ~/gems
export GEM_HOME="$HOME/gems"
export PATH="$HOME/gems/bin:$PATH"
```
If you've been using bash shell, you can synchronize your `~/.bashrc` or `~/.bash_profile` with `~/.zshrc`.
```
ln -s $SOURCE $TARGET
```
The file at `$TARGET` references the source file at `$SOURCE`. This means all the contents of `$TARGET` file is replaced with the contents of the file at `$SOURCE`. You can check this out [here](https://askubuntu.com/questions/56339/how-to-create-a-soft-or-symbolic-link).

### 1-3. Neovim
```
sudo apt-get install neovim
```
You can set the configuration of neovim at `~/.config/nvim/init.vim`. If the file doesn't exist, just make one.
```vim
"installing Vim Plug-in Using Vim-Plug
call plug#begin('~/.config/nvim/plugged')
Plug 'sheerun/vim-polyglot'
Plug 'https://github.com/joshdick/onedark.vim.git'
Plug 'scrooloose/nerdtree'
Plug 'Raimondi/delimitMate'
"Plug 'neomake/neomake'
"Plug 'dojoteef/neomake-autolint'
Plug 'vim-syntastic/syntastic'
Plug 'romainl/vim-qf'
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'Yggdroot/indentLine'
Plug 'suan/vim-instant-markdown'
call plug#end()

"Use 24-bit (true-color) mode in Vim/Neovim when outside tmux.
"If you're using tmux version 2.2 or later, you can remove the outermost $TMUX check and use tmux's 24-bit color support
"(see < http://sunaku.github.io/tmux-24bit-color.html#usage > for more information.)
if (empty($TMUX))
  if (has("nvim"))
    "For Neovim 0.1.3 and 0.1.4 < https://github.com/neovim/neovim/pull/2198">
    let $NVIM_TUI_ENABLE_TRUE_COLOR=1
  endif
  "For Neovim > 0.1.5 and Vim > patch 7.4.1799 < https://github.com/vim/vim/commit/61be73bb0f965a895bfb064ea3e55476ac175162 >
  "Based on Vim patch 7.4.1770 (`guicolors` option) < https://github.com/vim/vim/commit/8a633e3427b47286869aa4b96f2bfc1fe65b25cd >
  " < https://github.com/neovim/neovim/wiki/Following-HEAD#20160511 >
  if (has("termguicolors"))
    set termguicolors
  endif
endif
 
let base16colorspace=256
 
"Configuration for lightline
"let g:lightline = {
"  \ 'colorscheme': 'onedark',
"  \ }
 
let g:lightline = {
      \ 'colorscheme': 'onedark',
      \ 'component_function': {
      \   'filename': 'LightlineFilename',
      \ },
      \ }
 
function! LightlineFilename()
  return &filetype ==# 'vimfiler' ? vimfiler#get_status_string() :
        \ &filetype ==# 'unite' ? unite#get_status_string() :
        \ &filetype ==# 'vimshell' ? vimshell#get_status_string() :
        \ expand('%:t') !=# '' ? expand('%:t') : '[No Name]'
endfunction
 
let g:unite_force_overwrite_statusline = 0
let g:vimfiler_force_overwrite_statusline = 0
let g:vimshell_force_overwrite_statusline = 0
 
"NEOVIM Syntax allowed
syntax on
 
"NEOVIM Theme
colorscheme onedark
 
set bs=2  "allow backspacing over everything in insert mode
set ruler
set number
set noshowmode
set autoindent
set smartindent
set noswapfile
set clipboard+=unnamedplus
autocmd FileType python setlocal tabstop=4
set tabstop=4
set softtabstop=0 noexpandtab
set shiftwidth=4
let python_version_3=1
let python_highlight_all=1

let mapleader = ","
nnoremap <leader>1 :bp<CR>
nnoremap <leader>2 :bn<CR>
"nnoremap <leader>q :bp<CR> | :bd!<CR>

"delimitMate
let delimitMate_expand_cr=1

"Nerd Tree
autocmd VimEnter * NERDTree
let NERDTreeShowLineNumbers=1
let NERDTreeWinSize = 17
autocmd VimEnter * wincmd p
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif

"Neomake
"call neomake#configure#automake('nrwi')
"let neomake_list_height = 8
"let neomake_open_list = 2

"Vim-qf
augroup my_neomake_qf
    autocmd!
    autocmd QuitPre * if &filetype !=# 'qf' | lclose | endif
augroup END

"Vim-airline
let g:airline#extensions#tabline#enabled = 1 " turn on buffer list
let g:airline_theme='onedark'
set laststatus=2 " turn on bottom bar
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'
let g:airline#extensions#tabline#formatter = 'unique_tail'

"indentLine
let g:indentLine_enabled = 1
let g:indentLine_showFirstIndentLevel = 1
let g:indentLine_char = '│'
let g:indentLine_first_char = '│'

"Syntastic
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*
let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 3
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0
let g:syntastic_loc_list_height = 8

"vim-instant-markdown
filetype plugin on
```
### 1-4. Vim-plug
You can install by commiting this commend.
```
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```
You can check more about this manager at [here](https://github.com/junegunn/vim-plug).

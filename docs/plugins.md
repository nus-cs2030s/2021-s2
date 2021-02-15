## Vim Plugins/Extensions on PE Hosts

We allow the following plugins on the PE nodes during the practical exams.  

Note that for your labs, you are free to install any plugins you find useful.

## Background

`vim` plugins are installed under `~/.vim/pack/plugins/start`.  We will auto-create this directory for you in the exam environment.

`vim` colorschemes are installed under `~/.vim/colors`.  You put `<scheme>.vim` under `~/.vim/colors` directly.

## Color Schemes

We have installed the [Sweeter-than-Fiction](https://vimawesome.com/plugin/vim-colorschemes-sweeter-than-fiction) colorscheme bundle under `~cs2030s/.vim/colors`.

To use this, run
```
ln -s ~cs2030s/.vim/vim-colorschemes/colors ~/.vim/colors
```

After that, your can change your vim color scheme as usual. 
```
:color gruvbox
```

The bundle includes some of the popularly requested color schemes, such as `monokai`, `solarized`, and `gruvbox`

## Approved Plugins

### DelimitMate

`DelimitMate` is a plugin that automatically inserts a closing `}`, `)`, `>`, etc when you type the opening symbol.

To install this plugin in the exam environment, run
```
ln -s ~cs2030s/.vim/pack/plugins/start/delimitMate ~/.vim/pack/plugins/start
```

### Vim-Rainbow

`vim-rainbow` is a plugin that matches opening and closing brackets and colors the matching pairs with matching colors.

To install this plugin in the exam environment, run
```
ln -s ~cs2030s/.vim/pack/plugins/start/vim-rainbow ~/.vim/pack/plugins/start
```

The following line will be added on your `~/.vimrc` in the environment:

```
let g:rainbow_active = 1
```

### NERDTree

NERDTree provides a file browsing pane on the left (activated with `:NERDTree`).

To install this plugin in the exam environment, run
```
ln -s ~cs2030s/.vim/pack/plugins/start/nerdtree ~/.vim/pack/plugins/start
```

### Lightline

Lightline provides a more useful status line for `vim`.

To install this plugin in the exam environment, run
```
ln -s ~cs2030s/.vim/pack/plugins/start/lightline ~/.vim/pack/plugins/start
```

## Unavailable Plugins

The following plugins are not allowed as they provide too much help for writing Java programs.

- `coc`
- `syntastic`
- `youcompleteme`


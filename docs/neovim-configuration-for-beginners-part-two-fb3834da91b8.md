# Neovim 初学者配置—第二部分

> 原文：<https://medium.com/geekculture/neovim-configuration-for-beginners-part-two-fb3834da91b8?source=collection_archive---------7----------------------->

![](img/d40063ad10af91319fa822e759acc1d0.png)

插件让已经很棒的编辑器变得更棒。根据您的需要，vim 有许多可用的插件，这有助于提高您的生产率。

这将是我上一篇文章的延续。如果你还没有检查过，就去那里，它会值得你花时间。

[](/geekculture/neovim-configuration-for-beginners-b2116dbbde84) [## 面向初学者的 Neovim 配置

### 如果您可能认为 vim 是一个旧的、过时的文本编辑器，具有很高的学习曲线和有限的…

medium.com](/geekculture/neovim-configuration-for-beginners-b2116dbbde84) 

为了方便使用，我们将配置一些在早期文章中讨论过的插件。

绝对每个人都在他们的代码中写注释，NerdCommenter 是一个流行的注释插件。我为 NerdCommenter 使用的配置是

```
“ Create default mappings
let g:NERDCreateDefaultMappings = 1“ Add spaces after comment delimiters by default
let g:NERDSpaceDelims = 1“ Use compact syntax for prettified multi-line comments
let g:NERDCompactSexyComs = 1“ Align line-wise comment delimiters flush left instead of following code indentation
let g:NERDDefaultAlign = ‘left’“ Set a language to use its alternate delimiters by default
let g:NERDAltDelims_java = 1“ Add your own custom formats or override the defaults
let g:NERDCustomDelimiters = { ‘c’: { ‘left’: ‘/**’,’right’: ‘*/’ }}“ Allow commenting and inverting empty lines (useful when commenting a region)
let g:NERDCommentEmptyLines = 1“ Enable trimming of trailing whitespace when uncommenting
let g:NERDTrimTrailingWhitespace = 1“ Enable NERDCommenterToggle to check all selected lines is commented or not 
let g:NERDToggleCheckAllLines = 1“ for motions
nnoremap <silent> <leader>c} V}:call NERDComment(‘x’, ‘toggle’)<CR>
nnoremap <silent> <leader>c{ V{:call NERDComment(‘x’, ‘toggle’)<CR>
```

NerdCommenter 最有用的默认键绑定是*<leader><c-space>*，它根据第一个选中的行切换注释。

另一个伟大的插件是 *vim-startify* ，这是一个可定制的 vim 开始屏幕。即使它有 MRU 列表(最近使用)，添加自定义书签有时是有用的。您可以轻松地将它们添加为

```
let g:startify_bookmarks = ['~/.bashrc','~/Documents/']
```

而 startify 屏幕中的任何文件/目录都可以通过输入相应的数字轻松打开。

现在让我们定义代码片段的目录(UltSnips 是代码片段引擎)，

```
let g:UltiSnipsSnippetDirectories=[$HOME.’/.config/nvim/UltiSnips’]
```

特定语言的代码片段可以放在这里，例如 cpp(C++)

将文件保存为 cpp。文件的片段和内容应采用以下格式

```
snippet inc "snippet name"
#include <iostream>
$1
endsnippet
```

现在让我们转到完成引擎 Coc。Coc 本身没有任何用处，我们应该对它进行扩展。

```
let g:coc_global_extensions = [
             \ 'coc-pairs',
             \ 'coc-snippets',
             \ 'coc-html',
             \ 'coc-tsserver',
             \ 'coc-css',
             \ 'coc-clangd',
             \ 'coc-json',
             \ 'coc-pyright',
             \ 'coc-sh',
             \ 'coc-flutter',
             \ ]
```

然后你应该运行命令*:co install*来安装这些扩展。

要实现类似于 VSCode 的制表符结束，

```
set signcolumn=yes
inoremap <silent><expr> <TAB>
   \ pumvisible() ? coc#_select_confirm() :
   \ coc#expandableOrJumpable() ?
   \ "\<C-r>=coc#rpc#request('doKeymap', ['snippets-expand-jump',''])\<CR>" :
   \ <SID>check_back_space() ? "\<TAB>" :
   \ coc#refresh()function! s:check_back_space() abort
   let col = col('.') - 1
   return !col || getline('.')[col - 1]  =~# '\s'
 endfunctionlet g:coc_snippet_next = '<tab>'
```

我们的代码中可能会有一些不需要的尾随空格。为什么我们要费心手动删除它，而 vim 可以照顾它。为了删除尾随空格，创建一个函数，

```
function TrimWhitespace()
 let l = line(“.”)
 let c = col(“.”)
 %s/\s\+$//e
 call cursor(l, c)
endfun
```

您可以通过 auto 命令运行这个函数，或者只绑定到一个键，比如 F5。只需使用其中一种方法即可。

```
autocmd BufWritePre * :call TrimWhitespace()
nnoremap <F5> :call TrimWhitespace()<CR>
```

现在还有一件事，一些 vim 用户喜欢做，即重新映射退出键，特别是大写锁定。但是在 vim 本身中没有完全直接的方法来做到这一点。有变通办法，但都是全球性的变化。

在 windows 中，你可以使用*自动热键*，在 mac 中你可以在 *iTerm* 中更改它，但是在 Linux 中，这就有点难了。如果您使用 Xorg 作为显示服务器，有一种方法可以使用 *setxkbmap。*

```
setxkbmap -option caps:swapescape
```

这在 Escape 和 CapsLock 键之间切换。要清除所有已做的更改

```
setxkbmap -option 
```

所以创建一个 bash 函数，比如

```
nv(){
 setxkbmap -option caps:swapescape && nvim $* && setxkbmap -option}
```

会成功的。但是请记住，只要 vim 是开放的，这种改变就将是有效的。但就我个人而言，我不喜欢将其重新映射到任何其他键。这是个人喜好。

到目前为止，您已经有了足够好的设置，可以开始使用 vim 了。但这取决于他们想如何使用它。每个人都有自己的喜好。在 vim 中，几乎所有的事情都可以用这样或那样的方式来完成，所以按照您喜欢的方式来配置它。
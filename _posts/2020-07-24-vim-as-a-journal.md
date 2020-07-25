---
layout: post
title: "Vim as a journal"
categories: blog
published: true
permalink: /:categories/vim-as-a-journal/
---

Previously I published the take aways from writing journals, and in this post I will go over my writing set up.

There are many schools of thought on which medium to write journals on (e.g., physical journal, audio recordings, etc). I like using text files for couple reasons:
- Search. It's nice to be able to look for certain event.
- Storage. Every journal entry is stored on Dropbox.
- Analyze. The journal evolved to be a habit tracking system, and it lets me analyze a pattern over the years by text parsing.

The biggest draw for me is the writing experience. It's worth noting that I'm **deep** into vim's text manipulation. The thinking process here is to lower the barrier for me to write consistently, and writing on vim did just that.

While Vim excels at the editing experience, it's lack in built in thought organization features. This is where [vim-wiki](https://github.com/vimwiki/vimwiki) comes in. Here's the overview of the features:

> With VimWiki, you can:
> - Organize notes and ideas
> - Manage to-do lists
> - Write documentation
> - Maintain a diary
> - Export everything to HTML

There are three features I use the most:
- Index file (`<Leader>ww`)
  - The index file for your journal project.
- Create a new daily diary file (`<Leader>w<Leader>w`)
  - Creates a file with the name `YYYY-MM-DD.md`
- Load diary (`<Leader>wi`) (and update the file with `<Leader>w<Leader>i` )
  - Index page for all the diary entries you've written
  - It automatically parses `H1` headers as the title so I format it as `<Day of the week> MM/DD @ <Main event of the day>` (filtered out in the following screenshot)

![image](https://user-images.githubusercontent.com/9669739/88445083-57069a00-cdd5-11ea-8751-f5dbcf50e80b.png)

I created a `template.md` file, and used the following vimscript to achieve three tasks:
- Load the template
- Format the header with today's date
- Load a random vocabulary from [Kindle's vocab builder](https://yatsushi.com/blog/kindle-vocab-builder/)

```vimscript
function! LoadVocab()
  return systemlist('shuf -n 1 ~/Dropbox/wiki/diary/vocab.txt')
endfunction
function! JournalHeader()
  return "# " . strftime('%a') . " " . strftime('%m/%d') . " @"
endfunction
nmap <Space>l :r ~/Dropbox/wiki/diary/template.md<CR>gg"=JournalHeader()<C-M>p2j"=LoadVocab()<C-M>pgg
```


Here are other plugins that help me write better in vim (h/t to [@reedes](https://github.com/reedes)) on GitHub
- [vim-pencil](https://github.com/reedes/vim-pencil) - enables paragraph soft wrapping
- [vim-litecorrect](https://github.com/reedes/vim-litecorrect) - fixes simple typo like `teh` to `the`
- [vim-lexical](https://github.com/reedes/vim-lexical) - enables spell checks

Do you also write a lot on vim? Feel free to reach out with your tips & tricks!

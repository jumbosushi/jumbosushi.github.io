---
layout: post
title: "Vim as a journal"
categories: blog
published: true
permalink: /:categories/vim-as-a-journal/
---

Previously I published the take aways from writing journals, and in this post I will go over my writing set up.

There are many schools of thought on which medium to write journals on (e.g., physical journal, audio recordings, etc). I like using text files for couple reasons:
- Search. It's nice to be able to look for certain events.
- Storage. Every journal entry is stored on Dropbox.
- Analysis. Lets me answer questions like "How many journals did I write this year?"

The biggest draw for me is the writing experience. As I'm already fairly experienced with vim text manipulation, I can write my journals swiftly. It ultimately lowers the barrier for me to write consistently.

While Vim excels at the editing experience, it's lacks built-in organization features. This is where [vim-wiki](https://github.com/vimwiki/vimwiki) comes in. Here's the overview of the features:

> With VimWiki, you can:
> - Organize notes and ideas
> - Manage to-do lists
> - Write documentation
> - Maintain a diary
> - Export everything to HTML

There are three features I use the most:
- Index file (`<Leader>ww`)
  - The index file for your journal project. You can think of this as the "top page" for your journal. I use it mainly to add random notes.
- Create a new daily diary file (`<Leader>w<Leader>w`)
  - Creates a file with the name `YYYY-MM-DD.md`
- Load diary (`<Leader>wi`)
  - This page lists all the journal files you've written under the "./diary" directory
  - It automatically parses `H1` Markdown header as the title so I standardize all titles to be in the format like `<Day of the week> MM/DD @ <Main event of the day>`

![image](https://user-images.githubusercontent.com/9669739/88445083-57069a00-cdd5-11ea-8751-f5dbcf50e80b.png)

### Using a template

I use `template.md` file as a journal template. The following vimscript achieves three tasks:
- Loads the template
- Formats the header with today's date
- Loads a random vocabulary from [Kindle's vocab builder](https://yatsushi.com/blog/kindle-vocab-builder/)

```vimscript
function! LoadVocab()
  return systemlist('shuf -n 1 ~/Dropbox/wiki/diary/vocab.txt')
endfunction
function! JournalHeader()
  return "# " . strftime('%a') . " " . strftime('%m/%d') . " @"
endfunction
nmap <Space>l :r ~/Dropbox/wiki/diary/template.md<CR>gg"=JournalHeader()<C-M>p2j"=LoadVocab()<C-M>pgg
```

### Writing plugins

Here are other plugins that help me write better in vim (h/t to [@reedes](https://github.com/reedes)) on GitHub
- [vim-pencil](https://github.com/reedes/vim-pencil) - enables paragraph soft wrapping
- [vim-litecorrect](https://github.com/reedes/vim-litecorrect) - fixes simple typo like `teh` to `the`
- [vim-lexical](https://github.com/reedes/vim-lexical) - enables spell checks

Do you also write a lot on vim? Feel free to reach out with your tips & tricks!

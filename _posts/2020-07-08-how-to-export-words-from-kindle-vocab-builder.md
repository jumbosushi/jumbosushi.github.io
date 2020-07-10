---
layout: post
title: "How to export words from Kindle vocab builder"
categories: blog
published: true
permalink: /:categories/kindle-vocab-builder/
---

Learning to love reading again has been the best thing to happen after college. I prefer e-books over other mediums due to several reasons, but the biggest draw for me is the dictionary feature. It's incredible how many new words reading introduce you to, and I thought using this new exposure to grow my vocabulary would be a great idea. To be more specific, I saught for a way to 1. export words from Kindle's vocab builder feature and 2. create a vim keybinding to import a random word from the list. Here's how I did it.

Note: this tutorial is based on Kindle Paperwhite. Other firmware might have a different configuration.

### Exporting from `vocab.db`

What I didn't know was that all the words you've ever looked up in Kindle is stored in a SQLite database file called `vocab.db`. Once you connect the Kindle to your computer, it will be located at `Kindle/system/vocabulary/vocab.db`.

Open the database file with `sqlite3 vocab.db` command.

You can try `.schema` command to see table schemas:

![schema][schema_img]

We're only interested in exporting the `word` column from the `words` table. The following command will export them to `vocab.txt` file in a directory you called `sqlite3` to open the database from:

```sql
.output vocab.txt
select word from words;
.exit
```

After you're done, you can delete the `vocab.db` file from the kindle. The device will create a new database on the next lookup.

### Importing new words from a text file

Here's a vimscript snippet to import a random vocab from a text file with `<Leader>v` in a normal mode:

```vimscript
function! LoadVocab()
  return systemlist('shuf -n 1 ~/vocab.txt')
endfunction

nmap <Leader>v "=LoadVocab()<C-M>p
```

I write my journal within Vim, and managed to integrate this function into my script that loads the daily template which is working surprisingly well.

Feel free to reach out to me if you have any feedbacks about the key binding!

[schema_img]: https://user-images.githubusercontent.com/9669739/87009020-710b7000-c179-11ea-9f1d-1099c1af6fb0.png

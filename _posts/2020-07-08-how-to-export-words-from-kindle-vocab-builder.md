---
layout: post
title: "How to export words from Kindle vocab builder"
categories: blog
published: true
permalink: /:categories/kindle-vocab-builder/
---

[[Learning]] to love reading again has been the best thing I discovered in my life after college. I prefer e-books over other mediums due to several reasons, but the biggest draw for me is the dictionary feature. I'm continued to be amazed by how many new words you are introduced to just by reading, and thought using the words from the list would be a great way to grow my vocabulary. Here's how I exported the words and added vim keybind to import the word.

Note: this tutorial is based on Kindle Paperwhite. Other firmware might have a different configuration.

### Exporting from `vocab.db`

What I didn't know was that all the words you've ever looked up in Kindle is stored in a SQLite database called `vocab.db`. Once you connect your Kindle to the laptop, it will be located at `Kindle > system > vocabulary > vocab.db`.

Open the database file with `sqlite3 vocab.db` command.

You can try `.schema` command to see table schemas:

![schema][schema_img]

We're only interested in exporting the `word` column from `words` table. The following command will export them to `vocab.txt` file in a directory you called `sqlite3` to open the database from:

```sql
.output vocab.txt
select word from words;
.exit
```

After you're done, you can remove the `vocab.db` from the kindle. The device will create a new database on the next lookup.

### Importing new words from a text file

Here's a short vimscript snippet to import the random vocab from a text file with `<Leader>v` in a normal mode:

```vimscript
function! LoadVocab()
  return systemlist('shuf -n 1 ~/vocab.txt')
endfunction

nmap <Leader>v "=LoadVocab()<C-M>p
```

Feel free to reach out to me if you know a better way!

[schme_img]: https://user-images.githubusercontent.com/9669739/87009020-710b7000-c179-11ea-9f1d-1099c1af6fb0.png

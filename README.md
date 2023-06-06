# Omnisearch for Obsidian 

> I've gotten used to zettelkasten based linking in Zettlr and I especially liked how my titles weren't restricted. Unfortunately, this wasn't easy to replicate in Obsidian and it was quite annoying seeing uuids ("20230407193944") instead of titles ("# § 📗 notes") in omnisearch. Anyway, here's the [code](https://github.com/mournfully/obsidian-omnisearch-with-prefer-headings/tree/feature/prefer-heading-titles).

I found the following thread [^1] on the obsidian forum and all the recommended plugins to be quite helpful when I first encountered this problem.

I used `front-matter-title` [^2] for a long time, until I grew frustrated trying with it's codebase. I especially struggled trying to integrate the provided api (`front-matter-title-api-provider`) with `omnisearch` [^3]. 

I wasn't able to make much progress with `front-matter-title`, but I did manage to get somewhat close with just `omnisearch`. I added `headings1` to the type `ResultNote`, after realizing that it would be automatically populated by `IndexedDocument`. Then I just changed how titles were rendered by using `note.headings1` instead of `note.basename`. 

[obsidian-omnisearch/ResultItemVault.svelte · scambier/obsidian-omnisearch · GitHub](https://github.com/scambier/obsidian-omnisearch/blob/master/src/components/ResultItemVault.svelte)
```diff
  export let note: ResultNote
  let title = ''

  $: {
-   title = note.basename
+   title = note.headings1
    notePath = pathWithoutFilename(note.path)
    if (settings.ignoreDiacritics) {
      title = removeDiacritics(title)
    }
  }
```

[obsidian-omnisearch/globals.ts at master · scambier/obsidian-omnisearch · GitHub](https://github.com/scambier/obsidian-omnisearch/blob/master/src/globals.ts)
```diff
export type ResultNote = {
  score: number
  path: string
  basename: string
+ headings1: string  
  content: string
  foundWords: string[]
  matches: SearchMatch[]
}
```

For a myriad of reasons that I can't be bothered to go over here. The approach above was quite naive and couldn't work as I wanted. So, I've chosen to write my own [plugin](https://github.com/mournfully/obsidian-title-overhaul), and to borrow heavily from the `omnisearch` codebase (mostly cause I like the way it's designed).

[^1]: [Use H1 or front-matter title instead of or in addition to filename as display name - Feature requests - Obsidian Forum](https://forum.obsidian.md/t/use-h1-or-front-matter-title-instead-of-or-in-addition-to-filename-as-display-name/687/125)

[^2]: [snezhig/obsidian-front-matter-title: Plugin for Obsidian.md](https://github.com/snezhig/obsidian-front-matter-title)

[^3]: [scambier/obsidian-omnisearch: A search engine that "just works" for Obsidian. Includes OCR and PDF indexing.](https://github.com/scambier/obsidian-omnisearch)


---
title: 'YAML And Markdown Example'
date: 2016-06-23
categories: Markdown
tags: ['Starting Up','YAML','Markdown','Blogging']
---

Markdown (.md) files offer the ability to use shorthand methods to generate HTML. Simply create a text file in your
favourite text editor and save it as FileName.md. Then you have yourself a markdown file.
This is particularly useful for bloggers where markdown is easier to quickly type and potentially easier to read as
raw code than HTML. Github already supports markdown files and a number of static site generators use markdown as their
raw source files to generate HTML. Where markdown caters for the content, YAML, which stands for "YAML ain't markup language", handles the metadata for the markdown document.

## YAML
**YAML** stands for "YAML aint markup language" and is the metadata component of the page which can contain title, date and tags among others.
So at the top of my markdown document I could contain some metadata in YAML that would look something like the following.
```
---
title: 'YAML And Markdown Example'
date: 2016-06-23
categories: Markdown
tags: ['Learning','YAML','Markdown']
---
```

## Markdown
**Markdown** is an easy way to write content with shorthand methods instead of using something with tags (like HTML) so prose is easier to write and read when coded.
You can get more information on markdown at the creators of markdown [Daring Fireball](https://daringfireball.net/ "Daring Fireball") or this nice cheatsheet from [github](https://enterprise.github.com/downloads/en/markdown-cheatsheet.pdf" "Github Markdown Cheatsheet").
The Markdown that generated the HTML above looks like this.

```
## Markdown
**Markdown** is an easy way to write content with shorthand methods instead of using something with tags (like HTML) so prose is easier to write and read when coded.
You can get more information on markdown at the creators of markdown [Daring Fireball](https://daringfireball.net/ "Daring Fireball") or this nice cheatsheet from [github](https://enterprise.github.com/downloads/en/markdown-cheatsheet.pdf" "Github Markdown Cheatsheet").
The Markdown that generated the HTML above looks like this.
```
Where the ## translates to a header h1 tag in HTML and the asterisks wrapped around a word translate to bold. Links are represented as \[Name to Display](URL "Title").

### Markdown Lists
You can use asterisks (\*) to create unordered lists in your markdown
* I
* am
* unordered

Where the markdown that generates this HTML looks like the following.

```
### Markdown Lists
You can use asterisks (\*) to create unordered lists in your markdown
* I
* am
* unordered
```

You can see that ### creates a h3, the backslash is an escape character (for the asterisk) and asterisks generate the unordered bullets.

or the number 1 and a full stop (1.) to create ordered lists
1. I
1. am
1. ordered

Where the markdown that generates this HTML looks the following.

```
or the number 1 and a full stop (1.) to create ordered lists
1. I
1. am
1. ordered
```

You can see that 1. creates an ordered list.

### Markdown Code blocks
You can also create code snippets using markdown by using three backticks above and below your markdown code.
Where is backtick on your keyboard?? It's the Windows key and the backtick key (top left on a Qwerty keyboard)..

```
Message("Hello World");
```

I can display the code specifically (ish) as SQL by adding the keyword sql after the top three backticks if I like too...

```sql
CREATE TABLE dbo.TestTable
(RowNo INT, RowValue CHAR(1));

SELECT * FROM dbo.TestTable
WHERE RowNo = 1;
```

### Markdown Editors

I currently use [Atom](https://atom.io/ "Atom")   as my local markdown editor but there are also plenty of online markdown editors if you want to just try it out, such as [stackedit](https://stackedit.io/ "stackedit") or [dillinger](http://dillinger.io/ [dillinger] "dillinger"). Also remember any github files saved as markdown (.md) will also display accordingly as HTML when viewed.

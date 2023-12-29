---
description:
aliases: 
tags: 
created: 2023-04-22T13:56:15
updated: 2023-09-16T15:46:02
title: Obsidian Basic Formatting Syntax
---
- <https://help.obsidian.md/Editing+and+formatting/Basic+formatting+syntax>
- Image size control
	- ![Engelbart](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
	- ![Engelbart|100x145](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)

## Quotes

You can quote text by adding a `>` symbols before the text.

```md
> Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.

\- Doug Engelbart, 1961
```

> Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.

- Doug Engelbart, 1961

Tip

You can turn your quote into a [callout](https://help.obsidian.md/Editing+and+formatting/Callouts) by adding `[!info]` as the first line in a quote.

## Callouts

<https://help.obsidian.md/Editing+and+formatting/Callouts>

Use callouts to include additional content without breaking the flow of your notes.

To create a callout, add `[!info]` to the first line of a blockquote, where `info` is the _type identifier_. The type identifier determines how the callout looks and feels. To see all available types, refer to [Supported types](<https://help.obsidian.md/Editing+and+formatting/Callouts#Supported> types).

```markdown
> [!info]
> Here's a callout block.
> It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]!
> ![[og-image.png]]
```

> [!info]  
> Here's a callout block.  
> It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]!  
> ![[jeff.jpg]]  
Callouts are also supported natively on [Obsidian Publish]().

Note

If you're also using the Admonitions plugin, you should update it to at least version 8.0.0 to avoid problems with the new callout feature.

### Change the title

By default, the title of the callout is its type identifier in title case. You can change it by adding text after the type identifier:

```markdown
> [!tip] Callouts can have custom titles
> Like this one.
```

> [!tip] Callouts can have custom titles  
> Like this one.

You can even omit the body to create title-only callouts:

```markdown
> [!tip] Title-only callout
```

> [!tip] Title-only callout

### Foldable callouts

You can make a callout foldable by adding a plus (+) or a minus (-) directly after the type identifier.

A plus sign expands the callout by default, and a minus sign collapses it instead.

```markdown
> [!faq]- Are callouts foldable?
> Yes! In a foldable callout, the contents are hidden when the callout is collapsed.
```

> [!faq]- Are callouts foldable?  
> Yes! In a foldable callout, the contents are hidden when the callout is collapsed.

### Nested callouts

You can nest callouts in multiple levels.

```markdown
> [!question] Can callouts be nested?
> > [!todo] Yes!, they can.
> > > [!example]  You can even use multiple layers of nesting.
```

> [!question] Can callouts be nested?
>
> > [!todo] Yes!, they can.
> >
> > > [!example]  You can even use multiple layers of nesting.

## Bullet Lists

![[Pasted image 20230228142139.png]]  

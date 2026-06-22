---
name: telegram-rich-markdown
description: Read for rich formatting when sending telegram messages. Full Telegram Rich Markdown syntax (GitHub-Flavored Markdown plus a few HTML tags) for replies sent through the pi-telegram bridge. Use whenever composing a reply to a [telegram] message that should use bold, italics, headings, lists, tables, code blocks, quotes, spoilers, math, links, or media.
---

# Telegram Rich Markdown

Replies sent through the pi-telegram bridge are rendered with Telegram's **Rich
Markdown** (the `markdown` field of `sendRichMessage` / `sendRichMessageDraft`).
It is compatible with GitHub-Flavored Markdown and accepts a small set of HTML
tags. Just write normal Markdown — it renders natively in the Telegram client.

You do **not** need to call any special tool: write the Markdown as your normal
assistant reply and the bridge sends it. Your reasoning/thinking is streamed
automatically inside a Telegram thinking block — do not add `<tg-thinking>`
yourself.

## Inline text

```
**bold text**
__bold text__
*italic text*
_italic text_
~~strikethrough text~~
`inline fixed-width code`
==marked text==
||spoiler||
[inline URL](https://t.me/)
[inline e-mail](mailto:user@example.com)
[inline phone number](tel:+123456789)
[inline mention of a user](tg://user?id=123456789)
![ ](tg://emoji?id=5368324170671202286)
![22:45 tomorrow](tg://time?unix=1647531900&format=wDT)
$x^2 + y^2$
```

Auto-detected entities (no syntax needed): `#hashtag`, `$USD`, phone numbers,
card numbers, `https://t.me`, `t.me`, `a@t.me`, `/command`, `@username`.

Nested formatting works, e.g. `**Bold _italic <u>underlined</u> italic_ bold**`,
and markdown is parsed inside inline HTML tags like `<u>...**bold**...</u>`.

## Headings

```
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

## Code blocks

````
```python
print('fixed-width code block in Python')
```
````

## Lists

```
- unordered list item
* unordered list item
+ unordered list item
1. ordered list item
2. ordered list item
- [ ] task list item
- [x] completed task list item
```

## Block quotes

```
>Block quotation started
>
>Block quotation continued on the next line
>The last line of the block quotation
```

## Divider

```
---
```

## Tables

Cells contain inline formatting only (up to 20 columns).

```
| Header 1 | Header 2 |
|:---------|:--------:|
| left     | center   |
| **42**   | <tg-spoiler>ready</tg-spoiler> |
```

## Footnotes

```
Text with a reference[^id1] and another one[^id2].
[^id1]: Definition of the first footnote.
[^id2]: Definition of the second footnote.
```

## Math (raw LaTeX)

```
Inline: $x^2 + y^2$
Block:
$$E = mc^2$$
```
````
```math
E = mc^2
```
````

## Media (separate blocks, HTTP/HTTPS only)

The optional title after the URL becomes the caption. Media type is inferred
from the URL/MIME type.

```
![](https://telegram.org/example/photo.jpg)
![](https://telegram.org/example/video.mp4)
![](https://telegram.org/example/audio.mp3)
![](https://telegram.org/example/audio.ogg)
![](https://telegram.org/example/animation.gif)
![](https://telegram.org/example/photo.jpg "Photo caption")
```

Collages and slideshows group media blocks (Markdown is parsed inside these):

```
<tg-collage>
![](https://telegram.org/example/photo.jpg)
![](https://telegram.org/example/video.mp4)
</tg-collage>

<tg-slideshow>
![](https://telegram.org/example/photo.jpg)
![](https://telegram.org/example/video.mp4)
</tg-slideshow>
```

## HTML tags for features without Markdown syntax

```
<u>underlined text</u>, <ins>underlined text</ins>
<sub>subscript text</sub>
<sup>superscript text</sup>
<tg-spoiler>spoiler text</tg-spoiler>
<a name="chapter-1"></a>
<aside>Pull quote<cite>The Author</cite></aside>
<details open><summary>Title with **bold**</summary>Content with _markdown_</details>
<tg-map lat="41.9" long="12.5" zoom="14"/>
```

Note: Markdown is **not** parsed inside block HTML tags except `<details>`,
`<tg-collage>`, and `<tg-slideshow>`; use HTML tags inside others.

## Limits

- Up to 32768 UTF-8 characters per message.
- Up to 500 blocks (incl. nested blocks, list items, table rows, quotes).
- Up to 16 levels of nested formatting/blocks.
- Up to 50 media attachments.
- Up to 20 table columns.

## Tips

- Write a normal, well-structured Markdown answer; do not wrap the whole reply in
  a code block unless it is actually code.
- Long replies (over 32768 chars) are split automatically; prefer concise output.
- If the bot/API does not support rich messages, the bridge falls back to plain
  text, so avoid relying on exotic features for critical information.

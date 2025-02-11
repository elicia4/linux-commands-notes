# `nl` 

[***Table of Contents***](/README.md)

`nl` is used to number the lines in a file (output file contents with a line
number in front of each line). Resembles `cat -n`:

```bash
nl file.txt
```

By default, `nl` doesn't count empty lines. You can add `-b a` to cause all of
the lines to be numbered. `nl` supports a surprisingly complex and unusual
model for numbering lines in a file, e.g.:

```bash
nl -b a -h n -f n -s ' ---> ' -n rz -w 12 -v 9373
```

- `-b a` = all lines numbered
- `-h n`, `-f n` = header and footer should have no lines numbered. Header,
body, and footer are delimited using special separators: `\:\:\:`, `\:\:`, and
`\:`.
- `-s ' ---> '` provides a separator to add after the line number
- `-n rz` causes right justified line numbers with leading zeros
- `-w 12` indicates how many characters the line numbers should be
- `-v 9373` provides an explicit starting line number

You can also use `-b` to only number lines that match a given basic regular
expression:

```bash
nl -b pGNU -s ' ===> ' -n rz
```

It accepts multiple files as well as STDIN (just like `cat`). It supports
markup as well.

`nl` supports a concept called "logical pages" when numbering. This allows `nl`
to start over the numerical sequence when numbering. It is possible to set the
starting number to a specific value and change its format. A logical page is
broken down into a header, body, and footer. Within each of these sections,
line numbering may be reset and/or be assigned a different style. If `nl` is
given multiple files, it treats them as a single stream of text. Sections in
the text stream are indicated by markup added to the text:

- `\:\:\:` - start of logical page header
- `\:\:` - start of logical page body
- `\:` - start of logical page footer

Each of the markup elements must appear alone on its own line. After processing
a markup element, `nl` deletes it from the text stream.

`nl` options:
- `-b style` - set *body* numbering to style:
  - `a` = number all lines
  - `t` = number only non-blank lines. This is the default.
  - `n` = none
  - `p`*regexp* = number only lines matching basic regular expression *regexp*.
- `-f style` - set *footer* numbering to style. The default is `n`.
- `-h style` - set header numbering to style. The default is `n`.
- `-i number` - set page numbering increment to `number`. The default is one.
- `-n format` - sets numbering format to format, where format is one of the 
following:
  - `ln` = left justified, without leading zeros.
  - `rn` = right justified, without leading zeros. This is the default.
  - `rz` = right justified, with leading zeros.
- `-p` - do not reset page numbering at the beginning of each logical page.
- `-s string` - add string to the end of each line number to create a
  separator. The default is a single tab character.
- `-v number` - set first line number of each logical page to number. The
  default is one.
- `-w width` - set width of the line number field to width. The default is 6.

Try creating a file with the following contents:

```bash
echo "\:\:\:
title
\:\:
item 4
item 3
item 2
item 9
item 6
item 8
\:
footer" > file.txt
```

Now pass its contents to `nl`:

```bash
cat text.txt | nl -s ' ' -w 2 -n rz
```

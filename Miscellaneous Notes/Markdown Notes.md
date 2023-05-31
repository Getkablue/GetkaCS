When writing code in your responses and solutions, enclose it in sets of three backticks (\`).
If you are using a live markdown editor/viewer (which I recommend), code written like this will appear in a nice clean block. For example:

\`\`\`
print("Hello world!")
\`\`\``
becomes:

```
print("Hello world!")
```

You may also specify a language next to the first set of three backticks in order to get syntax highlighting. For example, writing \`\`\`python at the beginning will give syntax highlighting that works specifically for Python:
```python
print("Hello world!")
```

This feature is useful both for spotting mistakes in your code and for indicating which language you are using

You can also mark headings with one or more #. The more #, the lower the level of the heading. For example:

\# Heading 1
\## Heading 2
\### Heading 3

becomes

# Heading 1

## Heading 2

### Heading 3

# LaTeX formatting

If you want, you can also use LaTeX to generate really nice-looking equations and formulae and such. To do so in Markdown (at least in Obsidian) you can start a LaTeX block with two dollar signs (\$\$) and end it with the same.

I can't give a full LaTeX tutorial here, but LaTeX commands generally start with a backslash and then some keyword. For example:

``` LaTeX
$$
f(x, y): \mathbb{R, R} \to \mathbb{C}
$$
```

renders to:
$$
f(x, y): \mathbb{R, R} \to \mathbb{C}
$$
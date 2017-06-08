# Contributing

This book is developed using [GitBook](https://github.com/GitbookIO/gitbook). Authored in Markdown files (I use [atom](http://atom.io)).

Here's how to setup a Dev Environment:

```
npm install gitbook-cli -g
gitbook serve .
```
> Note: serve needs port `35729` (for live reload) and `4000` for serving http://localhost:4000.

Also you can mostly just edit the `.md` files in [`/docs`](https://github.com/roylory/typescript-book-ko/docs) using github and create a Pull Request (PR).

# Reloading issue with `gitbook serve`

Currently, there is an issue with reloading in Windows as described in https://github.com/GitbookIO/gitbook/issues/1379.

I have found two temporary solutions:

1. Run `serve.bat` and refresh the browser 20 seconds after changing anything.

2. Or, use the old gitbook version 2.6.7.
```
:: change the gitbook version in book.json
gitbook fetch 2.6.7
gitbook -v 2.6.7
gitbook serve --gitbook=2.6.7
:: ignore warnings
```

# Code
All the code for the book is in the `/code` folder. Tested with `atom-typescript`.

### More Gitbook Tips
* Links best work if they are relative (e.g. `./foo.md`) to the *current* file.
* For links in the same file (`#foo-bar` style links) best to click the heading on github to get what gitbook expects.

### TypeScript Compiler Docs
Thanks to the TypeScript team for providing much of the docs: https://github.com/Microsoft/TypeScript/wiki/Architectural-Overview that are used to write the compiler story.

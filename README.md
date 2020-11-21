# table-of-contents-action

Githbu action to automatically generate a Table of content section in your README.md

Table of Contents
=================

<!--ts-->
<!--te-->

## Credits

The script generating the markdown "Table of contents" section is from [ekalinin](https://github.com/ekalinin/github-markdown-toc)

In the workflow example below, I'm using the github action made by [ad-m](https://github.com/ad-m/github-push-action)

**This github action is only an automation of existing scripts**

## Usage

1. Add those two lines into your mardown file to specify where the section should be generated

```markdown
<!--ts-->
<!--te-->
```

2. Add this workflow to your github repository (usually in `.github/workflows/table_of_contents.yml)

```yaml
name: Generate Table of contents section

on:
  push:
    paths:
      - README.md
      - .github/workflows/table_of_contents.yml

jobs:
  table-of-contents:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v1

      - name: Generate table of content
        id: generate
        uses: LeChatErrant/table-of-contents-action@v1.0.0

      - name: Commit
        if: ${{ steps.generate.outputs.changes }} == 'true'
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add README.md
          git commit -m "doc: README.md table of contents updated [AUTO]"

      - name: Push changes
        if: ${{ steps.generate.outputs.changes }} == 'true'
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: ${{ github.ref }}
```

3. Enjoy!

> This single workflow ensure a "Table of contents" section is automatically generated and pushed to your repository

## Contributors

![GitHub Logo](https://github.com/LeChatErrant.png?size=30) &nbsp; [LeChatErrant](https://github.com/LeChatErrant) - creator and maintainer

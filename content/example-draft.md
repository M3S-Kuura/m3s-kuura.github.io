---
draft: true
title: "Example Draft"
---

This file works as an example to get you started in creating a new document. Please keep this as a draft!

## Working on a draft

In the root folder of the repository, run `hugo new <page-name>`. A new page name is a `draft` by default, and therefore not public.

In order to view your draft locally, run `hugo serve -D` in the root folder, and navigate to [http://localhost:1313/\<page-name\>](http://localhost:1313/<page-name>) (fix the url suffix with the name you chose).

### Syntax

The text part at the top of the page is not normal markdown, but hugo reads it to render the page properly.

`date`: generated automatically if you run `hugo new <page-name>` in the root folder. However it's not a necessary line to have.

`draft`: if true, the page is not rendered for public access. To make the file public, remove the draft line or make it false.

`title`: Main title of the document, and also the HTML document title that is read on the browser tab.

Otherwise refer to normal markdown documentation.

## Make your page public

Remove the `draft` line at the top of the page or assign it `false`. Then add your page to content/\_index.md file similar to how other links are listed. Pushing changes to github main branch builds the website anew.

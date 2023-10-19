---
title: "Visualizing your npm dependencies with npmgraph"
description: "Describes how to check and update package versions in a package.json file using a CLI tool called npm-check-updates."
author: "Jan-Philipp Kempf"
pubDate: "2023-10-19T17:01:00+02:00"
---

[npmgraph.js.org](https://npmgraph.js.org/) lets you search for npm packages and view their dependency graph. It can also parse whole `package.json` files and turn those into larger graphs of the same kind. It's interesting, if somewhat unsettling, to see how many dependencies your app has—and how many of those are being maintained by a single person. It's a lot more than you'd expect, which brings to mind [this xkcd classic](https://xkcd.com/2347/).

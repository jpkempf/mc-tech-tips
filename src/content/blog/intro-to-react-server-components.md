---
title: "Intro to React Server Components"
description: "Notes on an article originally published on joshwcomeau.com"
author: "Jan-Philipp Kempf"
pubDate: "2023-09-14T17:53:00+02:00"
---

I'm reading this excellent [intro to React Server Components](https://www.joshwcomeau.com/react/server-components/) by Josh W. Comeau and taking notes as I go along. Here's what stuck with me the most:

- Server Components are considered _async_, which means you can do stuff like `await`ing data from a database or an API right on the top level.
- Each Server Component _only renders once_. It can never re-render, unless you reload the page of course. Which means that a whole bunch of existing React paradigms don't apply to Server Components; you can't use state or effects, for instance. But the previous point means you often won't have to, anyway!
- Normal components are now "Client Components" but they render on the server _and_ the client. 🙃 What "Server Component" means is that they are _exclusive_ to the server, but Client Components are not exclusive to the client.
- Server Components are a framework-level feature, since they tie in with fundamental things like build process and application server. The only way today for normal people to use Server Components is through the App Router in Next.js 13.4 and above. More frameworks that support Server Components will eventually be listed under [this link](https://react.dev/learn/start-a-new-react-project#bleeding-edge-react-frameworks).
- Server Components are the default; Client Components are opt-in. A Server Component turns into a Client Component if you include the directive `'use client'` at the top of your file.
- Server Components are automatically treated as Client Components if you import them from another (implicit or explicit) Client Component. Josh calls this a "client boundary" and he explains the implications in detail in his article.
- Someone built [an experimental dev tool for Server Components](https://www.alvar.dev/blog/creating-devtools-for-react-server-components)! More tooling is sure to be forthcoming.

Straight-up quote from Josh to sum things up:

> The big difference is that we've never before had a way to run server-exclusive code _inside our components_.

I warmly recommend [reading the whole article](https://www.specfy.io/blog/1-efficient-dockerfile-nodejs-in-7-steps)!

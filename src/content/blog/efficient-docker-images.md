---
title: "Creating efficient Node.js Docker images"
description: "Repost of an article originally published on the Specfy blog."
author: "Jan-Philipp Kempf"
pubDate: "2023-09-11T12:45:00+02:00"
---

There's [a nice article written by one Samuel Bodin over on the Specfy blog](https://www.specfy.io/blog/1-efficient-dockerfile-nodejs-in-7-steps) that sums up what it takes to make a Node.js Docker image as small and fast to build as possible.

The article is worth a read, even for someone like me who doesn't really know a whole lot about Docker in the first place. It helped me understand why a lot of the Docker files I've seen lately look the way they do, with lots of `FROM xyz as something`, `COPY --from=something` statements and so on. Spoiler: It all has to do with Docker's concept of "layers" and how caching can play into that.

In his example, Samuel managed to reduce a Docker image's size by 80%, down from 2.48GB to 0.5GB. Build times were also roughly cut in half. However, he also cautions his readers:

> tl;dr it's possible to save up to 80% of time to build and image size, but the complexity is not always worth it.

So your mileage may vary, but at least now you'll understand better what options you have for optimising things if need be. [See for yourself](https://www.specfy.io/blog/1-efficient-dockerfile-nodejs-in-7-steps)!

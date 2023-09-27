---
title: "The Temporal API looks very cool"
description: "Repost of an article originally published at taro.codes"
author: "Jan-Philipp Kempf"
pubDate: "2023-09-27T16:31:00+02:00"
---

The [Temporal API](https://tc39.es/proposal-temporal/docs/) is JavaScript's attempt to finally get working with dates and times right, and I have to say it looks very promising. It's in Stage 3 right now and has been for almost two years, but implementations have already landed (behind flags) in most major browsers. It lets you do stuff like this and much much more:
  
```ts
const now = Temporal.Now.zonedDateTimeISO()
const format = { weekday: 'long', hour: 'numeric', minute: 'numeric' }

const startedAt = now.subtract({ hours: 2 }).toLocaleString(navigator.language, format)
const endsAt = now.add({ hours: 2 }).toLocaleString(navigator.language, format)

console.log(`Event started at ${startedAt} and will end at ${endsAt}.`)
// Event started at Wednesday 2:34 PM and will end at Wednesday 6:34 PM.
```

Get the full rundown over at [taro.codes](https://taro.codes/posts/2023-08-23-temporal-api/)!
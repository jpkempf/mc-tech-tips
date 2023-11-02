---
title: "An introduction to state machines and XState"
description: "An outline in blog post form for a lightning talk on the same topic"
author: "Jan-Philipp Kempf"
pubDate: "2023-11-02T17:42:35+01:00"
---

## What's a state machine?

Great question! Let's ask Copilot:

> A state machine is a mathematical model of computation. It consists of a set of states, a set of events, and a set of transitions between those states. When an event occurs, the machine transitions from one state to another.

Okay, cool, but what does that actually mean? How about a nice picture instead? Sure! Here's a visualization of a simple state machine:

![Screenshot of a simple state machine created with the Stately Studio editor](/mc-tech-tips/images/intro-state-machines-xstate/state-machine.png)

The screenshot is from [Stately Studio](https://stately.ai/studio), a tool for designing state machines and exporting them as code that you can use in your application. It also has [an interactive mode](https://stately.ai/registry/editor/0042094c-f8ef-4299-a8ca-a8129b6defb5?machineId=0ce50a28-7299-4a6e-8545-a025d635be86&mode=Simulate) to test the behaviour of your machine.

The folks at Stately are also the maintainers behind XState, a library for using state machines in your JS/TS application. The [XState docs](https://stately.ai/docs/xstate) and their [series of free video tutorials](https://www.youtube.com/playlist?list=PLvWgkXBB3dd4I_l-djWVU2UGPyBgKfnTQ) do a much better job of explaining how state machines work than I ever could and I highly recommend checking them out.

## Why use a state machine?

State machines are great for modelling complex behaviour in your application, instead of tightly coupling the required state and logic to your UI components. You can easily add conditions under which certain interactions are allowed or prohibited, define side effects such as API calls that should happen at specific points in the flow, have things happen automatically or after given delays, and more. A state machine is easy to reason about, easy to maintain, modify and extend and easy to test.

In UI development, even seemingly small interactions can quickly become more complex than you initially expected. Pretty soon you'll find yourself keeping track of various pieces of data in local state. You might feel the need to introduce a system for sharing data between components. You'll write a growing number of conditionals to control UI behaviour. These things creep up on you, and even though it may initially seem like overkill to learn and introduce a whole new additional concept to your application, it will almost certainly be worth it in the end, because the alternative is to basically invent the same concept yourself, but in a much more painful and much less well-defined way.

Here is a real-world example of a slightly more complex (but still pretty simple) machine that controls a kind of shopping cart:

![Visualization of a slightly more complex state machine from Stately Studio](/mc-tech-tips/images/intro-state-machines-xstate/cart-machine.png)

Some of the things that this machine accomplishes include: form validation, resetting the cart or replacing it with data coming in from an external event and automatically displaying a success messages on certain events. The components consuming this cart machine need to know about none of this logic; all they do is send events and render UI based on the current state of the machine.

## Using XState in React

There are a lot of scary-sounding concepts in state machines, such as _actors_, _services_, _guards_, _actions_, and more. There are also multiple possible ways of creating, initialising and consuming a machine in your React components. And weirdly—the last time I checked—there isn't a good guide in the XState docs that shows you how to best create a machine in a standalone file and then consume a single instance of that machine, with a shared context, across multiple React components. So here are the steps that I would recommend to achieve this goal:

### Define your machine

For anything more than a very simple, single-purpose machine, you're going to want to be able to read that machine's current state and context, as well as dispatch events for the machine, from more than one component. This means that you need a single instance of the machine that you can share across components. In state machine lingo, that means you need an [actor](https://stately.ai/docs/actors). This is how you can define one:

```ts
// in myExampleMachine.ts

import { createActorContext } from "@xstate/react";
import { assign, createMachine } from "xstate";

export const myExampleMachine = createMachine({
  {
    // your machine definition ...
  },
  {
    // any actions and guards that don't require external context
    actions: {}, guards: {}
  }
});

export const MyExampleMachineContext = createActorContext(myExampleMachine);
export const useMyExampleMachine = () => MyExampleMachineContext.useActor();
```

In that single file, we've defined most of the logic for our machine, plus we've exported a piece of context that gives us access to this machine wherever we need it.

### Provide context

The next thing we need to do is to make our machine context available to your React components. Pick the right place in your app component tree and wrap your context provider around it:

```tsx
// in _app.tsx or wherever else it's appropriate for you

import { MyExampleMachineContext } from "path/to/cartMachine";

// ...

const AppComponent = () => {
  return (
    <MyExampleMachineContext.Provider
      options={{
        // actions and guards that require runtime data
        actions: {},
        guards: {},
      }}
    >
      {/* your lower-level components */}
    </MyExampleMachineContext.Provider>
  );
};

export default AppComponent;
```

### Consume the machine

Finally, in the components where you want to use your machine, you can now use the hook we defined earlier:

```tsx
import { useMyExampleMachine } from "path/to/cartMachine";

const MyComponent = () => {
  const [state, send] = useMyExampleMachine();

  return (
    <div>
      <button onClick={() => send({ type: "MY_EVENT" })}>Send event</button>
      <p>Here is some data from the machine: {state.context.someData}</p>
    </div>
  );
};
```

That should be everything you need to get an idea of the general approach, and what to search for in the docs if you have more questions. Now go forth and build some state machines!

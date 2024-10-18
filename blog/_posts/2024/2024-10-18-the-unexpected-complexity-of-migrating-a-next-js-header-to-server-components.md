---
title: 'The Unexpected Complexity of Migrating a Next.js Header to Server Components'
tags:
  [Software Engineering, Javascript, Next.js, Server Side Rendering, Routing]
---

I recently began the process of refactoring my Next.js App Router project. It was initially built as a client-side app using the Page Router, but now I want to simplify the architecture and improve performance by migrating more to server-side components. To my surprise, something as seemingly simple as the `Header` component has proven more difficult than expected.

## The Problem: The `Header` Component

The Header component serves a small but important role in the app:

- It's rendered in the root layout.
- By default, it shows the website logo.
- When the URL includes a game slug (e.g., `/[game]`), it fetches the game's title from the database and displays it. This works for all nested paths, such as `/[game]/[variant]/[step]`.

Currently, the `Header` is a client component, and all the URL reading and data fetching happen on the client. My goal is to refactor this to a server component, but that's easier said than done.


Default header with logo |  Header with game title
:-------------------------:|:-------------------------:
![BGRH Home page with default logo header](/assets/images/2024-10-18/bgrh-home-header.png) | ![BGRH Game page with game titile header](/assets/images/2024-10-18/bgrh-game-header.png)



## The Challenges I Faced:

### 1. No Access to Request URL in Server Components.

I can't access the request URL directly from a server component, which complicates dynamic rendering based on the current route.

### 2. Page Params Don't Work for Layouts.

The header is part of the root layout, which spans the whole site. Using page parameters only works for specific segments like `/[game]`, but it doesn't apply to more deeply nested paths such as `/[game]/[variant]/[step]`.

### 3. Route Groups Seemed Promising, But...

I considered using Route Groups and creating separate layouts for different routes. However, this came with several drawbacks:

- I'd have to duplicate the root layout across multiple files, which leads to unnecessary maintenance overhead.
- It messes with the HTML structure, such as replacing the `<body>` tag.
- Moving all game related pages under a group just for this detail lacks flexibility.
- What if I need another component with similar logic?

### 4. Route Slots Didn't Work Either

I tried using Route Slots, but they also didn't solve the issue because of inconsistent rendering logic between soft and hard navigation events.

### 5. Route Groups + Route Slots: The Only Workable Solution?

After some trial and error, combining Route Groups and Route Slots finally worked as a proof of concept. However, this approach has its own issues:

- It requires redundant files and a tricky layout structure to ensure the correct component is rendered:

  - `@header/(root)/layout.tsx` has Header component with default title
  - `@header/(game)/layout.tsx` has nothing and just renders children. It's because it replaces the (root) group layout.
  - `@header/(game)/[game]/layout.tsx` finally has Header component with fetched game title.

- It's challenging to keep the layouts in sync.
- I'm afraid to imagine the complexity if I need another component with similar logic.

![BGRH Slot Group Files Structure](/assets/images/2024-10-18/bgrh-slot-group-files-structure.png)

## My Conclusion:

After trying multiple approaches, it seems like the simplest option is to stick with client-side rendering for the Header component. It feels counterintuitive, given my goal to move to server components, but the workarounds for server-side rendering create more complexity than they solve—at least for now.

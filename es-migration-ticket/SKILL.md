---
name: es-migration-ticket
description: Write a Jira story ticket for moving one dashboard tile from the MySQL service to the ES service. The ticket says what the tile does today, so QA can check that nothing breaks. Use when the user names a tile (and its pages/controllers) and asks for an ES migration ticket, story, or requirement.
---

# ES Migration Ticket

## Goal

Write **one** Jira story ticket for **one tile**.

- The tile moves from the **MySQL service** to the **ES service**.
  - **MySQL service:** code in this project that reads data with MySQL queries (DAO classes in `src/main/java/com/zph/dao`).
  - **ES service:** a separate service that reads data with Elasticsearch queries and shares it through APIs. This project calls those APIs.
- The ticket is the **reference for QA testing**.
- It says **what exists today**, so the move does not break it.

## Input

Ask for these if the user did not give them:

1. The tile name, for example "Brand Drug Use".
2. The pages that show the tile, and their controllers. For example, Population = `Pop01Controller` `index`, Pharmacy = `Phm01Controller` `index`.

## Steps

1. **Find the tile in the code** on every page:
   - The view: `grails-app/views/<page>/index.gsp`. Search for the tile name and the tile IDs (like `z5popstk001`).
   - The controller actions that the tile calls (the `action="..."` attributes).
   - The DAO methods and the SQL behind them. Check both the normal DAO and the custom-period DAO (`dao/custom/...`).
   - The Vue component and JS that draw the tile (`grails-app/assets/javascripts/...`).
2. **Check, do not guess:**
   - Titles: look up the message codes in `src/main/webapp/z5application.properties`.
   - IDs, tabs, labels, buttons, links, and what a click does.
   - What is different between the pages.
3. **Write the ticket.** Use the format below.
4. **List your assumptions.** After the ticket, list every point you did not check in the code. Ask the user about them.

## Output format

Use **only these 3 sections**. Use the example as your model: `examples/brand-drug-use-tile.md`.

```
# ES Migration | <Page 1> + <Page 2> | <Tile name> tile

## 1. Summary
- What moves: this tile, from MySQL to the ES service.
- The user must see the same thing as before. Only the data source changes.

## 2. Where
A table with one column for each page:
URL, tile name (as shown on screen), tabs, and tile IDs.
Explain short words (like PMPM) right after the table.

## 3. Expected behavior (must not change)
### 3.1 / 3.2 / ... one sub-section for each part of the tile
- What the user sees: charts, tabs, labels, numbers, colors.
- What each click, button, or link does. Say what it does NOT do, too.
### Last sub-section: other things on the tile
- Help icon, add to story, custom period, and differences between the pages.
```

## Do not include

- Test cases.
- Dev notes: SQL, DAO names, JSON shape, file paths, line numbers.
- Lists of "ignore this" vs "take care of this". Only describe what exists today.

## Writing style

The readers include non-native English speakers.

- Put the main point first.
- Use small words, short sentences, and short lists.
- If you must use a big word, explain it right after.
- Write only what is needed.
- Keep URLs, IDs, and labels exactly as they are in the app.

---
name: es-migration-ticket-v2
description: Writes a Jira story ticket for moving one dashboard tile from the MySQL service to the ES service. The ticket describes what the tile does today, so QA can check that nothing breaks. Use when the user names a tile and its pages or controllers and asks for an ES migration ticket, story, or requirement.
---

# ES Migration Ticket (v2)

## Goal

Write **one** story ticket for **one tile**. The ticket covers every page that shows the tile.

- **MySQL service:** code in this project that reads data with MySQL queries.
- **ES service:** a separate service that reads data with Elasticsearch and shares it through APIs. This project calls those APIs.
- **Reader:** QA. The ticket is their reference for testing.
- **Content:** what the tile does **today**. That behavior must not break.

## Input

You need 2 things. Ask the user if one is missing.

1. **Tile name.** Example: "Brand Drug Use".
2. **Pages and controllers.** Example: Population = `Pop01Controller` `index`, Pharmacy = `Phm01Controller` `index`.

## Steps (tick each one)

Use [REFERENCE.md](REFERENCE.md) to find files.

- [ ] 1. **Find the tile** in each page's view (`index.gsp`). Note the tile IDs, tabs, and the `action` each part calls.
- [ ] 2. **Follow the data.** For each action, find the controller method, the DAO method, and the SQL. Check both the normal DAO and the custom-period DAO.
- [ ] 3. **Follow the screen.** Open the Vue component and the chart script. Note the labels, colors, buttons, links, and what a click does.
- [ ] 4. **Check titles.** Look up each message code in the titles file. Do not guess a title.
- [ ] 5. **Compare the pages.** Note every difference, such as add to story, filters, or when a part loads.
- [ ] 6. **Write the ticket** in the format below.
- [ ] 7. **Run the self-check** below. Fix anything that fails.
- [ ] 8. **List your assumptions** after the ticket. Ask the user about each one.

## Output format

Only these 3 sections. Model: [examples/brand-drug-use-tile.md](examples/brand-drug-use-tile.md).

```
# ES Migration | <Page 1> + <Page 2> | <Tile name> tile

## 1. Summary
What moves, and from where to where.
The user must see the same thing as before. Only the data source changes.

## 2. Where
Table with one column per page: URL, tile name on screen, tabs, tile IDs.
Explain any short word (like PMPM) under the table.

## 3. Expected behavior (must not change)
### 3.1, 3.2, ... one sub-section per part of the tile (chart, summary box, ...)
What the user sees: tabs, labels, numbers, colors.
What each click, button, or link does. Also say what it does NOT do.
### Last sub-section: Other things on the tile
Help icon, add to story, custom period, differences between pages.
```

## Do not include

- Test cases.
- Dev notes: SQL, DAO or method names, JSON, file paths, line numbers.
- "Ignore this / take care of this" lists. Only describe what exists today.

## Self-check (before you answer)

- [ ] Only sections 1, 2, 3. Nothing after section 3.
- [ ] No code, SQL, file paths, or method names in the ticket.
- [ ] Every title, ID, URL, and label is copied exactly from the code.
- [ ] Every page that shows the tile is covered.
- [ ] Each point is about what the user sees or does.
- [ ] Short sentences. Small words. Big words are explained.
- [ ] Every point you did not confirm in the code is in the assumptions list.

## Writing style

The readers include non-native English speakers.

- Put the main point first.
- Use small words, short sentences, and short lists.
- If you must use a big word, explain it right after.
- Write only what is needed.

## Answer to the user

1. The ticket. Also save it as `docs/es-migration-<tile-name>-tile.md` if you can write files.
2. Your assumptions list, as short questions.

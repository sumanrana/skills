# Code map: where a tile lives

Use this map to find the parts of a tile. All paths start at the project root.

## 1. Screen (front end)

| What | Where |
|---|---|
| Page view (the tile's HTML) | `grails-app/views/<page>/index.gsp` (for example `pop01`, `phm01`) |
| Page Vue app (which tiles load, page cohort) | `grails-app/assets/javascripts/pages/<page>/index.js` |
| Vue components (`draw-chart`, `pharmacy-use-summary`, `card-header-menu`, ...) | `grails-app/assets/javascripts/components/` |
| Chart drawing scripts (d3) | `grails-app/assets/javascripts/zph/z5-*.js.es6`. The `js-call` attribute in the gsp names the function, for example `drawStackAreaChart` |
| Titles and labels | `src/main/webapp/z5application.properties`. The gsp uses `<g:editMessage code="..."/>` |

How to read a tile in the gsp:
- Look for a comment block such as `----Brand Drug Use----`.
- `<span class="id-code">` shows the code that appears on screen, for example `POPSUM010`.
- `id="z5..."` is the tile ID. `graphsObjects['z5...']` decides when it loads and shows.
- `controller="..."` + `action="..."` is the backend call.
- `card-header-menu` holds the help icon (`help-id`) and add to story (`show-add-to-story`).

## 2. Data (backend)

| What | Where |
|---|---|
| Controllers | `grails-app/controllers/com/zph/<Page>Controller.groovy` |
| Shared controller code, ES call helper `fetchDashboardEs` | `grails-app/controllers/com/zph/BaseController.groovy` |
| DAO picker (normal or custom period) | `src/main/java/com/zph/dao/factory/*Factory.java`, `getDao(isCustomPeriodEnabled)` |
| Normal DAO (SQL) | `src/main/java/com/zph/dao/*MySql.java` |
| Custom-period DAO (SQL) | `src/main/java/com/zph/dao/custom/*CustomMySql.java` |
| DAO interfaces | `src/main/java/com/zph/dao/I*.java` |

Custom period = the user picks their own dates. The custom DAO is used only when the `CUSTOM_PERIOD` feature is on **and** the request sends `customPeriod: true`.

## 3. Tiles already moved to ES (use as a pattern)

- Search the controllers for `fetchDashboardEs`, `fetchPopulationEs`, and `fetchPharmacyEs`.
- Search git history: `git log --oneline --grep="es-migration" -i`.
- Cohort ID flow:
  - Population sends `cachedCohortId` when the page is opened from a member drill.
  - Pharmacy sends the page `cachedCohortId` for its tiles.
  - Search for `cachedCohortId` and `cohortCacheId`.

## 4. Things to check on every tile

- Tabs (for example Spend and PMPM) and the tile ID behind each tab.
- Labels and units on the axes, legends, and summary numbers.
- Colors that carry meaning (for example red = up, green = down).
- What a click does: a pop-up, a member list (drill), or nothing.
- Buttons and links, and where they go.
- Help icon and add to story: when they show.
- Custom period on and off.
- Differences between pages.

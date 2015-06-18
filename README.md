v0.2
# `<zero-pages>`
A better pages element for Polymer 1.0 wrapping Polymer Elements' neon-animated-pages.

### Right to the money

Example usage:

```html
<zero-pages id="pages" initial-selection="page1" animate-initial-selection>

  <!-- Page 1 -->
  <zero-page page-id="page1">
    <div>This is Page One.</div>
  </zero-page>

  <!-- Page 2 -->
  <zero-page page-id="page2">
    <div>This is Page Two.</div>
  </zero-page>

</zero-pages>
```

Change pages programatically with fade effect (default) by either:

1. Calling `this.$.pages.changePage("page2")` in the parent.
2. Firing `this.fire("zero-change-page", {pageId: "page2"}` event from your child page.

Example usage with sugar:

```html
<!-- Since initial-selection is not defined, nothing will be loaded until the first
     "changePage(page-id)" is called. The animate-initial-selection attribute tells
     <zero-pages> to animate the first page. -->
<zero-pages id="pages" animate-initial-selection>

  <!-- Main Page (This gets loaded in DOM when the first "changePage" is called,
       and remains in DOM throughout. Listens for a "page-ready" event from
       <main-page> and only performs the change when the event is received.) -->
  <zero-page page-id="main" unveil-on-page-ready is-parent-of="*">
    <page-main custom-var="[[myVarThatTakesTimeToLoad]]"></page-main>
  </zero-page>

  <!-- Info Page (<page-info> loaded into DOM when "changePage('info')" is called.
       The page is unveiled immediately without waiting for a "page-ready" event.
       On exit, <page-info> is removed from DOM after default 500ms delay -->
  <zero-page page-id="info">
    <page-info></page-info>
  </zero-page>

  <!-- Settings Page (This is dynamically loaded into DOM only when "changePage"
       is called. When user leaves the page, that page is removed from DOM -
       thereby triggering its "detached" callback after default 500ms delay.) -->
  <zero-page page-id="settings" unveil-on-page-ready>
    <page-settings></page-settings>
  </zero-page>

  <!-- Some Important Page (When changePage('impt') is called, both "settings" and
       "info" will also be loaded into DOM. When exited, it will be removed from DOM
       after a 2000ms delay) -->
  <zero-page page-if="impt" is-parent-of="info settings" delay-on-detach="2000">
    <page-important var1="[[someVarFromSettings]]" var2="[[somVarFromInfo]]"></page-important>
  </zero-page>

</zero-pages>
```

If you are (like me) concerned about the DOM, `<zero-pages>` allows succinct, declarative control of what gets loaded, how it's loaded, and what's not.

### Published Attributes

`<zero-pages>`

| Property                  | Description                                                |
|---------------------------|------------------------------------------------------------|
| `initialSelection`        | *String*. Display the initial page immediately on load.    |
| `animateInitialSelection` | *Boolean*. When true, animates the first page when shown.  |
| `transition`              | *String*. Defines the transition between pages. ***TODO*** |

`<zero-page>`

| Property            | Type    | Description                                            |
|---------------------|---------|--------------------------------------------------------|
| `pageId`            | String  | Compulsory to set a unique identifier for that page.   |
| `unveilOnPageReady` | Boolean | When true, the actual page change occurs only after a `page-ready` event is received from its child page. |
| `isParentOf`        | String  | Space delimited string of `page-id`s to ensure that this page is kept in DOM when its "children" are shown. Set to `*` if you want this page to be in DOM at all times. |
| `delayOnDetach`     | Number  | Delay in ms when detaching the old page after the new page is called. Defaults to 500ms. |

### Public Methods

`<zero-pages>`

| Function             | Parameters           | Description                              |
|----------------------|----------------------|------------------------------------------|
| `changePage(pageId)` | *pageId* as *String* | Actually begin the change page process from current page to `pageId`. |

### Events

| Event              | Description                                                       |
|--------------------|-------------------------------------------------------------------|
| `zero-page-ready`  | If a `zero-page` is set to `unveilOnPageReady`, your child page must fire a `zero-page-ready` event. This notifies `<zero-pages>` to actually change the page |
| `zero-change-page` | On child pages, firing a `zero-change-page` event will notify `<zero-pages>` to begin the `changePage` method on `event.detail.pageId`. For example, calling a `this.fire("zero-change-page", {pageId: "page2"});` will immediately change the current page to `page2`. |

### Convenience Properties

`<zero-pages>`

| Property    | Type   | Description                                                     |
|-------------|--------|-----------------------------------------------------------------|
| `_curpage`  | String | Contains `pageId` of the current page.                          |
| `_pageList` | Array  | Contains array of valid `pageId`s that were declared.           |

### To-Do

1. Expose a `transition` attribute on `<zero-pages>` allowing selection of transition type between pages.
2. Add Demo.

### Version History

1. 2015-06-03: v0.1 - first commit
2. 2015-06-18: v0.2 - renamed `page-ready` event to `zero-page-ready`, add `zero-change-page` feature, add `_pageList` convenience property.


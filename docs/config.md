# Configuration

A `observablehq.config.js` (or `observablehq.config.ts`) file located in the project root allows configuration of your project. For example, a site might use a config file to set the project’s title and the sidebar contents:

```js run=false
export default {
  title: "My awesome project",
  pages: [
    {name: "Getting awesome", path: "/getting-awesome"},
    {name: "Being awesome", path: "/being-awesome"},
    {name: "Staying awesome", path: "/staying-awesome"}
  ]
};
```

Configuration files are optional. Any options you don’t specify will use the default values described below.

Configuration files are code. This means they can be dynamic, for example referencing environment variables. The configuration is effectively baked-in to the generated static site at build time. During preview, you must restart the preview server for changes to the configuration file to take effect.

The following options are supported.

## root

The path to the source root; defaults to `docs`.

## output

The path to the output root; defaults to `dist`.

## theme

The theme names, if any; defaults to `auto`. Themes affect the visual appearance of pages by specifying colors and fonts, or by augmenting default styles. The theme option is a convenient shorthand alternative to specifying a [custom stylesheet](#style).

The current built-in themes are:

- `auto` (default) - `light` or `dark` depending on [user preference](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)
- `auto-alt` - `light-alt` or `dark` depending on [user preference](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)
- `light` - a white background with dark text
- `light-alt` - a light gray background with dark text
- `dark` - a dark gray background with light text
- `dark-alt` - a very dark gray background with light text
- `wide` - a full-width main column

You can combine themes like so:

```js run=false
theme: ["auto-alt", "wide"]
```

A theme can be applied to an individual page via the [front matter](./markdown.md#front-matter):

```yaml
---
theme: [auto-alt, wide]
---
```

## style

The path to a custom stylesheet, relative to the source root. This option takes precedence over the [theme option](#theme) (if any), providing more control by allowing you to remove or alter the default stylesheet and define a custom theme.

The custom stylesheet should typically import `observablehq:default.css` to build on the default styles. You can also import any of the built-in themes. For example, to create a stylesheet that builds up on the `light` theme, create a `custom-style.css` file in the `docs` folder, then set the style option to `custom-style.css`:

```css
@import url("observablehq:default.css");
@import url("observablehq:theme-light.css");

:root {
  --theme-foreground-focus: green;
}
```

If you build on the `auto` or `auto-alt` themes, note that these themes’ colors are chosen according to the user’s [preferred color scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme).

The default styles are implemented using CSS custom properties. These properties are designed to be defined by themes or custom stylesheets. The following custom properties are supported:

- `--theme-foreground` - page foreground color, _e.g._ black
- `--theme-background` - page background color, _e.g._ white
- `--theme-background-alt` - block background color, _e.g._ light gray
- `--theme-foreground-alt` - heading foreground color, _e.g._ brown
- `--theme-foreground-muted` - secondary text foreground color, _e.g._ dark gray
- `--theme-foreground-faint` - faint border color, _e.g._ middle gray
- `--theme-foreground-fainter` - fainter border color, _e.g._ light gray
- `--theme-foreground-faintest` - faintest border color, _e.g._ almost white
- `--theme-foreground-focus` - emphasis foreground color, _e.g._ blue

A custom stylesheet can be applied to an individual page via the [front matter](./markdown.md#front-matter):

```yaml
---
style: custom-style.css
---
```

In this case, the path to the stylesheet is resolved relative to the page’s Markdown file rather than the source root.

## title

The project’s title. If specified, this text is used for the link to the home page in the sidebar, and to complement the titles of the webpages. For instance, a page titled “Sales” in a project titled “ACME, Inc.” will display “Sales | ACME, Inc.” in the browser’s title bar. If not specified, the home page link will appear as “Home” in the sidebar, and page titles will be shown as-is.

## pages

An array containing pages and/or sections. If not specified, it defaults to all Markdown files found in the source root in directory listing order.

The following TypeScript interfaces describe pages and sections:

```ts run=false
export interface Page {
  name: string;
  path: string;
}
```
```ts run=false
export interface Section {
  name: string;
  pages: Page[];
  open?: boolean;
}
```

If a section’s **open** option is not set, it defaults to true.

Projects can have “unlisted” pages that are not included in the pages list. These pages will still be accessible if linked from other pages or visited directly, but they won’t be listed in the sidebar or linked to via the previous & next footer.

The pages list should _not_ include the root page, `index.md`. Also, we don’t recommend using query strings or anchor fragments, as these will prevent the previous & next footer links from navigating.

## pager

Whether to show the previous & next footer links; defaults to true.

## footer

An HTML fragment to add to the footer. Defaults to `Built with Observable`.

## toc

The table of contents configuration.

The following TypeScript interface describes this option:

```ts run=false
export interface TableOfContents {
  show?: boolean;
  label?: string;
}
```

If **show** is not set, it defaults to true. If **label** is not set, it defaults to “Contents”. The **toc** option can also be set to a boolean, in which case it is shorthand for **toc.show**.

If shown, the table of contents enumerates the second-level headings (H2 elements, such as `## Section name`) on the right-hand side of the page. The currently-shown section is highlighted in the table of contents.

The table of contents configuration can also be set in the page’s YAML front matter. The page-level configuration takes precedence over the project-level configuration. For example, to disable the table of contents on a particular page:

```yaml
---
toc: false
---
```
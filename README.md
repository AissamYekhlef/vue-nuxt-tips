# Welcome to VuejsTips

A [Slidev](https://sli.dev/) presentation containing practical Vue, Nuxt, and Vite tips.

## Getting Started

### Install dependencies

```bash
npm install
```

### Start the development server

```bash
npm run dev
```

Then open **http://localhost:3030**.

Edit [`slides.md`](./slides.md) to add or update slides. Changes are reflected automatically during development.

## Project Structure

```text
.
├── slides.md          # Main presentation
├── public/            # Static assets
├── components/        # Reusable Vue components
├── styles/             # Custom styles
├── package.json        # Scripts and dependencies
└── README.md           # This file
```

## Useful Commands

### Development

```bash
npm run dev
```

Use another port when needed:

```bash
npm run dev -- --port 4000
```

### Build for Deployment

Build the presentation as a static web application:

```bash
npm run build
```

The output is generated in:

```text
dist/
```

Preview the production build:

```bash
npx vite preview
```

### Export to PDF

Install the Chromium renderer if it is not already installed:

```bash
npm install -D playwright-chromium
```

Then export:

```bash
npm run export
```

By default, the PDF is generated as:

```text
slides-export.pdf
```

You can also run:

```bash
npx slidev export
```

> PDF/PPTX/PNG exports use Playwright/Chromium to render the slides.

### Export to PowerPoint

```bash
npx slidev export --format pptx
```

This exports slides as images inside the PowerPoint file.

For an editable PowerPoint export:

```bash
npx slidev export --format pptx-editable
```

### Export to PNG

```bash
npx slidev export --format png
```

### Export to Markdown

```bash
npx slidev export --format md
```

### Export Specific Slides

```bash
npx slidev export --range 1,4-6,10
```

This exports slides 1, 4, 5, 6, and 10.

### Export Click Steps

By default, click animations are not exported as separate pages.

To export every click step:

```bash
npx slidev export --with-clicks
```

### Export in Dark Mode

```bash
npx slidev export --dark
```

### Add a PDF Table of Contents

```bash
npx slidev export --with-toc
```

## Presenting

Run:

```bash
npm run dev
```

Then open:

```text
http://localhost:3030
```

### Presenter Mode

Open:

```text
http://localhost:3030/presenter
```

Presenter mode provides a separate view for controlling the presentation and viewing presenter information/notes.

A typical setup is:

```text
Laptop
└── Presenter mode

Projector / second screen
└── Normal presentation
```

The presentation windows stay synchronized.

## Speaker Notes

Add speaker notes using an HTML comment:

```md
# Vue Reactivity

Vue tracks reactive state automatically.

<!--
Explain that Vue tracks refs and reactive objects.
Mention the main caveats here.
-->
```

If you are building a public version and do not want speaker notes included:

```bash
npx slidev build --without-notes
```

## Creating Slides

Separate slides with `---`:

```md
# Vue Reactivity

Vue tracks reactive state automatically.

---

# Example

```ts
const count = ref(0)
```

---

# Another Slide

Keep each slide focused on one idea.
```

## Slide Frontmatter

Presentation-wide configuration goes at the beginning of `slides.md`:

```md
---
theme: default
title: VuejsTips
author: Aissam
class: text-center
transition: slide-left
---

# Welcome to VuejsTips
```

Export configuration can also be defined in frontmatter:

```yaml
---
exportFilename: vuejs-tips
export:
  format: pdf
  withClicks: false
  withToc: true
---
```

## Using Vue Components

Slidev is powered by Vue, so Vue components can be used inside slides:

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<button @click="count++">
  Clicked {{ count }} times
</button>
```

Reusable components can be placed in:

```text
components/
```

## Code Blocks

Use fenced Markdown code blocks:

````md
```ts
const message = 'Hello Vue!'
console.log(message)
```
````

Slidev provides syntax highlighting and supports interactive code examples.

## Images and Assets

Place static assets in:

```text
public/
```

Then reference them from slides:

```md
![Vue Logo](/vue-logo.png)
```

## Useful Slidev Features

- Markdown-based slides
- Vue components and reactivity
- Syntax-highlighted code
- Interactive code examples
- Animations and click steps
- Presenter mode
- Speaker notes
- LaTeX / math
- Mermaid diagrams
- Icons
- Custom themes
- Custom CSS
- Static-site builds
- PDF, PNG, PPTX, and Markdown exports

## Deployment

Build the presentation:

```bash
npm run build
```

Deploy the generated `dist/` directory to a static hosting provider.

If deploying under a sub-path:

```bash
npx slidev build --base /vuejs-tips/
```

## Troubleshooting

### Port 3030 is already in use

```bash
npm run dev -- --port 4000
```

### PDF export fails

Install Chromium support:

```bash
npm install -D playwright-chromium
```

Then retry:

```bash
npm run export
```

### Changes are not visible

Make sure you are editing:

```text
slides.md
```

Then restart:

```bash
npm run dev
```

### Need more CLI options

```bash
npx slidev --help
```

## Recommended Workflow

```bash
# Install
npm install

# Develop
npm run dev

# Edit
# slides.md

# Build
npm run build

# Export PDF
npm run export
```

For presenting, use the normal browser view for the audience and `/presenter` for presenter controls.

## Documentation

- [Slidev Documentation](https://sli.dev/)
- [Getting Started](https://sli.dev/guide/)
- [Slidev CLI](https://sli.dev/builtin/cli)
- [Exporting](https://sli.dev/guide/exporting)
- [Building and Hosting](https://sli.dev/guide/hosting)
- [Customizations](https://sli.dev/custom/)

## License

This project is a personal collection of Vue, Nuxt, and Vite tips presented with Slidev.


## Credits: 
-  [VuejsTips](https://VuejsTips.com/) By [MichałKuncio](https://michalkuncio.com/)
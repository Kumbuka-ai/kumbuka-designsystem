# Kumbuka Design System

The design system of the Kumbuka suite: tokens, components, interaction rules and, from the
pattern round on, page patterns and templates. It is the leading design source; the Kumbuka
console and the service interfaces are concrete applications built on it.

Status: **0.1, work in progress.** Sections of the documentation are marked ratified,
proposed or open. Nothing here is a stable API yet.

## Contents

| Path | What |
|---|---|
| `tokens/tokens.css` | Primitives and semantic tokens for the light and the dark theme, density |
| `css/components.css` | Components; they read semantic tokens only |
| `docs/DESIGN-SYSTEM.md` | The rules: what is ratified, proposed and open |
| `showcase/index.html` | Renders every component and state for review (open it in a browser) |
| `assets/brand/` | Kumbuka logo, not covered by the code licence, see `TRADEMARKS.md` |

## Using it

Link the two stylesheets and put `class="k-root"` on the element that hosts the interface;
set `data-theme="dark"` on `<html>` for the dark theme and `data-density="compact"` for the
compact density.

```html
<link rel="stylesheet" href="tokens/tokens.css">
<link rel="stylesheet" href="css/components.css">
```

Fonts: Inter, Space Grotesk, JetBrains Mono. Icons: Lucide (ISC). The product components
will be built on TanStack Table, React Aria Components and dnd-kit (see section 23 of the
documentation); the showcase script is a hand-written demonstration, not the implementation.

## Licence

Code and documentation: MIT, see `LICENSE`. The Kumbuka name and logo are not licensed
under MIT, see `TRADEMARKS.md`.

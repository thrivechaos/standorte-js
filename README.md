# Webflow Custom Scripts

This repository hosts custom JavaScript files used across Webflow projects.

## primes-cantonales/js/

Custom PLZ (postal code) search scripts for the [Primes Cantonales](https://primes-cantonales.webflow.io) site.

| File | Description |
|------|-------------|
| `standorten.js` | German version — 4121 Swiss postal code entries, search dropdown |
| `standorten-fr.js` | French version — 4121 Swiss postal code entries, search dropdown |

### How it works

Each file contains a hardcoded `locations` array with all 4121 Swiss postal codes sourced from 4 Webflow CMS collections (Standorte 1–4). Two functions are exposed:

- `showResults()` — called on input focus, shows top 5 entries
- `filterResults()` — called on each keyup, filters by typed text and shows/hides the dropdown

### Bug fix (2026-11-17)

`filterResults()` was not setting `dropdownContent.style.display = "block"` after populating results. This caused the dropdown to stay hidden after the user typed anything. Fixed by:
- Setting `display: block` when results exist
- Setting `display: none` when no results match or input is cleared

### Usage in Webflow (Page Settings → Head Code)

```html
<script>
  function loadLanguageSpecificJS() {
    let script = document.createElement('script');
    const normalizedPath = window.location.pathname.endsWith('/') ? window.location.pathname : window.location.pathname + '/';
    script.src = normalizedPath === '/fr-ch/' 
      ? 'https://cdn.jsdelivr.net/gh/thrivechaos/webflow@main/primes-cantonales/js/standorten-fr.js' 
      : 'https://cdn.jsdelivr.net/gh/thrivechaos/webflow@main/primes-cantonales/js/standorten.js';
    document.head.appendChild(script);
  }
  loadLanguageSpecificJS();
</script>
```

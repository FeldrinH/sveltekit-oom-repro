# Minimal reproduction of SvelteKit dev server OOM

### Steps to reproduce the issue:

1. Run `npm run dev`.
2. Load `localhost:5173/largefile` 

### Observed behavior:   
Upon loading the page the dev server (not the browser) suddenly consumes more than 2GB of memory and crashes due to reaching the heap allocation limit (on my system).

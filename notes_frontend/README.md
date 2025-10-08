# Notes Frontend (Svelte + SvelteKit)

A simple notes manager with create, list, edit, and delete functionality. Data is stored locally in your browser using localStorage — no backend needed.

- Theme: Ocean Professional (primary #2563EB, secondary/success #F59E0B, error #EF4444, background #f9fafb, surface #ffffff, text #111827).
- Responsive split layout: notes list on the left, editor on the right.
- Keyboard: Ctrl/Cmd+S to save the current note.

## Development

Install and run:

```bash
npm install
npm run dev
```

App runs on http://localhost:3000 (configured via Vite). No additional configuration required.

## Build

```bash
npm run build
npm run preview
```

## Notes

- Data persists in localStorage under key `notes_manager_notes_v1`.
- You can safely clear storage from your browser to reset the app.

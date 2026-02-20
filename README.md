# Headless UI Vue Popover focus-restore repro

Minimal reproduction for Headless UI Vue Popover focus restoration behavior.

## Run

```bash
npm install
npm run dev
```

Open `http://localhost:5173`.

## Repro steps

1. Click the text input (it opens the popover).
2. Click in the gray "Outside click area".
3. Observe the input regains focus after close.

## Expected

When popover closes on outside click, there should be a supported way to disable trigger focus restoration (for this UX).

## Actual

Popover restores focus to the trigger input by default, which causes refocus loops/flicker when trying to keep input unfocused after outside close.

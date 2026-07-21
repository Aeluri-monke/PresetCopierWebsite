# Preset Merger

Copy prompts between SillyTavern Chat Completion presets without re-typing them.

**Use it here → https://aeluri-monke.github.io/Aeluris-preset-merger/**

---

## Nothing is saved here ^_^

This is a single HTML page with no backend. There's no server, no database, no analytics, no account.

Your preset files are read by your own browser using the standard file picker, held in memory while the tab is open, and written back out as a download. They never leave your computer — nothing is uploaded anywhere, and closing the tab discards everything.

If you'd rather not take my word for it: save the page, unplug your wifi, and it still works exactly the same. You can also read the whole thing in `index.html` — it's one file, no dependencies.

---

## How to use it

1. In SillyTavern, export the preset you want to copy **from**, and the one you want to copy **into**
   (AI Response Configuration → Chat Completion Presets → export)
2. Open the tool and load your source preset, then your target preset
3. Tick the prompts you want to bring over
4. Click **Merge and download**
5. Import the downloaded file back into SillyTavern

Your originals aren't modified. You get a new file with `(merged)` in the name, so if something looks wrong you still have what you started with.

---

## How it works

A prompt in a Chat Completion preset lives in **two** places in the JSON, and it needs both to actually show up:

```
prompts[]        ->  the prompt itself: name, role, content, identifier
prompt_order[]   ->  where it sits in the list, and whether it's switched on
```

`prompts[]` is the definition. `prompt_order[]` is a separate list of `{identifier, enabled}` entries telling SillyTavern what to display and in what order — and there's one of these blocks per `character_id`, so an entry has to be added to each of them.

This is the part that trips up hand-editing. Paste a prompt into `prompts[]` only and it's genuinely in the file but invisible in the interface, because nothing in `prompt_order[]` points at it. The tool writes both, every time, across every order block.

A few other things it handles:

- **Duplicate identifiers.** If the target preset already uses the identifier your copied prompt has, the copy gets a fresh UUID instead. Nothing overwrites anything.
- **Markers.** Chat History, World Info, Char Description and friends are structural placeholders that already exist in every preset. Copying them is almost never what you want, so they're behind a checkbox.
- **On/off state.** Copied prompts can be switched on automatically, or keep whatever state they had in the source preset.

---

## If something goes wrong

**"No prompts[] array"** — the file probably isn't a Chat Completion preset. Text Completion presets and settings backups have a different shape.

**A prompt didn't come through** — check whether it was a marker; those are skipped unless you tick the box.

**SillyTavern won't import the merged file** — open an issue with what it said and I'll take a look.

---

## License

Do whatever you like with it. Fork it, host your own copy, tear it apart. Just don't sell it. And credit me for the base ^_^

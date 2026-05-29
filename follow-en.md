# Following a display scene

## In one sentence

"Following" makes one display scene dynamically inherit **all the layout and bindings** of another. The only differences between them are the **name** and the **physical monitors** each is bound to. Change the source, and every scene following it picks up the change the next time you switch — no second edit needed.

## Why this exists

Say you have two landscape monitors at the office, with a whole set of bindings carefully arranged: browser on the left, editor on the right, chat pinned to a corner, and so on.

At home you also have two landscape monitors. To you this is "the same workspace, just different physical displays." But normally you'd have to rebuild every binding from scratch for the new "Home" scene — and whenever you tweak the office setup, you'd have to mirror the change at home by hand.

Following exists to remove that duplication.

## Following is not copying

- **Copying** is a one-time snapshot: duplicate the office config for home. After that they drift apart — change the office later and home stays stale until you patch it manually.
- **Following** is live inheritance: home always inherits the office layout in real time. Change one binding at the office and home reflects it on the next switch. **Maintain once, applies everywhere.**

## What it does for you

- Share one layout across several "same-shape" environments: home / office, desktop / laptop dock, different meeting rooms with dual screens. Set it up once; it's correct everywhere.
- The same hotkey lands on the right monitor under different hardware — gmux automatically picks the matching scene based on the displays you currently have attached.

## How it differs from an ordinary scene in daily use

The things worth remembering when you use a following scene:

1. **You don't edit the layout in the following scene.** A following scene has no bindings of its own — to adjust the layout, edit the **source scene it follows**; both update together.
2. **A following scene only lets you change:** its name, the monitors it's bound to, the display role mapping (which source monitor each of your monitors "plays"), and which scene it follows.
3. **Shapes must be compatible to follow:** the **same number** of monitors, each with **matching orientation** (landscape↔landscape, portrait↔portrait). Scenes that don't match won't appear in the follow list.
4. **Want to change just a binding or two? Detach.** A following scene's editor offers "Convert to an independent scene": it solidifies all currently inherited bindings into this scene's own config, stops following, and lets you edit freely from then on.
5. **A followed source can't be deleted outright.** While any scene still follows it, deletion is blocked, prompting you to first detach those scenes or repoint their follow target.
6. **A broken follow won't misplace windows.** If you later change the source's monitor count or orientation so the mapping no longer fits, the following scene is flagged in red to update its mapping; until fixed, gmux skips it rather than placing windows in the wrong spot.

## How to use it

1. Create or edit a display scene and choose the monitors it's bound to.
2. Expand "Advanced."
3. Under layout source, pick "Follow an existing display scene (auto-sync layout)."
4. Select the source scene to follow (only shape-compatible ones are listed).
5. Check the display mapping — which source monitor each of your monitors plays (gmux sets sensible defaults by orientation; adjust if needed).
6. Save. From now on this scene continuously follows the source's layout.

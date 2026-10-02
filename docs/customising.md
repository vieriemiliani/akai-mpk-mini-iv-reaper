# Customising

How to change what the MPK does: knob layouts, pad colours, pad functions and
the DAW buttons. Read the [user guide](user-guide.md) first; this page
assumes the setup described there is working.

Everything here is done in ReaLearn. You never deal with MIDI numbers, ports
or Shift.

---

## How it fits together

The **Controller preset** turns the MPK's MIDI into named controls. The
**Main presets** say what those names do. Customising almost always means
editing a Main preset.

```text
control        name                 where it comes from
knobs          encoder/1-64         knob N on page P = encoder/(8 x (P-1) + N)
CC mode pads   button/1-16          Bank A = 1-8, Bank B = 9-16
Notes pads     pad/1-16             Bank A = 1-8, Bank B = 9-16
Program pads   program/1-16         Bank A = 1-8, Bank B = 9-16
DAW buttons    daw/play-stop        daw/pause      daw/record     daw/quantize
               daw/loop             daw/undo       daw/redo
               daw/forward          daw/backward   daw/tap        daw/metronome
```

The `daw/` names already include Shift: `daw/redo` is Shift + Undo. Shift
itself has no name and cannot be mapped.

The default Controller preset, **MPK Mini IV Instrument**, keeps two things
for itself:

- **CC mode, Bank A pads 1–8** select the knob page. They do not send
  `button/1-8`.
- **Program mode pad colours** mark the last program sent. The pads still
  send `program/1-16`, but a Main preset cannot colour them.

To have all 16 CC pads and the Program mode colours for yourself, use the
Controller preset **MPK Mini IV Instrument (plain pads)**. It has no page
selection, so the knobs stay on page 1.

---

## Before you edit

**Work on a copy.** Load the preset, press **Save as…** and give it a new
name. Updating the presets from this repository then never overwrites your
work. Link your copy to the plugin instead of the original (user guide,
Step 5); a link stores the preset it points to.

**Editing in the instrument unit.** Auto-load swaps the Main preset whenever
the focus changes, and clicking into ReaLearn's own window changes it. So
switch Auto-load off while you edit:

1. In the instrument unit, set **Auto-load** to off.
2. Load the Main preset you want to change.
3. Edit it, then press **Save**.
4. Load the Main preset you want for unlinked plugins, or none.
5. Switch **Auto-load** back to **Based on unit FX**.

The DAW unit has no Auto-load; edit its preset directly and press **Save**.

**Opening a mapping.** Press **Edit** on a mapping row to open its mapping
panel. The panel has a Source, a Target and a Glue section. Use the
**Search** field above the list to find mappings by name.

---

## Knobs

Each knob mapping has a virtual source, `encoder/N`, and a target, usually
**FX parameter: Set value** on the `<Focused>` FX.

**To move a knob to another parameter,** open its mapping and choose another
parameter in the Target section. You can also press **Learn target** and
move the parameter in the plugin.

**To change how fast a knob turns,** change **Step size Min/Max** in the
Glue section. The supplied presets limit one step to 5% of the range.

### A preset for a new plugin

1. Switch Auto-load off and load no Main preset, so the list is empty.
2. Open the plugin in its own floating window.
3. Press **Learn many** and follow ReaLearn's instructions: turn a knob on
   the MPK, then move the parameter you want in the plugin. Repeat for each
   knob, then press **Stop**.
4. Fix the targets. Learning records the plugin by its position, for
   example *Track #5, FX #1*, so the preset would only work on that one
   track. With the Search field empty, so that every mapping is listed,
   choose **Menu › Modify multiple mappings › Make targets of listed
   mappings non-sticky › With track "<Unit>" … › … and FX "<Focused>"**.
   The targets then show *Track <Irrelevant>* and *FX <Focused>*: they
   control whichever plugin has focus. To fix a single mapping instead,
   press **Edit** and set the FX to `<Focused>` in its Target section.
5. **Save as…** with a name, link it to the plugin, and switch Auto-load
   back on.

Learned sources use the names above (`encoder/1`, `encoder/9` …) because
the Controller preset is loaded. To use more than one page, select the page
with the CC mode pads before learning: knob 1 on page 2 learns as
`encoder/9`.

---

## Pad colours

A pad shows the colour its Main preset sends for the control it is sending
in the current pad mode: `button/N` in CC mode, `pad/N` in Notes mode,
`program/N` in Program mode.

### The colours

The colour is a palette number. These are confirmed on the MPK:

```text
0    off
4    light red
5    red
9    amber
21   green
22   dim green
40   lighter blue
41   light blue
```

The palette has 128 entries in groups of four (light, full, dim, very dim).
Other numbers work but have not been checked on the MPK; 6 shows as off.

### Changing a mode colour

The supplied presets colour the pads with mappings named like
**Pad 1 LED in CC mode**, **Pad 1 LED in Notes mode** and **Pad 1 LED in
Program Change mode**. Their target is **ReaLearn: Dummy**, so they only
light the pad.

The colour is in the mapping's Glue section, as the feedback transformation:

```text
x = 5/127      red
x = 21/127     green
x = 41/127     light blue
```

Change the number before `/127` to another palette number and save.

There are 16 such mappings per mode. Instead of editing each one, edit the
preset file in a text editor:

1. Make your copy in ReaLearn with **Save as…**, so the new preset is
   listed straight away.
2. Open its file in a text editor. Main preset files are in
   `%APPDATA%\REAPER\Data\helgoboss\realearn\presets\main\`, in the
   subfolder you chose.
3. Replace, for example, `x = 5/127` by `x = 9/127` throughout, and save.
4. In ReaLearn, set the Main preset to `<None>` and then back to your
   preset. It loads the changed file; there is no need to restart REAPER.

### A colour that shows a state

A mapping with a real target can colour its own pad. The Helix Native
preset does this for its switches:

```text
x = y > 0.5 ? 21/127 : 0/127      green when on, off when off
```

`y` is the target's value, from 0 to 1. Any mapping that controls something
on/off can use the same line in its feedback transformation.

### What a Main preset cannot change

- **The held-pad colour.** While you hold a pad it shows the dim entry of its
  colour's group, for example dim green for green and light red for red.
- **The page pads and the Program mode marker.** In the default Controller
  preset these are amber for the current page and red for the others, and
  lighter blue for the last program sent, light blue for the others. They
  live in the Controller preset, in the mappings **Pad LED shows page 1** …
  **8** and **Pad LED shows last program sent**: replace `9/127`, `5/127`,
  `40/127` or `41/127` there. To see them, set **Show** to the Controller
  compartment. Save the Controller preset under a new name first.

---

## Pads

**CC mode, Bank B** (`button/9-16`) is free in every supplied preset. Map a
pad to anything ReaLearn can target: a plugin parameter, a REAPER action,
a track's mute. For on/off targets, set the Glue section's mode to
**Toggle button** so that each press flips the state.

**Notes mode** pads play the instrument through the track. A Main mapping on
`pad/N` does not stop that: the pad plays its note and runs the mapping.

**Program mode** pads send program changes to the track. A Main mapping on
`program/N` runs as well, as the Helix preset does to select snapshots.

---

## DAW buttons

The Main preset **MPK Mini IV DAW Actions** holds one mapping per button:

```text
name                            control          target
Play/Stop                       daw/play-stop    Project: Invoke transport action
Pause                           daw/pause        Project: Invoke transport action
Record                          daw/record       Project: Invoke transport action
Repeat (Loop)                   daw/loop         Project: Invoke transport action
Undo                            daw/undo         REAPER action 40029
Redo                            daw/redo         REAPER action 40030
Quantize (active MIDI editor)   daw/quantize     MIDI editor action 40009
Go to next marker               daw/forward      REAPER action 40173
Go to previous marker           daw/backward     REAPER action 40172
Toggle metronome                daw/metronome    REAPER action 40364
Tap tempo                       daw/tap          REAPER action 1134
```

**To give a button another action:**

1. In the DAW unit, open the mapping of that button.
2. In the Target section, keep **Project: Invoke REAPER action** and choose
   the action. Alternatively press **Learn target** and run the action from
   REAPER's Actions list.
3. For a MIDI editor action, set the target's section to **Active MIDI
   editor**, as the Quantize mapping does.
4. **Save** the preset.

An action's command ID is shown in REAPER's Actions list. Right-click an
action there to copy its ID.

---

## Keeping your changes

Presets are plain files under `%APPDATA%\REAPER\Data\helgoboss\realearn\presets\`,
in `controller\` and `main\`. Back up the ones you made. When you update from
this repository, files with the same name are replaced, which is why your own
work should carry its own name.

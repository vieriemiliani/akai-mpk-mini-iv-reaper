# MPK mini IV for REAPER — User Guide

This guide sets up an AKAI MPK mini IV as a REAPER controller with ReaLearn:
the knobs control whichever plugin you are looking at, the pads light up to
show what they do, and the DAW buttons drive REAPER's transport.

You need no programming and no extra software beyond REAPER and ReaLearn.

---

## What you get

- **64 knob slots on the focused plugin.** The 8 knobs control the plugin
  whose window you clicked last. Eight pages of eight give 64 parameters.
  The knobs are endless and relative, so nothing jumps when you change
  page or plugin.
- **Page selection on the pads.** In CC mode, pads 1–8 of Bank A pick the
  knob page. The current page's pad is amber, the others red.
- **Pad colours that show the pad mode.** Red in CC mode, green in Notes
  mode, light blue in Program Change mode. A pad dims while you hold it.
  In Program Change mode the last program you sent is marked.
- **A Main preset per instrument.** When you focus a plugin, ReaLearn loads
  the knob layout linked to it.
- **DAW buttons.** Play, Record, Loop, Undo, Tap and `+` work with REAPER,
  each with a second function while Shift is held.

Keys, pads and the mod wheel still play your instrument as usual, through a
track whose input is the MPK. ReaLearn listens alongside the track and does
not take the notes away.

---

## Requirements

```text
Tested with
    Windows 11
    REAPER
    ReaLearn 2.18.2 (part of Helgobox, installed through ReaPack)
    MPK mini IV, firmware 1.4.1.0
```

ReaLearn 2.18.2 is the minimum. Later versions, macOS and Linux have not
been tested.

---

## Step 1 — Set up a user preset on the MPK

The MPK's factory user presets send pad and knob messages that would
interfere with instruments. Fix one user preset once, on the MPK's own
screen; it cannot be written from a computer.

Start from any user preset (this guide was tested with **USER1**) and
change three things:

```text
setting                  factory value           set to
pad CC#, Bank A          32 33 34 35 36 37 38 39  102 103 104 105 106 107 108 109
pad CC#, Bank B          40 41 42 43 44 45 46 47  110 111 112 113 114 115 116 117
CC#, Knob-1 to Knob-8    24 25 26 27 28 29 30 31  20 21 22 23 24 25 26 27
Mode, Knob-1 to Knob-8   absolute                 Rel (relative, INC/DEC)
```

Then save the preset on the device.

Leave everything else as it is: pad notes (Note#, 36–51), pad program
changes (PC#, 0–15), pad MIDI channels (MidiCh), knob range (LoVal 0,
HiVal 127) and pad colours (Off C, On C). ReaLearn sets the pad colours
itself.

Why these numbers: CC 102–117 and CC 20–27 have no standard meaning, so an
instrument ignores them. The factory pad CCs 32–47 are the fine-resolution
halves of common controllers such as Mod Wheel, Volume and Expression.

Use this preset whenever you work in REAPER.

---

## Step 2 — Enable the MPK's ports in REAPER

Open **Options › Settings › MIDI Devices** and enable the input of these two
devices:

```text
MPK mini IV               the knobs, pads and keys
MIDIIN2 (MPK mini IV)     the DAW buttons
```

Enable the output of **MPK mini IV** too. ReaLearn uses it to colour the
pads.

Leave MIDIIN3 and MIDIIN4 disabled. They are not used.

---

## Step 3 — Install the presets

Copy the preset files from this repository into ReaLearn's preset folders.
A subfolder of your own keeps them apart from ReaLearn's factory presets:

```text
realearn/controller/mpk-mini-iv-instrument.json
realearn/controller/mpk-mini-iv-instrument-plain-pads.json
realearn/controller/mpk-mini-iv-daw.json
    ->  %APPDATA%\REAPER\Data\helgoboss\realearn\presets\controller\mpk-mini-iv\

realearn/main/mpk-mini-iv-daw-actions.json
realearn/main/kk-instrument-controller.json
realearn/main/sic.json
realearn/main/helix-native-mpk.json
    ->  %APPDATA%\REAPER\Data\helgoboss\realearn\presets\main\mpk-mini-iv\
```

Restart REAPER so ReaLearn lists them.

The subfolder name becomes part of each preset's ID, for example
`mpk-mini-iv/sic`, and the links you make in Step 5 store that ID. Choose
the folder name before linking and do not rename it afterwards.

The two kinds of preset do different jobs:

- **Controller presets** describe the MPK. You never need to edit them.
- **Main presets** say what the controls do in REAPER. These are yours to
  change; see [Customising](customising.md).

---

## Step 4 — Add ReaLearn to the Monitoring FX

Use **one** ReaLearn instance on REAPER's **Monitoring FX** chain (View ›
Monitoring FX). It runs for every project and never ends up on an
instrument track by accident. Inside it, create two units.

### Unit 1 — instrument

```text
Input               MIDI: MPK mini IV
Output              MIDI: MPK mini IV          pad colours
Controller preset   MPK Mini IV Instrument
Unit FX             <Focused>
Auto-load           Based on unit FX           switch on in Step 5
```

### Unit 2 — DAW buttons

```text
Input               MIDI: MIDIIN2 (MPK mini IV)
Output              <None>
Controller preset   MPK Mini IV DAW Port
Main preset         MPK Mini IV DAW Actions
Auto-load           Off
```

Choose the MIDI devices directly. Do not use `<FX input>`: on the
Monitoring FX chain it receives nothing.

The order of the two units does not matter. Do not make either unit
superior.

Keep **one** unit on the `MPK mini IV` input, and do not make a unit per
instrument. Every unit on that input receives every knob turn and pad
press, drives the focused plugin and writes the pad colours, so a second
unit applies its own mappings to the same plugin. Auto-load in the single
instrument unit switches between instruments (Step 5).

---

## Step 5 — Link your instruments

Auto-load needs to know which Main preset belongs to which plugin. The links
belong to the instrument unit; ReaLearn calls them unit-wide links. Make
them in the instrument unit, not in the DAW unit.

For each instrument:

1. Open the plugin in its own floating window and click into it.
2. Open ReaLearn, select the instrument unit and choose **Menu › Unit-wide
   FX-to-preset links › <Add link from FX "…" to …>**. The quoted name is
   the plugin you focused last; check that it is the right one.
3. Pick the plugin's Main preset.

The same submenu lists the links you have made.

Then load the Main preset you want for plugins without a link, and switch
Auto-load to **Based on unit FX**.

Focusing a linked plugin loads its preset. Focusing an unlinked plugin, or
no plugin, brings back the Main preset that was loaded when you switched
Auto-load on. Choose that preset with care: it drives whatever unlinked
plugin you focus. With KK Instrument Controller there, the knobs write
parameters 101–164 of any plugin you click.

You can leave the default empty: load no Main preset before switching
Auto-load on. Unlinked plugins are then not controlled, and the pads go
dark until you focus a linked plugin again.

Three Main presets come with this repository. Use them as they are or as
starting points. Each has its own page with setup and details:

| Main preset | Plugin | Page |
|---|---|---|
| KK Instrument Controller | Native Instruments Komplete Kontrol | [Komplete Kontrol](instruments/komplete-kontrol.md) |
| SIC | Akai Studio Instrument Collection | [Studio Instrument Collection](instruments/sic.md) |
| Helix Native (MPK) | Line 6 Helix Native | [Helix Native](instruments/helix-native.md) |

For any other plugin, make a Main preset with ReaLearn's Learn function and
link it: see [Customising](customising.md#a-preset-for-a-new-plugin).

### Making a plugin "focused"

ReaLearn controls the **focused** plugin, the one you clicked last.
Open instruments in their own floating window and click into it. An FX
chain window does not count as focused until you select the plugin inside
it.

---

## Using it

### Knobs

```text
knob N on page P    ->   encoder/(8 x (P-1) + N)     64 slots
```

The page stays where it is when you focus another plugin.

### Pads

Switch the pad mode with the MPK's pad mode buttons, as usual. The colours
come from the instrument's Main preset. With the presets supplied:

```text
CC mode        Bank A pads 1-8   select knob pages 1-8
                                 amber = current page, red = others
               Bank B pads 1-8   red, free for your own mappings
Notes mode     all pads          green, play notes
Program mode   all pads          light blue, send program changes;
                                 the last program sent is marked
```

With Helix Native (MPK), Notes and Program mode are different: the pads
toggle Helix's switches and select its snapshots. See
[Helix Native](instruments/helix-native.md).

A pad dims while held, in CC and Notes mode. Page pads do not dim.

If you want all 16 CC pads for your own mappings, choose the Controller
preset **MPK Mini IV Instrument (plain pads)** instead. It has no page
selection, so the knobs stay on page 1.

### DAW buttons

```text
button     alone                      with Shift held
Play       Play / Stop                Pause / resume
Record     Record                     Toggle Quantize in the MIDI editor
Loop       Toggle Repeat              (reserved by the MPK, not mapped)
Undo       Undo                       Redo
+          Go to next marker          Go to previous marker
Tap        Tap tempo                  Toggle metronome
```

Shift on its own does nothing. Bank A/B, Oct- and Oct+ keep their normal MPK
functions.

To change what a button does, edit the Main preset **MPK Mini IV DAW
Actions**. Its mappings use names such as `daw/play-stop` and `daw/redo`, so
you never deal with MIDI numbers or Shift. See
[Customising](customising.md#daw-buttons).

---

## Known limits

- **After switching the MPK's preset, the pads show its stored colours.** The
  MPK tells REAPER nothing when you change presets or when the pad mode
  changes with them. Press the pad mode button of the mode you are in, once,
  and the colours come back.
- **At REAPER start-up** the pads are coloured for the mode ReaLearn was in
  when REAPER closed. If the MPK is now in another mode, press its pad mode
  button once.
- **A note held while you change pad mode stays stuck.** The MPK sends the
  release in the new mode. Release pads before changing mode.
- **Helix pads show their Switch slot, assigned or not.** ReaLearn cannot
  see whether the current Helix preset assigns a block to a slot, so a pad
  shows the slot's on/off state even when nothing is assigned to it.
- **Shift + Loop** is used by the MPK itself and is not mapped.
- **The MPK's display** is not used by this integration.
- The pad colours are a convenience. Everything works without them.

---

## Troubleshooting

The most common causes:

- **The knobs move the wrong plugin, or nothing:** the plugin is not
  focused. See *Making a plugin "focused"*.
- **The knobs work but the pads do nothing:** the MPK is on pad Bank B.
- **Two things happen from one knob:** a second unit listens to the
  `MPK mini IV` input.

These and more, with fixes, are in [Troubleshooting](troubleshooting.md).

---

## Customising

To change knob layouts, pad colours, pad functions or the DAW buttons, see
[Customising](customising.md).

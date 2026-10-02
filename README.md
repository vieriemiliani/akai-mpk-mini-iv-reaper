# AKAI MPK mini IV for REAPER

A REAPER integration for the AKAI MPK mini IV, built on
[ReaLearn](https://www.helgoboss.org/projects/realearn/). It provides 64 paged
knob slots on the focused plugin, pad colours that follow the pad mode, a Main
preset loaded per instrument, and the MPK's DAW buttons with a Shift layer.

It is not a copy of Akai's factory DAW preset. It is built around one user
preset you set up once on the MPK, and it needs nothing at runtime except
REAPER and ReaLearn.

**→ Start with the [User Guide](docs/user-guide.md).**

## Status

```text
Tested       Windows 11, REAPER, ReaLearn 2.18.2, MPK mini IV firmware 1.4.1.0
Working      paged knobs, focused-plugin control, per-instrument Auto-load,
             pad LED colours, DAW buttons with Shift
Instruments  Komplete Kontrol, Akai Studio Instrument Collection,
             Line 6 Helix Native
Not done     MPK display, macOS / Linux
```

## What is here

```text
docs/user-guide.md        installation and use
docs/instruments/         one page per supported plugin
docs/customising.md       knob layouts, pad colours, pad functions, DAW buttons
docs/troubleshooting.md   symptoms and fixes
realearn/controller/      Controller presets: the MPK's MIDI, translated into
                          named controls (encoder/1-64, pad/1-16, daw/play-stop ...)
realearn/main/            Main presets: what those controls do in REAPER
```

## How it is built

Two ReaLearn compartments keep the hardware apart from the behaviour. The
Controller presets hold every MIDI number, port and quirk of the MPK, and turn
them into named controls. The Main presets map those names to REAPER. Shift is
resolved in the Controller, so a Main preset sees `daw/redo`, never "Shift +
Undo". To change what something does, edit a Main preset; you never touch MIDI
numbers.

Nothing is sent to the MPK except pad colours; the integration never writes
to the MPK's presets.

## Credits

- [ReaLearn / Helgobox](https://github.com/helgoboss/helgobox) by Benjamin
  Klum (Helgoboss), which does all the runtime work.
- [reason_akai_mpk_mini_mk4](https://github.com/philoSurfer/reason_akai_mpk_mini_mk4)
  by PhiloSurfer, a Reason Remote integration for the MPK mini IV, was the
  starting point: its transport and Shift layout, port map and SysEx notes
  showed where to look.
- AKAI's own integrations for the MPK mini IV, its factory DAW preset and its
  Ableton Live Remote Script, served as references for the DAW buttons and
  for the pad colour palette and LED channels.

Facts taken from these sources were checked against the hardware before use.

AKAI and MPK are trademarks of inMusic Brands. Komplete Kontrol, Helix and
the other product names are trademarks of their owners. This project is not
affiliated with or endorsed by any of them.

## License

[MIT](LICENSE)

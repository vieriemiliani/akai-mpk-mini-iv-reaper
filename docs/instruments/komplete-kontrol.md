# Komplete Kontrol

Native Instruments' Komplete Kontrol plugin, with any instrument loaded in it.

```text
Main preset    KK Instrument Controller   (kk-instrument-controller.json)
Plugin         VST3i: Komplete Kontrol (Native Instruments)
```

## Setup

1. Put Komplete Kontrol on an instrument track whose input is the MPK, and
   arm the track so the keys and pads play it.
2. Open the plugin in its own floating window, click into it, and link it to
   **KK Instrument Controller** (user guide, Step 5).

The link matches the plugin's name as REAPER shows it, for example
`VST3i: Komplete Kontrol (Native Instruments) (32 out)`. A different variant
of the plugin, such as another output count, needs its own link.

## Knobs

The 64 knob slots drive parameters 101–164 of Komplete Kontrol, in order.
That is the block where Komplete Kontrol exposes the loaded instrument's
controls.

```text
page 1   knobs 1-8   ->   parameters 101-108
page 2   knobs 1-8   ->   parameters 109-116
...
page 8   knobs 1-8   ->   parameters 157-164
```

Which control lands on which knob depends on the instrument.

### Knob 2 moves the first control

On some instruments the block starts at 102, not 101. Parameter 101 is then
an empty slot, and every control sits one knob later than you expect.

To check, turn knob 1 and then knob 2 on page 1 and watch the instrument's
first control. If knob 2 moves it, the block is shifted for that instrument.

There is no setting that fits every instrument: Massive X starts at 101,
Massive at 102. For an instrument that starts at 102, make a copy of the
preset and add one to each mapping's parameter number (see
[Customising](../customising.md#knobs)).

## Pads

```text
CC mode        Bank A pads 1-8    select knob pages 1-8
               Bank B pads 1-8    red, no action
Notes mode     all pads           green, play the instrument
Program mode   all pads           light blue, send program changes
```

The pads play the instrument through the track, as usual. ReaLearn only
colours them.

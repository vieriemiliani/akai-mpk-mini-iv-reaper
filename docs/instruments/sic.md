# Akai Studio Instrument Collection

Akai's Studio Instrument Collection plugin.

```text
Main preset    SIC   (sic.json)
Plugin         VST3i: Studio Instrument Collection (Akai Professional)
```

## Setup

1. Put Studio Instrument Collection on an instrument track whose input is the
   MPK, and arm the track so the keys and pads play it.
2. Open the plugin in its own floating window, click into it, and link it to
   **SIC** (user guide, Step 5).

The link matches the plugin's name as REAPER shows it. A different variant
of the plugin needs its own link.

## Knobs

The 64 knob slots drive parameters 0–63 of the plugin, in order.

```text
page 1   knobs 1-8   ->   parameters 0-7
page 2   knobs 1-8   ->   parameters 8-15
...
page 8   knobs 1-8   ->   parameters 56-63
```

## Pads

```text
CC mode        Bank A pads 1-8    select knob pages 1-8
               Bank B pads 1-8    red, no action
Notes mode     all pads           green, play the instrument
Program mode   all pads           light blue, send program changes
```

The pads play the instrument through the track, as usual. ReaLearn only
colours them.

# Helix Native

Line 6 Helix Native, the guitar and bass processor plugin.

```text
Main preset    Helix Native (MPK)   (helix-native-mpk.json)
Plugin         VST3: Helix Native (Line 6)
```

Helix Native is an effect, not an instrument, but Auto-load treats it like
any other plugin.

## Setup

1. Put Helix Native on the audio track you play through.
2. Open it in its own floating window, click into it, and link it to
   **Helix Native (MPK)** (user guide, Step 5).

The link matches the plugin's name as REAPER shows it. A different variant
of the plugin needs its own link.

## How it works

Helix Native shows REAPER the same fixed set of parameters whatever Helix
preset is loaded: Knob 01–16, Switch 01–16, the master levels and the
snapshot. Each Helix preset decides what its Knob and Switch slots do. So one
Main preset fits every Helix preset, and you assign the slots inside Helix.

```text
knobs      page 1   knobs 1-8   ->   Knob 01-08
           page 2   knobs 1-8   ->   Knob 09-16
           page 3   knob 1      ->   Master Input Level
                    knob 2      ->   Master Output Level
pads       Notes mode, pads 1-16       ->   Switch 01-16, toggle on press
           Program mode, pads 1-8      ->   snapshots 1-8
           CC mode, Bank A pads 1-8    ->   select knob pages, as usual
```

Pages 4–8 and Program mode pads 9–16 do nothing.

In Notes mode a pad is **green when its switch is on** and **dark when it is
off**. Bank A holds Switch 01–08, Bank B Switch 09–16.

In Program mode the last snapshot you picked from the pads is marked.

## Assign blocks and parameters in Helix

The Knob and Switch slots do nothing until the Helix preset assigns them.
Assign a block's Bypass to a Switch to turn the block on and off from a pad,
and any parameter to a Knob to turn it from a knob.

**From the Automation/Controller Assign tab**, which also reaches Bypass:

1. Open the **Automation/Controller Assign** tab.
2. Select the block.
3. In the parameter dropdown next to the block's name, choose the
   parameter, for example **Bypass**.
4. In **Automation**, choose a Knob or a Switch.

**From the Edit view**, for a block's parameters: right-click the
parameter, choose **Automation Assign**, then a Knob or a Switch. The list
shows what each Knob and Switch is already assigned to, and an assigned
parameter shows its knob number next to its name.

Use **Automation Assign**, not **Controller Assign**. Controller Assign and
its MIDI Learn are for MIDI sent to Helix, and ReaLearn sends Helix no MIDI:
it moves the Knob and Switch slots directly.

Assignments belong to the Helix preset, so each Helix preset needs its own.
Save the Helix preset when you are done if you want to keep them.

A useful habit: assign the switches in signal order, left to right, so pad 1
is always the first block.

## Things to know

- **A pad shows its Switch slot, assigned or not.** ReaLearn cannot see
  whether the current Helix preset assigns anything to a slot, so the pad of
  an unassigned slot simply shows that slot's state, normally dark.

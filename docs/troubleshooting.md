# Troubleshooting

Find your symptom, try the fix. If it is still broken, see
[Seeing what ReaLearn receives](#seeing-what-realearn-receives) at the end.

---

## The knobs move the wrong plugin, or nothing

ReaLearn controls the **focused** plugin: the one you clicked last.

```text
plugin in its own floating window, clicked     focused
FX chain window open, plugin selected in it    focused
FX chain window open, nothing selected         not focused
```

Open the plugin in its own floating window and click into it. Then check
that the instrument unit shows that plugin's Main preset. If it shows another
preset, or `<None>`, the plugin has no link: see Step 5 of the
[user guide](user-guide.md#step-5--link-your-instruments).

ReaLearn can report success while it controls a different plugin from the one
you are looking at, so check the focus before anything else.

## The knobs work but the pads do nothing

Check the pad bank. In Bank B the pads act as pads 9–16, which may have
nothing mapped for this plugin. For example, with Helix Native they toggle
Switch 09–16, which a Helix preset often leaves unassigned. Press **Bank A/B**
to go back to Bank A.

Also check the pad mode. Each mode does something different, and a Main
preset may use only some of them.

## The pads show the wrong colours

The MPK does not tell REAPER when you switch its preset, or when the pad
mode changes with it. Press the pad mode button of the mode you are in, once,
and the colours come back. The same applies after starting REAPER if the MPK
is in a different pad mode from last time.

## The pads are dark

No linked plugin has focus, and the instrument unit's default Main preset is
empty. Focus a linked plugin. This is expected with an empty default; see
Step 5 of the [user guide](user-guide.md#step-5--link-your-instruments).

## Two things happen from one knob

Two units listen to the `MPK mini IV` input, and both drive the focused
plugin. Keep one instrument unit and switch between instruments with
Auto-load.

## A plugin I never linked reacts to the knobs

The instrument unit's default Main preset drives every plugin without a
link. It is the Main preset that was loaded when you switched Auto-load on.
To change it, switch Auto-load off, load the preset you want (or none), and
switch Auto-load on again.

## A unit has gone completely silent

Check that no unit is superior: **Menu › Unit options › Make unit superior**
must be unticked in every unit. A superior unit switches off the other units
on the same device.

## With Komplete Kontrol, knob 2 moves the first control

On some instruments the block of controls starts one parameter later.
See [Komplete Kontrol](instruments/komplete-kontrol.md#knob-2-moves-the-first-control).

## A Helix block does not react

The block's parameter must be assigned to a Knob or Switch with Helix's
**Automation Assign**; Controller Assign and MIDI Learn do not work with
ReaLearn. Also check the pad bank: Bank B pads are Switch 09–16. See
[Helix Native](instruments/helix-native.md#assign-blocks-and-parameters-in-helix).

## A pad note keeps sounding

You changed the pad mode while holding a pad. The MPK sends the release in
the new mode, so the note never ends. To stop it, switch back to Notes mode and
press the same pad once. To avoid it, release pads before changing mode.

## Nothing responds at all, and ReaLearn's log stays empty

Close REAPER, unplug the MPK's USB cable, plug it back into the same port,
wait a few seconds, and start REAPER again. This happened once after a
Windows update.

To tell where the signal stops:

```text
1  Press Play on the MPK                    DAW buttons, the DAW unit
2  Arm a track with input MPK mini IV       the keys and pads, outside ReaLearn
3  Options › Settings › MIDI Devices        the MPK rows: enabled, not
                                            "(not present)", no new names
```

If 1 and 2 both do nothing, REAPER receives nothing from the MPK and the
problem is on the Windows side. If 2 works, check the instrument unit's
Input.

---

## Seeing what ReaLearn receives

In a unit, choose **Menu › Logging › Log real control messages**. Every
message the unit receives appears in REAPER's console. Press a pad or turn a
knob and look at the line it prints:

```text
pad 1, Notes mode, Bank A     note 36 on channel 10
pad 1, Notes mode, Bank B     note 44 on channel 10
knob 1                        CC 20 on channel 1
```

If nothing is printed, the unit is not receiving that device. Switch logging
off again afterwards.

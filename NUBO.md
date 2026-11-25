# NUBO.MOD — what it changes

NUBO.MOD is a modified version of [MONO2K](https://github.com/InigoHernandez/mono2k) by Inigo Hernandez, for the
M-VAVE FM-1, on MONO2K v1.13 (GPL-3.0, as the original). Everything below is what differs from MONO2K; the rest is
MONO2K as its author made it.

## Battery indicator

- **MONO2K shows no battery charge on the FM-1; NUBO.MOD does.** Open GLO: the header shows `B99`, `B60` or `B30` by
  the tempo, from the battery's voltage (the ADC's battery channel), smoothed so it does not jump.

## Amp envelope

- **RELEASE OFF — a drone.** AMP 1, REL turned to zero reads `OFF`: the note does not let go after its end; the
  envelope's picture has no release segment.
- **STOP silences drones too.** A new note drones again.
- **HOLD INF with RELEASE OFF:** the note's end goes on to DECAY; only DEC INF holds it forever.
- **ADSR (SUS above 0) with RELEASE OFF:** after the note's end the SUS level holds; STOP silences it.

## Saving

- **No autosave.** Nothing goes into the flash but an explicit SAVE while stopped: a tap saves the pattern, a hold
  saves everything.
- **While playing,** SAVE and saving a sound are refused: `STOP TO SAVE`.
- **GLO settings** are saved with SAVE only.
- **No "project" on the FM-1** (it keeps one): a held SAVE reads `ALL DATA SAVED`; out of room, `PATTERNS FULL`.

## Patterns

- **No pattern names** in the header, on HOME or in messages (the field stays in the save file: older saves load).
- **HOME held:** the patterns with a trig in them are lit dim, in every bank.
- **A saved sound** is named after its slot and track: `B03T2`.

## Parameter locks

- **Locks on steps without a note** (Elektron's trigless locks). REC + PLAY: a knob turned writes a lock on the
  current step, a note on it or not; the track's sound stays as it was.
  - Played back, a step without a note applies its locks to the note that goes on: no retrigger, no pitch change;
    the value holds until the next note.
  - While recording, the knob and the page's picture show the value heard.
  - A track following a Grids lane takes no locks: the knob changes its sound.
- **GRID:** a step with locks alone is underlined; its key is lit dim.
- **A step held in GRID:**
  - holding a step with locks alone places no note: look at and edit its locks; a quick tap places a note, its
    locks kept;
  - the knob shows the lock's value, the page's picture the locks' values;
  - the page buttons of the sections its locks are in blink: EDIT (SYN), ENV (AMP), SEL (FILT), FX, LFO, ARP,
    HOME (LEVL / PTCH).
- **Clear a track's locks:** SEQ + white 9. Its steps stay; `T<n> P LOCKS CLEARED`;
  GLO + OCT− undoes it.

## Control All

- **GRID, hold OCT+ and turn KNOB 1–4.**
- The parameter moves on every track by the same clicks, each from its own value, each stopping at its own ends.
  The screen reads `CTRL ALL` and the parameter's name.
- **Where:** HOME (LEVL, PTCH), SYN (each machine's own knob in that place; a machine without one is left alone),
  AMP, FILT, FX, LFO. Not ARP, BPM, SWNG or the pattern's other settings: there the knob is the track's own.
- **REC + PLAY:** a lock on each track's current step.
- **POLY:** the chord's voices are passed over; they follow their lead.
- **Undo:** one GLO + OCT− undoes one hold.
- **OCT+ in GRID** turns the page on its release, when no knob was turned.

## SEQ mode (GRID)

- **The hint on entering:** `GRID: OCT-/+ PAGES` / `HOLD DIM LIT P.LEN`.
- **Pattern length:** black keys 7–10 (the pages) are lit dim; held 0.6 s, the length is 16 / 32 / 48 / 64 steps,
  `PATTERN <n> STEPS`. A longer length copies the pattern's start over the steps past the old length.
- **F#5 (the rightmost black key) mutes nothing in GRID.** Mutes: GLO + white 1–6; in PADS F#5 mutes as before.
- **GLO > HELP, STEPS:** `HOLD 7-10: PAT LENGTH`.

## Panic

- **SEQ + white 10.**
  - The first press stops everything that feeds the reverb: the sequencer (and drones), the arp's hold, the
    voices, the delay's echoes, PERFORM's latched effects. Only the reverb's tail rings: `INPUTS CUT` /
    `AGAIN: CUT THE REVERB`.
  - The second press: silence, `ALL CUT`.
- **SEQ held:** `9 CL.LOCKS` and `10 PANIC` on the screen, white 1–10 lit.

## Screen and keys

- **Splash:** no animation, the finished frame at once for 2.1 s, dark in both themes; NUBO.MOD in a pixel face
  under MONO2K; the version (`--release` below).
- **Dim key LEDs (new in the FM-1 driver):** two levels, 10 % (steps with locks alone) and 20 % (the length keys,
  patterns with trigs on HOME): a LED lit one or two frames of the scan in ten.

## Build and flash

```sh
python tools/fm1/build.py --release 1.13-6n
```

The package is `build/fm1/mono2k-1.13-6n.fwsc` (MONO2K's build, its toolchain as the main README says). The
`--release X.Y-Nn` suffix shows on the splash and the update screen. Flash it with [M-Flasher](https://github.com/jvitkauskas/m-flasher), which also backs up the whole flash first.
`web/firmware/mono2k.fwsc` in this repository
is MONO2K's own package, not NUBO.MOD.

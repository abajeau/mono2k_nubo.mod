# NUBO.MOD — what it changes

NUBO.MOD is a modified version of [MONO2K](https://github.com/InigoHernandez/mono2k) by Inigo Hernandez, for the
M-VAVE FM-1, on MONO2K v1.13 (GPL-3.0, as the original). Everything below is what differs from MONO2K; the rest is
MONO2K as its author made it.

## Battery indicator

- **MONO2K shows no battery charge on the FM-1; NUBO.MOD does.** Open GLO: the header shows `B99`, `B60` or `B30` by
  the tempo, from the battery's voltage (the ADC's battery channel), smoothed so it does not jump.

## Amp envelope

- **RELEASE OFF — a drone.** On AMP 1, turn REL all the way down: it reads `OFF`, and a note no longer fades out
  after its end (the envelope's picture shows no release).
- **To end a drone:** the panic (SEQ + white 10), or turn REL back up; stopping the sequencer silences it too. The
  next note drones again.
- **With HOLD INF:** the note holds while the key is down (or the step's length lasts), then falls by DECAY; only
  DEC INF holds it forever.
- **With a sustain (SUS above 0):** after the note's end the SUS level holds.

## Saving

- **Save only when you mean it.** MONO2K wrote the flash on its own; NUBO.MOD writes nothing until you press SAVE.
  So you can take a pattern apart live, copy it to a new slot or rework its steps mid-set, and the saved version
  stays as it was: GLO + SAVE brings it back.
- **SAVE:** a tap keeps the open pattern, a hold keeps everything (patterns, sounds, GLO settings). It works while
  stopped (`STOP TO SAVE` while playing), so writing the flash never interrupts the sound.
- **The messages:** a held SAVE reads `ALL DATA SAVED`, out of room `PATTERNS FULL`. MONO2K said "project 1", but
  the FM-1 keeps banks and patterns, no projects.

## Patterns

- **Patterns at a glance.** In MONO2K a pattern's key lit on HOME only if it had a name, and there is no way to name
  one on the device or in the web editor, so your own patterns never lit. NUBO.MOD drops the names: hold HOME and
  the patterns with steps in them are lit dim (in every bank), the open one fully.
- **A sound saved to the library** is named after its pattern slot and track, e.g. `B03T2`. (The names stay in the
  save file, so older saves load.)

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

## GRID reworked

- **Page keys, lit dim.** Black 7–10 are the pattern's four pages of 16 steps: tap one to see it, hold it 0.6 s to make
  the pattern that long (16 / 32 / 48 / 64 steps, `PATTERN <n> STEPS`). A longer length copies the pattern's start
  into the new steps.
- **Leftover shortcuts removed.** F#5 (the rightmost black key) no longer mutes the track in GRID; mute with GLO +
  white 1–6 (in PADS, F#5 still mutes).
- **Hints that match.** Entering GRID: `GRID: OCT-/+ PAGES` / `HOLD DIM LIT P.LEN`; GLO > HELP: `HOLD 7-10: PAT
  LENGTH`.

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
- **Dimmed key lights.** The FM-1's key LEDs could only be on or off, so a key could show one state. The FM-1's
  LED driver now dims them too: 10 % for a step with locks only, 20 % for the page keys in GRID and the filled
  patterns on HOME (a LED lit in one or two of every ten scans of the keyboard, too fast to flicker).

## Build and flash

```sh
python tools/fm1/build.py --release 1.13-6n
```

The package is `build/fm1/mono2k-1.13-6n.fwsc` (MONO2K's build, its toolchain as the main README says). The
`--release X.Y-Nn` suffix shows on the splash and the update screen. Flash it with [M-Flasher](https://github.com/jvitkauskas/m-flasher), which also backs up the whole flash first.
`web/firmware/mono2k.fwsc` in this repository
is MONO2K's own package, not NUBO.MOD.

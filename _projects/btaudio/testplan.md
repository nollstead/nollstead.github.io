---
title: "BTAudio Test Plan"
layout: default
permalink: /btaudio/testplan/
breadcrumb:
  - { title: "Home", url: "/" }
  - { title: "Projects", url: "/projects/" }
  - { title: "Bluetooth Audio", url: "/btaudio/" }
  - { title: "Testbed" }
---

# BTAudio Test plan

## Setup

1. Connect to the BTAudio device via Bluetooth from a phone or tablet
2. Open the web interface at **http://192.168.4.1** (connect to the "BTAudio" WiFi network first)
3. Play a well-produced track with a full frequency range — something with clear bass, vocals, and cymbals/hi-hats (pop, rock, or R&B work well)
4. Set a comfortable listening volume (level 25-30 is a good starting point)
5. **Reset all settings to defaults before starting** — this ensures a known baseline:
   - Volume: 20, Input Gain: 0, Loudness: On
   - Bass/Middle/Treble gain: 0 dB
   - All Q values and center frequencies at their defaults

Keep music playing continuously throughout all tests. The goal is to hear the change happen in real time as you toggle settings.

---

## Test 1: Bass Gain — Sanity Check

**What it does:** Bass Gain boosts or cuts low frequencies (below ~200 Hz). This is a straightforward level control — more gain means more bass energy, less gain means thinner sound.

**This is the most important test.** If you can't hear a difference here, something is fundamentally wrong and there's no point continuing to the other tests.

**Starting point:** All settings at defaults (bass gain = 0 dB).

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set bass gain = **+8 dB** | Bass should increase noticeably — kick drums hit harder, bass guitar is more prominent, overall sound is warmer/heavier |
| 2 | Set bass gain = **-8 dB** | Bass should drop significantly — sound becomes thin and tinny, kick drums lose their punch, bass guitar fades into the background |
| 3 | Set bass gain = **0 dB** | Return to flat — should sound like the original track again |

**Pass criteria:** Clear, obvious difference between +8 and -8 dB. This is a 16 dB swing — it should be impossible to miss.

**If this test fails:** Stop here. The BD37033 may not be receiving data or audio may be bypassing it. Check serial logs for I2C errors.

---

## Test 2: Bass Q — Width of the Bass Boost

**What it does:** Q (quality factor) controls how wide or narrow the bass boost/cut is. A low Q spreads the effect across a broad range of frequencies. A high Q concentrates the effect into a tight band around the center frequency, creating a more "resonant" or "peaky" sound.

Think of it like a flashlight: low Q = flood light (wide coverage), high Q = spotlight (focused beam).

**Starting point:** Set bass gain = **+8 dB**, bass f0 = **80 Hz**. Leave all other settings at defaults.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set Q = **0.5** | Wide, warm boost — the bass increase is spread broadly, affecting everything from sub-bass up through the low mids. Sound is "fat" and "smooth" |
| 2 | Set Q = **2.0** | Narrow, focused boost — the bass increase concentrates around 80 Hz specifically. Sound becomes more "boomy" or "resonant" with a peak. Upper bass and low mids are less affected |
| 3 | Toggle back and forth between 0.5 and 2.0 | The tonal character of the bass should clearly change even though the amount of boost is the same |

**Pass criteria:** Audible change in the character (not just the amount) of the bass boost.

---

## Test 3: Bass Center Frequency (f0)

**What it does:** The center frequency determines where in the bass range the boost/cut is applied. Lower values affect deeper bass, higher values affect upper bass. Combined with Q, this lets you target specific parts of the bass spectrum.

- **60 Hz** — Deep sub-bass. This is the rumble you feel in your chest. On smaller speakers it may be barely audible but you might feel it.
- **80 Hz** — Low bass. Kick drum fundamental, bass guitar body.
- **100 Hz** — Upper bass. More audible punch, easier to hear on all speaker sizes.
- **120 Hz** — Upper bass / low midrange. Kick drum attack, bass guitar note definition.

**Starting point:** Set bass gain = **+8 dB**, bass Q = **2.0** (narrow Q makes the frequency shift most obvious). Leave all other settings at defaults.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set f0 = **60 Hz** | Deep, subby rumble — may be hard to hear on small speakers but audible on anything with decent low-end |
| 2 | Set f0 = **120 Hz** | Boost moves up into punch/attack territory — kick drums sound punchier, bass is more "heard" than "felt" |
| 3 | Toggle back and forth | The character of the bass should clearly shift between deep rumble and upper punch |

**Pass criteria:** Audible shift in which part of the bass spectrum is boosted.

---

## Test 4: Treble Center Frequency (f0)

**What it does:** Same concept as bass f0 but for the high frequency range. It sets where in the treble spectrum the boost/cut is centered.

- **7.5 KHz** — Presence range. Vocal consonants (S, T, F sounds), guitar string attack, snare drum snap. Boosting here makes the mix sound more "forward" and detailed but can also make it harsh or sibilant.
- **10 KHz** — Brilliance. Cymbal shimmer, acoustic guitar sparkle.
- **12.5 KHz** — Upper air. Subtle sparkle and openness.
- **15 KHz** — Extreme top end. "Air" and "space" — a sense of openness without adding harshness. Less sibilance than lower settings.

**Starting point:** Set treble gain = **+8 dB**. Leave all other settings at defaults.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set f0 = **7.5 KHz** | Vocals sound crisper, S-sounds (sibilance) become more prominent, cymbals have more "splash" |
| 2 | Set f0 = **15 KHz** | The brightness shifts to the very top — more "air" and less "bite". Sibilance reduces, but cymbals still shimmer |
| 3 | Toggle back and forth | The texture of the high end should change — 7.5 KHz is more aggressive, 15 KHz is more airy |

**Pass criteria:** Audible shift in where the treble boost is focused.

---

## Test 5: Loudness Center Frequency (f0)

**What it does:** Loudness compensation is a feature based on the Fletcher-Munson equal-loudness curves. Human hearing is less sensitive to bass and treble at low volumes — music sounds "thin" when played quietly. The loudness circuit boosts bass and treble at low volumes to compensate, making quiet music sound more natural and full.

The loudness f0 setting controls the center frequency of this bass compensation curve:
- **400 Hz** — Compensates deeper into the bass range, producing a warmer, boomier low end at quiet levels.
- **800 Hz** — Middle ground (default).
- **2400 Hz** — Shifts compensation higher, into the lower midrange. Less deep bass boost, more mid-bass presence.

**Important:** Loudness compensation has the most effect at **low volumes**. At high volumes the compensation is minimal and changes to f0 will be nearly inaudible.

**Starting point:** Set volume = **level 10-15**, loudness = **ON**. Leave all other settings at defaults.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set f0 = **400 Hz** | At this low volume, bass should sound warmer and fuller — the compensation reaches down into the deep bass |
| 2 | Set f0 = **2400 Hz** | Bass thins out somewhat, but the lower midrange (male vocals, guitar body) becomes more present |
| 3 | Toggle between 400 Hz and 2400 Hz | The tonal balance at low volume should shift — 400 Hz is warmer, 2400 Hz is leaner |

**Pass criteria:** Audible tonal shift at low volume levels. If testing at higher volumes (35+) the difference will be very subtle or inaudible — this is expected.

---

## Test 6: Loudness High-Cut

**What it does:** In addition to boosting bass, the loudness circuit also boosts treble at low volumes. The high-cut setting controls the upper corner frequency of this treble compensation — essentially how far up into the high frequencies the loudness boost extends.

- **Setting 1** — Least treble compensation (the boost rolls off earlier).
- **Setting 4** — Most treble compensation (the boost extends further into the highs).

Higher settings mean more high-frequency energy gets added at low volumes.

**Starting point:** Set volume = **level 10-15**, loudness = **ON**. Leave all other settings at defaults.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set high-cut = **1** | Baseline — quieter high end at low volume |
| 2 | Set high-cut = **4** | Highs should brighten up slightly — cymbals and vocal sibilance become a bit more present |
| 3 | Toggle back and forth | Listen for subtle changes in treble brightness |

**Pass criteria:** This is the subtlest test in the plan. The difference may be slight, especially on speakers without strong high-frequency response. A small change in treble brightness is a pass.

---

## Test 7: Subwoofer Cutoff Frequency

**What it does:** The BD37033 has a built-in low-pass filter (LPF) for the subwoofer outputs (Sub 1 and Sub 2). The cutoff frequency determines which frequencies are sent to the sub channels. Frequencies below the cutoff pass through; frequencies above it are rolled off.

- **55 Hz** — Very low cutoff. Only the deepest bass reaches the sub. Tight, clean sub output but may sound thin if your sub can't reproduce frequencies that low.
- **85 Hz** — Common crossover point. Good balance between sub and mains for most speaker setups.
- **120 Hz** — Higher cutover (default). More bass content sent to the sub, including upper bass/kick drum punch. Good for smaller subs or if you want more bass weight from the sub channel.
- **160 Hz** — Highest cutoff. Sub handles a wide range of bass, approaching lower midrange. Can sound muddy if the sub and mains overlap too much.
- **Pass** — LPF bypassed. Full-range signal passes through to the sub outputs unfiltered.

**Starting point:** All settings at defaults (sub cutoff = 120 Hz). Set volume to a comfortable level (25-30).

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set cutoff = **55 Hz** | Sub output should lose most of its mid-bass content — only the deepest rumble comes through. If listening to the sub channel directly, it should sound noticeably thinner |
| 2 | Set cutoff = **160 Hz** | Sub output now includes much more bass content — kick drums, bass guitar body, and even some lower mids come through. Sub sounds fuller and heavier |
| 3 | Set cutoff = **Pass** | Sub gets the full-range signal with no filtering — vocals, guitars, everything comes through the sub outputs. This will sound wrong/muddy but confirms the filter was working |
| 4 | Set cutoff = **120 Hz** | Return to default — sub should sound clean and focused on bass again |

**Pass criteria:** Audible difference in what frequency content reaches the sub outputs, especially between 55 Hz and 160 Hz, and a clear change when switching to Pass (full-range).

**Tip:** If all speakers are in the same room and playing together, the cutoff changes may be subtle since the mains still reproduce bass. For the clearest test, listen near the sub speaker specifically, or temporarily mute the mains if possible.

---

## Test 8: Subwoofer Phase

**What it does:** Phase controls the timing alignment of the sub output relative to the main channels. At **0 degrees** (normal), the sub's waveform moves in the same direction as the mains. At **180 degrees** (inverted), the sub's waveform is flipped — when the mains push out, the sub pulls in, and vice versa.

Why this matters: If the sub is physically far from the main speakers, sound waves from each can arrive at the listening position partially out of phase, causing bass cancellation (thin, weak bass). Flipping the sub's phase by 180 degrees can correct this, resulting in stronger bass at the listening position.

- **0 Degrees** — Normal phase. Sub and mains are in sync.
- **180 Degrees** — Inverted phase. Sub output is flipped.

**Starting point:** All settings at defaults (phase = 0 degrees). Set volume to a comfortable level with some bass-heavy content playing.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Set phase = **0 Degrees** | Baseline — note the bass level and fullness |
| 2 | Set phase = **180 Degrees** | Bass may get noticeably weaker or change character if the sub and mains are reinforcing each other at 0 degrees. Or bass may get stronger if they were partially canceling at 0 degrees |
| 3 | Toggle back and forth | One setting should sound fuller/punchier than the other. The "better" setting depends on your physical speaker placement |

**Pass criteria:** An audible change in bass level or character when toggling. The difference is most noticeable at the crossover frequency region where both sub and mains are producing the same frequencies. If your sub is very close to the mains, the difference may be small.

**Note:** There is no universally "correct" setting — it depends on speaker placement. In a real installation, you'd pick whichever sounds fuller at the listening position.

---

## Test 9: Subwoofer Input Source

**What it does:** This selects where the sub channel gets its audio signal from within the BD37033's internal signal chain.

- **Variable** (default) — The sub receives its signal from after the loudness processing stage. The sub level tracks with the volume knob and benefits from loudness compensation at low volumes (bass boost).
- **Fixed** — The sub receives its signal from the input selector, before volume and loudness processing. The sub gets a constant-level "raw" signal regardless of volume setting.

**Starting point:** All settings at defaults (input = Variable). Set bass gain = **+8 dB** and volume to a comfortable level.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Confirm input = **Variable** | Sub should sound bass-boosted since the +8 dB bass EQ and loudness compensation are applied before the sub receives the signal |
| 2 | Set input = **Fixed** | Sub level no longer tracks with the volume knob — it stays at a constant level. The sub output should sound flatter/cleaner, while the main speakers still have the +8 dB bass boost |
| 3 | Toggle back and forth | The sub's tonal character should change — Variable has more bass weight, Fixed is flatter |

**Pass criteria:** The sub's response to volume and EQ changes should differ between the two settings. With "Fixed" input, changing volume should NOT affect the sub output level. With "Variable" input, it should.

---

## Test 10: Subwoofer Output Mode

**What it does:** This controls what signal is routed to the sub fader outputs (Sub 1 and Sub 2). It determines whether those physical outputs carry a filtered sub signal or a copy of another channel.

- **LPF** (default) — The sub outputs carry the low-pass filtered signal. This is the normal subwoofer mode — only bass below the cutoff frequency reaches the sub outputs.
- **Front** — The sub outputs carry a copy of the front channel signal (full range, no LPF). Useful if you want to use the sub outputs as a second zone or extra front speakers.
- **Rear** — The sub outputs carry a copy of the rear channel signal (full range, no LPF).
- **Sub** — Dedicated sub mode. Similar to LPF but repurposes the mixing volume register for sub 2-channel control.

**Starting point:** All settings at defaults (output = LPF, cutoff = 120 Hz). Play music at a comfortable volume.

| Step | Change | What to listen for |
|------|--------|--------------------|
| 1 | Confirm output = **LPF** | Sub outputs should only reproduce bass — no vocals, no cymbals, just low frequencies |
| 2 | Set output = **Front** | Sub outputs now play the full front channel signal — you should hear vocals, guitars, cymbals, everything through the sub speakers. This will sound wrong/muddy but proves the routing changed |
| 3 | Set output = **Rear** | Same as above but sourced from the rear channels instead of front |
| 4 | Set output = **LPF** | Return to default — sub goes back to bass-only. Clean separation restored |

**Pass criteria:** Clear difference between LPF (bass only) and Front/Rear (full range). When set to Front or Rear, you should hear full-range audio from the sub speakers including vocals and high frequencies.

---

## Troubleshooting

If **none** of the tests produce audible changes (including Test 1 — bass gain):

- **Check the serial log** for I2C errors on BD37033 register writes. Every successful write logs `ESP_OK`.
- **Verify the input selector** is set to the correct input (A1/A2, which receives audio from the ES8388).
- **Confirm the signal path** — audio should flow through the BD37033, not bypass it. If you hear audio at all, it's likely going through the BD37033 (since it controls mute and volume), but it's worth confirming.
- **Note:** The BD37033 is write-only — there is no way to read registers back to verify their contents. We rely on the I2C write return codes and our software shadow registers to track state.

## Results

| Test | Parameter | Pass/Fail | Notes |
|------|-----------|-----------|-------|
| 1 | Bass Gain | | |
| 2 | Bass Q | | |
| 3 | Bass f0 | | |
| 4 | Treble f0 | | |
| 5 | Loudness f0 | | |
| 6 | Loudness High-Cut | | |
| 7 | Sub Cutoff Frequency | | |
| 8 | Sub Phase | | |
| 9 | Sub Input Source | | |
| 10 | Sub Output Mode | | |

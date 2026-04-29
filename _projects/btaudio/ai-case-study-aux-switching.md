---
layout: default
title: "AI Case Study: AUX Input Switching"
description: "How an AI coding assistant was used to reason through edge cases, refine requirements, and implement firmware fixes for AUX/Bluetooth input switching"
tags: [AI, Claude, ESP32, Firmware, Case Study]
weight: 90
permalink: /btaudio/ai-case-study/   
breadcrumb:
breadcrumb:
  - { title: "Home", url: "/" }
  - { title: "Projects", url: "/projects/" }
  - { title: "Bluetooth Audio", url: "/btaudio/" }
  - { title: "AI Case Study: AUX Switching" }
---

# AI Case Study: AUX Input Switching

This page documents a real development session using Claude Code (Anthropic's AI coding assistant) to identify, reason through, and fix edge cases in the BTAudio firmware related to AUX/Bluetooth input switching. It is intended as an example of effective AI-assisted development — specifically how clear problem framing, structured Q&A, and plan review lead to better outcomes than simply asking the AI to "fix the bug."

---

## Background

BTAudio supports two audio input sources:

- **Bluetooth (A2DP)** — digital audio from a paired phone
- **AUX** — analog line input from an auxiliary device

Output routing is controlled separately by headphone jack detection:

- **Headphones connected** — audio goes directly to headphones; the BD37033 speaker processor and amplifier are bypassed
- **Headphones disconnected** — audio routes through the BD37033 to the speakers via the REM-controlled amplifier

The headphone jack detection feature was recently added. Once it was in place, it became clear that some edge cases involving the combination of input source and output routing had not been fully thought through.

---

## The Conversation

---

### Initial Request

> **User:**
>
> Now I'd like to focus on the functionality of the input setting when toggled between Bluetooth and aux. In testing I noticed some strange behavior that I haven't quite been able to document yet, so I think we have some edge cases to verify. Rather than list each one it might be better to list the expected behavior and verify that that's what the code is doing. Below are the requirements.
>
> Here are requirements when the headphone jack is disconnected, so the output already is routed through the BD37033 to the speakers:
>
> - If not connected to Bluetooth the BD37033 is already muted and REM is disabled — I'm not sure if we can get away with leaving it muted since we may not have a signal to tell us when the music starts, like we do with A2DP, so we might need to set it to be unmuted — but we need to make sure REM is enabled (which would normally only occur when Bluetooth is connected).
> - If Bluetooth is connected but not streaming then the BD37033 is muted and REM is enabled. This case is similar to the previous, although REM is already enabled.
> - If we're currently streaming audio over Bluetooth then the BD37033 is unmuted and REM is enabled. So this should just be changing the input, which we likely already do.
>
> If the headphone jack is connected this might be a simpler case where we need to mute the BD37033 and disable REM. We just need to make sure we handle the case where I then unplug the headphones and make sure that mute/unmute and REM are correct.
>
> So, in general we need to look at all edge cases of BD37033 mute/unmute, REM activation (which powers the speaker amplifier) and audio routing — including before/after headphone jack insertion, before/after Bluetooth connectivity, etc.
>
> **Before any deep planning, take a look at these requirements and let me know if there are edge cases you're not sure about. Once I answer those we can take a look at a plan.**

---

### AI Code Exploration

At this point Claude Code explored the codebase to understand the current implementation before asking questions — reading `board.c`, `bt_audio.c`, `device_info.c`, and the headphone detection logic — then surfaced the following questions:

---

### AI Edge Case Questions

> **Claude:**
>
> The overall model is clear and makes sense. HP state takes priority — it gates everything else. One thing to flag:
>
> **Edge case 1: Switch to AUX while headphones are already inserted**
> The current `audio_board_set_input_source(AUX)` always unmutes the BD37033 and enables REM, with no check for HP state. So if you switch to AUX while HP is in, the amp would turn on and audio would bleed to speakers. Is the right behavior to suppress unmute/REM when HP is connected, or to ignore input switches entirely while HP is in?
>
> **Edge case 2: HP removed while AUX is selected**
> The `on_hp_event` callback only re-enables REM based on BT connection state. If you're in AUX mode with no BT, removing headphones won't restore REM. Should REM restore be conditioned on "BT connected OR AUX selected"?
>
> **Edge case 3: BT disconnects while in AUX mode**
> The A2DP disconnect handler currently always mutes the BD37033 and disables REM. If the user is using AUX, a BT disconnect would wrongly kill the speaker amp and mute audio. Should BT disconnect events be a no-op for mute/REM when AUX is the selected input?
>
> **Edge case 4: BT connects while in AUX mode**
> Currently BT connect enables REM (if no HP). If AUX is selected, we presumably don't want BT connection to change anything. Should BT connect events be similarly ignored in AUX mode?
>
> **Edge case 5: Boot with AUX saved in NVS and HP already inserted**
> NVS load enables REM for AUX, then HP detection fires and disables it — or the reverse depending on task timing. Do we need a deterministic ordering here, or is the HP check at boot already reliable?
>
> The core question underlying most of these: should AUX mode be a persistent override that makes BT events (connect/disconnect) not touch mute or REM, or should BT events always take precedence?

---

### User Answers

> **User:**
>
> **Edge case 1:** In this case if the headphones are already plugged in then the BD37033 should have already been muted and REM disabled — from HP insert logic, which we need to confirm — since the speakers aren't being used and we don't want to worry about noise and no need to power the amp. So if that's the case then simply switching to AUX wouldn't have any effect — music is just routed from AUX to headphones instead of Bluetooth to headphones without the BD37033 being involved.
>
> **Edge case 2:** If the headphones are removed while AUX is selected then presumably the BD37033 is already muted and REM is disabled (from headphone logic). So in this case we need to undo that — we'd unmute the BD37033 and enable REM irrespective of whether Bluetooth is enabled (since AUX is selected, not Bluetooth).
>
> **Edge case 3:** If we're in AUX mode then we really don't care about Bluetooth. If it disconnects then we likely don't want to do anything. The BD37033 should already be muted and REM disabled (from AUX logic) so I don't think anything needs to happen if everything else is working correctly.
>
> **Edge case 4:** This is similar to edge case 3 in some ways. If in AUX mode we don't care about Bluetooth. The BD37033 should already be muted and REM disabled and connecting to Bluetooth shouldn't have any effect. Note that if we connect to Bluetooth we should remember that because we might change from AUX to Bluetooth mode and have to unmute the BD37033 and enable REM, but that's likely handled in other edge cases.
>
> **Edge case 5:** I'm not sure, given the others, if this is a case we need to address or not, once the other edge cases are correct. But to be clear, if we boot with AUX saved then the input will be AUX. If the HP is already inserted then I would think the other logic would prevail — BD37033 is muted and REM disabled since the headphone is inserted. What's interesting in this case is that I'm not sure it matters on boot if AUX or Bluetooth are set when the HP is already inserted — in both cases audio doesn't go out the speakers so the speakers should be muted (which means BD37033 is muted and REM is disabled).

---

### AI Follow-Up

> **Claude:**
>
> The overall model is clear and makes sense. HP state takes priority — it gates everything else. One thing to flag:
>
> **Potential logic issue in your edge case 3 response:**
>
> You said "the BD37033 should already be muted and REM disabled (from AUX logic)." But if we're in AUX mode with no headphones connected, the correct state is that BD37033 is **unmuted** and REM is **enabled** — because AUX audio needs to go to the speakers. So a BT disconnect in that scenario shouldn't find muted/REM-disabled state; it should find unmuted/REM-enabled, and it should leave it alone.
>
> I think you may have been picturing the HP-also-connected scenario when you wrote that, where muted/REM-disabled would already be true — and in that case the BT disconnect would correctly be a no-op because the HP logic already set things right. Either way the conclusion is the same (BT disconnect is a no-op in AUX mode), but I want to confirm the expected state table:
>
> | Input | HP in | BT state | BD37033 | REM |
> |-------|-------|----------|---------|-----|
> | AUX | No | Any | Unmuted | Enabled |
> | AUX | Yes | Any | Muted | Disabled |
> | BT | No | Connected | Unmuted | Enabled |
> | BT | No | Not connected | Muted | Disabled |
> | BT | Yes | Any | Muted | Disabled |
>
> Does this match your intent? If yes, I'll build the plan around this table — the fix essentially becomes: any function that touches mute/REM needs to evaluate all three state variables (input mode, HP, BT) rather than reacting to just the one event that fired.

---

### User Clarification

> **User:**
>
> I see, my response might not have included conditions for the headphones as it wasn't included in your questions and may have been addressed in other ones. In the end if the headphones are connected then the BD37033 should always be muted and REM disabled since they're not used when headphones are connected (addressed in other cases I think). So I probably interpreted that as headphones disconnected — in which case we need the BD37033 unmuted and REM enabled, even in AUX mode, so that we get speaker output.
>
> The key to understand is the difference between source and output:
>
> - **Bluetooth vs AUX is all about input** — where the source comes from. If it comes from AUX then we really don't care what happens to the Bluetooth connection — turn it on, turn it off — doesn't matter because we're routing input from AUX.
> - **Headphone present is all about output** — where we route the sound to. If the headphone is inserted then the BD37033 should always be muted and powered down since we're not using it (irrespective of input).

---

### AI Response

> **Claude:**
>
> That's very clear — input and output are orthogonal concerns and the table holds. No more questions from me.

Claude then read the relevant source files to map exact line numbers before writing the implementation plan.

---

### Plan Review — First Draft

Claude presented a plan with 5 bugs to fix across 3 files. The user requested an addition:

> **User:**
>
> I believe you need an additional state in your table, related to A2DP. It's currently already working but likely worth including for completeness so we don't break anything.
>
> If the input is Bluetooth and the streaming is paused — as told by A2DP — we should mute the BD37033 and disable REM if the headphones are not plugged in (like we do now) but also reverse that when music plays again. If the headphones are plugged in then the BD37033 is already muted and REM disabled as per your last case so we don't need to do anything.
>
> Update the plan and I'll continue reviewing.

Claude added the A2DP streaming rows to the state table, and also identified that A2DP audio state events (pause/stop/start) firing while in AUX mode would corrupt the `s_audio_muted` flag — which would later prevent the BD37033 from unmuting correctly when headphones are removed. This became **Bug 4** in the final plan.

---

## Final Approved Plan

### State Table

| Input | HP in | BT state | BD37033 | REM |
|-------|-------|----------|---------|-----|
| AUX | No | Any | Unmuted | Enabled |
| AUX | Yes | Any | Muted | Disabled |
| BT | No | Connected, streaming | Unmuted | Enabled |
| BT | No | Connected, not streaming | Muted | Disabled |
| BT | No | Disconnected | Muted | Disabled |
| BT | Yes | Any | Muted | Disabled |

The BT streaming rows were already working correctly. The HP-in rows override everything else regardless of streaming state.

---

### Bug 1 — `audio_board_set_input_source(AUX)` never enables REM at runtime

**File:** `board.c`

`audio_board_set_mute(false)` already guarded BD37033 unmute via `!s_hp_connected`, but REM was never toggled at runtime. Switching to AUX left the amplifier off.

```c
// Before
if (input == AUDIO_INPUT_AUX) {
    audio_board_set_mute(false);
} else {
    audio_board_set_mute(true);
}

// After
if (input == AUDIO_INPUT_AUX) {
    audio_board_set_mute(false);
    audio_board_set_amp_remote(!s_hp_connected);
} else {
    audio_board_set_mute(true);
    audio_board_set_amp_remote(false);
}
```

---

### Bug 2 — HP removal only considered BT state for REM

**File:** `bt_audio.c` — `on_hp_event()`

```c
// Before
audio_board_set_amp_remote(s_bt_connected);

// After
bool amp_on = s_bt_connected || (audio_board_get_input_source() == AUDIO_INPUT_AUX);
audio_board_set_amp_remote(amp_on);
```

---

### Bug 3 — A2DP disconnect muted BD37033 and disabled REM even in AUX mode

**File:** `bt_audio.c` — `a2dp_callback()` disconnect branch

```c
// After
if (audio_board_get_input_source() == AUDIO_INPUT_BLUETOOTH) {
    audio_board_set_mute(true);
    audio_board_set_amp_remote(false);
}
```

---

### Bug 4 — A2DP audio state events (pause/stop/start) fired in AUX mode

**File:** `bt_audio.c` — `ESP_A2D_AUDIO_STATE_EVT`

The pause/stop handler called `audio_board_set_mute(true)`, which silenced AUX audio and set `s_audio_muted = true`. That flag is checked when headphones are removed — a stale `true` value would prevent BD37033 from unmuting after headphone removal.

```c
// After — both started and paused/stopped branches guarded
if (audio_board_get_input_source() == AUDIO_INPUT_BLUETOOTH) {
    // existing mute logic unchanged
}
```

---

### Bug 5 — A2DP connect enabled REM in AUX mode

**File:** `bt_audio.c` — `a2dp_callback()` connect branch

```c
// After
if (audio_board_get_input_source() == AUDIO_INPUT_BLUETOOTH) {
    audio_board_set_amp_remote(!audio_board_get_hp_connected());
}
```

---

### Bug 6 — NVS load enabled REM for AUX without checking HP state at boot

**File:** `device_info.c` — `device_load_settings()`

At boot, `s_hp_connected` is set immediately from GPIO before NVS loads, so the HP state is available.

```c
// Before
if ((audio_input_t)u8val != AUDIO_INPUT_BLUETOOTH)
    audio_board_set_amp_remote(true);

// After
if ((audio_input_t)u8val != AUDIO_INPUT_BLUETOOTH)
    audio_board_set_amp_remote(!audio_board_get_hp_connected());
```

---

## Test Plan

| # | Starting State | Action | Expected Result | Test Result |
|---|---------------|--------|-----------------|-------------|
| 1 | HP out, BT disconnected, input=BT | Boot device | BD37033 muted, REM off |Confirmed |
| 2 | BT disconnected, input=BT, HP out | Connect to Bluetooth | BD37033 unmuted, REM on |Confirmed |
| 3 | BT connected, streaming, HP out | Disconnect from Bluetooth | BD37033 muted, REM off |Confirmed |
| 4 | BT disconnected, input=BT, HP out | Switch input to AUX | BD37033 unmuted, REM on |Confirmed|
| 5 | Input=AUX, HP out | Connect to Bluetooth | No change to BD37033 mute or REM |Confirmed |
| 6 | Input=AUX, BT connected, HP out | Disconnect from Bluetooth | No change to BD37033 mute or REM |Confirmed |
| 7 | Input=AUX, BT disconnected, HP out | Insert headphones | BD37033 muted, REM off |Confirmed |
| 8 | Input=AUX, BT disconnected, HP in | Remove headphones | BD37033 unmuted, REM on |audio plays in HP before removing but not through speakers afterwards |
| 9 | Input=AUX, BT connected, HP out | Insert headphones | BD37033 muted, REM off | confirmed |
| 10 | Input=AUX, BT connected, HP in | Remove headphones | BD37033 unmuted, REM on | confirmed |
| 11 | Input=BT, BT streaming, HP out | Insert headphones | BD37033 muted, REM off | Confirmed |
| 12 | Input=BT, BT connected, HP in | Remove headphones | BD37033 unmuted, REM on | Confirmed |
| 13 | Input=AUX, HP out | Pause playback on phone | No change to BD37033 mute or REM | |
| 14 | Input=AUX, HP out | Resume playback on phone | No change to BD37033 mute or REM | |
| 15 | HP in, AUX saved in NVS | Boot device | BD37033 muted, REM off | |

- Aux volume is very low
---

## Key Takeaways

This session illustrates several practices that made the AI collaboration effective:

**Give context, not just a ticket.** The initial message explained the purpose of REM, the role of the BD37033, and the relationship between AUX and Bluetooth before asking any question. This let the AI reason about correctness rather than pattern-match against familiar code.

**Let the AI ask questions before planning.** The AI explored the code first, then surfaced specific ambiguities before writing any plan. This prevented a plan built on wrong assumptions.

**Catch logic errors in user responses.** The AI flagged an inconsistency in the edge case 3 answer — the user had conflated two different HP scenarios. Catching this before coding prevented a subtle bug.

**Separate input from output.** The user's clarification — "Bluetooth vs AUX is about input, headphone present is about output" — became the design principle that unified all the fixes. Naming the model clearly made the implementation straightforward.

**Review the plan before executing.** The user reviewed the draft plan and identified a missing A2DP streaming state. This led to the discovery of Bug 4, which had a non-obvious secondary effect (corrupting `s_audio_muted` in a way that would later break headphone removal behavior).

**The AI found a bug the user didn't ask about.** Bug 4's secondary effect — the stale `s_audio_muted` flag — was not mentioned in the requirements. It was discovered by tracing the code path during plan review.

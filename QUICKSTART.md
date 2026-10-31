# Quick Start

> New to Áron? The app's **?** (Help) button at the top right opens an illustrated
> step-by-step guide. This page is the operator's reference.

Everything runs in the cloud — there's nothing to install and no special
hardware, just the single `church-translate.exe` and your API keys. Two
audiences: **A. Operators** (machine is already set up — just run it) and
**B. New machine** (standing it up from scratch).

---

## A. Operate it (already set up)

1. **Double-click `church-translate.exe`** (Windows). A console window opens
   (the server log), and your browser opens at `http://localhost:5000`.
2. Pick the language to translate into with the language button (it reads
   **→ English**). Every segment, in whatever language it's spoken, is
   translated into this one target; tick two languages for a bilingual service.
3. Click **Start listening** (or press Space). It turns into a red **Stop**.
4. Speak, in any supported language. The **Original** column fills as it's
   transcribed; the **Translation** column fills with the chosen target
   language when the cloud returns it.
5. Optional: press **Speak** to also hear the translation aloud (adds
   latency; uses ElevenLabs or Soniox, whichever key is configured — the Soniox
   key already covers Soniox's voices).
6. To stop: click **Stop**, then close the console window.

What a service needs is in one row: **Start listening**, the languages,
**Mic** (mute it for music), **Speak**, **Project**, **Clear wall** and, last,
**Settings**, which holds everything else by pipeline stage: **1 Listen**
(input / gain / **Pace** / **Gate music** / **Extra ears**), **2 Translate**
(**Model** and the **Decision Router**), **3 Wall** (the projector's knobs) and
**4 Voice**. Across the top, one light for each part of the pipeline — grey
idle, green working, orange worth a look, red broken; hover one for details.

**How big each piece is (Pace).** Pace, in Settings → Listen, sets how much
text goes to the translator at a time: **Pronto** (~3–7 words), **Moderato**
(~7–10, the default), **Lento** (~10–14). Smaller pieces reach the screen
sooner; bigger ones give the translator more context. A change applies at the
next cut. Under the **Decision Router** Pace is greyed out: Soniox's own
segments set the size.

**Which model translates.** The **Model** menu picks the translator.
`claude-haiku-4-5` is the default and among the fastest (~0.6 s per caption).
One entry uses no model at all and captions Soniox's own translation:
**none, fast** shows only final text, a few words at a time, about 1 s sooner
than any model path, and nothing on the wall changes once shown — but Soniox
translates Romanian idioms word for word. Nothing is spoken under it.

- **Source language** is an accuracy *hint* for Soniox, not a restriction —
  leave it on **Auto-detect** (the default) unless you know only one language
  will be spoken and want to pin it for slightly better accuracy. Soniox
  streams continuously and marks phrase ends itself (lowest latency); it
  falls back automatically to per-segment transcription if the streaming
  socket ever fails.
- **Voice** (Settings → 4 Voice): **Speech** picks the service and model in
  one menu — **Soniox v2**, **ElevenLabs v4** or **ElevenLabs v4 Turbo** (the
  default) — then the **Voice** (one voice covers every target language) and
  where it plays (**Output**). Soniox also has a speed slider. Stream,
  Expression, Stability and Tighten pauses are set in `.env`
  (`CT_EL_STREAM`, `CT_EL_EXPRESSION`, `CT_EL_STABILITY`, `CT_SX_TIGHTEN`).

**Keeping the service.** With Speak on, the spoken translation is recorded as
it goes (the switch is **Record speech**, in Settings → 4 Voice). The
**download** button at the top right offers **Spoken audio** — or press `A` —
as one `.zip`: the
whole service as a single mp3, subtitle files timed to it, a timestamped map of
every clip, and a source-beside-translation transcript. **Auto-save on exit**
(on by default) writes the same thing into the `recordings` folder when you
close the app, so there is nothing to remember; **Save now** does it mid-service
and **Discard** starts a clean recording for the next one. The clips are held by
the app itself, not the browser tab, so a reload never loses them, and nothing
is uploaded anywhere.

**Costs.** The **$** at the end of the lights is the estimated spend of this run
across every API; hover for the breakdown. **Diagnostics** also shows ElevenLabs'
remaining credits and this run's usage (STT minutes, TTS characters,
translation calls). *(If it shows "key lacks 'user read' scope", your ElevenLabs
key is scoped to STT+TTS only — the remaining-credit line needs user read, but
the this-run tally still works.)*

### Tuning the mic / feed (do this once per venue)

1. **Microphone** (Settings → 1 Listen) — pick the microphone or
   soundboard/SDI capture device.
2. Speak normally and watch the small level bar under the buttons: speech should
   sit well into the middle (about **−20 to −12 dBFS**). If it barely moves,
   raise **Gain**; don't let it hit the end (clipping).
3. If captions cut mid-sentence, move **Pace** one step slower; if they lag,
   one step faster.

Each translated line has a **▶** button to replay its speech (instant if Speak
was on — the already-synthesized clips are reused; synthesized on demand if
Speak was off). The **?** button at the top right opens the illustrated guide,
`QUICKSTART.pdf`. The **Diagnostics** button beside it opens a diagnostics
panel (engine/capability status + a live event log, with a Copy button), and
the bin above the Translation column wipes both caption columns without
stopping the session. **Clear wall**
(or `W`) wipes only what the room is reading — projector, website, ProPresenter —
and keeps the transcript.

Settings — including your **Source language** and **Translate into** picks —
are remembered in the browser between sessions.

---

## B. Set up a new machine

Two files and your API keys — no install, no Python.

### 1. Get the app
From the latest [release](../../releases), download **`church-translate.exe`**
and **`.env.example`** into the same folder.

### 2. API keys
Copy `.env.example` to `.env` in that folder and fill in:

```
SONIOX_API_KEY=...             (required: STT, its own translation, voice)
ANTHROPIC_API_KEY=sk-ant-...   (optional: Claude translators)
OPENAI_API_KEY=...             (optional: GPT translators, the Decision Router, OpenAI ear)
GEMINI_API_KEY=...             (optional: Gemini translators, Gemini Live ear)
ELEVENLABS_API_KEY=...         (optional: ElevenLabs voice and ear)
```

The Soniox key alone gives captions (Soniox's own translation, **none, fast**)
and a voice. Each LLM key adds its translators to the **Model** menu. The app
reads `.env` directly — no shell setup needed.

- Anthropic key: <https://console.anthropic.com> (per-token billing, **separate
  from Claude.ai Pro**).
- Soniox key: <https://console.soniox.com>.
- ElevenLabs key + voice id: <https://elevenlabs.io> (voice ids live in your
  Voice Library).

### 3. Run
Double-click **`church-translate.exe`** and open <http://localhost:5000>.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| **Model menu greyed out** | That provider has no key in `.env` (step 2). With no LLM key the app translates with **none, fast**. |
| **`Soniox key not configured`** | Add `SONIOX_API_KEY` to `.env` and relaunch — it's the only STT engine. |
| **Speak does nothing / "TTS not configured"** | Pick **Soniox v2** under **Speech** (it uses the Soniox key), or add `ELEVENLABS_API_KEY`. |
| **Can't start the server — "address already in use"** | An old server is still running on port 5000. Stop it (or set `CT_PORT`) and relaunch. |
| **Start button does nothing** | Hard-refresh the page (**Ctrl+Shift+R**). If still dead, press **F12 → Console** and read the red error. |
| **Mic blocked / "Mic error"** | Browsers only allow mic capture over **`localhost`** or **HTTPS**. Opening the page via a LAN IP over `http://` will fail — keep it on `localhost`, or set up HTTPS before using another device. |
| **Captions cut mid-sentence** | Move **Pace** one step slower (Pronto → Moderato → Lento). **Captions lag** → one step faster. |
| **Source and translation rows don't line up** under **none, fast** | Fixed 2026-09-24 — update to the current version. |

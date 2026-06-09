# Drums 129bpm – Dmaj → Strudel

A Strudel.cc drum set recreated from `source/Drums 129bpm - Dmaj.mp3`, built by
analysing the audio band-by-band and transcribing each layer.

## TL;DR — how to play it

Quick start (zero setup):

1. Open <https://strudel.cc>
2. Paste the contents of **`drums.strudel`**
3. Press **Ctrl/Cmd + Enter** to play.

### Which file should I use? (fidelity vs. setup)

| Goal | File | Setup | Sounds like the track? |
|---|---|---|---|
| **Sounds *exactly* like the track** | `drums-samples.strudel` → Version 2 (`mploop2.loopAt(2)`) | host/import the loop sample | ✅ it *is* the audio |
| Real timbres **+ separate tweakable layers** | `drums-samples.strudel` → Version 1 | host/import the one-shots | ✅ very close (real one-shots) |
| Instant, no setup, fully programmable | `drums.strudel` | none | ◑ same groove, *approximate* timbres (built-in 909/perc) |

Because you said it **must sound like the track**, use `drums-samples.strudel`.
`drums.strudel` is the convenient paste-and-play version — same rhythm and accents,
but Strudel's built-in kit, so the *timbre* is in the ballpark, not identical.

## What the track actually is

| Property | Finding |
|---|---|
| Tempo | **129 BPM**, 4/4 (confirmed by autocorrelation + on-bar silence cuts) |
| Downbeat | t = 0 (sections start exactly on bar lines) |
| Structure | drum stem in blocks: bars 0–31, 34–49, 61–68, 77–92, with silent breaks between |
| Loop | ~1-bar core (2-bar feel with tiny variation) |
| Style | dense **percussive / afro-tech house** — not a plain drum-machine beat |

### The layers (this is the "multiple instruments" part)

| Layer | Where it lives | Pattern (16th steps, 1 bar) |
|---|---|---|
| **Kick** | ~60 Hz, tuned, short | **syncopated** — steps **6 & 12** ("and of 2" + beat 4), *not* four-on-the-floor |
| **Low conga / tom** | ~130–150 Hz, tonal (the *D major* colour) | 16th roll into beat 2, accents on 6 & 12 |
| **Mid percussion** | 250–800 Hz, woody | syncopated 16ths |
| **Tambourine / metal perc** | 1.5–4.5 kHz | busy 16ths |
| **Shaker** | 4.5–8.5 kHz | continuous 16ths (groove engine) |
| **Closed hi-hat** | >9 kHz | continuous 16ths with accents |

There is **no clean clap/snare on beats 2 & 4** — the backbeat is woven from the
percussion web, which is typical of this style.

The `.gain("...")` numbers in the Strudel files are the **measured accent levels**
from the track (per 16th note), so the dynamics follow the original.

## How faithful is it? (objective check)

I resynthesised the **full 6-layer transcription using the track's own one-shots**
(i.e. the `drums-samples.strudel` Version 1 path) and correlated its per-band onset
envelope against the original (bars 24–32):

| Band | Correlation |
|---|---|
| Low (kick) | **+0.95** ✅ kick placement nailed |
| Broadband (overall groove) | **+0.73** ✅ feel matches |
| Low-mid (congas) | +0.20 |
| Highs (hats/shaker) | +0.39 … +0.45 |
| Mid (260–1500 Hz perc) | ≈ 0.0 ⚠️ |

Honest takeaways:
- The **kick and overall groove are spot-on**.
- The **busy mid percussion does not reduce to a clean 16th grid** — even with the
  mid layer included it barely correlates (≈0). That's the inherent ceiling of
  transcribing organic, layered percussion to a step sequence.
- These numbers measure the **sample/one-shot version**. `drums.strudel` (built-in
  sounds) shares the *same rhythm grid* but different timbres, so it will sound
  recognisably like the groove without matching tone-for-tone.
- For a 1:1 match — including those mids — use the **loop sample**
  (`s("mploop2").loopAt(2)`); it is the original audio.

Listen and judge: `preview_resynth.mp3` is 8 bars of the one-shot version on the
grid; `AB_original_then_resynth.mp3` plays the original 8 bars, then that resynth.

## Files

```
out/
  drums.strudel            ← MAIN: built-in sounds, paste & play
  drums-samples.strudel    ← exact timbres (uses the sliced samples)
  README.md
  patterns.txt             ← raw per-step velocity/gain data from the analysis
  preview_resynth.mp3/.wav ← audio preview of the transcription
  AB_original_then_resynth.mp3/.wav ← A/B against the original
  samples/
    strudel.json           ← sample map (set "_base" to your host URL)
    mpkick/kick.wav  mptom/tom.wav  mpperc/perc.wav  mpclap/clap.wav
    mpchat/chat.wav  mpshkr/shkr.wav         ← one-shots sliced from the mp3
    mploop2/loop_2bar.wav   mploop4/loop_4bar.wav   ← the real loop
```

Each one-shot sits in its own subfolder so the sound names line up whether you
host them or import the folder locally (`mpkick`, `mptom`, `mpperc`, `mpclap`,
`mpchat`, `mpshkr`, `mploop2`, `mploop4`).

## Using the sample version

**Option A — hosted (this repo, already wired up).** The samples live at
<https://github.com/cosmiclavaflow/-strudel-drums-129-dmaj>. Just load them:
```js
samples('https://raw.githubusercontent.com/cosmiclavaflow/-strudel-drums-129-dmaj/main/samples/strudel.json')
```
(To host elsewhere, drop the `samples/` files on any CORS-friendly static host and
point `samples/strudel.json` `_base` at it.)

**Option B — import locally.** In strudel.cc → **sounds** tab → **import-sounds**
→ **import sounds folder** → pick the `samples/` folder. Each subfolder becomes a
sound name (`mpkick`, `mptom`, …) automatically — the same names the code uses, so
no edits needed. It's stored in your browser, never uploaded.

## Tweaking

- **Swing/human feel:** append `.swingBy(1/24, 16)` to a layer.
- **Less busy:** change a `*16` to `*8`, or mute a layer by changing `$:` to `_$:`.
- **Deeper kick:** in `drums.strudel`, swap the kick to `.bank("RolandTR808")`.
- **Re-tune the congas:** change `note("d3")` (try `a2`, `d2`, `f3`).
- **Different kit feel:** add `.bank("RolandTR909")` (or any machine) to a layer.

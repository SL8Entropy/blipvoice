# Blip Voice Generator

A single-file, offline-capable tool that turns typed text into an Undertale-style "blip" voice and, optionally, a typewriter-style text video. Open `blip-voice-local.html` in a browser. There is nothing to install and no Python or ffmpeg is needed.

It can export:

- **Audio only**: a `.wav` of the blips.
- **Video only**: the typewriter text on a solid background, with no sound.
- **Video + audio**: both in one file.

The video has **no text box**. It is only text on a flat colour (green by default) so you can chroma-key it over your own footage.

> Not affiliated with Undertale or Toby Fox. The character presets are my own approximations of each character's *feel*, not extracted game audio.

---

## 1. Requirements

| | |
|---|---|
| Browser | **Chrome or Edge** (recommended). Firefox works but records **WebM** only. Safari is untested. |
| Internet | Optional. Only needed for the pixel fonts (loaded from Google Fonts). Offline, those fall back to a plain font. |
| Files | Just `blip-voice-local.html`. |

## 2. Quick start

1. Double-click `blip-voice-local.html`.
2. Type your text in the top box.
3. Press **▶ PLAY** to preview the sound and the typing animation.
4. Pick an output type in the dropdown (Audio / Video only / Video + audio) and press **⬇ EXPORT**.
5. Your browser downloads `blip-voice.wav`, `blip-voice.mp4` or `blip-voice.webm`.

---

## 3. Writing your text

How text turns into sound and timing:

| You type | What happens |
|---|---|
| Letters and digits (any language) | One blip each, and the character appears on screen. |
| Space | A short silent gap (0.6 letter-lengths). |
| `, ; :` | A silent gap of 1.5 letter-lengths after the character. |
| `. ! ?` | A silent gap of 3 letter-lengths after the character. |
| New line | A silent gap of 3 letter-lengths, and a line break on screen. |
| `^` (pause character) | A silent beat. **Invisible and takes no space.** |
| `///` (wipe marker) | **Clears the screen** and starts the next part. Invisible, takes no space, and adds **no time**. |

"Letter-length" means one letter's slot, which is `1 ÷ Letters per second`. At 13 letters/sec, one letter-length is about 0.077 s.

### Pause character (`^`)

- Put several in a row for a longer pause: `Well...^^^ okay.`
- Each `^` lasts **Pause length** letter-lengths (default 3, which is about 0.23 s at 13 letters/sec). Change it with the **Pause length** slider under Voice → **more**.
- Because it is removed before layout, it never shifts centred text or changes line wrapping.
- A trailing `^` adds silence at the end of the clip.
- You can change the character in the small **pause** box at the top right, or empty the box to turn the feature off.
- Avoid using `*` if you type Undertale-style lines that start with `*`, because every `*` would become a pause.

### Wipe marker (`///`)

- `Hello there.///Goodbye.` shows "Hello there." and then, when it finishes, clears the screen and starts typing "Goodbye."
- The marker itself costs **zero time**. Add `^` where you want a pause:
  - `Hello^^^///Goodbye` waits **before** wiping, so the old text stays visible during the pause.
  - `Hello///^^^Goodbye` wipes **first**, so the screen is blank during the pause.
- Spaces and new lines directly before or after `///` are ignored. You can put the marker on its own line for readability.
- Each section is wrapped, aligned and positioned on its own, so a short section centres correctly by itself.
- A trailing `///` ends the video on a blank screen.
- The small **wipe** box at the top right changes the marker (up to 6 characters). Empty turns it off.
- The on-page preview shows only the first section. Press **▶ PLAY** to see wipes happen.

---

## 4. Voice settings

One dropdown holds every voice preset: the built-in ones (Sans, Papyrus, Toriel, Flowey, Asgore, Ghost) and your own saved ones under "Mine". Picking one loads it, and everything is adjustable afterwards. Pitch, jitter, speed and wave are shown up front. Blip length, crush bits, crush rate, pulse width and pause length are under **more**.

| Setting | What it does |
|---|---|
| **Wave** | Shape of the blip: *square* (classic), *triangle* (softer), *saw* (buzzy), *noise* (breathy or hissy). |
| **Pitch (Hz)** | Base pitch. Lower is deeper. |
| **Pitch jitter (st)** | Random pitch variation per letter, in semitones. 0 gives a monotone voice. Higher sounds more chattery. Results are deterministic, so the same text and settings always produce the same voice. |
| **Blip length (ms)** | Length of each blip. Short is sharp, long is sustained. |
| **Letters / sec** | Speaking speed. This also sets all pause lengths. |
| **Crush bits** | Bit depth. Lower is more crunchy and pixelated. |
| **Crush rate (Hz)** | Sample-rate reduction. Lower is more lo-fi. |
| **Pulse width** | Duty cycle of the square wave. Away from 0.5 gives a thinner, nasal tone. |
| **Pause length** | Length of each `^`, in letter-lengths. |

Starting points: for a sharp Undertale feel try square wave, 4 to 6 bits, and 8000 to 11025 Hz. For a darker voice lower the pitch, lengthen the blips and slow the speed.

---

## 5. Video settings

| Setting | What it does |
|---|---|
| **Font** | Pixel fonts (VT323, Press Start 2P, Pixelify Sans, Silkscreen, DotGothic16), plus system fonts (Comic Sans MS, Papyrus, Courier New and others). |
| **Own font** (under **more**) | Upload a `.ttf`, `.otf`, `.woff` or `.woff2`. Use this for Determination Mono or any font you have a licence for. |
| **Size / Line gap** | In pixels at the chosen resolution. Line gap is under **more**. |
| **Text / Background** colours | Background defaults to `#00ff00` (chroma green). |
| **Align / Vertical** | Left or centre, and top, middle or bottom. |
| **Resolution** (under **more**) | 1920×1080, 1280×720, 3840×2160 or vertical 1080×1920. |
| **End hold (s)** | How long the finished text stays on screen after the last blip. |
| **File type** | Auto (MP4 if the browser supports it), MP4, or WebM. |
| **Hard edges** | Removes soft anti-aliasing so every text pixel is fully solid. This gives much cleaner keying with no green fringe. |

Layout is computed from the **full** text before typing starts, so words never jump between lines as they appear. Centred lines stay centred while typing.

---

## 6. Saving presets

There are **two independent preset sets**:

- **Voice presets** save every Voice setting, including pause length.
- **Video/text presets** save the font, size, colours, alignment, resolution, hold time, file type, hard edges, and the pause and wipe characters. If you used an uploaded font, the **font file is stored inside the preset**, so it comes back when you load it.

How to use them:

1. Set things up the way you like.
2. Type a name in the small **name** box next to the Voice or Video dropdown, then click **SAVE**. Saving with an existing name overwrites it. Choosing a saved preset fills its name into the box.
3. Choose it later from that dropdown. It loads immediately. Voice presets appear under "Mine" below the built-in ones.
4. **DEL** removes the selected saved preset. Click it twice (it turns into **SURE?**) to confirm. Built-in voices can't be deleted.

Presets live in your browser's local storage. To back them up or move them to another computer or browser, use **export presets** at the bottom of the page (downloads `blip-presets.json`) and **import** on the other end. Imports merge with what is already there.

Note: clearing your browser data erases saved presets, and some browsers treat local files (`file://`) storage differently. Keep an exported copy of presets you care about.

---

## 7. Exporting

- **Audio only** is instant and writes a 16-bit mono WAV at 44.1 kHz.
- **Video exports record in real time.** A 10-second line takes about 10 seconds plus the hold time. A status line shows progress.
- **Keep the tab open and visible while recording.** Browsers throttle hidden tabs, which can freeze the animation.
- The video begins with 0.3 s of blank background and ends with the hold time, which gives you trim room.
- Video runs at 30 fps with a 12 Mbit/s target. Flat colour compresses well, so files stay small.
- The MP4 route depends on your browser's encoder. If an editor rejects the MP4, switch **File type** to WebM, or convert it with any converter such as HandBrake.

### Using it over footage (green screen)

1. Place the exported clip on a track above your footage.
2. Apply your editor's chroma key (Ultra Key in Premiere, Qualifier/Delta Keyer in DaVinci Resolve, Chroma Key in CapCut, Kdenlive and others) and pick the green.
3. Leave **Hard edges** on for the cleanest result. If your footage contains green, change **Background** to blue or magenta.
4. Add your own text box or frame in your editor.

---

## 8. Troubleshooting

| Problem | Fix |
|---|---|
| Text uses the wrong font | Pixel fonts need internet. Upload a font file to use one offline. |
| System font (Papyrus, Comic Sans) not changing | It isn't installed on your computer, so the browser falls back. |
| No sound while previewing | Click the page first, since browsers block audio until you interact. Check your volume. |
| Video won't record | Use Chrome or Edge. Some browsers lack `MediaRecorder` or MP4 support. |
| Video is stuttery or frozen | You switched tabs during recording. Keep the tab in front and avoid heavy load. 4K is demanding, so try 1080p. |
| Green fringes on text edges | Turn on **Hard edges**, or tighten your editor's key settings. |
| Preset didn't save | If your browser blocks local storage, presets last only until you close or reload the page, and the status line says so. Use **export presets** to keep them. |
| Some characters make no sound | Only letters and digits blip. Symbols, emoji and punctuation are silent by design, though they still display. |

---

## 9. How it works

- **Audio**: for each letter and digit, a short waveform (square, triangle, saw or noise) is generated into a sample buffer. Then the whole buffer is bit-crushed by holding samples (rate reduction) and rounding to a limited number of levels (bit reduction).
- **Parsing**: the text is split into letters, pause markers and wipe markers *before* layout, so markers can't affect spacing or alignment.
- **Video**: text is drawn onto a canvas, revealed letter by letter in sync with the blip timings, and the canvas plus the audio are recorded together with the browser's `MediaRecorder`.
- **Hard edges**: the text is drawn to an offscreen canvas and every pixel's alpha is snapped to fully on or fully off before compositing.

## 10. Known limitations

- Video export is real-time, not faster than playback.
- No transparent (alpha) video. Use the chroma key route.
- Only one pitch pattern per voice: pitch is random per letter rather than tied to specific letters.
- Not every browser can encode MP4, and I have not tested every browser and editor combination.
- Custom fonts stored in presets can make the preset data large.

## 11. Files

| File | Purpose |
|---|---|
| `blip-voice-local.html` | The whole app. |
| `README.md` | This guide. |
| `blip-presets.json` | Created by you when exporting presets. |

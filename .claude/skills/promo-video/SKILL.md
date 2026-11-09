---
name: promo-video
description: Make a short A-PLAYERS promo video in Remotion from an idea, a headline and a call to action, following the A-PLAYERS brand kit (colours, Inter Tight, logo rules, light blue/pink/lavender gradient, calm one-idea-per-scene tone, a hook in the first second, eased transitions, beat-synced royalty-free music). Use when asked for a promo video, teaser, announcement clip, social video or animated ad for A-PLAYERS.
---

# Promo video (A-PLAYERS)

## When to use

Use this skill when someone asks for a promo, teaser, announcement or social video for **A-PLAYERS**, or for a storyboard or script for one.

Don't use it for other brands. The kit below covers A-PLAYERS only.

## Input needed

Ask for anything missing before you start. Don't invent it.

| Input | What it is | Example |
|---|---|---|
| **Idea** | The one thing the video should make people feel or understand | "Impact comes before titles" |
| **Headline** | The main line shown on screen | "Impact - First" |
| **Call to action** | What the viewer should do at the end | "Apply by Friday" |

Optional: format (16:9, 9:16 or 1:1), length, where it will be posted, and any link or date for the end card. If you don't know the format or length, ask. "Link in bio" means Instagram, so default to 9:16.

Don't add facts the requester didn't give (topics, prices, durations, names). If a scene needs one, ask.

## Tools

Videos are built and rendered with **Remotion**. The Remotion skills are installed next to this one. Use them for anything specific to Remotion:

| Skill | Use it for |
|---|---|
| `/remotion-best-practices` | Picking the right Remotion skill when you're not sure which one |
| `/remotion-create` | Setting up the project the first time |
| `/remotion-markup` | Compositions, scenes, `TransitionSeries`, timing, fonts and images |
| `/remotion-studio` | Opening the live preview |
| `/remotion-render` | Exporting the MP4 and still frames |
| `/remotion-license` | Licence terms. Check them before any commercial use. |

**Project location:** `~/Build/a-players-video`. Keep the project **outside** this brain repo, because the brain holds documents, not video source code. If the folder doesn't exist, scaffold it there with `/remotion-create`.

## Steps

1. **Collect the input.** Confirm idea, headline and call to action. Restate them in one line each and get a yes before going on.
2. **Write the storyboard.** One idea per scene. For each scene write: the on-screen text, the background (gradient, light or dark), whether the logo appears, the duration, and the transition into the next scene (type, length, and the beat it starts on). Name the music track, its tempo, source and licence. Keep text short enough to read in the time it's on screen.
   - Opening scene: the hook (or the headline), following the **Hook** rule.
   - Middle scenes: the idea, one point per scene.
   - Closing scene: the call to action, then the logo on a light background.
3. **Check the storyboard against the brand rules** below, including **Motion**. Fix every scene that breaks a rule.
4. **Show the storyboard** to the requester and get approval.
5. **Build the video in Remotion.**
   - Open the project at `~/Build/a-players-video`, or create it with `/remotion-create`. Start Remotion Studio (`/remotion-studio`) so the requester can watch while you build.
   - Add one `<Composition>` per video, with an id like `AiIntensive9x16`. Use 30 fps and these sizes: 1080×1920 for 9:16, 1080×1080 for 1:1, 1920×1080 for 16:9.
   - Build the scenes with `<TransitionSeries>`, giving each scene its own `<TransitionSeries.Sequence>`. Every cut is a `<TransitionSeries.Transition>` using `fade()`, `slide()`, `wipe()` or a scale presentation, and must follow the Motion rules (at least 2 of them are not fades).
   - Add the music track with `<Audio src={staticFile(...)}>` and set the scene and transition durations from the beat grid (see **Sound**).
   - Drive all animation with `useCurrentFrame()` and `interpolate()` or `spring()`. Don't use CSS `transition` or `animation`, because Remotion won't render them.
   - Load Inter Tight with `@remotion/google-fonts/InterTight`, and only the weights from 400 to 800.
   - Copy `assets/logo.png` and `assets/banner.png` from this skill into the project's `public/` folder, unchanged, and load them with `staticFile()`.
   - Put the brand colours in one constants file and import them everywhere, so no other colour slips in.
6. **Render** with `/remotion-render`:
   - The video: `npx remotion render <CompositionId> out/<name>.mp4`
   - Stills for review, taken at the middle of every scene and the middle of every transition: `npx remotion render <CompositionId> out/frames --frames=<list> --image-format=png`
7. **Review the stills** against the checklist at the bottom. Fix anything that fails and render again.
8. **Hand it over.** Save the MP4 to `~/Downloads/` with a clear name, such as `a-players-<topic>-9x16.mp4`. Send it with the storyboard and the music credit: track title, artist, source URL and licence (plus the exact attribution line to put in the post, if the licence asks for one).

## Brand rules (from the A-PLAYERS brand kit)

### Colours

| Name | Hex | Use |
|---|---|---|
| Dark background | `#07070B` | Dark scenes |
| Ink | `#0B0B0F` | Text on light backgrounds |
| Light text | `#F4F4F8` | Text on dark backgrounds |
| Light blue | `#BCD4ED` | Gradient |
| Pink | `#F6D2E4` | Gradient |
| Lavender | `#B6C4E4` | Gradient |
| Accent | `#5197FF` | Highlight, e.g. the call to action |

Use only these colours.

### Gradient

Light blue, pink and lavender, as in the banner (`assets/banner.png`). Use it as the main light background.

### Font

**Inter Tight**, weights 400 to 800. No other font.

### Logo

- File: `assets/logo.png` (mark + "A-PLAYERS" wordmark).
- **Never recolour, stretch or crop it.** Scale it only proportionally.
- **It is black, so it goes on light backgrounds only**: the gradient or Light text `#F4F4F8`. Never on Dark background or Ink.
- Tagline from the banner: "Impact - First".

### Tone

**Calm and confident. One idea per scene.** No crowded scenes, no stacked messages, no frantic cuts.

### Motion

**Do not make sudden transitions.** Every change on screen should feel smooth.

- **Hook first.** The first scene shows motion and the core message **within 1 second**. Something moves from frame 0 (no blank or black opening, no slow fade-in from nothing), and the hook text is fully visible by frame 15 and stays readable through frame 30. Scene 1 is the only exception to the 20-frame text fade-in below.
- **No hard cuts.** Every scene change is a transition: a `fade()`, or one of the real transitions below.
- **Fades** last **at least 30 frames (1 s at 30 fps)**, with eased timing (ease-in-out, or `springTiming` with no overshoot). Never use `linearTiming` without an easing.
- **Light to dark is slower.** Any change between a dark scene and a light one (gradient or Light text) is a `fade()` of **at least 45 frames (1.5 s)**, so the brightness doesn't jump.
- **Use at least 2 real transitions.** At least 2 scene changes per video use `slide()`, `wipe()` or a scale presentation (the incoming scene grows from about 92% to 100% while the outgoing one fades). They are eased like fades (ease-in-out, or `springTiming` with no overshoot) and only go between two scenes of the same brightness (light to light, or dark to dark).
- **A real transition must be visible.** Text is already gone when a transition starts, so the two scenes' backgrounds must look clearly different, for example the gradient and a solid Light text `#F4F4F8` background. A slide or wipe between two identical backgrounds (two flat dark scenes, or the same drifting gradient) can't be seen and doesn't count towards the 2. Check it on the still from the middle of each transition.
- **Never a hard flash.** No transition passes through a white or black frame, and none flashes, glitches, shakes, zoom-punches or pixelates.
- **Gentle text.** Text fades in over **at least 20 frames** and rises by no more than 40 px. If there are several lines, start each one 8 to 10 frames after the one before.
- **Texts never overlap.** A scene's text must finish fading out before its outgoing transition begins, so two scenes' texts are never on screen together. The next scene's text may start fading in during the transition, once the old text is gone. Use this to avoid long empty screens, especially around the slow fade from dark to light.
- **Give the viewer time to read.** Each scene holds its text, fully visible, for at least 1.5 s. Remember that each transition shortens the timeline by its own length, so add that time back to the scene durations.
- **The background keeps moving.** The gradient drifts slowly across the whole video and never snaps.

### Sound

- **Every video has music**: a royalty-free track with a clear beat.
- **Only use a track whose licence allows commercial use** (for example CC0, CC BY, or a royalty-free licence that names commercial use). Read the licence on the track's own page; don't assume it. No CC BY-NC, no "personal use only", no unclear licence. If you can't confirm the licence, don't use the track.
- **Note the source and licence in the output**: track title, artist, source URL and licence go in the hand-over, and in a `CREDITS.md` next to the audio file in the project's `public/` folder. If the licence needs attribution, give the exact credit line for the post.
- **Scene changes land on beats.** Know the track's tempo and first-beat offset (measure them; don't trust a stated BPM alone). One beat lasts `30 × 60 / BPM` frames. Every transition starts on a beat (within 1 frame), and the hook text lands on the first beat. Choose scene and transition lengths in whole beats and still meet the timing rules above.
- **End cleanly.** The music fades out over at least 1 s at the end of the last scene. No cut-off mid-note.

## Final checklist

- [ ] Idea, headline and call to action match what the requester approved
- [ ] Every scene carries one idea only
- [ ] Only kit colours are used
- [ ] All text is Inter Tight, weight 400 to 800
- [ ] Logo is unchanged (not recoloured, stretched or cropped) and sits only on light backgrounds
- [ ] Gradient uses light blue, pink and lavender
- [ ] Hook: motion from frame 0 and the core message fully visible within 1 s
- [ ] No sudden transitions: every fade lasts at least 1 s (1.5 s between dark and light), every transition is eased, and none flashes
- [ ] At least 2 real transitions (slide, wipe or scale), each between scenes of the same brightness but with clearly different backgrounds, visible on the mid-transition still
- [ ] Music has a clear beat, every transition starts on a beat, and the track fades out at the end
- [ ] Music licence allows commercial use; source and licence are noted in the hand-over and `CREDITS.md`
- [ ] No two scenes' texts are on screen at the same time (check the stills taken at the middle of each transition)
- [ ] Every text stays fully visible for at least 1.5 s
- [ ] Pace and wording feel calm and confident
- [ ] Call to action is clear on the last scene

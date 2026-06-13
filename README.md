# Raunak Srivastava · The Signal

An immersive, scroll driven portfolio. One continuous WebGL world sits behind the page; the visitor is a signal traveling through the machine of a career. Five chapters, one corridor, zero page loads.

The previous portfolio is preserved untouched at `/classic.html`.

## The story architecture

The visitor is the signal. Scroll is the only timeline. The camera travels a single z corridor from `z = 6` to `z = -258`, and each chapter is a physical place along it.

| Chapter | Scene | What happens |
| --- | --- | --- |
| CH.01 THE SIGNAL | An ember field of unformed ideas, three orbiting thought orbs | Identity, tagline, and the three service currents |
| CH.02 THE CIRCUIT | A manhattan style 3D circuit trace energizes station by station | Seven career stations light up as their cards cross midscreen |
| CH.03 THE PAYLOAD | Wireframe monoliths over a shader grid, instanced equalizer bars | Six headline metrics count up; ten case files open as dossiers |
| CH.04 THE ENGINE | A gyroscopic four ring constellation around a core | Hovering or scrolling a skill group heats its matching ring |
| ECHOES | A quiet interlude in the corridor | Two testimonials presented as intercepted transmissions |
| CH.05 THE HORIZON | A rising signal sun with scanline bands and ascending sparks | The ask, contact actions, live status readout |

A fixed circuit rail on the right edge mirrors the journey with a traveling pulse. On mobile it collapses to a 2px progress beam.

## Stack

- Vite + React 19 + TypeScript, strict mode
- three.js (vanilla, no R3F) loaded by dynamic import after first paint
- No animation libraries: native scroll, rAF easing, IntersectionObserver, CSS transitions

## Performance posture

- One renderer, no postprocessing, no lights: additive sprites, instancing, and two small custom shaders carry all the glow
- Device pixel ratio capped at 1.75 (1.4 on low tier devices), quality heuristic by screen width, device memory, and core count
- Chapter groups are visibility culled by camera depth; offscreen worlds cost nothing
- The render loop pauses when the tab is hidden
- `prefers-reduced-motion` renders a single static frame and switches all reveals to fade only

## Run it

```bash
npm install
npm run dev        # http://localhost:3000
npm run build      # production build to dist/
npm run preview    # serve the production build
npm run typecheck  # strict TS pass
```

## Deploy

The build is fully static. Point Vercel, Netlify, or GitHub Pages at the repo with build command `npm run build` and output directory `dist`. No environment variables, no server.

## Editing the content

Everything a recruiter or client reads lives in one file: `src/content/story.ts`. Chapters, currents, stations, metrics, case files, skill rings, foundations, echoes, and the horizon block are plain typed arrays. Edit the words there and the world reflows around them.

If you change the pacing (section heights in `src/styles/global.css`), keep the `ZONE` table at the top of `src/three/World.ts` roughly in sync so scenes appear behind their chapters.

## Structure

```
index.html                  Font loading, meta, first paint shell
public/classic.html         The original portfolio, untouched
src/
  main.tsx                  Entry
  App.tsx                   World lifecycle, scroll binding, chrome
  content/story.ts          Single source of truth for all copy
  lib/hooks.ts              Scroll progress, reveals, active section, quality
  three/World.ts            The continuous WebGL corridor (5 scenes)
  styles/global.css         Design system and chapter layouts
  components/
    Boot.tsx                Boot sequence preloader
    TopBar.tsx              Monogram, index, CV, contact
    RailNav.tsx             Circuit progress rail
    IndexOverlay.tsx        Chapter map (Esc to close)
    Ch1.tsx ... Ch5.tsx     The five chapters
    Echoes.tsx              Testimonial interlude
    CaseDossier.tsx         Case file overlay (dialog, Esc, scroll lock)
```

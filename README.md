# @seansleblanc/pink-trombone

headless port of [Neil Thapen's Pink Trombone](https://dood.al/pinktrombone/)

```sh
npm i @seansleblanc/pink-trombone
```

```js
import {
	Glottis,
	Tract,
} from './pink-trombone';

Glottis.isTouched = true;
```

## notes

- `alwaysVoice` and `autoWobble` from the original are disabled, and not exposed
  - `alwaysVoice` is not needed, since you can set `Glottis.isTouched = true` and never turn it off to get the same behaviour
  - `autoWobble` is relatively simple to reproduce: it's just noise applied to the vibrato
- the interfaces are currently singletons and are initialized on import
- if running in chrome, autoplay restrictions will prevent the `AudioContext` from playing sound immediately; clicking or pressing a key will unmute it
- i'm no expert on linguistics/vocalization and the original source is not documented in much detail, so take my explanations of the api with a grain of salt

## api

- `Glottis.isTouched`: whether it's speaking
  - just `true` or `false`
- `Glottis.UIFrequency`: pitch
  - usually want something in the triple digits
  - <50 is barely audible
  - \>2000 is a screech and has audible artifacting
- `Glottis.UITenseness`: breathiness
  - usually want something around 0.5
  - 0 is more breathy
  - 1 is sharp and a bit robotic
  - \>1 kinda does both
  - negative breaks it
- `Glottis.vibratoAmount`:
  - usually want 0 - 0.1 range
  - 0 is none
  - 1 is "full" range
  - \>1 is broken cheeping
  - negative is the same
- `Tract.velumTarget`: back roof of the mouth; controls how nasal things sound
  - 0 is closed
  - 1 is very nasal
  - \>1 is even more nasal
  - negative is the same
- `Tract.diameter`: shape of the tract, represented by a 44-index array from throat to lips
  - standard range of each point is roughly 0-3, with the throat being smaller
  - the original UI modifies the tract shape with radial deformers to simulate a tongue
- `Tract.targetDiameter`: interpolation target for `Tract.diameter`; usually you want to set this instead of modifying `Tract.diamater` directly
- `Tract.restDiameter`: initial shape of the tract; useful for resetting
- `Tract.noseDiameter`: shape of the nose, represented by a 28-index array from velum to nostril
  - standard range is roughly 0-2, with edges being smaller

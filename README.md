# Black Hole Web Simulation

This project is an interactive 3D visualization of a spinning stellar-mass black hole rendered in the browser with WebGL using Three.js. It features a dynamic accretion disk, relativistic jets, starfield and nebula background, and a custom post-processing shader that approximates gravitational lensing around the black hole.

## Features

- 3D black hole core with event horizon glow and photon sphere
- High-density starfield with twinkling shader and distant nebula clouds
- Particle-based accretion disk with color/brightness tied to approximate temperature
- Relativistic-style bipolar jets with animated shaders and supporting lights
- Infalling particle system approximating accretion dynamics and tidal effects
- Multiple gravitational-lensing effects:
  - Concentric lensing rings around the hole
  - Full-screen lens distortion pass using a custom fragment shader
  - Simple chromatic aberration near the center
- Real-time adjustment of:
  - Black hole mass (affects Schwarzschild radius and Hawking temperature display)
  - Accretion rate (affects inflow strength and particle behavior)
  - Rotation speed of the disk
- On-screen, auto-updated physical readouts:
  - Mass (in solar masses)
  - Schwarzschild radius (km)
  - Hawking temperature (nK)

The entire experience runs client-side in a single HTML file with an embedded script, using a CDN version of Three.js. fileciteturn1file0

## File structure

- `blackhole.html` – Main entry point containing:
  - HTML UI (`#info`, `#controls`, sliders, buttons)
  - Embedded styles for full-screen canvas and neon HUD look
  - Embedded JavaScript (Three.js setup, shaders, animation loop, lensing pass)

No extra build tooling is required. You only need this one HTML file and network access to the Three.js CDN.

## Requirements

- A modern desktop browser with WebGL support (Chrome, Edge, Firefox, or similar)
- Internet access to load the Three.js library from the CDN

If you want to run it fully offline, download the same Three.js version used in the `<script>` tag and serve it locally, then update the script source path in the HTML.

## Running the simulation

### Quick preview (local file)

1. Save `blackhole.html` to a folder on your machine.
2. Open it in a modern browser by double-clicking it or dragging it into a browser window.

Some browsers restrict local file access for WebGL scripts; if you run into security issues, use a simple static server.

### Recommended: static server

From the folder containing `blackhole.html`, run one of:

```bash
# Python 3
python -m http.server 8000

# Node.js (if you have serve installed)
npx serve .
```

Then open:

```text
http://localhost:8000/blackhole.html
```

## Controls

On-screen info panel explains this too:

- Mouse drag – Rotate the entire scene (orbit around the black hole)
- Mouse scroll – Zoom in/out (changes camera distance)
- Spacebar – Toggle jets visibility
- Mass slider – Adjust black hole mass (M☉), updates physical readouts and dynamics
- Accretion slider – Adjust accretion rate (%), controls particle inflow strength
- Rotation slider – Adjust disk rotation speed multiplier
- “Toggle Jets” button – Show/hide the relativistic jets
- “Toggle Lensing” button – Show/hide the ring-style lensing effect

## Implementation details

Main components inside the script:

- **Scene, camera, renderer**
  - Perspective camera with adjustable zoom
  - WebGLRenderer with antialiasing

- **Starfield and nebula**
  - Point cloud geometries
  - Custom ShaderMaterial for twinkling stars
  - Additive blending and depthWrite disabled for a soft glow effect

- **Black hole core**
  - Central dark sphere
  - Event horizon shell using a ShaderMaterial with fresnel-style glow
  - Photon sphere shell with pulsing highlight

- **Accretion disk**
  - Point-based disk around the black hole
  - Per-particle color based on radius (hot inner region, cooler outer region)
  - Animated rotation controlled by the rotation slider

- **Jets**
  - Cylindrical “cone” meshes and inner cores with animated shaders
  - Additional point lights at jet tips for visual impact

- **Infalling particles (accretion stream)**
  - BufferGeometry with attributes for position, velocity, age, lifetime
  - Per-frame update:
    - Radial gravitational pull scaled by mass and accretion rate
    - Tangential (orbital) velocity for approximately Keplerian orbits
    - Simple drag and stochastic vertical “tidal” components
    - Respawn logic if particles are captured or age out

- **Gravitational lensing pass**
  - Renders the scene to a WebGLRenderTarget
  - Custom full-screen quad shader uses black hole screen-space position
  - Distorts UVs around the black hole
  - Adds chromatic aberration close to the center

- **Physics readouts**
  - Uses standard formulae for:
    - Schwarzschild radius: \( r_s = \frac{2GM}{c^2} \)
    - Hawking temperature (approximate): proportional to \( 1 / M \)

## Customization ideas

You can extend or tweak this project by:

- Changing star and nebula density or colors
- Adjusting disk size, thickness, and particle count
- Modifying lensing radius and strength for more dramatic visuals
- Adding keyboard shortcuts for lens strength presets
- Wiring sliders to jet intensity, disk color, or background nebula
- Hooking up a GUI (e.g., dat.GUI or modern equivalent) instead of DOM sliders

## Contributing

If you improve the visual fidelity, add educational overlays, or optimize the shaders, keep these in mind:

- Keep everything in a single HTML file (unless you intentionally modularize)
- Avoid breaking WebGL support on mid-range hardware
- Make added features togglable so people can dial the complexity up/down

## License

This project is licensed under the MIT License. See [`LICENSE`](./LICENSE) for details.

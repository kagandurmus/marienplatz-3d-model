<!-- Created by kagandurmus (https://github.com/kagandurmus/3d-models). Please leave attribution if reused. -->
# ROLE & OBJECTIVE
You are a Senior WebGL & Three.js Developer. Your task is to build a modular, single-page HTML/JS 3D web application showcasing the **Neues Rathaus (New Town Hall) at Marienplatz in Munich, Germany** from an interactive **drone-view perspective**. 

Because the Neues Rathaus is an intricate Neo-Gothic building, DO NOT attempt to model the final high-detail gargoyles or statues using primitive code shapes. Instead, you will build a dual-mode 3D Viewer Pipeline that:

# STRICT RULES FOR YOUR WORKFLOW (READ CAREFULLY)
1. **DO NOT USE BROKEN EXTERNAL URLs:** Never hardcode guessed URLs for `.glb` models or `.jpg` textures from the internet—they will fail with 404 or CORS errors. Use procedural Canvas textures or reliable public CDNs for libraries (`unpkg` or `esm.sh`).
2. **MODULAR ITERATION:** Do not write sloppy, incomplete code. Write clean, fully functional ES6 Module code inside a single `index.html` file (or separated neatly into `index.html`, `style.css`, and `app.js` if requested).
3. **FOLLOW THE 5 PHASES SEQUENTIALLY:** Complete every requirement in the 5 architectural phases below.

---

# TECHNICAL STACK
- **Core:** HTML5, CSS3, Vanilla JavaScript (ES6 Modules via `<script type="importmap">`).
- **3D Engine:** Three.js (`https://unpkg.com/three@0.160.0/build/three.module.js`).
- **Addons:** `OrbitControls`, `GLTFLoader`, `DRACOLoader`, `Sky` shader (`three/addons/...`).

---

# STEP-BY-STEP IMPLEMENTATION PHASES

## PHASE 1: Boilerplate & Scene Environment
1. Create a full-viewport canvas (`100vw`, `100vh`, `overflow: hidden`) with a sleek, semi-transparent glassmorphism HUD overlay.
2. Initialize a `THREE.WebGLRenderer` with `antialias: true`, `powerPreference: "high-performance"`, `shadowMap.enabled = true` (`PCFSoftShadowMap`), and `ACESFilmicToneMapping`.
3. Create a realistic Munich sky and lighting setup:
   - Add a `DirectionalLight` representing the sun with shadow casting configured (`shadow.mapSize` at `2048x2048`, bias tuned to prevent shadow acne).
   - Add an `AmbientLight` and `HemisphereLight` for soft architectural fill lighting.
   - Add a subtle exponential fog (`THREE.FogExp2`) to blend the distant horizon.

## PHASE 2: Drone Camera & Flight Controller
1. Initialize a `PerspectiveCamera` (`fov: 45`, `near: 0.5`, `far: 2000`).
2. Set the initial **Drone Spawn Position** looking down at Marienplatz from the south-west at an elevated angle: `camera.position.set(-60, 75, 95)` looking at `0, 20, 0`.
3. Configure `OrbitControls` to feel like a stabilized camera drone:
   - `enableDamping = true`, `dampingFactor = 0.05`.
   - `maxPolarAngle = Math.PI / 2 - 0.03` (prevent drone from clipping underground).
   - `minDistance = 15`, `maxDistance = 300`.
4. Implement **WASD / Arrow Key Drone Flight** in the animation loop so the user can fly horizontally and adjust altitude (`Q`/`E` or `Space`/`Shift`), plus an **"Auto-Orbit Drone Tour"** toggle button that smoothly circles the Glockenspiel tower.

## PHASE 3: Architectural Blockout & Procedural Detailing (Neues Rathaus)
Build a detailed, properly proportioned procedural model (`THREE.Group`) of the Neues Rathaus and Marienplatz so the scene is visually impressive before external `.glb` files are loaded:
1. **Marienplatz Square Ground:** Paved square grid with procedural cobblestone/paving texture generated via HTML5 `<canvas>`, plus the **Mariensäule** (Mary's Column) in the center foreground and the **Fischbrunnen** (Fish Fountain) to the east.
2. **Main Facade & Wings:** 
   - Create the roughly 100m-wide Neo-Gothic main facade facing south toward the square, plus the surrounding wings enclosing the internal courtyards (Innenhöfe).
   - Use procedural Canvas bump/color maps to simulate Gothic pointed-arch windows, tracery, limestone blocks, and steep copper/terracotta gabled roofs.
3. **The 85m Rathausturm (Main Tower) & Glockenspiel:**
   - Construct the central tiered Neo-Gothic tower rising above the main facade, complete with spire, corner turrets, clock faces, and the two-tier **Glockenspiel bay** (featuring small colorful rotating procedural figurines).

## PHASE 4: Photo-Based High-Detail Asset Loader & Hotspots
Build the bridge between internet photos and the 3D scene:
1. **Drag-and-Drop / URL `.GLB` & Photogrammetry Loader:**
   - Configure `GLTFLoader` (with `DRACOLoader`).
   - Add a UI button & drag-and-drop zone allowing the user to drop a `.glb` / `.gltf` photogrammetry model of the Neues Rathaus (or input a Google Map Tiles API Key) which automatically replaces the procedural blockout mesh.
2. **3D Photo-Detail Hotspots (Raycasting + HTML Markers):**
   - Place interactive 3D floating pins on key architectural features:
     - *Pin 1: The Glockenspiel (43 bells & 32 life-sized figures)*
     - *Pin 2: The Münchner Kindl (statue at the top of the 85m tower)*
     - *Pin 3: The Prunkhof (Grand Inner Courtyard)*
     - *Pin 4: The Mariensäule (1638 Marian Column)*
   - Clicking a pin smoothly animates the drone camera to that exact feature (`lerp` camera position and target) and opens a detail card where high-resolution reference photos can be displayed.

## PHASE 5: Drone HUD UI & Controls
Create a clean UI overlay in the top-right and bottom-left corners containing:
- **Camera Preset Buttons:** "High Drone Overview", "Glockenspiel Close-Up", "Courtyard Top-Down", "Street Level".
- **Time-of-Day Slider:** Adjusts sun angle and sky color from Dawn to Golden Hour to Night (turning on warm window/facade uplights on the Town Hall at night).
- **Model Mode Switcher:** Toggle between "Procedural Neo-Gothic Model", "Wireframe Inspection", and "Upload Custom .GLB Photogrammetry".
- **Drone Telemetry HUD:** Real-time readout of virtual Altitude (m), Pitch, and Coordinates (`48.1376° N, 11.5755° E`).

---

# OUTPUT REQUIREMENT
Deliver the complete, runnable, self-contained code in a single `index.html` file first. Do not use placeholders like `// TODO: add building here`. Write out the complete geometry generation functions, procedural canvas texture generators, camera flight math, and UI event listeners.
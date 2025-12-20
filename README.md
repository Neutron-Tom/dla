# Diffusion Limited Aggregation (DLA) Simulator

An interactive web-based simulation of Diffusion Limited Aggregation, demonstrating the physics of particle aggregation and fractal formation through Brownian motion.

**Live Demo**: [https://neutron-tom.github.io/dla/](https://neutron-tom.github.io/dla/)

![DLA Simulation](https://img.shields.io/badge/Physics-Simulation-blue) ![HTML5](https://img.shields.io/badge/HTML5-Canvas-orange) ![Vanilla JS](https://img.shields.io/badge/JavaScript-Vanilla-yellow)

## Overview

This simulation visualizes how particles undergoing random Brownian motion aggregate when they encounter boundaries, forming fractal-like structures. Users can draw custom boundaries and watch as particles spawn from the center, diffuse through space, and stick to create complex patterns.

## Features

- **Interactive Boundary Drawing**: Click and drag to draw freehand boundaries
- **Dual Spawn Modes**: Center spawn with clogging detection, or edge spawn with periodic boundaries
- **Fractal Dimension Analysis**: Built-in box-counting algorithm with interactive visualization
- **Brownian Motion Physics**: Authentic random walk particle movement
- **Pause/Resume Control**: Freeze and inspect the simulation at any time
- **Responsive Design**: Works on desktop and mobile devices
- **Iframe-Ready**: Optimized for embedding in blog posts and portfolios
- **Adjustable Parameters**:
  - Particle speed (0.5x to 10x)
  - Exponential spawn rate (1-1000 particles/sec)
  - Performance limits for smooth operation

## Physics Background

### Diffusion Limited Aggregation (DLA)

DLA is a process in which particles undergoing random Brownian motion cluster together to form aggregate structures. First introduced by Witten and Sander in 1981, DLA has been used to model various natural phenomena:

- **Crystal growth**
- **Coral formation**
- **Lightning patterns**
- **Bacterial colonies**
- **Mineral deposits**
- **Snowflake formation**

### Brownian Motion

Named after botanist Robert Brown, Brownian motion describes the random movement of particles suspended in a fluid (liquid or gas) resulting from collisions with fast-moving molecules.

In this simulation, Brownian motion is implemented as a **random walk**:
- Each particle moves in a random direction at each time step
- The direction is uniformly distributed (0 to 360 degrees)
- The step size is controlled by the speed parameter

**Mathematical representation**:
```
x(t+1) = x(t) + v * cos(θ)
y(t+1) = y(t) + v * sin(θ)

where:
- v = particle speed
- θ = random angle ∈ [0, 2π]
```

### Aggregation Mechanism

When a diffusing particle comes within a critical distance of an existing boundary or stuck particle:

1. **Collision Detection**: The particle checks distances to all boundary points and stuck particles
2. **Sticking**: If distance < collision radius, the particle "sticks"
3. **Boundary Growth**: The stuck particle becomes part of the aggregate boundary
4. **Fractal Formation**: Over time, the aggregate develops a fractal dimension typically between 1.7-1.8

## How to Use

### Basic Interaction

1. **Draw Boundaries**:
   - Click and drag on the black canvas to draw boundaries
   - Particles will stick to these boundaries when they collide
   - You can add more boundaries at any time during the simulation

2. **Observe Particle Behavior**:
   - Yellow particles spawn from the center (white dot)
   - They move randomly via Brownian motion
   - When particles hit boundaries, they turn pink and stick
   - Stuck particles become part of the boundary for future collisions

3. **Adjust Parameters**:
   - **Spawn Mode**: Toggle between center and edge spawning
   - **Particle Speed**: Controls how fast particles move (0.5x to 10x)
   - **Spawn Rate**: Exponential scale from 1 to 1000 particles/sec
   - **Pause**: Freeze the simulation to inspect patterns

4. **Calculate Fractal Dimension**: Analyzes stuck particles using box-counting with interactive box visualization

5. **Reset**: Click the "Reset" button to clear all boundaries and particles

### Tips for Interesting Patterns

- **Circular Boundaries**: Draw circular shapes to create radial fractal patterns
- **Linear Boundaries**: Draw straight lines to observe edge growth
- **Multiple Centers**: Draw several separate boundary regions to see competing aggregation
- **Slow Growth**: Lower spawn rate + lower speed = more detailed fractal structure
- **Rapid Growth**: Higher spawn rate + higher speed = faster but more chaotic patterns

## Technical Implementation

### Architecture

The simulation is built with vanilla JavaScript and HTML5 Canvas, with no external dependencies:

- **Single HTML File**: All code (HTML, CSS, JavaScript) in one file
- **Canvas Rendering**: Hardware-accelerated 2D graphics
- **RequestAnimationFrame**: Smooth 60 FPS animation loop
- **Responsive Layout**: CSS Flexbox with viewport units
- **Touch Support**: Full mobile and tablet compatibility

### Key Components

#### 1. Particle System
```javascript
class Particle {
    constructor(x, y)      // Initialize at spawn point
    update()               // Apply Brownian motion
    draw()                 // Render on canvas
    isOffScreen()          // Check boundary escape
    checkCollision()       // Detect boundary/aggregate collision
}
```

#### 2. Boundary Management
- **Data Structure**: `Set` of coordinate strings for O(1) lookup
- **Format**: `"x,y"` keys for efficient collision detection
- **Persistence**: User-drawn boundaries remain distinct from aggregated particles

#### 3. Collision Detection
- **Boundary Check**: Iterate through boundary points, calculate Euclidean distance
- **Aggregate Check**: Check distance to all stuck particles
- **Threshold**: Collision occurs when distance < 5 pixels

#### 4. Animation Loop
```javascript
function animate(timestamp) {
    1. Clear canvas
    2. Spawn new particles (based on spawn rate)
    3. Update all particles (Brownian motion)
    4. Check collisions and remove escaped particles
    5. Render: boundaries → stuck particles → active particles
    6. Update UI statistics
    7. Request next frame
}
```

### Color Scheme

The simulation uses a harmonious color palette designed for visual clarity:

| Element | Color | Hex Code | Purpose |
|---------|-------|----------|---------|
| Background | Deep Blue | `#0a0e27` | High contrast, reduces eye strain |
| Boundary | Cyan/Teal | `#4dd0e1` | User-drawn structures |
| Active Particles | Warm Yellow | `#ffd93d` | Particles in motion |
| Stuck Particles | Coral Pink | `#ff6b9d` | Aggregated structure |

### Performance Optimizations

1. **Efficient Collision Detection**: Uses Set for O(1) boundary lookups
2. **Particle Culling**: Removes off-screen particles immediately
3. **Canvas Clearing**: Full repaint each frame (faster than partial updates for this use case)
4. **Integer Coordinates**: Boundary points rounded to integers for memory efficiency
5. **Shadow Caching**: Shadow blur applied strategically to minimize GPU overhead

## Embedding in an Iframe

This simulation is optimized for iframe embedding:

```html
<iframe
    src="https://neutron-tom.github.io/dla/"
    width="100%"
    height="600px"
    frameborder="0"
    title="DLA Simulation">
</iframe>
```

### Iframe Compatibility Features

- ✅ No popups (alert/confirm/prompt)
- ✅ No external navigation
- ✅ Touch-friendly controls
- ✅ Responsive width (works at any width)
- ✅ Self-contained (no external resources except inline)
- ✅ No scrollbars (uses overflow: hidden)

## Browser Compatibility

- ✅ Chrome/Edge (v90+)
- ✅ Firefox (v88+)
- ✅ Safari (v14+)
- ✅ Mobile Safari (iOS 14+)
- ✅ Chrome Mobile (Android 5+)

Requires: HTML5 Canvas, ES6 JavaScript

## File Structure

```
dla/
├── index.html          # Main simulation (self-contained)
├── README.md           # This file
└── .gitignore         # Git ignore rules
```

## Development

This is a static site with no build process. To develop locally:

1. Clone the repository:
```bash
git clone https://github.com/Neutron-Tom/dla.git
cd dla
```

2. Open `index.html` in a browser:
```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

Or use a local server:
```bash
# Python 3
python -m http.server 8000

# Node.js (with http-server)
npx http-server
```

3. Make changes and refresh your browser

## Deployment

The site is automatically deployed to GitHub Pages from the `main` branch:

1. Push changes to `main`
2. GitHub Pages automatically serves from the repository root
3. Site is live at: https://neutron-tom.github.io/dla/

No build process or GitHub Actions required.

## Physics Extensions

Potential enhancements for deeper physics simulation:

1. **Electric Fields**: Add attractive/repulsive forces to particles
2. **Particle Charge**: Different particle types with varying interaction strengths
3. **Diffusion Coefficient**: Temperature-dependent particle speed
4. **Cluster-Cluster Aggregation**: Allow multiple growth centers that can merge
5. **3D Visualization**: Extend to three-dimensional DLA using WebGL

## Mathematical Details

### Random Walk Statistics

For a 2D random walk with step size `v`:
- **Mean displacement**: `<r> = 0` (symmetric random walk)
- **RMS displacement**: `<r²> = v² * t`
- **Diffusion coefficient**: `D = v² / 4`

### Fractal Dimension

DLA structures typically exhibit fractal behavior with dimension `d ≈ 1.71` in 2D:

```
N(r) ∝ r^d

where:
- N(r) = number of particles within radius r
- d = fractal dimension
```

This is less than the Euclidean dimension (2), indicating a sparse, branching structure.

### Sticking Probability

In this simulation, sticking probability is 100% when a particle enters the collision radius. More sophisticated models use:

```
P_stick = f(angle, velocity, surface_properties)
```

## References

1. Witten, T. A., & Sander, L. M. (1981). "Diffusion-limited aggregation, a kinetic critical phenomenon." *Physical Review Letters*, 47(19), 1400.

2. Meakin, P. (1983). "Formation of fractal clusters and networks by irreversible diffusion-limited aggregation." *Physical Review Letters*, 51(13), 1119.

3. Einstein, A. (1905). "Über die von der molekularkinetischen Theorie der Wärme geforderte Bewegung von in ruhenden Flüssigkeiten suspendierten Teilchen." *Annalen der Physik*, 322(8), 549-560.

## License

MIT License - feel free to use this simulation in your own projects, educational materials, or blog posts.

## Author

Created for RISCfolio physics demonstrations.

## Contributing

Contributions welcome! Please feel free to submit a Pull Request.

---

**Enjoy exploring the beautiful mathematics of diffusion and aggregation!**

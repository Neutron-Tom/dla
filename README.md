# Diffusion Limited Aggregation (DLA) Simulator

An interactive web-based simulation demonstrating fractal formation through Brownian motion and particle aggregation.

**Live Demo**: [https://neutron-tom.github.io/dla/](https://neutron-tom.github.io/dla/)

## Overview

This simulation visualizes particles undergoing random Brownian motion that aggregate when they encounter boundaries, forming fractal-like structures. Draw custom boundaries and watch as particles spawn, diffuse through space, and stick to create complex patterns.

## Physics Background

### Diffusion Limited Aggregation

DLA is a process where particles undergoing random Brownian motion cluster together to form aggregate structures. First introduced by Witten and Sander in 1981, DLA models various natural phenomena including crystal growth, coral formation, lightning patterns, and snowflake formation.

### Brownian Motion

Brownian motion is implemented as a random walk where each particle moves in a random direction at each time step:

```
x(t+1) = x(t) + v * cos(θ)
y(t+1) = y(t) + v * sin(θ)

where:
- v = particle speed
- θ = random angle ∈ [0, 2π]
```

### Aggregation

When a diffusing particle comes within the collision radius (5 pixels) of an existing boundary or stuck particle, it sticks and becomes part of the aggregate. DLA structures typically exhibit a fractal dimension of approximately 1.7 in 2D.

## How to Use

1. **Draw Boundaries**: Click and drag on the canvas to draw boundaries
2. **Spawn Modes**:
   - **Center**: Particles spawn from the center (white dot). System detects clogging and pauses spawning if the spawn point becomes blocked
   - **Edge**: Particles spawn near existing boundaries/stuck particles with periodic boundary conditions
3. **Adjust Parameters**:
   - **Particle Speed**: 0.5x to 10x
   - **Spawn Rate**: Exponential scale from 1 to 10,000 particles/sec (slider 0-100 maps to `10^(slider/25)`)
4. **Calculate Fractal Dimension**: Box-counting algorithm with interactive visualization and log-log regression plot
5. **Pause/Resume**: Freeze simulation to inspect patterns

## Tips for Interesting Patterns

- **Circular boundaries**: Create radial fractal patterns
- **Linear boundaries**: Observe edge growth
- **Multiple centers**: Draw separate regions for competing aggregation
- **Slow growth**: Lower spawn rate + lower speed = more detailed fractal structure
- **Rapid growth**: Higher spawn rate + higher speed = faster but more chaotic patterns

## Technical Implementation

### Architecture

Built with vanilla JavaScript and HTML5 Canvas (no dependencies):
- Single self-contained HTML file
- Canvas rendering with `requestAnimationFrame` (60 FPS)
- Full touch support for mobile/tablet

### Key Components

**Particle Class**:
```javascript
class Particle {
    constructor(x, y)      // Initialize at spawn point
    update()               // Brownian motion: random angle, move by particleSpeed
    isOffScreen()          // Boundary check (center mode only)
    checkCollision()       // Spatial hash collision detection
}
```

**Collision Detection**: Uses spatial hash grid (10px cells) for O(1) collision detection instead of checking all particles. Each particle only checks the 9 nearby grid cells.

**Spawn Rate**: Exponential mapping `rate = 10^(slider/25)` where slider 0→1/sec, 25→10/sec, 50→100/sec, 75→1000/sec, 100→10000/sec.

**Periodic Boundaries**: Edge spawn mode wraps particles using modulo arithmetic when they exit canvas bounds.

**Fractal Dimension**: Box-counting algorithm tests 15 different box scales (factor of 1.5 between scales) and performs linear regression on log-log plot to extract dimension as the slope.

**Performance Limits**:
- Max active particles: 10,000
- Max stuck particles: 50,000
- Spatial grid with 10px cells for efficient collision detection
- Batch rendering with shadow effects applied per group



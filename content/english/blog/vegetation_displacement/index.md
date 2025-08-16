---
title: "Scaling Up: Multi-Unit Vegetation Displacement in Grey"
meta_title: "Scaling Up: Multi-Unit Vegetation Displacement in Grey"
description: "Building on our previous vegetation system with high-performance interactive displacement"
date: 2025-08-16
image: "title.png"
categories: ["Development", "Game Design"]
author: "Diener Brothers"
tags: ["Grey", "Game Mechanics", "Vegetation"]
draft: false
---

### From Single Hand to Hundreds of Units

In our [previous post](https://www.dienerbrothers.de/blog/vegetation_system/), we showcased Grey's vegetation system that responds dynamically to the player's divine hand. Today, we're excited to share a major evolution: multi-unit vegetation displacement that allows hundreds of units to interact with grass and crops simultaneously while maintaining 60+ FPS performance.

{{< video src="2025-08-16_vegetation_displacement.mp4" type="video/webm" preload="auto" >}}


### The Challenge

Our original system worked beautifully for a single character displacing vegetation. But as Grey's world grew more complex with dozens of units working the fields, we faced a critical question: How do we scale vegetation displacement to support hundreds of units without destroying performance?

The naive approach of calculating displacement for each unit individually would have brought our game to its knees. We needed something smarter.

### The Solution: Texture-Based Position Storage

After researching GPU optimization techniques, we implemented an approach using texture-based position storage:

```glsl
// Store unit positions in RGB channels, alpha indicates active units
uniform sampler2D unit_positions_texture;
uniform int unit_positions_count;
uniform ivec2 unit_positions_size = ivec2(64, 64);
```

Instead of passing individual unit positions to the shader, we pack all unit positions into a single 64x64 texture that can store up to 4,096 active units. The GPU processes this data in parallel, making vegetation displacement scale almost linearly.

### Smart Culling for Massive Worlds

To handle even larger worlds, we implemented spatial culling that only processes units within camera range:

```csharp
var nearbyUnits = _displacers
    .Where(unit => unit.GlobalPosition.DistanceTo(cameraPos) <= CullingDistance)
    .OrderBy(unit => unit.GlobalPosition.DistanceSquaredTo(cameraPos))
    .Take(MaxActiveUnits)
    .ToList();
```

This ensures we're always working with the most relevant units while maintaining consistent performance regardless of total world population.

### Preventing Displacement Stacking

One challenge we discovered was that multiple units in close proximity would create unrealistic "super-displacement" effects. We solved this with a weighted average approach:

```glsl
// Weighted average prevents displacement stacking
float influence = falloff * unit_displacement_strength;
combined_displacement = (combined_displacement * total_influence + displacement_dir * influence) / (total_influence + influence);
total_influence += influence;
```

Now vegetation bends naturally away from groups of units rather than creating extreme displacement effects.

### Architecture: Signal-Based Integration

The system integrates seamlessly with our existing codebase through Godot's signal system:

-   **VegetationDisplacementManager**: Centralized singleton managing texture updates
-   **Unit Registration**: Automatic registration/cleanup as units spawn/despawn
-   **Hand Integration**: Maintains backward compatibility with our divine hand system
-   **Material Updates**: Real-time shader parameter updates across all vegetation materials

### Performance Results

The results speak for themselves:

-   ✅ 4,096 units: Supported with spatial culling
-   ✅ 60+ FPS: Maintained even with hundreds of active units
-   ✅ Real-time updates: Smooth vegetation response as units move
-   ✅ Memory efficient: Single texture vs. hundreds of individual parameters

### Looking Forward

This foundation opens exciting possibilities for Grey's future:

-   **Material Interaction**: Different unit types could affect vegetation differently
-   **Dynamic Weather**: Rain and wind could modify displacement behavior further
-   **Player Feedback**: Visual vegetation trails showing unit movement history

---

Stay tuned, and remember to [wishlist Grey on Steam](https://store.steampowered.com/) for the latest updates!

*David & Paul*

{{< x user="dienerbrothers" id="1956834008391672048" >}}
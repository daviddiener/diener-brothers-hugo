---
title: "Performance Challenge: Scaling Vegetation Displacement in Grey"
meta_title: "Performance Challenge: Scaling Vegetation Displacement in Grey"
description: "How we solved Grey's biggest performance challenge to support hundreds of units displacing vegetation"
date: 2025-08-16
image: "title.png"
categories: ["Development", "Game Design"]
author: "Diener Brothers"
tags: ["Grey", "Game Mechanics", "Vegetation"]
draft: false
---

### The Challenge We Didn't See Coming

In our [previous post](https://www.dienerbrothers.de/blog/vegetation_system/), we showcased Grey's vegetation system that responds beautifully to the player's divine hand. But success brought an unexpected challenge: what happens when you have 200 workers all displacing vegetation at once?

The answer? 3 FPS and a serious reality check.

### The Performance Wall

Our original displacement system was elegant in its simplicity - each blade of grass calculated its distance to the divine hand and bent accordingly. Perfect for a single point of interaction, catastrophic for a bustling civilization.

The math told the brutal story: 200 units × 10,000 grass blades × 60 FPS = 120 million calculations per second. We'd hit the performance wall hard, and something had to give.

But here's the thing about game development - constraints often lead to the most creative solutions.

### The Breakthrough: Think Like a GPU

The solution came from rethinking the problem entirely. Instead of treating each unit as an individual calculation burden, what if we could batch them together? Enter texture-based position storage - a technique that turns our 200-unit nightmare into a GPU-friendly feast.

```glsl
// Store unit positions in RGB channels, alpha indicates active units
uniform sampler2D unit_positions_texture;
uniform int unit_positions_count;
uniform ivec2 unit_positions_size = ivec2(64, 64);
```

Here's the magic: instead of sending 200 individual unit positions to our shader, we pack them all into a single 64x64 texture. This texture can hold up to 4,096 unit positions, and the GPU processes them all in parallel. Suddenly, our performance scales almost linearly instead of exponentially.

{{< video src="2025-08-16_vegetation_displacement.mp4" type="video/webm" preload="auto" >}}

It's like the difference between having 200 people knock on your door one by one, versus having them all stand in an organized grid where you can see everyone at once.

### Why Process What Players Can't See?

But we didn't stop there. Even with our texture optimization, why process displacement for units halfway across the map that players can't even see? Enter spatial culling - we only process units within camera range:

```csharp
var nearbyUnits = _displacers
    .Where(unit => unit.GlobalPosition.DistanceTo(cameraPos) <= CullingDistance)
    .OrderBy(unit => unit.GlobalPosition.DistanceSquaredTo(cameraPos))
    .Take(MaxActiveUnits)
    .ToList();
```

This smart filtering ensures we're always working with the most relevant units. Whether you have 100 or 10,000 units in your world, performance stays consistent because we only care about what matters: the units you're actually looking at.

### The "Super-Displacement" Problem

Early testing revealed an unexpected issue: when multiple units clustered together, the vegetation would bend so dramatically it looked like a hurricane had hit. We dubbed this "super-displacement," and it was breaking immersion fast.

The fix? A weighted average approach that makes displacement feel natural:

```glsl
// Weighted average prevents displacement stacking
float influence = falloff * unit_displacement_strength;
combined_displacement = (combined_displacement * total_influence + displacement_dir * influence) / (total_influence + influence);
total_influence += influence;
```

Now when a group of farmers work the same field, the wheat bends naturally around them instead of creating dramatic cartoon effects. It's these subtle details that make Grey's world feel believable.

### Building for the Future

The entire system integrates cleanly with our existing architecture through Godot's signal system:

-   **VegetationDisplacementManager**: Centralized singleton handling all texture updates
-   **Unit Registration**: Automatic registration and cleanup as units spawn and despawn
-   **Divine Hand Compatibility**: Works alongside our existing divine hand system
-   **Material Updates**: Real-time shader parameter updates across all vegetation materials

This modular approach means we can easily extend the system as Grey evolves.

### The Numbers Don't Lie

From 3 FPS disaster to smooth performance:

-   ✅ **4,096 units supported** with spatial culling active
-   ✅ **60+ FPS maintained** even with hundreds of units moving
-   ✅ **Real-time responsiveness** - vegetation reacts instantly to unit movement
-   ✅ **Memory efficient** - single texture replaces hundreds of individual parameters

### What This Means for Grey

This breakthrough unlocks some exciting possibilities we're already exploring:

-   **Unit-Specific Effects**: Different unit types affecting vegetation differently (heavy armor vs. light scouts)
-   **Environmental Storytelling**: Vegetation trails that show where your civilization has been active
-   **Dynamic Weather Integration**: Rain and wind modifying displacement behavior
-   **Player Strategy**: Using vegetation displacement as visual feedback for unit positioning

Performance optimization isn't just about making things run faster - it's about making ambitious gameplay possible. Now we can focus on what really matters: creating those divine moments that make Grey special.

---

The journey from 3 FPS catastrophe to smooth 60+ FPS performance taught us that the best solutions often come from changing perspective entirely. Sometimes you need to think like a GPU to solve a CPU problem.

Stay tuned for more development updates, and don't forget to [wishlist Grey on Steam](https://store.steampowered.com/) to follow our progress!

*The Diener Brothers*

{{< x user="dienerbrothers" id="1956834008391672048" >}}
---
title: "Building a High-Performance Vegetation System"
meta_title: "Vegetation System in Grey"
description: "Deep dive into creating a unified vegetation system supporting both organic grass generation and structured crop fields, with real-time building integration and performance optimization."
date: 2025-08-08T14:30:00Z
image: "title.png"
categories: ["Development", "Game Design"]
author: "Diener Brothers"
tags: ["Grey", "Game Mechanics", "Vegetation"]
draft: false
---

One of the most immersive aspects of world-building games is watching a living, breathing environment respond to player actions. In our god simulation RTS *Grey*, we needed vegetation that could handle both the wild, organic growth of grasslands and the structured precision of agricultural fields. Here's how we built a unified system that delivers both.

## The Challenge

We faced several technical requirements:
- Dual aesthetics: Natural grass vs. organized crop rows
- Building integration: Vegetation should respond to structures in real-time
- Performance: Thousands of instances without frame drops
- Designer-friendly: Immediate visual feedback in the editor

## Architecture: Three-Layer Approach

Our solution uses a hierarchical architecture:

VegetationManager → VegetationZone → VegetationChunk

- VegetationManager: Central coordinator handling LOD, culling, and building exclusion
- VegetationZone: Resource-based configuration defining vegetation properties
- VegetationChunk: Performance units containing MultiMeshInstance3D for rendering

This separation allows different vegetation types to coexist while sharing performance optimizations.

## Organic Grass Generation

For grass, we use FastNoiseLite with dual sampling:

```C#
// Base vegetation pattern
float vegetationNoise = _noise.GetNoise2D(worldPos.X, worldPos.Z);

// Organic clearings at half frequency
float holeNoise = _noise.GetNoise2D(worldPos.X * 0.5f, worldPos.Z * 0.5f);

// Combine for natural-looking distribution
float threshold = (1.0f - density) / CalculateHoleFactor(holeNoise);
return vegetationNoise >= threshold;
```
This creates natural-looking grass coverage with organic circular clearings, mimicking how vegetation grows in nature.

## Agricultural Precision

Crop fields use a completely different approach - grid-based generation:

```C#
float spacing = zone.RowSpacing;
for (float x = fieldMin.X; x < fieldMax.X; x += spacing)
{
    for (float z = fieldMin.Z; z < fieldMax.Z; z += spacing)
    {
        if (rng.Randf() > density) continue; // Preserve grid structure

        // Small organic offset (10% of spacing)
        Vector3 position = gridPos + RandomOffset(spacing * 0.1f);
        PlantCrop(position);
    }
}
```

The key insight: even when reducing density, we maintain the grid structure by randomly skipping entire plants rather than breaking the row pattern.

## Real-Time Building Integration

Buildings automatically clear grass through EventBus signals:

1. Building placed → Signal emitted
2. VegetationManager receives signal
3. Affected chunks identified via AABB intersection
4. Chunk regeneration removes grass within building footprint
5. Visual update happens next frame

## Performance Optimization

We implemented several layers of optimization:

Chunking System
- Terrain divided into chunks (32 units for grass, 2-4 for crops)
- Each chunk contains 500-1000 vegetation instances
- Camera distance culling with configurable MaxRenderDistance

LOD (Level of Detail)
- Full detail: All vegetation instances visible
- Low detail: 25% density when distance > LODDistance
- Separate transform arrays prevent allocation overhead
- Smooth transitions eliminate "popping" artifacts

MultiMesh Instancing
- Hardware-accelerated rendering through Godot's MultiMeshInstance3D
- Thousands of instances with minimal CPU overhead
- Material override for shader parameters
- Shadow casting disabled for performance

## Editor Integration

The most satisfying feature for ourself: automatic editor previews.

```C#
public override void _ValidateProperty(Godot.Collections.Dictionary property)
{
    if (Engine.IsEditorHint())
    {
        CallDeferred(nameof(CreateEditorPreview));
    }
}

```

Crop fields now show instant previews in the scene editor using identical generation logic as runtime. The preview persists when switching scenes and uses a fixed seed for consistency.

## Results

The final system delivers:
- Organic grass that feels natural and responds to buildings
- Structured crops maintaining agricultural aesthetics at any density
- 60+ FPS with thousands of vegetation instances
- Designer-friendly workflow with instant visual feedback

{{< video src="2025-08-08_vegetationSystem.mp4" type="video/webm" preload="auto" >}}

## What's Next

Future enhancements we're considering:
- Seasonal variation systems
- Growth/lifecycle simulation

The foundation we've built makes these additions straightforward while maintaining the performance characteristics we need for large-scale world simulation.

---

Stay tuned, and remember to [wishlist Grey on Steam](https://store.steampowered.com/) for the latest updates!

*David & Paul*

{{< x user="dienerbrothers" id="1953942963278721396" >}}
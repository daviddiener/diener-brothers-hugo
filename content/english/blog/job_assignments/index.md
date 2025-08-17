---
title: "Why RTS Job Assignment is Broken (And How We Fixed It)"
meta_title: "Why RTS Job Assignment is Broken (And How We Fixed It)"
description: "From menu-diving frustration to divine hand satisfaction - reimagining worker management in Grey"
date: 2025-08-07T14:30:00Z
image: "/images/vegetation-system.png"
categories: ["Development", "Game Design"]
author: "Diener Brothers"
tags: ["Grey", "God Simulation", "Game Mechanics", "Vegetation"]
draft: true
---

### The RTS Frustration We All Know

Picture this: you're commanding your civilization, feeling like a divine overseer, when suddenly you need to assign a worker. The magic breaks. Click unit. Right-click building. Navigate dropdown menu. Select job type. Confirm. Hope it worked.

Welcome to traditional RTS job assignment - where managing your workforce feels more like filing spreadsheets than divine intervention.

In Grey, we asked a simple question: What if assigning jobs felt as natural as picking up your units with your divine hand? What if you could literally see every available role the moment you grab a worker?

The answer became our JobPreview Assignment System - and it transforms one of RTS gaming's most tedious mechanics into something genuinely satisfying.

### Why Traditional Job Systems Break Immersion

Every RTS player knows the drill: you want to assign a farmer, but instead of a smooth, intuitive action, you're thrown into menu navigation. The problems are universal:

- **Menu Diving**: Multiple clicks and UI navigation break gameplay flow
- **Spatial Disconnect**: No visual connection between worker and their future workplace
- **Guesswork**: "Can this building even provide jobs? What kind?"
- **Confirmation Anxiety**: "Did that actually work, or do I need to check again?"

These aren't just minor annoyances - they pull you out of the god role entirely. You stop feeling like a divine overseer and start feeling like you're managing paperwork.

### The Divine Hand Solution

Our JobPreview Assignment System brings back the physical, intuitive feel that made classic god games so satisfying. Here's how it works:

**Pick Up and Place**: Grab your units with the divine hand and drop them directly onto buildings. Want a farmer? Pick up a worker and place them on your farm. Need a woodcutter? Drop them at the lumber mill. The physical action feels natural and immediate.

**Instant Visual Feedback**: The moment you lift a unit, every compatible building lights up with clear job preview icons. No guessing, no menu navigation - you can see exactly where that worker can be productive.

**Smart Building Recognition**: Our system automatically detects job opportunities through a clean interface:

```csharp
// The magic happens through our IJobRoleProvider interface
public interface IJobRoleProvider
{
    JobRole GetJobRole();
    bool CanAcceptWorker();
    Vector3 GetWorkerPosition();
}
```

Any building implementing this interface becomes a valid job provider. Want to add a new profession? Just implement the interface. The system scales naturally as Grey grows.

### Under the Hood: Making It Work

The magic happens through a streamlined pipeline that feels instant to players:

1. **Divine Hand Detection**: When you grab a unit, our HandManager triggers the job preview system
2. **Building Query**: The system scans all buildings implementing IJobRoleProvider
3. **Icon Generation**: SVG-based job icons appear instantly above compatible buildings
4. **3D Billboard Display**: Icons exist in 3D space but always face the camera for perfect visibility
5. **Real-Time Updates**: Everything updates smoothly as you move the unit around

**Why SVG Icons?** We chose SVG rendering because it gives us crisp, scalable icons that look perfect at any distance while using minimal memory. When you're holding a worker above a bustling town, dozens of job previews can appear simultaneously without affecting performance.

**Billboard Magic**: The trickiest part was making 2D icons work seamlessly in 3D space:

```csharp
// Billboard behavior ensures perfect visibility
private void UpdateBillboardOrientation()
{
    if (camera != null)
    {
        LookAt(camera.GlobalPosition, Vector3.Up);
        RotationDegrees = new Vector3(0, RotationDegrees.Y + 180, 0);
    }
}
```

This ensures job icons are always readable, whether you're viewing your settlement from above or swooping down for a closer look.

### From Frustration to Flow

The transformation is dramatic:

**Traditional RTS Job Assignment:**
1. Select unit → Navigate menus → Select job → Confirm → Hope it worked

**Grey's Divine Hand Assignment:**
1. Pick up unit → See all available jobs instantly → Drop unit → Watch them start working

It's the difference between filling out forms and conducting an orchestra. The physical act of placing workers creates a direct connection between your divine will and your civilization's prosperity.

### Why This Matters Beyond Convenience

The JobPreview system does more than eliminate menu clicks - it fundamentally changes how you think about your civilization:

**Spatial Intelligence**: You naturally learn your kingdom's layout and job distribution without studying lists or charts.

**Strategic Optimization**: Visual job previews make inefficiencies obvious. Empty lumber mills and understaffed farms reveal themselves instantly.

**Immersive Authority**: The physical placement reinforces your role as a divine overseer. You're not clicking buttons - you're guiding lives.

**Cognitive Ease**: No more memorizing which buildings provide which jobs. The information is always there when you need it.

### Building for Tomorrow

Our modular architecture means the system grows with Grey:

- **Skill-Based Assignments**: Visual previews could show worker skill compatibility
- **Resource Requirements**: Icons could display required tools or materials  
- **Queue Management**: Visual indicators for buildings with worker waiting lists
- **Dynamic Availability**: Jobs that appear or disappear based on seasons, research, or story events

The interface-driven design means adding new professions requires minimal code changes. As Grey's world expands, job assignment will remain effortless.

### Why We Built It This Way

This system embodies our core belief: **gameplay over graphics, substance over flash**. The JobPreview system isn't about fancy animations or flashy effects - it's about solving a fundamental problem that has plagued RTS games for decades.

We took inspiration from the intuitive interactions that made classic god games like Black & White so compelling, then built it with modern technology and performance standards. The result is something that feels both familiar and revolutionary.

When you can see your divine influence shaping your civilization through intuitive, physical interactions, strategy gaming feels fresh again. Every job assignment becomes a deliberate act of divine guidance rather than a tedious administrative task.

---

This is how we approach every system in Grey: identify what breaks immersion in existing games, then build something that enhances it instead. Job assignment was just the beginning.

Stay tuned for more development insights, and [wishlist Grey on Steam](https://store.steampowered.com/) to experience divine strategy gaming for yourself.

*The Diener Brothers*
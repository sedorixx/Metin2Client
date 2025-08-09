# Sedometin (iOS) — Full Swift Rewrite Plan (No Bridge)

Goals
- Platform: iOS (16+)
- Language: Swift only (no C/C++ interop)
- Rendering: Metal (MetalKit)
- Project: Xcode project (generated via XcodeGen)

Principles
- Clean-room Swift reimplementation; no linkage to the legacy client.
- Vertical slices to keep the app runnable (UI + renderer + a minimal scene).
- Subsystems under a modular SedometinEngine framework.

Architecture
- Sedometin (SwiftUI app): app shell, lifecycle, UI.
- SedometinEngine (framework):
  - Core: math, timing, logging, config
  - IO: file system abstraction, resource locating
  - Assets: textures, meshes, animations
  - Rendering: Metal renderer (pipelines, resources, passes)
  - Scene: simple scene graph or ECS-lite
  - Input: touch/gesture mapping
  - Gameplay: progressively reimplemented logic and systems

Milestones
- M0: Scaffolding compiles; renderer draws a rotating triangle.
- M1: Core math/timing; texture loading (PNG via CoreGraphics, then DDS if needed).
- M2: Scene graph + camera; draw textured quads.
- M3: Materials + batching; text rendering.
- M4: Asset pipeline; begin gameplay systems.

Porting Guidance
- Reimplement data readers in Swift; design for testability.
- Map legacy rendering features to Metal with modern pipeline/shader organization.
- Prefer value types and contiguous buffers in hot paths; minimize ARC churn.
- Use simd for math.

Risks/Notes
- Rendering parity is the largest effort.
- Asset compatibility may require conversion tools.
- Add unit tests for parsers/math; profile renderer on device.

Build & Run
- Generate Xcode project: `cd ios/Sedometin && xcodegen generate`
- Open `Sedometin.xcodeproj` and run on iOS 16+.
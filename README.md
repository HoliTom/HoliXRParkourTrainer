# HoliXRParkourTrainer
HoliXRParkourTrainer is an open-source AR game that turns your real-world environment into a dynamic parkour playground. Scan your surroundings, tag walls, rails, and edges, and the system generates safe, AI-ranked vaults, jumps, and combos. Flow through custom courses, earn scores, and level up your movement—all from your phone.

![HoliXR Parkour Trainer](assets/holibanner.png)
Chat gpt 

/*
Title: Holistic AR Setup in Unreal Engine 5 for XR Parkour Trainer
Author: Holi Flying Farms XR Dev Team
Purpose: Create a fully modular, scalable, and performance-optimized AR system that enables real-world object tagging, move suggestion, and flow-score generation
*/

///////////////////////////////////////////
// I. FILE TREE STRUCTURE (PROJECT ROOT)
///////////////////////////////////////////

XRParkourTrainer/
├── Config/
├── Content/
│   ├── AR/
│   │   ├── BP_ARManager.uasset             // AR session and tracking logic
│   │   ├── BP_PlaneVisualizer.uasset       // Plane mesh preview overlay
│   │   ├── BP_SurfaceMarker.uasset         // User-placed interaction points
│   ├── UI/
│   │   ├── WBP_Reticle.uasset              // Center cursor for tagging
│   │   ├── WBP_TagOverlay.uasset           // Contextual UI when tagging
│   ├── FX/
│   │   ├── M_PlaneHighlight.uasset         // Holographic material for surfaces
│   ├── Logic/
│   │   ├── BFL_GradingSystem.uasset        // Blueprint function library for move grading
├── Source/ (if using C++)
├── .uproject
├── README.md

///////////////////////////////////////////
// II. AR SETUP PROCEDURE (STEP BY STEP)
///////////////////////////////////////////

// Step 1: Enable Plugins
// -----------------------
// Go to Edit > Plugins and enable:
// - OpenXR
// - ARCore (for Android) or ARKit (for iOS)
// - Augmented Reality (core AR module)
// - Live Link XR (optional for external trackers)

// Step 2: Project Settings
// ------------------------
// Platform → Android / iOS:
// - Minimum SDK: 29+
// - Enable camera usage, AR permissions
// Engine → Input:
// - Add touch input mappings (e.g. TapToTag, DoubleTapToConfirm)
// Engine → Rendering:
// - Enable Mobile HDR

// Step 3: Create AR Session Config
// --------------------------------
// Right click > Misc > Data Asset > ARSessionConfig
// Set properties:
// - Session Type: World
// - Plane Detection: Horizontal + Vertical
// - Enable Auto Focus, Light Estimation

// Step 4: AR Manager Blueprint (BP_ARManager)
// -------------------------------------------
// - Add an "ARSessionConfig" variable (public)
// - On BeginPlay:
//     → Start AR Session (node: Start AR Session)
//     → Set tracking quality overlays
// - Tick:
//     → Use GetAllGeometries to pull updated planes
//     → Spawn/update BP_PlaneVisualizer for each

// Step 5: Plane Visualizer Blueprint (BP_PlaneVisualizer)
// --------------------------------------------------------
// - Add Procedural Mesh Component
// - Bind geometry update event
// - Color with M_PlaneHighlight (animated translucent mat)

// Step 6: Tagging System (BP_SurfaceMarker)
// ------------------------------------------
// - On Tap:
//     → Line Trace against AR plane
//     → Spawn BP_SurfaceMarker actor
//     → Assign metadata: location, normal, size
// - Show WBP_TagOverlay UI:
//     → Choose move type: vault, climb, gap
//     → Set grading parameters: height, risk, distance

// Step 7: Grading Logic (BFL_GradingSystem)
// ------------------------------------------
// Blueprint Function Library:
// - Input: MoveType, Height, Distance, Angle
// - Output: Grade (A–D), Style Score (0–1)
// - Example function: CalculateMoveGrade()

// Step 8: UI Elements
// -------------------
// WBP_Reticle: Simple dot or shape in center of view
// - Add pulsing animation when looking at valid surface
// WBP_TagOverlay:
// - Dropdown for move type
// - Sliders or auto-fields for grading metrics
// - Confirm / Cancel tagging

///////////////////////////////////////////
// III. BEST PRACTICES
///////////////////////////////////////////

// ✅ Mobile Performance
// - Keep shaders simple (unlit, translucent)
// - Use Level of Detail (LOD) for AR meshes
// - Cull unneeded actors (LODVolume + Tick pruning)

// ✅ Safety & Experience
// - Guardian check radius: don’t allow tagging beyond a 5–10 m radius without warnings
// - Always show “Scan Area First” tutorial
// - Add fade-to-black fail-safe if camera loses tracking

// ✅ Modularity
// - Blueprint Interfaces between ARManager and Taggers
// - Store metadata in structs for easy expansion
// - Separate UI logic from AR logic

// ✅ Cross-platform
// - Test on ARKit and ARCore separately
// - Use device capability checks: supports plane tracking, depth, anchors

///////////////////////////////////////////
// IV. FUTURE EXTENSIONS
///////////////////////////////////////////

// - YOLOv8 integration to auto-detect rails/benches
// - VPS support via Lightship for multiplayer maps
// - Route optimization via Reinforcement Learning
// - Graphene energy tags: place eco-points that relate to Holi Flying Farms mission

/* End of AR Setup Doc */

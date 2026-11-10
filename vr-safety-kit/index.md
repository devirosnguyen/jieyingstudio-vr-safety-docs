---
title: VR Safety Kit
---

# VR Safety Kit – Occupational Safety Training for Meta Quest

[All kits](../) · [Third-party notices](THIRD-PARTY-NOTICES.txt)

A complete Unity 6 project with five interactive VR/XR occupational-safety training lessons, a lobby, scoring and reporting, and English/Vietnamese localisation. It targets Meta Quest 3 / 3S through OpenXR and is built on XR Interaction Toolkit 3.

## Lessons

| # | Lesson | Scene |
|---|--------|-------|
| 0 | Lobby (lesson picker, language and comfort options) | `Hub/00_Hub.unity` |
| 1 | Personal Protective Equipment: pick the right PPE and put it on | `Demos/01_PPE/01_PPE.unity` |
| 2 | Fire Safety: raise the alarm, choose the right extinguisher, use P.A.S.S. | `Demos/02_FireSafety/02_FireSafety.unity` |
| 3 | Work at Height: inspect the harness, use 100 % tie-off, climb the ladder | `Demos/03_WorkAtHeight/03_WorkAtHeight.unity` |
| 4 | Lockout / Tagout: isolate energy before clearing a jam | `Demos/04_LOTO/04_LOTO.unity` |
| 5 | Hazard Hunt (VR): find and classify six hazards against the clock | `Demos/05_HazardHunt/05_HazardHunt.unity` |
| 5b | Hazard Hunt (Mixed Reality, passthrough): hazards placed in your real room | `Demos/05_HazardHunt/05b_HazardHuntMR.unity` |

Every lesson provides step instructions, hints, mistake detection with explanations, a score, a pass/fail result screen, and a JSON session log.

## Requirements

- Unity **6000.3** (Unity 6.3) or newer, with **Android Build Support** to deploy to Quest.
- These packages are installed automatically from the project manifest: URP 17.3, XR Interaction Toolkit 3.3.1, OpenXR 1.16, XR Hands 1.7, AR Foundation 6.3, Meta OpenXR 2.3, Input System 1.18.

## Quick start

1. Create the project from this package. It is a Complete Project, so it includes the Project Settings.
2. When the editor opens, the **"VR Safety Kit – required content"** dialog appears. Click **Import**. This imports the XR Interaction Toolkit samples (*Starter Assets*, *XR Interaction Simulator*) and *TextMesh Pro Essential Resources*. Those files are Unity content and are not redistributed in this package.
   - You can run it again at any time from **Tools > VR Safety Kit > Setup > 0. Install Required Unity Content**.
3. Open `Hub/00_Hub.unity` and press **Play**. Without a headset, the **XR Interaction Simulator** starts and lets you play with mouse and keyboard (controls are listed below).
4. To deploy to Quest, switch the platform to **Android**, then use **Tools > VR Safety Kit > Build > Android APK**.

If XR settings were reset (for example after importing into an existing project), run **Tools > VR Safety Kit > Setup > 1. Configure Project**. It is safe to run repeatedly.

When the kit is imported into a new URP project, Unity may leave its own default `Assets/UniversalRenderPipelineGlobalSettings.asset` and `Assets/DefaultVolumeProfile.asset` in place. The kit's Project Settings point to the copies in `JieYingStudio/VRSafetyKit/Settings`, so you can delete the ones in the `Assets` root.

## Playing in the editor (XR Interaction Simulator)

| Input | Action |
|-------|--------|
| `W A S D`, `Q / E` | Move, lower / raise |
| Hold right mouse button + move mouse | Look around |
| `]` / `[` / `Tab` | Control right hand / left hand / cycle devices |
| `G` | Grip (grab) |
| `T` | Trigger (press UI buttons, squeeze the extinguisher) |
| `Shift` + key | Perform the action with the left hand |
| `R` | Reset |

Each lesson's instruction panel has a **Hint** button and a **Skip step (debug)** button.

## Project structure

```
JieYingStudio/VRSafetyKit/
  Core/        Reusable framework: scenario runner, scoring, UI, localisation, comfort, interactions, tests
  Demos/       One folder per lesson: scripts, editor scene builder, scenario asset, voice-over, tests
  Hub/         Lobby scene
  Shared/      Shared audio (SFX), materials, generated textures
  Settings/    URP, XR / OpenXR, XRI and Composition Layers settings used by Project Settings
  ThirdParty/  Kenney models and Poly Haven textures (CC0)
  Documentation/
```

### Core framework

| Component | Purpose |
|-----------|---------|
| `ScenarioDefinition` (ScriptableObject) | Lesson data: steps, hints, reasons, scoring, ordering, time limit, pass threshold |
| `ScenarioRunner` | Runs a lesson: `CompleteStep(id)`, `ReportMistake(...)`, `RequestHint()`, scoring, events, `AssessmentResult` |
| `StepTrigger` / `MistakeTrigger` | Connect any UnityEvent (XRI select, socket, trigger zone) to the runner |
| `HingeHandle`, `PressButton`, `ItemSocket`, `TrainingItem` | Reusable physical interactions |
| `InstructionPanel`, `ResultPanel`, `FollowHead` | World-space HUD and result report |
| `SessionLogger` | Writes a JSON log per session to `Application.persistentDataPath/sessions` |
| `ComfortSettings` | Teleport or smooth locomotion, snap or smooth turn, vignette, seated mode |
| `Loc`, `LocString`, `LocalizedText` | English (default) and Vietnamese, switchable at runtime |

Rule: lesson scripts only talk to `ScenarioRunner` (complete a step or report a mistake). They never score anything themselves.

### Scenes are generated

All scenes are built by code: `SceneGenerator` plus one `*SceneBuilder` per lesson, with lesson text in `Core/Scripts/Editor/DemoContent.cs`. **Tools > VR Safety Kit > Setup > 2. Generate Scenes** rebuilds every scene.

- Edits made by hand inside a scene are overwritten the next time you regenerate. Make changes in the builder or in `DemoContent.cs`.
- If you prefer to edit scenes by hand, simply stop using Generate Scenes.

### Adding or changing content

- Write every user-facing string in both languages: `new LocString("English", "Tiếng Việt")`, or `L("en", "vi")` in builders. EditMode tests enforce this.
- To replace placeholder props, each interactive item has a `Visual` child node. Put your model there; the logic does not need to change.

## Tests

The project includes EditMode and PlayMode tests (Window > General > Test Runner). They cover scoring, localisation coverage, generated scenes and smoke-loading every scene.

## Customising for your company

- **Company name, product name, package id:** set them in Project Settings > Player, or edit `ProjectSetup.ConfigurePlayer`.
- **Lesson text and scoring:** edit `DemoContent.cs`, then regenerate the scenes.
- **Training content disclaimer:** the procedures in this kit are generic examples. Have them reviewed by a qualified safety professional against your local regulations before you use them for real training.

## Support

Publisher: JieYingStudio

- E-mail: devirosnguyen@gmail.com
- Online documentation: https://devirosnguyen.github.io/jieyingstudio-vr-safety-docs/

When reporting a problem, please include your Unity version, the lesson name and the Console log.

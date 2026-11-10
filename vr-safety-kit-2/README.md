# VR Safety Kit 2 – Advanced Occupational Safety Training for Meta Quest

A complete Unity 6 project with five advanced, fully interactive VR safety lessons for high-risk work, plus a lobby,
Training and Assessment modes, digital permits, animated co-workers with voice-over, xAPI reporting and
English/Vietnamese localisation. It targets Meta Quest 3 / 3S through OpenXR and is built on XR Interaction Toolkit 3.

## Lessons

| # | Lesson | What the trainee does | Scene |
|---|--------|-----------------------|-------|
| 0 | Lobby | Pick a lesson, Training / Assessment mode, language, comfort options | `Hub/00_Hub.unity` |
| 1 | Confined Space Entry | Isolate energy, test the atmosphere at three levels with a 4-gas detector, ventilate, fill in and sign the entry permit, brief the attendant, rig the tripod and rescue line, enter; a co-worker collapses inside – respond without becoming the second victim | `Demos/01_ConfinedSpace/01_ConfinedSpace.unity` |
| 2 | Hot Work Permit | Clear combustibles, test for flammable vapour, check the oxy-acetylene cylinders (chain, flashback arrestors, leak test), place the extinguisher, sign the permit, wear welding PPE, brief the fire watch, weld, then deal with a smouldering spark during the 30-minute fire watch | `Demos/02_HotWork/02_HotWork.unity` |
| 3 | Forklift Operation | Walk-around inspection, seat belt, control checks, load check against the capacity plate, pick up a pallet, drive it through a warehouse with a blind pedestrian crossing to a rack, park correctly. The truck is drivable with a virtual steering wheel | `Demos/03_Forklift/03_Forklift.unity` |
| 4 | Crane & Rigging | Complete the lift plan, inspect and quarantine damaged slings, rig with a safe sling angle, set barriers and tag lines, agree signals with the operator, signal an overhead crane by hand (XR Hands) or with the signal panel, land the load on timber blocks | `Demos/04_Lifting/04_Lifting.unity` |
| 5 | Chemical Spill Response | Raise the alarm and isolate, identify the chemical from its label and Safety Data Sheet, choose the PPE that SDS requires, contain the spill and cover the drain, absorb, bag and label the waste, report to the supervisor | `Demos/05_ChemicalSpill/05_ChemicalSpill.unity` |

Every lesson gives step instructions with voice-over, hints and the reason behind each step, detects mistakes and explains
them, scores the attempt, shows a pass/fail result screen and writes a JSON session log. Critical mistakes (for example
standing under a suspended load, overloading the forklift, striking a pedestrian) end the attempt immediately.

## Key features

- **Training and Assessment modes.** Training shows hints, step text and step voice-over. Assessment hides them, doubles
  penalties, requires 80 % to pass and **randomises the situation** from a seed: a different chemical, a different
  damaged sling and load weight, a missing fork pin, the job pallet's position and weight, gas readings, hazards…
- **Digital permits on a wrist tablet.** Readings are filled from the instruments (gas detector, weight tags, pallet
  labels), never typed. Signing with a missing or unsafe value is a mistake – a critical one for unsafe readings.
- **Co-workers.** Rigged, animated NPCs (CC0 characters) walk their routes, give way to the trainee and hold short
  voiced conversations; the trainee's choice of reply completes a step or is scored as a mistake.
- **Driving.** Counterbalance forklift with rear-wheel steering, a multi-turn steering wheel held by hand (trigger =
  accelerator, release = brake), forward/neutral/reverse lever, lift and tilt levers, parking brake, key, horn, lights.
- **Hand signals.** Crane signals are recognised from hand-tracking poses (XR Hands); a signal panel is the fallback.
- **xAPI.** Optional statements to your LRS with an offline queue (off by default, see below).
- **Comfort.** Teleport or smooth locomotion, snap or smooth turn, vignette, seated mode. A tunnelling vignette is always
  applied while the forklift moves, and its speed is limited (8 km/h forward, 4.3 km/h reverse).
- **Content standard.** Procedures follow international practice (OSHA, NFPA 51B, ASME B30, GHS).

## Requirements

- Unity **6000.3** (Unity 6.3) or newer, with **Android Build Support** to deploy to Quest.
- Packages are installed automatically from the project manifest: URP 17.3, XR Interaction Toolkit 3.3.1,
  OpenXR 1.16, XR Hands 1.7, AR Foundation 6.3, Meta OpenXR 2.3, Input System 1.18.

## Quick start

1. Create the project from this package. It is a Complete Project, so it includes the Project Settings.
2. When the editor opens, the **"VR Safety Kit 2 – required content"** dialog appears. Click **Import**. This imports
   the XR Interaction Toolkit samples (*Starter Assets*, *XR Interaction Simulator*) and *TextMesh Pro Essential
   Resources*. Those files are Unity content and are not redistributed in this package.
   - You can run it again at any time from **Tools > VR Safety Kit 2 > Setup > 0. Install Required Unity Content**.
3. Open `Hub/00_Hub.unity` and press **Play**. Without a headset, the **XR Interaction Simulator** starts and lets you
   play with mouse and keyboard.
4. To deploy to Quest, switch the platform to **Android**, then use **Tools > VR Safety Kit 2 > Build > Android APK**.

If XR settings were reset (for example after importing into an existing project), run
**Tools > VR Safety Kit 2 > Setup > 1. Configure Project**. It is safe to run repeatedly.

When the kit is imported into a new URP project, Unity may leave its own default
`Assets/UniversalRenderPipelineGlobalSettings.asset` and `Assets/DefaultVolumeProfile.asset` in place. The kit's Project
Settings point to the copies in `JieYingStudio/VRSafetyKit2/Settings`, so you can delete the ones in the `Assets` root.

## Controls

### On Quest

| Action | Input |
|--------|-------|
| Grab, use sockets, turn handles and levers | Grip |
| Press buttons, use tools (extinguisher, spray) | Trigger, or poke with the controller / finger |
| Select far objects (labels, tags, NPCs, the forklift seat) | Point the ray and press Grip or Trigger |
| Drive the forklift | Select the seat to get on. Hold the steering wheel with Grip and squeeze **Trigger** to accelerate; release to brake. F-N-R lever on the left, lift / tilt levers on the right (pull back = raise / tilt back) |
| Crane hand signals (hand tracking on) | HOIST = index finger up · LOWER = index finger down · STOP = flat hand, palm down · EMERGENCY STOP = both hands flat, palms down · TRAVEL = flat hand pointing left or right. Hold the pose for half a second |

The moving ASME crane signals are reduced to static poses so they can be recognised reliably; the signal panel in the
lesson offers the same signals as buttons.

### In the editor (XR Interaction Simulator)

| Input | Action |
|-------|--------|
| `W A S D`, `Q / E` | Move, lower / raise |
| Hold right mouse button + move mouse | Look around |
| `]` / `[` / `Tab` | Control right hand / left hand / cycle devices |
| `G` | Grip (grab, hold the steering wheel) |
| `T` | Trigger (press UI buttons, accelerate while holding the wheel) |
| `Shift` + key | Perform the action with the left hand |

Each lesson's instruction panel has a **Hint** button and a **Skip step (debug)** button.

## Training and Assessment

The mode is chosen in the lobby (or set `TrainingModeSettings.Mode` from code). Each lesson component that implements
`IScenarioRandomizer` gets its own seeded random stream, so an assessment can be repeated exactly with
`ScenarioRunner.SetSeed(seed)`.

## xAPI reporting

Edit `Core/Resources/XapiSettings.asset`:

| Field | Meaning |
|-------|---------|
| `enabled` | Off by default. Turn on to send statements |
| `endpoint` | Your LRS base URL (`…/xapi/`) |
| `authKey`, `authSecret` | Basic-auth credentials of the LRS client |
| `activityIriBase`, `accountHomePage` | IRIs used for activities and the learner account |
| `sendAttempted` | Also send an *attempted* statement when a lesson starts |

The kit has no login screen. Set `TraineeProfile.Id` and `TraineeProfile.DisplayName` from your own sign-in or LMS launch
before a lesson starts; otherwise a random per-device id is used. Statements are written to
`persistentDataPath/xapi-queue` first and sent when the network is available, so nothing is lost offline. Do not ship
production credentials inside an APK you distribute publicly.

## Performance

Scenes are kept small for standalone Quest: about 20–30k triangles and fewer than 200 draw calls per lesson, one
realtime directional light with hard shadows (1024 shadow map, 15 m), no HDR, 4× MSAA, SRP Batcher on. Parts that never
move are marked static for batching when scenes are generated, and text, floor markings and small parts do not cast
shadows. **Tools > VR Safety Kit 2 > Quality > Performance Report** writes the numbers for every scene to
`Logs/PerfReport.md`, and an EditMode test keeps every scene within budget. Always confirm the frame rate on your target
headset (for example with the OVR Metrics Tool).

## Project structure

```
JieYingStudio/VRSafetyKit2/
  Core/        Framework: scenario runner, scoring, modes, permits, NPCs, gas instruments, interactions, UI,
               localisation, comfort, xAPI, editor tools (scene generator, builders), tests
  Demos/       One folder per lesson: scripts (own assembly), editor scene builder, scenario asset, voice-over
  Hub/         Lobby scene
  Shared/      Shared audio (SFX) and materials
  Settings/    URP, XR / OpenXR, XRI and Composition Layers settings used by Project Settings
  ThirdParty/  Kenney models and characters, Poly Haven textures (CC0)
  Documentation/
```

### Core framework

| Component | Purpose |
|-----------|---------|
| `ScenarioDefinition`, `ScenarioRunner` | Lesson data and runtime: `CompleteStep`, `ReportMistake`, hints, ordering, scoring, `AssessmentResult` |
| `TrainingModeSettings`, `IScenarioRandomizer`, `ScenarioRandom` | Training / Assessment modes and seeded randomisation |
| `PermitForm`, `PermitFormController`, `PermitTablet`, `ReadingSource` | Permits and checklists on a tablet, readings from instruments |
| `NpcActor`, `NpcPath`, `NpcConversation` | Walking, voiced co-workers and reply choices |
| `GasDetector`, `SamplePort`, `IGasSource` | Gas testing used by the confined space and hot work lessons |
| `HingeHandle`, `PressButton`, `ItemSocket`, `TrainingItem`, `BodyAnchor` | Reusable physical interactions |
| `InstructionPanel`, `ResultPanel`, `FollowHead` | World-space HUD (lazy-follows the head) and result report |
| `Xapi`, `XapiSettings`, `TraineeProfile`, `SessionLogger` | Reporting |
| `ComfortSettings`, `Loc` / `LocString` / `LocalizedText` | Comfort options; English (default) and Vietnamese |

Rule: lesson scripts only talk to `ScenarioRunner` (complete a step or report a mistake). They never score anything
themselves.

### Scenes are generated

All scenes are built by code: `SceneGenerator` plus one `*SceneBuilder` per lesson, with lesson text in
`Core/Scripts/Editor/DemoContent.cs`. **Tools > VR Safety Kit 2 > Setup > 2. Generate Scenes** rebuilds every scene.

- Edits made by hand inside a scene are overwritten the next time you regenerate. Make changes in the builder or in
  `DemoContent.cs`, or stop using Generate Scenes if you prefer to edit scenes by hand.
- Write every user-facing string in both languages: `new LocString("English", "Tiếng Việt")`, or `L("en", "vi")` in
  builders. EditMode tests enforce this.
- To replace placeholder props, each interactive item has a `Visual` child node. Put your model there; the logic does not
  need to change.

## Tests

EditMode and PlayMode tests (Window > General > Test Runner) cover scoring, modes, permits, xAPI (against an in-test
mock LRS), hand-signal recognition, localisation, generated scenes, performance budgets and a full play-through of every
lesson, including its main mistakes. Tests in `VisualCaptureTests` are explicit: they write screenshots and need a GPU.

## Customising for your company

- **Company name, product name, package id:** Project Settings > Player, or edit `ProjectSetup.ConfigurePlayer`.
- **Lesson text, scoring and permits:** edit `DemoContent.cs`, then regenerate the scenes.
- **Voice-over:** step clips are `Demos/<lesson>/VO/en/<scenario>__<step>.wav` (English) and
  `Demos/<lesson>/VO/<scenario>__<step>.wav` (Vietnamese); co-worker lines are `Demos/<lesson>/VO/npc/en|vi/<npc>__<line>.wav`.
  Replace them with your own recordings using the same names.

## Important notes

- **Training content disclaimer.** The procedures in this kit are generic examples based on international practice.
  Have them reviewed by a qualified safety professional against your local regulations and site procedures before you use
  them for real training. This kit is not a certified operator-training or licensing program.
- The forklift and crane are simplified, kinematic simulations for procedure training, not engineering simulators.
- Hand-signal recognition depends on tracking quality; keep the signal panel available.
- The voice-over was generated with Windows text-to-speech voices. Replace it with your own recordings if your use
  requires it (see *Third-Party Notices*).

## Support

Publisher: JieYingStudio

- E-mail: devirosnguyen@gmail.com
- Online documentation: https://github.com/devirosnguyen/jieyingstudio-vr-safety-docs

When reporting a problem, please include your Unity version, the lesson name and the Console log.

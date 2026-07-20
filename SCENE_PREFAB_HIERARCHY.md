# Scene Layout and Prefab Hierarchy

## Purpose
This document describes the main scene layout and prefab structure for the `OneDayOld` Unity project. It is based on the scene metadata in `Assets/Scenes/Game.unity`, the prefab asset files in `Assets/Prefabs`, and the project scripts that reference those objects.

## Main Scene: `Assets/Scenes/Game.unity`

### High-level root objects
The scene contains the following primary root GameObjects:

- `GameManager` — game state controller and entry point for gameplay management
- `LevelManager` — runtime level streaming, segment spawning, difficulty scaling
- `MapManager` — map-specific content loader for obstacles, skybox, and player character
- `AudioManager` — music and SFX manager
- `DataPersistenceManager` — save/load game data manager
- `UI_Manager` — centralized UI controller for menu and HUD transitions
- `MainMenu_Canvas` — main menu UI
- `HUD_Canvas` — in-game heads-up display
- `GameInstr_Canvas` — instruction overlay for startup tutorial text
- `MapSelectionCanvas` — map selection UI
- `GameOver_Canvas` — game over screen UI
- `Settings_Canvas` — settings menu UI
- `PauseUI` — pause menu overlay
- `EventSystem` — Unity input event system
- `Main Camera` — main gameplay camera
- `Player Camera` — secondary or follow camera object
- `Directional Light` — scene lighting source
- `Background` / `Background` variants — scene background visuals
- Map lock objects: `Map2PadLock`, `Map3`, `Map4`, `Map5`, `MapLock (2)`, `MapLock (3)`, `MapLock (4)`, `MapLock (5)`
- `Audio_btn`, `Credits_Button`, `Exit_Button`, `Play_Button`, `PlayAgain_btn`, `Menu_btn` — UI button objects
- Score and HUD objects: `Distance_text`, `CurrentScore`, `HudHighScore`, `high_score`, `HighScore_Text`, `newHigh_score`, `beaten_HScore`, `Did_not_beat_HScore`

### UI canvases and layout
The scene appears to use multiple canvases, each representing a major UI flow:

- `MainMenu_Canvas`
  - likely contains `Play_Button`, `MapSelectionCanvas` trigger, map lock buttons, and menu navigation
- `HUD_Canvas`
  - likely contains `Distance_text`, `HudHighScore`, `pause_button`, and live gameplay HUD elements
- `GameOver_Canvas`
  - likely contains `PlayAgain_btn`, `HighScore`, `newHigh_score`, `high_score`, and related retry/exit buttons
- `Settings_Canvas`
  - likely contains `Audio_btn`, `Credits_Button`, `Exit_settings`, volume controls, and `VolumeSettings_UI`
- `MapSelectionCanvas`
  - likely contains `Map1`, `Map2`, `Map3`, `Map4`, `Map5` selection buttons and padlock icons for locked maps
- `GameInstr_Canvas`
  - used for short instructional text when game starts

### Manager and system objects
The scene includes several non-visual manager objects that coordinate gameplay:

- `GameManager` — handles `StartGame()`, `PauseGame()`, `GameOver()`, `RestartGame()`, `BackToMenu()`
- `LevelManager` — handles `SetObstacles()`, `ClearMap()`, `SpawnSegment()`, `SpawnObstacles()`, and speed/launch scaling
- `MapManager` — handles `LoadObstacles()`, `LoadSkyBox()`, `LoadPlayerCharacter()` from `MapData`
- `AudioManager` — plays music and sound effects, supports pause/resume
- `DataPersistenceManager` — finds all `IDataPersistence` objects, loads and saves `GameData`
- `UI_Manager` — responds to button clicks and coordinates gameplay UI transitions

### Special scene objects
- `DigitalFootPrint` — appears to be a decorative or effect object
- `Duck_waiting_BG` — likely an idle or menu background object
- `Player Camera` — likely separate from the main camera to support cinematic or UI camera behavior
- `Global Volume` — likely used for audio mixer control and volume settings

## Prefab Asset Summary

### Prefab files in `Assets/Prefabs`
The project contains the following prefab files and folders:

- `CharacterMap1-4.prefab`
- `EagleStationaryObstacle.prefab`
- `Kite.prefab`
- `Map1-4 Prefabs/` (folder appears present but empty in asset metadata)
- `Map5_Crow.prefab`
- `Map5_Duck.prefab`
- `Map5_eagle.prefab`
- `Map5_Kite.prefab`
- `Map5_Plane.prefab`
- `PlaneObstacle.prefab`
- `Player.prefab`
- `PredatorObstacle.prefab`

### Prefab usage and hierarchy patterns

#### `Player.prefab`
- Root GameObject: `Player`
- Tag: `Player`
- Components:
  - `Transform`
  - `Rigidbody` (physics body)
  - `PlayerMovement` script
  - `SphereCollider`
- Contains a child object named `Predator` (or a nested prefab instance based on the YAML snippet) used for visual display
- This prefab is likely instantiated or reset by `GameManager` and `MapManager`

#### `PredatorObstacle.prefab`
- Root GameObject: `PredatorObstacle`
- Tag: `Predator`
- Components:
  - `Transform`
  - `MeshFilter`
  - `MeshRenderer`
  - `BoxCollider` (trigger)
  - `PredatorObstacle` script
- Contains a child GameObject named `Predator` with a `SpriteRenderer`
- This prefab is likely part of `LevelManager` obstacle spawning

#### `Map5_Plane.prefab`
- Root GameObject: `Map5_Plane`
- Tag: `Plane`
- Components:
  - `Transform`
  - `Obstacle` script
  - `BoxCollider` (trigger)
- Contains a child GameObject named `plane` with a `SpriteRenderer`
- Used as one of the obstacle prefabs for map 5 or later levels

### Prefab categories by naming
- `Map5_*` prefabs appear to belong to a specific map or level theme
- `CharacterMap1-4.prefab` likely represents the player character variant for maps 1 through 4
- `EagleStationaryObstacle.prefab`, `Kite.prefab`, `PlaneObstacle.prefab` are reusable obstacle types
- `Player.prefab` is the main player character prefab instanced via `MapManager`

## Scene / Prefab Relationships

### Scene objects vs script systems
The scene root objects connect to scripts as follows:

- `GameManager` is the runtime controller used by `PlayerMovement`, `UIManager`, and `Obstacle`
- `LevelManager` receives obstacle prefab arrays via `MapManager.LoadObstacles()` and spawns them in runtime segments
- `MapManager` is driven by `UIManager.OnMapButtonPressed(MapData mapToLoad)`
- `AudioManager` is referenced by UI actions and obstacle triggers
- `DataPersistenceManager` calls `LoadData()` and `SaveData()` on `DistanceTracker` and any other `IDataPersistence` objects in the scene

### MapData connection
`MapData` scriptable objects define:
- `mapName`
- `skyboxMaterial`
- `playerCharacter`
- `backgroundGameObject`
- `obstaclePrefabs[]`

This data is loaded by `MapManager` and used to configure the scene dynamically at runtime.

## Notes for Scene Layout

- Many named roots in the scene are UI or label objects (`Text (TMP)`, `Title_txt`, `Music_text`, etc.) that are likely children of canvas GameObjects.
- The exact transform hierarchy was inferred from scene metadata; root objects with `parent == 0` are top-level in the scene.
- `LevelManager` has a direct child count of 1 in the scene metadata, suggesting it may own a configured child such as a spawn anchor or debug element.

## Recommended Scene Entry Points

- `Assets/Scenes/Game.unity` — primary gameplay scene configuration
- `Assets/Prefabs/Player.prefab` — player prefab and movement setup
- `Assets/Prefabs/PredatorObstacle.prefab` — enemy/obstacle prefab structure
- `Assets/Prefabs/Map5_Plane.prefab` — map 5 obstacle template
- `Assets/Scripts/MapManager.cs` — runtime prefab swap and scene configurator
- `Assets/Scripts/LevelManager.cs` — obstacle spawning and level streaming
- `Assets/Scripts/UIManager.cs` — button wiring and UI canvas control

## Usage Summary

The scene appears structured around a central runtime manager layer, multiple canvas-based UI flows, and a set of obstacle and player prefabs that are swapped in at runtime. Scene root objects wrap UI, manager objects, and environmental setup, while prefab assets provide reusable obstacles and player character variants.
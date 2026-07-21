# Requirements Specification – OneDayOld

## 1. Purpose
This document defines the user requirements and system specifications for the OneDayOld game project. It captures the expected player experience, core gameplay behavior, and the technical environment needed to build and run the game.

## 2. Product Overview
- Project name: OneDayOld
- Genre: 2D endless runner / obstacle avoidance game
- Core gameplay: the player moves forward automatically while avoiding hazards by flipping gravity at the right time
- Platform target: Android mobile, with Unity editor support for development and testing
- Current project state: gameplay prototype with menus, score tracking, audio settings, and map-based content

## 3. User Requirements

### 3.1 Core Gameplay
The player should be able to:
- Start a new game from the main menu
- Select a map before gameplay begins
- Control the character with simple input (tap on screen or press a keyboard key)
- Flip gravity to avoid oncoming obstacles
- Survive as long as possible while the level continues to scroll
- See the current distance or score during play

### 3.2 Game Flow
The game should provide:
- A clear main menu with options to play, adjust settings, or exit
- An instructional screen or brief on-screen guidance at game start
- A pause and resume flow during gameplay
- A game over screen with retry and return-to-menu options

### 3.3 Progression and Feedback
The game should:
- Track and display the player’s high score
- Save score data between sessions
- Show whether the player beat their previous record
- Unlock additional maps as the player reaches score thresholds

### 3.4 Audio and Settings
The player should be able to:
- Adjust music and sound effects volume
- Hear button sounds and gameplay audio feedback
- Return to the main menu from settings or game over screens

### 3.5 Usability Expectations
The experience should be:
- Easy to understand for casual players
- Responsive to input
- Clear enough to play without a long tutorial
- Stable on mobile devices

## 4. Functional Requirements

### 4.1 Game Start and Menus
- The system shall display a main menu when the game starts
- The system shall allow the player to begin gameplay from the menu
- The system shall provide a map selection screen before gameplay begins
- The system shall allow the player to return to the main menu from gameplay states

### 4.2 Player Control
- The system shall support input for flipping gravity
- The system shall move the player forward automatically during gameplay
- The system shall prevent the player from leaving the intended play area
- The system shall reset the player position when restarting or returning to menu

### 4.3 Obstacle and Level Handling
- The system shall spawn obstacles dynamically as the level progresses
- The system shall increase difficulty over time by gradually increasing speed and launch force
- The system shall recycle or regenerate level segments to support endless play
- The system shall trigger a game-over state when the player collides with an obstacle

### 4.4 Scoring and Persistence
- The system shall track the player’s distance or score during gameplay
- The system shall update the high score when a new record is achieved
- The system shall save high score data to persistent storage
- The system shall load saved data when the game starts

### 4.5 Audio and UI
- The system shall play background music and sound effects
- The system shall pause or resume music during gameplay state changes
- The system shall show HUD elements during gameplay
- The system shall show game over, pause, and settings interfaces at the appropriate times

### 4.6 Map System
- The system shall support multiple maps using data-driven definitions
- Each map shall be able to define its own visual theme, player character, and obstacle set
- The system shall load map-specific content when the player selects a map

## 5. Non-Functional Requirements

### 5.1 Performance
- The game shall target smooth gameplay at approximately 60 FPS
- Input response should feel immediate and not delayed
- Level generation and obstacle spawning should remain efficient during long sessions

### 5.2 Reliability
- The game shall remain stable during repeated start, pause, resume, and restart cycles
- The game shall handle save and load operations without corrupting user progress

### 5.3 Compatibility
- The game shall run in the Unity editor for development and testing
- The game shall be deployable to Android devices
- The game shall support touch input on mobile and keyboard input on desktop/editor

### 5.4 Maintainability
- The project should be structured so new maps, obstacles, and UI states can be added with minimal rework
- Core game behavior should remain modular and easy to extend

### 5.5 Data Handling
- Persistent data should be stored in a safe and accessible location for the target platform
- The system should gracefully handle missing or invalid save data

## 6. System Specifications

### 6.1 Software Environment
- Unity version: 6000.3.13f1
- C# language version: 9.0
- .NET target framework: 4.7.1
- Primary engine modules: 2D physics, input system, UI, audio, and cinematics support

### 6.2 Development Dependencies
- Unity Input System
- Cinemachine
- TextMeshPro
- Universal Render Pipeline (URP)
- Standard Unity UI and audio tools

### 6.3 Target Platform
- Primary target: Android mobile devices
- Secondary development/testing platform: Unity editor on Windows
- Input support: touch screen for mobile and keyboard for editor/testing

### 6.4 Runtime Requirements
- A device capable of running a lightweight 2D Unity game smoothly
- Adequate memory for asset loading, audio playback, and scene transitions
- Support for basic touch interaction and screen rendering

### 6.5 Content and Asset Requirements
- The game shall use reusable prefabs for obstacles and player characters
- Map content shall be configurable through scriptable data assets
- Audio assets, UI elements, and scene layouts shall be managed as project assets

## 7. Assumptions and Constraints
- The current project is a playable prototype rather than a fully commercial production build
- Multiplayer, account integration, and online services are not required at this stage
- The project relies on Unity’s scene-based architecture and singleton-style game managers
- Future expansion may require refactoring if the game grows beyond the current prototype scope

## 8. Acceptance Criteria
The project will be considered complete for its current scope when:
- The player can launch the game and reach the main menu
- The player can select a map and start gameplay
- The player can flip gravity and avoid obstacles successfully
- The game ends correctly on collision and displays the game over flow
- High score data is saved and restored correctly
- Audio settings can be adjusted and applied during play

## 9. Recommended Next Steps
- Validate the requirements against actual gameplay behavior in the current Unity scene
- Add any missing gameplay requirements discovered during playtesting
- Refine these requirements as the project moves from prototype to full production

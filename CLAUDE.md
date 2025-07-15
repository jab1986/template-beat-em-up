# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is "Downtown Beatdown", a 2D beat-em-up game template built with Godot 4. It's an open-source template created by Quiver that provides a foundation for making games like Streets of Rage.

## Development Environment

### Requirements
- Godot 4.0 RC6 or higher
- Git Large File Storage (LFS) for assets - run `git lfs install` before cloning

### Running the Game
- Open `project.godot` in Godot 4
- The main scene is `ui/main_menu/main_menu.tscn`
- Press F5 or click the play button to run the game

### Debug Controls
- R: Restart scene (debug_restart input)
- Escape: Pause menu

## Architecture Overview

### Core Plugin System
The game is built around the Quiver Beat'em Up plugin located in `addons/quiver.beat_em_up/`:

**Key Components:**
- `QuiverCharacter` - Base class for all characters (players/enemies)
- `QuiverCharacterAction` - Base class for character actions/states
- `QuiverAttributes` - Character stats and properties
- `QuiverStateMachine` - State management system
- `QuiverCombatSystem` - Combat mechanics and damage handling

### Character System
Characters are divided into:
- **Playable characters**: `characters/playable/` (e.g., Chad)
- **Enemy characters**: `characters/enemies/` (e.g., Sargent, Tax Man)

Each character has:
- Base scene inheriting from `quiver_character_base.tscn`
- Skin system for animations and visuals
- Action states for different behaviors
- AI state machines for enemies

### Combat System
- Hit boxes, hurt boxes, and grab boxes for collision detection
- Physics layers for different interaction types
- Knockback and damage systems
- Lane-based combat mechanics

### Stage System
- Base stage class at `stages/_base/base_stage.gd`
- Day/night cycle system
- Skybox and environmental effects
- Fight rooms that constrain player movement

### UI System
- Main menu, pause menu, and gameplay HUD
- Health bars for players and enemies
- How-to-play screens with input mapping

## Key Autoloads (Singletons)
- `Events` - Global event system
- `CombatSystem` - Combat mechanics
- `QuiverDebugLogger` - Debug logging
- `BackgroundLoader` - Asset loading
- `HitFreeze` - Combat hit effects
- `ScreenTransitions` - Scene transitions

## Custom Inspector Tools
The plugin provides several editor tools:
- Create new actions wizard
- Create new AI states wizard
- Collision type selectors
- Animation mirroring tools
- State dropdown properties

## Physics Configuration
- Default gravity: 3000.0
- Fall gravity modifier: 2.5
- Hit lane size: 60 pixels
- Multiple physics layers for different collision types

## Input Mapping
- Arrow keys/WASD: Movement
- X: Attack
- C: Jump
- Escape: Pause
- R: Debug restart (development only)

## File Structure Notes
- `addons/quiver.beat_em_up/` - Core plugin code
- `characters/` - Character implementations
- `stages/` - Level scenes and assets
- `ui/` - User interface screens
- `debug_tools/` - Development and testing utilities
- `script_templates/` - Code templates for new files

## Development Workflow
1. Create new characters by inheriting from the base character scene
2. Add custom actions by extending `QuiverCharacterAction`
3. Configure character attributes using the resource system
4. Use the custom inspector tools for rapid development
5. Test characters in the debug scenes under `debug_tools/test_scenes/`

## Character Replacement Guide

### Required Sprite Categories

**For All Characters:**
- **Idle**: 3-4 frames of standing animation
- **Walk**: 8-12 frames of walking cycle
- **Turn**: 2-3 frames for direction changes
- **Hurt (Mid)**: 1 frame for mid-level damage
- **Hurt (High)**: 1 frame for high-level damage
- **Die**: 1 frame for death state
- **Jump Impulse**: 1 frame for jump start
- **Jump Rising**: 1 frame for jump ascending
- **Jump Falling**: 1 frame for jump descending
- **Jump Landing**: 2-3 frames for landing
- **Knockout Launch**: 1 frame for being launched
- **Knockout Rising**: 1 frame for flying upward
- **Knockout Falling**: 1 frame for falling down
- **Knockout Bounce**: 2 frames for bouncing on ground
- **Knockout Landed**: 1 frame for lying on ground

**For Playable Characters Only:**
- **Attack 1**: 2-6 frames for first punch
- **Attack 2**: 6-11 frames for second punch
- **Attack 3**: 10-17 frames for third punch
- **Air Attack**: 2-4 frames for aerial attack

**For Enemy Characters Only:**
- **Attack 1**: 2-6 frames for first attack
- **Attack 2**: 6-11 frames for second attack
- **Attack 3**: 10-17 frames for third attack
- **Getting Up**: 2-3 frames for recovery

### Step-by-Step Process

#### For Playable Characters (Using Chad as template):

**Step 1: Prepare Your Sprites**
1. Create PNG sprites for each animation category listed above
2. Organize them in folders matching this structure:
   ```
   your_character/
   ├── idle/ (idle_00.png, idle_01.png, etc.)
   ├── walk/ (walk_00.png through walk_11.png)
   ├── punches/ (punch1_00.png, punch1_01.png, punch2_00.png, etc.)
   ├── jump/ (jump_01.png through jump_04.png)
   ├── air_attack/ (air_attack_00.png, air_attack_01.png)
   ├── hurt/ (hurt_high.png, hurt_mid.png)
   ├── knock_out/ (knockout_00.png through knockout_05.png)
   └── turn_around/ (turnaround_00.png through turnaround_02.png)
   ```

**Step 2: Create Character Folder Structure**
```bash
# Create your character folder
mkdir -p characters/playable/your_character/resources/sprites
mkdir -p characters/playable/your_character/resources/animations
mkdir -p characters/playable/your_character/resources/attacks
```

**Step 3: Copy Base Files**
```bash
# Copy Chad's structure as template
cp -r characters/playable/chad/chad.tscn characters/playable/your_character/
cp -r characters/playable/chad/chad_skin.tscn characters/playable/your_character/
cp -r characters/playable/chad/chad.gd characters/playable/your_character/
cp -r characters/playable/chad/chad_skin.gd characters/playable/your_character/
cp -r characters/playable/chad/resources/animations/ characters/playable/your_character/resources/
cp -r characters/playable/chad/resources/attacks/ characters/playable/your_character/resources/
```

**Step 4: Replace Sprites**
1. Copy your sprite folders into `characters/playable/your_character/resources/sprites/`
2. Open Godot and navigate to your character's sprite frames resource
3. For each animation in the SpriteFrames:
   - Select the animation (idle, walk, attack_1, etc.)
   - Delete existing frames
   - Add your new sprite frames in sequence
   - Adjust frame duration and loop settings as needed

**Step 5: Update Character Attributes**
1. Open `characters/playable/your_character/resources/chad_attributes.tres`
2. Update these values:
   - `display_name`: Your character's name
   - `profile_texture`: Path to your character's profile image
   - `health_max`: Starting health (default: 100)
   - `speed_max`: Movement speed (default: 500.0)
   - `jump_force`: Jump strength (default: -1200)

**Step 6: Update Attack Data**
1. In `characters/playable/your_character/resources/attacks/`:
   - `punch1_attack_data.tres`: First attack damage/knockback
   - `punch2_attack_data.tres`: Second attack damage/knockback
   - `punch3_attack_data.tres`: Third attack damage/knockback
   - `air_kick_attack_data.tres`: Air attack damage/knockback

2. Edit each file's properties:
   - `attack_damage`: Damage dealt
   - `hurt_type`: 1 for mid, 2 for high
   - `knockback`: Strength (0-7 scale)
   - `launch_angle`: Knockback angle in degrees

**Step 7: Update Scene Files**
1. Rename scene files to match your character
2. Update script references in the scene files
3. Update the skin scene reference in the main character scene

**Step 8: Create Character Profile**
1. Create a profile image (like `chad_profile.png`)
2. Create a health bar gradient texture if desired
3. Update the attributes file to reference these assets

#### For Enemy Characters (Using Sargent as template):

Follow the same process but:
1. Use `characters/enemies/sargent/` as your template
2. Enemy characters have AI state machines that control behavior
3. Copy the AI script and modify behavior patterns as needed
4. Enemies don't have air attacks but may have special abilities

### Animation Requirements:

**Frame Rates:**
- Idle: 8 FPS
- Walk: 24 FPS  
- Attacks: 24 FPS
- Jump: 24 FPS
- Hurt: 5 FPS

**Looping:**
- Idle, Walk, Hurt: Loop = true
- Attacks, Jump, Knockout: Loop = false

### Final Steps:

1. **Test Your Character**: 
   - Open the main scene in Godot
   - Play the game and test all animations
   - Use debug scenes in `debug_tools/test_scenes/` for focused testing

2. **Adjust Collision**: 
   - Modify the CapsuleShape2D in the character scene if needed
   - Update the collision size to match your sprite dimensions

3. **Update Main Menu**:
   - Replace character portraits in the main menu
   - Update any character selection screens

4. **Create Variations**:
   - For left/right facing animations, the system auto-generates mirrored versions
   - You only need to create sprites facing one direction

**Note**: The system automatically handles mirroring for left/right directions, so you only need to create sprites facing one direction. The process maintains all gameplay mechanics and state machine functionality while allowing complete visual customization.

## Character Creation Task List

### Pre-Production Phase

#### Art Asset Planning
- [ ] **Character Concept Design**
  - [ ] Create character concept art and reference sheets
  - [ ] Define character personality and fighting style
  - [ ] Plan color palette and visual theme
  - [ ] Determine sprite resolution (recommend 128x128 or 256x256)

- [ ] **Animation Planning**
  - [ ] Sketch keyframes for each required animation
  - [ ] Plan timing and frame counts for each animation
  - [ ] Consider character-specific special moves or abilities
  - [ ] Design idle pose and personality animations

#### Technical Setup
- [ ] **Project Structure**
  - [ ] Decide if creating playable character or enemy
  - [ ] Choose base template (Chad for playable, Sargent for enemy)
  - [ ] Plan unique abilities or mechanics for this character
  - [ ] Determine character stats and balance

### Asset Creation Phase

#### Sprite Creation Checklist
**Required for ALL Characters:**
- [ ] **Idle Animation** (3-4 frames at 8 FPS)
  - [ ] idle_00.png - Primary idle pose
  - [ ] idle_01.png - Subtle movement/breathing
  - [ ] idle_02.png - Return to primary
  - [ ] idle_03.png - (Optional) Additional frame

- [ ] **Walk Cycle** (8-12 frames at 24 FPS)
  - [ ] walk_00.png through walk_11.png
  - [ ] Ensure smooth looping cycle
  - [ ] Test walk speed matches game movement

- [ ] **Direction Change** (2-3 frames at 24 FPS)
  - [ ] turnaround_00.png - Start of turn
  - [ ] turnaround_01.png - Mid turn
  - [ ] turnaround_02.png - End of turn

- [ ] **Damage Reactions** (1 frame each at 5 FPS)
  - [ ] hurt_mid.png - Mid-level damage reaction
  - [ ] hurt_high.png - High-level damage reaction
  - [ ] die.png - Death state frame

- [ ] **Jump Sequence** (4 frames at 24 FPS)
  - [ ] jump_01.png - Jump impulse/takeoff
  - [ ] jump_02.png - Rising in air
  - [ ] jump_03.png - Falling down
  - [ ] jump_04.png - Landing recovery

- [ ] **Knockout Sequence** (6 frames at 24 FPS)
  - [ ] knockout_00.png - Launch frame
  - [ ] knockout_01.png - Rising in air
  - [ ] knockout_02.png - Falling down
  - [ ] knockout_03.png - First bounce
  - [ ] knockout_04.png - Second bounce
  - [ ] knockout_05.png - Lying on ground

**Additional for PLAYABLE Characters:**
- [ ] **Attack Combo** (Variable frames at 24 FPS)
  - [ ] punch1_00.png, punch1_01.png - Light attack (2-6 frames)
  - [ ] punch2_00.png through punch2_10.png - Medium attack (6-11 frames)
  - [ ] punch3_00.png through punch3_16.png - Heavy attack (10-17 frames)

- [ ] **Air Attack** (2-4 frames at 24 FPS)
  - [ ] air_attack_00.png, air_attack_01.png - Aerial combat move

**Additional for ENEMY Characters:**
- [ ] **Enemy Attacks** (Variable frames at 24 FPS)
  - [ ] attack1_00.png through attack1_05.png - Basic attack
  - [ ] attack2_00.png through attack2_10.png - Strong attack
  - [ ] attack3_00.png through attack3_16.png - Special attack

- [ ] **Recovery Animation** (2-3 frames at 24 FPS)
  - [ ] getting_up_00.png, getting_up_01.png - Getting up from ground

#### Profile and UI Assets
- [ ] **Character Profile**
  - [ ] Create character portrait (128x128 recommended)
  - [ ] Design health bar gradient (optional)
  - [ ] Create character icon for selection screens

### Implementation Phase

#### File Structure Setup
- [ ] **Create Directory Structure**
  ```bash
  characters/[playable|enemies]/your_character/
  ├── resources/
  │   ├── sprites/
  │   ├── animations/
  │   └── attacks/
  ├── your_character.tscn
  ├── your_character.gd
  ├── your_character_skin.tscn
  └── your_character_skin.gd
  ```

- [ ] **Copy Base Template Files**
  - [ ] Copy appropriate base scene (Chad/Sargent)
  - [ ] Copy base script files
  - [ ] Copy animation resources
  - [ ] Copy attack data resources

#### Godot Implementation
- [ ] **Import Sprites**
  - [ ] Import all sprite files into Godot
  - [ ] Configure import settings (Filter: Off, Mipmaps: Off)
  - [ ] Organize sprites in proper folders

- [ ] **Setup SpriteFrames Resource**
  - [ ] Open character's SpriteFrames resource
  - [ ] Create animation tracks for each sprite category
  - [ ] Add sprites to appropriate animations
  - [ ] Configure frame duration and looping settings
  - [ ] Test all animations in Animation preview

- [ ] **Configure Character Attributes**
  - [ ] Update character name and display properties
  - [ ] Set health, speed, and jump values
  - [ ] Configure character profile texture path
  - [ ] Adjust physics properties if needed

- [ ] **Setup Attack Data**
  - [ ] Configure damage values for each attack
  - [ ] Set hurt types (mid=1, high=2)
  - [ ] Define knockback strength and angles
  - [ ] Test attack timing and feel

#### Scene Configuration
- [ ] **Update Character Scene**
  - [ ] Rename scene file to match character
  - [ ] Update script references
  - [ ] Configure collision shapes to match sprite size
  - [ ] Test character physics and movement

- [ ] **Update Skin Scene**
  - [ ] Link SpriteFrames resource
  - [ ] Configure animation tree if needed
  - [ ] Test sprite mirroring for left/right movement

### Testing and Polish Phase

#### Functionality Testing
- [ ] **Basic Movement**
  - [ ] Test left/right movement
  - [ ] Verify jump mechanics
  - [ ] Check collision detection
  - [ ] Test direction changes

- [ ] **Combat Testing**
  - [ ] Test all attack animations
  - [ ] Verify damage dealing and receiving
  - [ ] Check knockback and hurt reactions
  - [ ] Test combo system (for playable characters)

- [ ] **Animation Quality**
  - [ ] Review all animations for smoothness
  - [ ] Check for proper looping
  - [ ] Verify frame timing feels good
  - [ ] Test visual feedback for player actions

#### Integration Testing
- [ ] **Game Integration**
  - [ ] Test character in actual gameplay
  - [ ] Verify AI behavior (for enemies)
  - [ ] Check character balance against others
  - [ ] Test in different stages/environments

- [ ] **UI Integration**
  - [ ] Update character selection screens
  - [ ] Test health bar display
  - [ ] Verify profile images show correctly
  - [ ] Check main menu character portraits

#### Polish and Optimization
- [ ] **Performance**
  - [ ] Optimize sprite file sizes
  - [ ] Check memory usage
  - [ ] Test on target platforms
  - [ ] Verify consistent framerate

- [ ] **Audio Integration**
  - [ ] Add character-specific sound effects
  - [ ] Configure voice/grunt sounds
  - [ ] Test audio timing with animations
  - [ ] Balance audio levels

### Finalization Phase

#### Documentation
- [ ] **Character Documentation**
  - [ ] Document character stats and abilities
  - [ ] Record attack frame data
  - [ ] Note any special mechanics
  - [ ] Create character guide for players

#### Version Control
- [ ] **Source Control**
  - [ ] Commit all character assets
  - [ ] Tag character completion milestone
  - [ ] Document changes in commit messages
  - [ ] Backup original template files

#### Release Preparation
- [ ] **Quality Assurance**
  - [ ] Full playthrough with new character
  - [ ] Cross-platform testing
  - [ ] Performance benchmarking
  - [ ] Final balance adjustments

### Character-Specific Considerations

#### For Unique Characters
- [ ] **Special Abilities**
  - [ ] Design unique moves or abilities
  - [ ] Create custom action scripts
  - [ ] Implement special input combinations
  - [ ] Balance against existing roster

- [ ] **Advanced AI (Enemies)**
  - [ ] Create custom AI states
  - [ ] Define behavior patterns
  - [ ] Set aggression and difficulty levels
  - [ ] Test AI decision making

#### For Character Variants
- [ ] **Palette Swaps**
  - [ ] Create color variations
  - [ ] Maintain readability and contrast
  - [ ] Test in different lighting conditions
  - [ ] Update character selection UI

This comprehensive task list ensures no critical steps are missed when creating new characters for the beat-em-up template. Each checkbox represents a concrete, actionable task that contributes to a polished, fully-functional character.
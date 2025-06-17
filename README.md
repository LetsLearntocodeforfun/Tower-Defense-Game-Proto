# Forest Guardians - Tower Defense

A fun little tower defense game where you protect the forest from dark spirits using elemental guardians. Features Ghibli-inspired art, multiple tower and enemy types, upgrades, and achievements.

## Table of Contents

- [Gameplay](#gameplay)
- [Features](#features)
- [Technical Overview](#technical-overview)
- [How to Run](#how-to-run)
- [File Structure](#file-structure)
- [Potential Future Enhancements](#potential-future-enhancements)

## Gameplay

The primary objective in Forest Guardians is to successfully defend the ancient forest through all 10 stages of encroaching dark spirits, culminating in the defeat of the final boss on the 50th wave. Players must strategically place various elemental guardian towers along the spirits' path to prevent them from reaching the forest's heart and depleting your Life Force.

### Core Mechanics

*   **Placing Towers:**
    *   Select a tower type from the "Forest Guardians" menu on the right side of the screen (e.g., 🌿 Nature Spirit, ✨ Magic Crystal). This can be done by clicking one of the tower buttons like "🌿 Nature Spirit ($60)".
    *   Once a tower type is selected, your cursor will change. Click on a valid, unoccupied spot on the game map to place the tower. Placing towers costs Spirit Gems.
    *   You cannot place towers directly on the enemy path or too close to other towers. Valid positions are checked by `isValidTowerPosition`.
*   **Game Progression and Waves:**
    *   The game is structured into 10 distinct stages, each presenting a greater challenge than the last.
    *   Each stage consists of 5 waves of enemies: 4 normal waves followed by 1 significantly more challenging boss wave.
    *   Waves are initiated by clicking the "🌊 Summon Wave" (or "Summon Next Wave" / "Wave in Progress..." when active) button in the tower menu. This action is handled by the `startWave()` function.
    *   Both stages and the waves within them become progressively more difficult, introducing tougher enemies or larger numbers.
    *   Boss waves feature unique and powerful boss enemies (e.g., larger, faster, more health) that serve as a major test at the end of each stage. Defeating all 10 stages, including the final boss on the 50th wave, is the ultimate goal.
    *   Be prepared! Enemies will start moving along the path once a wave is summoned.
*   **Upgrading and Managing Towers:**
    *   Click directly on a tower you've already placed on the map. This action, handled by `selectTowerForUpgrade(x,y)`, will open the "Tower Upgrades" panel (`upgradePanel`).
    *   Here, you can spend Spirit Gems to enhance the selected tower's damage, range, attack speed, or unlock special abilities, using the `upgradeTower(upgradeType)` function.
    *   The panel also allows you to sell a tower for a partial refund of its cost via the `sellTower()` function.
    *   Right-click on the map or select "Close" in the upgrade panel to deselect a tower or close the panel.

### Key UI Elements

*   **Main Status Display (Top-Left - `ui` div):**
    *   **Life Force (`health`):** Your remaining health. If it reaches zero, the game is over.
    *   **Spirit Gems (`gold`):** Your currency for buying and upgrading towers. Earned by defeating enemies and completing waves.
    *   **Wave (`wave`):** The current wave number.
    *   **Dark Spirits (`enemies`):** Shows the number of active enemies and progress of the current wave (e.g., "0 (K:0/0)").
    *   **XP & Level (`experience`, `level`):** Player experience points and current level.
    *   **Combo (`combo`):** Current enemy defeat combo (e.g., "0x").
    *   **Weather (`weatherStatus`, `weatherIcon`):** Current weather conditions affecting gameplay (e.g., "Sunny ☀️").
    *   **Music Controls (`muteBtn`):** Allows muting/unmuting background music.
*   **Tower Menu (Top-Right - `towerMenu` div):**
    *   Headed by "🌸 Forest Guardians 🌸".
    *   Lists available tower types with their costs (e.g., "🌿 Nature Spirit ($60)", "✨ Magic Crystal ($120)"). Clicking these calls `selectTower('type')`.
    *   Contains the button to start new waves (`waveBtn`).
    *   Includes a tip: "Click towers to upgrade them! (R-Click to cancel)".
*   **Upgrade Panel (Bottom-Left - `upgradePanel` div, appears when a tower is selected):**
    *   Displays "Tower Upgrades" for the selected tower, including its current level.
    *   Shows tower statistics like Kills and Total Damage.
    *   Lists available upgrades (e.g., Damage, Range, Speed) with their current level, max level, and cost in Spirit Gems. Buttons are provided for each upgrade.
    *   Provides an option to "Sell" the selected tower for a partial refund.
    *   Includes a "Close" button.
*   **Game Over Screen (Center - `gameOver` div, appears on defeat):**
    *   Displays "🌙 The Forest Rests 🌙".
    *   Shows final wave reached (`finalWave`), experience gained (`finalExp`), and highest combo (`finalCombo`).
    *   Option to restart the game ("🌱 Guardian Reborn") via the `restartGame()` function.
*   **Achievement Notifications (Pop-up from right - `achievement` div):**
    *   Alerts for unlocking achievements (e.g., "🎯 First Blood!"). Managed by `showAchievement(text)`.
*   **Daily Quest Panel (Bottom-Right - `dailyQuestPanel` div):**
    *   Displays "📜 Daily Quest" and the current quest objective (e.g., "Vanquish 10 Goblins!").

## Features

Forest Guardians boasts a variety of features to enhance the gameplay experience:

*   **Diverse Tower Types:** Command a selection of unique elemental towers, each defined in the `towerTypes` object:
    *   **🌿 Nature Spirit:** Versatile towers that can be upgraded with abilities like a healing aura for nearby towers (`upgrades.healing`).
    *   **✨ Magic Crystal:** Powerful offensive towers, featuring upgrades for chain lightning attacks (`upgrades.chain`) and slowing enemies (`upgrades.slow`).
    *   **💨 Wind Dancer:** Fast-attacking towers, capable of knocking enemies back (`upgrades.knockback`) and landing critical hits (`upgrades.crit`).
    *   **🏔️ Earth Guardian:** Heavy-hitting towers that can be upgraded to deal splash damage (`upgrades.splash`) and stun enemies (`upgrades.stun`).
*   **Varied Enemy Waves:** Face a horde of dark spirits, defined in `enemyTypes`, with different attributes and abilities:
    *   Includes Imps (`imp`), Goblins (`goblin` - fast), Orcs (`orc` - tanky), Trolls (`troll` - regenerating health), Shadows (`shadow` - stealthy), Phantoms (`phantom` - phasing ability), Golems (`golem` - armored), and formidable Dragons (`dragon` - boss).
*   **Structured Campaign with Boss Battles:** Progress through a 10-stage campaign, where each stage consists of 5 waves: 4 waves of increasingly challenging standard enemies, capped off by a formidable 5th wave featuring a unique boss. These bosses vary in their characteristics (e.g., some are exceptionally large and durable, others small and swift, or possess unique resistances), requiring adaptive strategies. Conquering all 10 stages, culminating in the defeat of the final boss on the 50th wave, marks the ultimate victory.
*   **In-depth Tower Upgrades:** Each tower (managed by the `Tower` class) has multiple upgrade paths defined in `towerTypes[towerType].upgrades`. These allow you to enhance core stats like damage, range, attack speed, and unlock special abilities tailored to its element by spending Spirit Gems.
*   **Rich Visual Effects:** The game utilizes a `Particle` class to generate various visual effects, including:
    *   Projectile trails and impacts (e.g., `Projectile.draw()`, `Projectile.hit()`).
    *   Tower actions like placement and level-ups.
    *   Enemy destruction effects.
    *   Power-up collection (`createBurstParticles`).
    *   Environmental effects related to weather (`drawWeatherEffects`).
*   **Helpful Power-Ups:** Collect randomly spawning power-ups (`PowerUp` class, `powerUpTypes` object) during waves:
    *   **💰 Gem Burst:** Instantly grants a sum of Spirit Gems (e.g., "+50G").
    *   **❄️ Frost Nova:** Applies a temporary slow effect to all enemies currently on the map.
    *   **⚔️ Guardian's Wrath:** Temporarily boosts the damage output of all placed towers.
*   **Dynamic Weather System:** Experience changing weather conditions (`gameState.weather`, `changeWeather()` function) like Sunny, Rainy, and Windy. These aren't just cosmetic; they can have subtle impacts on gameplay (e.g., rain can slightly slow enemies (`Enemy.update`), wind can affect Wind Dancer tower's projectile speed (`Projectile` constructor) or range (`Tower.currentRange` for wind towers in rain)).
*   **Achievement System:** Unlock various achievements (`checkAchievements()`, `showAchievement()`) for completing specific milestones and challenges, such as "🎯 First Blood!", "🌊 Wave 5 Cleared!", or "🐉 Dragon Slayer!".
*   **Daily Login Rewards:** Receive a bonus stash of Spirit Gems for playing the game daily, managed by `checkDailyLoginReward()` and stored via `localStorage`.
*   **Combo Counter:** String together enemy defeats to build up a combo meter (`gameState.combo`). Higher combos reward bonus Spirit Gems, as seen in `Projectile.onEnemyKilled()`.
*   **Audio Experience:** The game includes thematic background music (`backgroundMusic` audio element) and sound effects for game actions (e.g., `waveStartSoundEffect`). A mute/unmute button (`muteBtn`, `toggleMusic()` function) provides audio control.

## Technical Overview

*   **Core Technologies:** The game is built with HTML for structure, CSS for styling (inline and via `index.css`), and vanilla JavaScript for all interactive logic.
*   **Game Logic Implementation:** The core game logic—entity management (towers, enemies, projectiles), gameplay mechanics (waves, placement, upgrades), UI updates, and event handling—resides in a large `<script>` tag within `Index.html`.
*   **Rendering:** Visuals are rendered on two HTML5 Canvas elements:
    *   `gameCanvas`: For the main game elements (path, towers, enemies).
    *   `particles`: For particle effects (projectiles, impacts, weather), overlaid on the main canvas.
*   **TypeScript (`index.tsx`):** An `index.tsx` file exists, and `Index.html` includes a placeholder `<script type="module" defer>` for it (currently empty: `export {};`). This suggests an initial setup or future intent to use TypeScript for modularization or its other features, a plan not yet fully implemented. The game currently runs on the JavaScript within `Index.html`.
*   **Assets:** Audio assets (e.g., `wave_start.mp3`) are loaded via HTML `<audio>` elements. Visuals are procedurally drawn on the canvas.

## How to Run

1.  **Clone or Download:**
    *   Clone this repository to your local machine using Git:
        ```bash
        git clone <repository_url>
        ```
    *   Alternatively, download the project files as a ZIP archive and extract them.
2.  **Navigate to Directory:**
    *   Open your terminal or file explorer and navigate to the root directory where you cloned or extracted the project files.
3.  **Open `Index.html`:**
    *   Locate the `Index.html` file in the project directory.
    *   Double-click the file, or right-click and choose "Open with" your preferred web browser (e.g., Google Chrome, Mozilla Firefox, Microsoft Edge, Safari).

That's it! The game should load and start automatically in your browser window, and you can begin playing. No special servers or build steps are required.

## File Structure

The project includes the following key files:

*   `Index.html`: The main game file. It provides the HTML structure, canvas elements, and embeds the core JavaScript game logic and CSS styles (inline or linked).
*   `index.css`: A CSS file linked by `Index.html`, containing additional styles for UI elements.
*   `index.tsx`: A TypeScript file. Its current role is minimal, as the primary game logic is in `Index.html`. It's configured in an `importmap` and referenced by a deferred module script, suggesting potential for future modular development.
*   `README.md`: This document, offering an overview, gameplay details, technical information, and setup instructions.
*   `metadata.json`: Contains project metadata (name, description) used for this README's initial content.
*   `wave_start.mp3`: (Referenced in `Index.html`) An audio file for sound effects and background music. *Note: This file was not listed in the initial file tree but is essential for game audio.*

## Potential Future Enhancements

While Forest Guardians is a playable and enjoyable game in its current state, here are some ideas for future expansion and refinement:

*   **Expanded Content:**
    *   **More Tower Types:** Introduce new elemental towers with unique abilities, attack styles, and upgrade paths.
    *   **More Enemy Variety:** Design new dark spirits with distinct characteristics, resistances, and challenging boss encounters.
    *   **Multiple Levels/Maps:** Create different game maps with varying path layouts, tower placement challenges, and visual themes.
*   **Deeper Gameplay Mechanics:**
    *   **Player Abilities/Spells:** Introduce active player abilities or spells that can be used to support towers or directly impact enemies.
    *   **Advanced Tower Specializations:** Allow towers to specialize into distinct advanced forms upon reaching a certain upgrade level.
    *   **Interactive Map Elements:** Add elements on the map that players or enemies can interact with.
*   **Progression and Persistence:**
    *   **Save/Load Game State:** Implement functionality to save current game progress (wave, gold, towers) and load it later, possibly using browser `localStorage`.
    *   **Player Profile & Skill Tree:** Introduce a persistent player profile with experience that unlocks global upgrades or new towers/abilities over multiple playthroughs.
*   **User Experience & Interface:**
    *   **Game Speed Control:** Add options to speed up or slow down the game.
    *   **Difficulty Levels:** Implement different difficulty settings to cater to various player skill levels.
    *   **More Detailed Stats:** Provide more comprehensive post-game statistics or in-game performance metrics.
*   **Social and Competitive Features:**
    *   **Online Leaderboards:** Add a system to track high scores (e.g., waves survived, total damage dealt), fostering competition.
*   **Narrative Development:**
    *   **Expanded Story/Lore:** Develop the narrative around the Forest Guardians, the origin of the dark spirits, and the world they inhabit.
*   **Technical Refinements:**
    *   **Code Modularization:** Progressively refactor the large JavaScript block in `Index.html` by moving logic into separate modules, potentially leveraging the `index.tsx` file and TypeScript for better organization, maintainability, and type safety.
    *   **Performance Optimization:** Further optimize game logic, especially rendering and collision detection, to handle a larger number of units smoothly.
    *   **Asset Management:** Improve how assets (especially audio) are loaded and managed.
*   **Additional Features:**
    *   **New Power-Ups and Special Events:** Introduce more types of temporary boosts or random in-game events to add dynamism and replayability.
    *   **Customizable Game Modes:** Allow players to set up custom challenges (e.g., specific enemy waves, limited tower types).

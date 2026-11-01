# Random Heroes PvP

A self-contained, no-install, real-time browser multiplayer fantasy arena for **2–8 players**. No external npm packages or database required. Rooms and character progress are kept only in server memory. All gameplay is server-authoritative.

## Run locally

Requires Node.js 20 or newer.

```bash
npm start
```

Open `http://localhost:3000` in a browser. To test multiplayer, open a second tab, create a room, and join using its six-character code. Devices on the same local network can use `http://YOUR_COMPUTER_LAN_IP:3000` if the firewall allows it.

## Deploy to a public HTTPS URL

1. Push the folder to a Git repository.
2. Deploy it to a Node.js web service (for example, Render, Railway, or Fly.io). Set the start command to `npm start` and make sure the service forwards its `PORT` environment variable.
3. Enable public HTTPS and WebSocket upgrades (WSS). Use **one running instance**: rooms are held in process memory and are not shared across replicas.
4. Open the resulting HTTPS URL, create a room, and send the copied invite link to friends. Each browser connects directly to the same game server over WebSocket.

**Important:** A static-only host (such as GitHub Pages) will not run this game. A persistent Node.js service with WebSocket support is required. This package does **not** create a public deployment automatically.

## Game rules

- Host creates a room; 2–8 players join using room code, choose from three random heroes, then host starts.
- Everyone begins at Level 1 with 100 HP, 100 Mana, and a basic class weapon. No accounts, permanent progress, or respawns.
- First 2 minutes: PvP damage is disabled. Defeat monsters for XP and Gold. At level up choose one of three random buffs within five seconds.
- Manual movement, aiming, and attacks. Each hero has two abilities. Collect weapons on the ground, upgrade weapons and armor at two shop stations, and purchase health and mana potions.
- The arena begins shrinking after eight minutes. Last surviving player wins. At the time limit, storm damage increases and the remaining survivor wins; if multiple players survive the final cutoff, highest remaining HP wins.
- Eliminated players can spectate or leave. A disconnected player can reconnect within 60 seconds from the same browser session.

## Controls

Desktop: WASD or arrow keys move; mouse aims; hold left mouse to attack; Q/E abilities; 1/2 potions; F pickup; B shop. Mobile: left movement joystick, right aim/fire joystick, and touch action buttons.

## Operational notes

This is a playable MVP rather than a hardened commercial game. It does not yet include account persistence, anti-DDoS measures, distributed room storage, advanced anti-cheat, production analytics, or automated load testing. Server restarts end active rooms. WebSocket state is uncompressed and the server is intended for small private matches. The user interface is optimized for modern browsers.

## 3D edition
The arena now uses WebGL / Three.js with 3D hero bodies, heads, hats, weapons, monsters, projectile orbs, treasure, terrain, shadows and an angled follow camera. Game rules and the multiplayer server are unchanged. The client loads Three.js from jsDelivr; an internet connection is required on client devices, even when testing on localhost. Use a current browser with WebGL enabled. If you see a blank arena, check browser developer console for CDN/network/WebGL errors.

## Latest update: camera, balancing, equipment and map
- Click inside the arena to capture the mouse. Move mouse to look in **both** first-person and third-person modes. Press **V** to switch views, **Esc** to release the cursor. WASD movement follows the camera direction.
- Heroes are now assigned **randomly and automatically** on joining a room; no hero-selection step.
- Arena expanded from 180 × 140 world units to 240 × 200; player spawn positions and shop stations moved accordingly.
- Monsters spawn slowly early on (about every 6.5 seconds), ramping up toward the final minutes (about every 1.5 seconds), with a growing active-monster cap. Their damage and movement speed are reduced.
- Visible 3D weapon meshes vary by equipped weapon; armor upgrades show increasingly prominent armor plates.
- Note: camera input uses browser pointer lock and requires a click on the arena.


## 10-minute deluxe update
- Match timer is 10 minutes, with PvP enabled after 60 seconds.
- Storm starts at minute 4, tightens at minute 7, and closes at minute 10.
- Monster spawn interval begins at 17 seconds and accelerates to 5.5 seconds at the end; active-monster cap is reduced. Normal monsters deal 2 damage, strong monsters 5.
- Only two fixed golden shop stations support purchases. Stand within 115 world units and press B / SHOP. The server validates location on every purchase.
- Shop landmarks have golden circular interaction zones, illuminated roof markers, and counters.
- Character models have arms, legs, boots, belts, emblems, and upgraded armor; equipped weapons are larger. HUD includes equipment levels.
- All combat, inventory and health are server-authoritative.
- This remains a lightweight procedural 3D art style, not photorealistic assets.

## Exploration & combat update
- Player name is mandatory before hosting or joining (checked by client and server).
- Mouse horizontal look direction corrected. Click arena to lock pointer; Esc unlocks.
- Arena expanded to 600 x 500 visible world units (6000 x 5000 server coordinates).
- Distance fog and camera far-plane limit what can be seen as you explore; this is distance-based rendering, not persistent Minecraft chunk generation.
- First 30 seconds: server-enforced spawn shield, displayed with a blue bubble and HUD countdown. PvP unlocks at 30 seconds, and the shield expires then.
- More trees, stones, ancient columns, equipment details, rotating weapon pickups, and visible shield bubbles.
- Weapon pickups display actual weapon silhouettes; health/mana potions are purchased at shops and displayed in the HUD, not as ground weapon pickups.

## Equipment, shield, and spectator update
- Each hero starts with an empty single weapon slot. Move near a weapon to auto-equip when empty; press F near another weapon to replace it. There is no weapon inventory or dual-wielding.
- Warriors may pick up a shield with F in a separate off-hand slot. Hold R to block incoming damage (5% at level 1, increasing 5 percentage points per upgrade to a maximum of 20% at level 4). Shield blocking does not deal damage and disables weapon attacks while raised. Initial shield cooldown is 5 seconds after pickup, and a 5-second cooldown begins when the shield is lowered.
- Shields can only be upgraded at marked shops (120/220/320 gold for levels 2/3/4). Other classes cannot equip shields.
- The home page has an expandable game guide; players can surrender during a match to spectate. Mouse pointer lock is released on elimination and match end.
- The 10-minute final storm timing was corrected so the final damage phase starts at 9:50, and the match ends by 10:00 if no winner is determined earlier.

### Automatic weapon pickup
When unarmed, walk at least 12 world units and approach within 80 world units of a ground weapon to equip it automatically. Standing still near a weapon does not trigger pickup. Only one weapon may be equipped; use F to swap.

## Facing, weapons and spawning update
- Character bodies face their current mouse aim direction.
- Each ground weapon has a floating name and level label.
- Swapping a weapon with F drops the previous weapon on the floor nearby.
- Heroes spawn unarmed; left-click performs a short-range fist attack until they equip a weapon.
- Spawn points are randomized and spread across the map, with extra spacing in two-player matches.
- After 3, 6 and 10 monster kills, the server sends shop reminders; the HUD offers further reminders.

| Weapon | Damage | Attack delay (ms) | Reach |
|---|---:|---:|---:|
| Rusty Sword | 13 | 570 | 76 |
| Knight Longsword | 20 | 630 | 94 |
| Iron Cleaver | 25 | 850 | 82 |
| Hunter Bow | 15 | 680 | 780 |
| Swift Shortbow | 11 | 430 | 640 |
| War Bow | 23 | 950 | 980 |
| Oak Staff | 17 | 720 | 690 |
| Ember Staff | 23 | 900 | 730 |
| Crystal Wand | 12 | 420 | 560 |
| Twin Knives | 9 | 290 | 60 |
| Shadow Daggers | 13 | 360 | 67 |
| Assassin Blades | 17 | 470 | 73 |


## Player-scaled weapon spawning
Additional ground weapons spawn based on room size: 1 player ~110–122 seconds, 2 players ~100–112 seconds, 4 players ~80–92 seconds, 8 players ~40–52 seconds initially. Intervals shorten by up to 12 seconds near the end of the 10-minute match. Ground weapon limits scale with player count (3 per player, minimum 4, maximum 22). Each player still gets a starter weapon nearby, but starts unarmed and must walk over it to equip. Dropped/swapped weapons are still supported.

## Potion inventory and purchase notifications
- Maximum two health and two mana potions per player.
- Successful potion use consumes one and updates the HUD immediately on the next state update.
- Shop purchases/upgrades and failures send explicit in-game notifications, including insufficient gold, missing equipment, full inventory, and maximum levels.
- Monster potion drops respect the two-per-type limit.

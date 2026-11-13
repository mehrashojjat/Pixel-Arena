Pixel Arena

A lightweight, browser-based multiplayer shooting game where smartphones act as wireless controllers and a computer serves as the main game screen.

Players join by scanning a QR code, control their characters using two virtual joysticks, and compete in a shared arena.

The project is designed as a minimal, serverless multiplayer prototype that can run on GitHub Pages without deploying a custom backend.

1. Technology Stack

* HTML5: Application structure.
* CSS3: Interface styling and mobile joystick controls.
* Vanilla JavaScript: Game logic, rendering, input handling, and networking.
* HTML5 Canvas: Rendering the arena, players, bullets, and health bars.
* PeerJS: Simplified WebRTC peer-to-peer connections.
* WebRTC DataChannels: Real-time communication between smartphones and the host computer.
* QR Code: Allows smartphones to connect to the host session.
* GitHub Pages: Static website hosting.

External Libraries

PeerJS

Official website: https://peerjs.com/

GitHub repository: https://github.com/peers/peerjs

PeerJS simplifies WebRTC connection establishment and data transmission between browsers.

It uses a signaling service to establish connections. Once connected, gameplay data is exchanged directly through WebRTC DataChannels whenever network conditions permit.

QR Code Library

Used to generate a scannable connection link for mobile players.

The exact library and CDN dependency should be verified in index.html before modifying or upgrading it.

2. Project Structure

pixel-arena/
├── index.html
└── README.md

The entire application is contained in a single HTML file.

index.html includes:

* HTML structure
* CSS styling
* JavaScript game logic
* Canvas rendering
* PeerJS networking
* Mobile joystick controls
* Connection management

There is no build process, package manager, or backend application.

3. Multiplayer Architecture

The game uses a host-authoritative peer-to-peer architecture.

Host Computer

The host computer is responsible for:

* Creating the multiplayer session.
* Accepting incoming WebRTC connections.
* Assigning unique player identifiers.
* Assigning player colors.
* Maintaining the game state.
* Updating player positions.
* Creating and moving projectiles.
* Detecting collisions.
* Calculating damage.
* Managing player deaths and respawns.
* Rendering the game arena.

Mobile Clients

Smartphones function as controllers.

Each mobile client:

* Connects to the host through PeerJS.
* Receives its player identity and assigned color.
* Displays two virtual joysticks.
* Sends movement and shooting input to the host.
* Displays death and respawn information.

The smartphones do not independently simulate the entire game world.

Communication Flow

             HOST COMPUTER
             ┌─────────────┐
             │             │
             │ Game State  │
             │ Physics     │
             │ Collisions  │
             │ Rendering   │
             │             │
             └──────┬──────┘
                    │
              WebRTC / PeerJS
                    │
       ┌────────────┼────────────┐
       │            │            │
   ┌───▼───┐    ┌───▼───┐    ┌───▼───┐
   │Phone 1│    │Phone 2│    │Phone N│
   │       │    │       │    │       │
   │Inputs │    │Inputs │    │Inputs │
   └───────┘    └───────┘    └───────┘

The host is the authoritative source of truth.

Clients send control inputs rather than directly modifying game state.

4. Player Controls

Each smartphone displays two virtual joysticks.

Left Joystick — Aim and Shoot

* Drag to choose the shooting direction.
* The direction determines the projectile trajectory.
* Shooting continues while the joystick is held away from its center.
* Releasing the joystick stops shooting.

Right Joystick — Movement

* Drag to move the player’s square.
* Movement direction follows the joystick direction.
* Releasing the joystick stops movement.

Both joysticks must support multitouch so players can move and shoot simultaneously.

Touch handling must continue working when a finger moves outside the original joystick boundary.

5. Player Identification and Colors

Each connected player receives:

* A unique player identifier.
* A distinct player color.
* A corresponding colored square in the arena.
* Projectiles matching their player color.
* A health bar using the same color.

Player colors are automatically distributed using a golden-angle hue sequence.

The objective is to maximize visual distinction between players as additional connections are established.

Color distinction is approximate and cannot be guaranteed for an unlimited number of players.

6. Combat System

Players are represented by colored squares.

Each player can shoot smaller square projectiles.

Projectile Behavior

* Projectiles originate from the player’s position.
* Projectiles travel in the direction specified by the shooting joystick.
* Shooting continues while the joystick remains engaged.
* Projectile collisions are calculated by the host.

Health

Each player has a health bar displayed directly below their square.

* Maximum health: 100
* Damage per successful hit: 20
* Hits required for elimination: 5

The health bar decreases as the player takes damage.

Its color matches the player’s assigned color.

7. Death and Respawn

When a player’s health reaches zero:

1. The player is eliminated.
2. Their character becomes inactive.
3. Movement and shooting are disabled.
4. A 20-second respawn countdown begins.
5. The countdown is displayed on the player’s smartphone.
6. After 20 seconds, the player respawns with full health.

Respawn Rules

* Respawn delay: 20 seconds.
* Respawn health: 100%.
* Players cannot shoot or move while eliminated.
* The host controls the authoritative respawn timing.

The client-side countdown is a visual representation of the host-managed respawn state.

8. Hosting and Deployment

The application is designed for static hosting.

Recommended hosting:

GitHub Pages

https://pages.github.com/

Deployment

1. Create a GitHub repository.
2. Upload index.html and README.md.
3. Open the repository’s Settings.
4. Navigate to Pages.
5. Select the deployment branch.
6. Open the published website.

The host computer opens the website and starts the multiplayer session.

Other players scan the displayed QR code to connect.

Requirements

* A modern browser supporting WebRTC.
* JavaScript enabled.
* An internet connection for PeerJS signaling.
* Network conditions that permit WebRTC connections.
* HTTPS hosting for reliable browser compatibility.

No custom Node.js server, database, or WebSocket backend is required.

Important: Serverless hosting does not mean the application has no networking dependencies.

PeerJS signaling infrastructure is still required to establish WebRTC connections.

Depending on network conditions, some peer connections may require TURN relay infrastructure.

9. Performance and Scalability

The architecture is intended to support multiple simultaneous smartphone controllers.

The host computer performs the main simulation and rendering work.

Smartphones primarily transmit small control messages.

Performance Considerations

* Avoid transmitting rendered frames.
* Transmit joystick vectors rather than images.
* Keep the host authoritative.
* Minimize unnecessary network messages.
* Limit projectile lifetime.
* Remove inactive projectiles.
* Clean up disconnected peers.
* Avoid excessive object allocations inside the game loop.

Player Limits

The application should not impose an arbitrary player limit unless technically necessary.

However, the practical maximum depends on:

* Browser WebRTC capabilities.
* Available network bandwidth.
* Host computer performance.
* Number of simultaneous peer connections.
* Frequency of input messages.
* PeerJS signaling and connection reliability.

There is no guaranteed universal 32-player limit inherent to WebRTC.

The system should be tested under increasing player counts to determine practical capacity.

10. Important Instructions for AI Coding Agents

This section documents the intended architecture and constraints for future AI-assisted development.

Preserve the Single-File Architecture

The application is intentionally contained in index.html.

Unless explicitly requested:

* Do not introduce React, Vue, or another frontend framework.
* Do not introduce a build system.
* Do not require npm installation.
* Do not introduce a custom backend.
* Do not introduce a database.
* Do not replace PeerJS with another networking solution.

Preserve Host-Authoritative Gameplay

The host computer must remain responsible for:

* Player positions
* Projectile creation
* Collision detection
* Damage calculation
* Player health
* Death states
* Respawn timing

Mobile clients should primarily send controller input.

Do not allow clients to independently declare successful hits, change health values, or force respawns.

Preserve Mobile Controller Behavior

The controller layout must remain:

LEFT: AIM / SHOOT

RIGHT: MOVE

Both joysticks must:

* Support simultaneous touch input.
* Work reliably on iOS and Android.
* Support continuous directional input.
* Reset when released.
* Continue tracking a touch when it moves outside the joystick’s visual boundary.

Preserve Player Identity

Each player should retain their assigned identifier and color throughout their active session.

Their character, projectiles, and health bar must use consistent visual identification.

Preserve Respawn Rules

The respawn delay is 20 seconds.

The countdown must be visible on the eliminated player’s smartphone.

The host must remain responsible for determining when the player can respawn.

Avoid Unnecessary Complexity

This project prioritizes:

1. Minimal setup.
2. Browser compatibility.
3. Low-latency multiplayer input.
4. Reliable mobile controls.
5. Efficient host-side simulation.
6. Simple deployment.

When extending the project, prefer small, maintainable changes over introducing new infrastructure.

11. Potential Future Improvements

Possible future features include:

* Multiple game modes.
* Team-based gameplay.
* Player scores and leaderboards.
* Different weapons.
* Obstacles and collision boundaries.
* Power-ups.
* Spectator mode.
* Sound effects.
* Improved respawn animations.
* Better support for large player counts.
* Network latency monitoring.
* Connection recovery.
* Host migration.

These are potential improvements, not existing features.

12. Development Notes

This project is an experimental multiplayer prototype.

Its primary purpose is to demonstrate that multiple smartphones can control independent characters in a shared browser-based game using WebRTC, without deploying a dedicated game server.

The main computer acts as both the game display and authoritative simulation host.

The architecture is intentionally lightweight so that it can be extended, tested, and modified using AI coding assistants.

⸻

Project type: Browser-based multiplayer game
Architecture: Host-authoritative P2P
Networking: PeerJS / WebRTC
Rendering: HTML5 Canvas
Language: Vanilla JavaScript
Hosting: Static / GitHub Pages
Build process: None

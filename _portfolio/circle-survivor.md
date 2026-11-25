---
title: "Circle Survivor"
excerpt: "A Brotato-inspired survival game where you build up your arsenal between waves.<br/>**Tech Stack:** TypeScript, HTML5 Canvas, Koota ECS, Preact, Vite"
date: 2025-12-11
collection: portfolio
---

I built Circle Survivor as a browser-based take on Brotato. You move around the arena, dodge incoming attacks, and collect gold from defeated enemies. Between waves, you can spend that gold on weapons and upgrades for the next fight.

This was also a learning project for me. I relied heavily on GitHub Copilot’s agent mode to learn how to guide AI coding agents and manage their work. I also tried OpenCode and experimented with designing my own agent architectures. I rewrote the game several times, sometimes by hand. Those rewrites taught me a lot about working with AI-generated code and deciding when to step in and make changes myself.

One of my long-standing goals was to write a simple physics engine from scratch, and Circle Survivor gave me the chance to do it. The engine handles projectile motion, collisions between moving objects, and the push from explosions. Objects have mass, so the same impulse sends a lighter object flying faster than a heavier one.

The game uses Koota, an entity-component system for TypeScript. Enemies, projectiles, and pickups share basic components like position, while separate systems handle movement, collisions, damage, and rendering.

I also built tools for balancing the game. I can export the balance data to a spreadsheet, tweak values like weapon damage and fire rate side by side, and import the changes back into the game.

[Play Circle Survivor](https://petrzmax.github.io/Circle-Survivor/) · [View the source code](https://github.com/petrzmax/Circle-Survivor)

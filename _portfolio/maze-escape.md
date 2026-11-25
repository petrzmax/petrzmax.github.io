---
title: "Maze Escape"
excerpt: "A retro first-person horror game set in a procedurally generated maze. Built entirely with GitHub Copilot as an AI coding PoC.<br/>**Tech Stack:** Vanilla JavaScript, HTML5 Canvas, Web Audio API"
date: 2026-04-07
collection: portfolio
---

Maze Escape started as a proof of concept to see how quickly AI could turn a game idea into a playable prototype. GitHub Copilot generated the entire project using Claude Opus 4.6. A playable, nearly final version was ready about 30 minutes after I came up with the idea.

The goal is simple: find the exit before the enemy catches you. Each run generates a new maze with alternate routes, and the enemy uses breadth-first search to track you through its corridors.

My favorite part is the math behind the graphics. The renderer uses raycasting in vanilla JavaScript to turn a 2D grid into a textured first-person view, much like the original Wolfenstein 3D.

The game generates all its sounds in code using the Web Audio API. The heartbeat gets faster and louder as the enemy closes in, giving you an audible warning before it comes into view.

[Play Maze Escape](https://petrzmax.github.io/maze-escape/) · [View the source code](https://github.com/petrzmax/maze-escape)

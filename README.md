import React, { useEffect, useRef, useState } from "react";

export default function MiniBattleRoyale() {
  const canvasRef = useRef(null);

  const [screen, setScreen] = useState("menu"); // menu | game | gameover
  const [score, setScore] = useState(0);
  const [level, setLevel] = useState(1);

  useEffect(() => {
    if (screen !== "game") return;

    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");

    let player = { x: 250, y: 250, size: 15, speed: 4, hp: 100 };
    let bullets = [];
    let enemies = [];
    let animationId;
    let spawnRate = 2000;

    const shootSound = new Audio("https://actions.google.com/sounds/v1/impacts/wood_plank_flicks.ogg");
    const hitSound = new Audio("https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg");

    function spawnEnemy() {
      const size = 15;
      const x = Math.random() * (canvas.width - size);
      const y = Math.random() * (canvas.height - size);
      enemies.push({ x, y, size, speed: 1 + level * 0.3 });
    }

    function shoot() {
      bullets.push({ x: player.x, y: player.y, size: 5, speed: 6 });
      shootSound.play();

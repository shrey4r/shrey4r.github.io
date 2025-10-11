<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Happy Birthday ❤️</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      background-color: #1a0e0e;
      overflow: hidden;
    }

    body {
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
      filter: contrast(1.05);
    }

    canvas {
      width: 100vw;
      height: 100vh;
      object-fit: contain;
    }

    /* subtle grain overlay */
    .grain {
      pointer-events: none;
      position: absolute;
      width: 100%;
      height: 100%;
      background-image: url('https://grainy-gradients.vercel.app/static/noise.png');
      opacity: 0.12;
      mix-blend-mode: overlay;
      z-index: 10;
    }

    /* fade in / out */
    .fade {
      position: absolute;
      width: 100%;
      height: 100%;
      background-color: #1a0e0e;
      z-index: 20;
      transition: opacity 1.2s ease;
      opacity: 1;
    }

    .fade.hidden {
      opacity: 0;
      pointer-events: none;
    }

  </style>
</head>
<body>
  <canvas id="scene"></canvas>
  <div class="grain"></div>
  <div class="fade" id="fade"></div>

  <script>
    const canvas = document.getElementById("scene");
    const ctx = canvas.getContext("2d");
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    // simple parallax "camera shake"
    let t = 0;
    function drawScene(imgSrc) {
      const img = new Image();
      img.src = imgSrc;
      img.onload = function() {
        function animate() {
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          const shakeX = Math.sin(t * 0.05) * 1.5;
          const shakeY = Math.cos(t * 0.07) * 1.5;
          const x = canvas.width/2 - img.width/2 + shakeX;
          const y = canvas.height/2 - img.height/2 + shakeY;
          ctx.drawImage(img, x, y);
          t++;
          requestAnimationFrame(animate);
        }
        animate();
      };
    }

    // --- Scene system ---
    const scenes = [
      "cake1.png", // your cake frame (with candle)
      "cake2.png"  // next frame (after blowing)
    ];
    let currentScene = 0;

    const fade = document.getElementById("fade");
    function showScene(index) {
      fade.classList.remove("hidden");
      setTimeout(() => {
        drawScene(scenes[index]);
        fade.classList.add("hidden");
      }, 400);
    }

    // first scene
    showScene(currentScene);

    // advance on click/tap
    document.body.addEventListener("click", () => {
      currentScene = (currentScene + 1) % scenes.length;
      showScene(currentScene);
    });
  </script>
</body>
</html>

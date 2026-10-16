<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Happy Birthday Nicole 🎉</title>
  <style>
    :root {
      --bg1: #ffefef;
      --bg2: #fff8ff;
      --text: #1a1a1a;
    }
    html,body{
      height:100%;
      margin:0;
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background: linear-gradient(135deg,var(--bg1),var(--bg2));
      color:var(--text);
    }
    .wrap{
      min-height:100%;
      display:flex;
      align-items:center;
      justify-content:center;
      text-align:center;
      padding:32px;
      box-sizing:border-box;
    }
    h1{
      font-size: clamp(2.5rem, 6vw, 6rem);
      margin:0 0 0.3em;
      letter-spacing: -0.02em;
      text-shadow: 0 6px 18px rgba(0,0,0,0.08);
    }
    p{
      margin:0;
      font-size: clamp(1rem, 2.2vw, 1.5rem);
      color: #333;
    }
    .hint{
      margin-top:1.25rem;
      font-size:0.9rem;
      color:#555;
      opacity:0.9;
    }
    .replay {
      margin-top:1.2rem;
      display:inline-block;
      background:#fff;
      border:1px solid rgba(0,0,0,0.06);
      padding:0.5rem 0.9rem;
      border-radius:999px;
      cursor:pointer;
      box-shadow: 0 6px 18px rgba(0,0,0,0.06);
      font-weight:600;
    }
    .replay:active { transform: translateY(1px); }
    /* Make the whole page clickable for replay affordance */
    a.fullclick {
      position: absolute;
      inset: 0;
      text-decoration: none;
      z-index: 0;
    }
    /* Ensure confetti canvas sits above background but below text */
    #confetti-canvas {
      position: fixed;
      pointer-events: none;
      top:0;
      left:0;
      width:100%;
      height:100%;
      z-index: 0;
    }
    .content { z-index: 1; position: relative; }
  </style>
</head>
<body>
  <canvas id="confetti-canvas"></canvas>

  <a class="fullclick" href="#" aria-hidden="true"></a>

  <div class="wrap content" role="main">
    <div>
      <h1>Happy Birthday, Nicole! 🎉</h1>
      <p>Wishing you a day full of joy and surprises.</p>
      <div class="hint">Click anywhere to celebrate again</div>
      <div style="margin-top:10px">
        <button class="replay" id="replayBtn" aria-label="Celebrate again">Celebrate</button>
      </div>
    </div>
  </div>

  <!-- Lightweight confetti via CDN -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
  <script>
    // Use the canvas on the page (so the confetti aligns with our element)
    const myCanvas = document.getElementById('confetti-canvas');
    const myConfetti = confetti.create(myCanvas, { resize: true, useWorker: true });

    // Colors for confetti
    const COLORS = ['#ff0a54','#ff7b00','#ffd60a','#44d62c','#007bff','#7e5bef'];

    // Fire multiple bursts for a satisfying effect
    function fireConfetti() {
      const duration = 1800;
      const end = Date.now() + duration;

      // a few bursts from left and right over the duration
      (function frame() {
        myConfetti({
          particleCount: 8,
          startVelocity: 30,
          angle: 60,
          spread: 55,
          origin: { x: 0 },
          colors: COLORS
        });
        myConfetti({
          particleCount: 8,
          startVelocity: 30,
          angle: 120,
          spread: 55,
          origin: { x: 1 },
          colors: COLORS
        });

        // occasional center burst
        if (Math.random() > 0.6) {
          myConfetti({
            particleCount: 20,
            spread: 120,
            origin: { x: 0.5, y: 0.3 },
            colors: COLORS
          });
        }

        if (Date.now() < end) {
          requestAnimationFrame(frame);
        }
      }());
    }

    // Fire once on load, and again when user clicks or presses the button
    window.addEventListener('load', () => {
      // small delay so it feels like a surprise
      setTimeout(fireConfetti, 350);
      // announce to screen readers
      const live = document.createElement('div');
      live.setAttribute('aria-live', 'polite');
      live.style.position = 'absolute';
      live.style.width = '1px';
      live.style.height = '1px';
      live.style.overflow = 'hidden';
      live.style.clip = 'rect(1px, 1px, 1px, 1px)';
      live.textContent = 'Happy Birthday Nicole!';
      document.body.appendChild(live);
    });

    // Click anywhere to replay
    document.addEventListener('click', (e) => {
      // ignore clicks on links that would navigate away
      if (e.target && (e.target.tagName === 'A' && e.target.href)) return;
      fireConfetti();
    });

    document.getElementById('replayBtn').addEventListener('click', (e) => {
      e.stopPropagation();
      fireConfetti();
    });

    // Optional: allow space/enter to trigger
    document.addEventListener('keydown', (e) => {
      if (e.code === 'Space' || e.code === 'Enter') {
        fireConfetti();
      }
    });
  </script>
</body>
</html>

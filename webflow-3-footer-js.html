<!-- ============================================================
  TURRITO.AI — BLOCK 3 OF 3: FOOTER JAVASCRIPT
  Paste into: Webflow → Pages → (gear icon) → Custom Code → "Before </body> tag"
  ============================================================ -->
<script>
// scrollToScene must be global so onclick attributes can reach it
function scrollToScene(n) {
  const targets = ['#scene1','#scene2-wrapper','#scene3-s1-wrapper','#scene4-wrapper','#scene5-wrapper'];
  const el = document.querySelector(targets[n - 1]);
  if (el) el.scrollIntoView({ behavior: 'smooth' });
}

window.addEventListener('load', function () {
  setTimeout(function () {

  gsap.registerPlugin(ScrollTrigger);

  // ══════════════════════════════════════════
  // SCENE 1 — DNA HELIX
  // ══════════════════════════════════════════
  const canvas = document.getElementById('c');
  const ctx    = canvas.getContext('2d');
  let W, H, cx, cy;

  function resize() {
    W = canvas.width  = window.innerWidth;
    H = canvas.height = window.innerHeight;
    cx = W / 2; cy = H / 2;
  }
  resize();
  window.addEventListener('resize', resize);

  // ── Globals ───────────────────────────────────────────────────────────────────
  let t        = 0;
  let age      = 0;
  let textDone = false;
  let navDone  = false;

  // ── Assembly: mirrors v3 exactly ──────────────────────────────────────────────
  // v3 used a raw linear counter: assemblyProgress += 0.0038 per frame.
  // At 60fps that reaches 1.0 in ~263 frames = ~4.4 seconds.
  // We replicate that by treating age/4.4 as the raw input — no easing applied
  // to the progress value itself, matching v3's linear accumulation feel.
  let assemblyProgress = 0;
  const ASM_RATE     = 0.0038;              // same increment as v3 per frame
  const ASM_COMPLETE = 1.0 / (ASM_RATE * 60); // ~4.4s

  // Zip animation: zipper pull travels bottom→top after assembly completes
  let zipProgress  = 0.0;
  const ZIP_START  = 0.1;           // starts at 0.1s
  const ZIP_DUR    = 3.5;           // seconds to zip from bottom to top
  const ZIP_WINDOW = 0.08;          // smooth transition window around zip head

  // ── Timeline (seconds) ────────────────────────────────────────────────────────
  //
  // Assembly    0.0 → ~4.4s   Linear, +0.0038/frame (v3 exact)
  // Growth      0.0 → 7.5s    Single smooth curve, no kinks
  // Fly-in      2.0 → 7.0s    Camera moves inside as helix expands
  // Flash+text  5.2s           Midway through the final expansion phase
  // Nav         6.5s
  //
  // The final expansion phase runs roughly from 3.5s to 7.5s.
  // Midpoint of that = ~5.5s. Flash fires at 5.2s.

  const T_GROW_DUR    = 7.5;
  const T_FLYIN_START = 2.0;
  const T_FLYIN_DUR   = 5.0;
  const T_NAV         = 3.8;

  // ── Helix config ──────────────────────────────────────────────────────────────
  const POINTS       = 480;  // tripled — spheres pack tightly along each strand
  const COILS        = 3.0;  // fewer coils = spirals stretched further apart
  const RADIUS_START = 248;
  const RADIUS_END   = 248;  // locked — no horizontal expansion
  const RADIUS       = 248;  // fixed value used for depth/tilt calculations (310 × 0.8)

  const noiseA = Float32Array.from({length: POINTS * 2 + 100}, () => Math.random() * Math.PI * 2);
  const noiseB = Float32Array.from({length: POINTS * 2 + 100}, () => Math.random() * Math.PI * 2);
  const noiseC = Float32Array.from({length: POINTS * 2 + 100}, () => Math.random() * Math.PI * 2);

  // ── Easing ────────────────────────────────────────────────────────────────────
  function clamp01(x)        { return Math.max(0, Math.min(1, x)); }
  function easeOutCubic(x)   { return 1 - Math.pow(1 - x, 3); }
  function easeInOutCubic(x) { return x < 0.5 ? 4*x*x*x : 1 - Math.pow(-2*x+2, 3)/2; }
  function easeInOutQuart(x) { return x < 0.5 ? 8*x*x*x*x : 1 - Math.pow(-2*x+2, 4)/2; }
  // Single smooth curve with no kink: easeInOutSine
  // derivative is continuous everywhere — no discontinuity, no stutter
  function easeInOutSine(x)  { return -(Math.cos(Math.PI * x) - 1) / 2; }

  // ── Growth: single smooth curve, zero kinks ───────────────────────────────────
  // easeInOutSine has a continuous derivative (cosine wave), so the rate of
  // change is always smooth. No two-segment join = no pulse artefact.
  function getRadius() {
    const raw    = clamp01(age / T_GROW_DUR);
    const growth = easeInOutSine(raw);
    return RADIUS_START + (RADIUS_END - RADIUS_START) * growth;
  }

  // ── Fly-in depth ──────────────────────────────────────────────────────────────
  function getFlyDepth() {
    return easeInOutCubic(clamp01((age - T_FLYIN_START) / T_FLYIN_DUR));
  }

  // ── Colour per segment ────────────────────────────────────────────────────────
  function segColour(depth) {
    // depth: 0 = receding (electric blue), 1 = facing viewer (teal)
    // Teal:          #00FFD1  →  r=0,   g=255, b=209
    // Electric Blue: #0066FF  →  r=0,   g=102, b=255
    // Interpolate between the two based on depth
    const g = Math.round(102 + (255 - 102) * depth);   // 102 → 255
    const b = Math.round(255 - (255 - 209) * depth);   // 255 → 209
    return { r: 0, g, b };
  }

  // ── Get a point ───────────────────────────────────────────────────────────────
  // ── Perspective tilt ──────────────────────────────────────────────────────────
  // Top of helix (frac=0) tilts toward viewer, bottom (frac=1) tilts away.
  // Implemented as a Y-driven Z offset fed into a simple perspective projection.
  const TILT_AMOUNT = 480;  // increased lean — top closer, bottom further back
  const FOCAL       = 900;  // focal length — larger = subtler perspective distortion

  function getPoint(strand, i, radius, flyDepth) {
    const frac  = i / (POINTS - 1);
    const angle = frac * Math.PI * 2 * COILS + (strand * Math.PI) + t * 0.55;

    const ni = strand * POINTS + i;
    const organic =
        Math.sin(t * 0.85 + noiseA[ni]) * 22
      + Math.sin(t * 1.55 + noiseB[ni]) * 12
      + Math.sin(t * 2.30 + noiseC[ni]) *  6
      + Math.sin(frac * Math.PI * 12 + t * 0.6) * 9;

    // ── Zip radius modulation ──────────────────────────────────────────────────
    // zipHead moves from frac=1 (bottom) to frac=0 (top) as zipProgress goes 0→1
    const zipHead     = 1.0 - zipProgress;
    // How far this point is from the zip head (positive = above/open, negative = below/zipped)
    const zipDist     = frac - zipHead;
    // Smooth step across the ZIP_WINDOW: 1 = fully open, 0 = fully zipped
    const zipOpen     = clamp01((zipDist + ZIP_WINDOW) / (ZIP_WINDOW * 2));
    // Ease the open factor so the pinch point is sharp but not hard
    const zipEased    = zipOpen * zipOpen * (3 - 2 * zipOpen); // smoothstep
    const minR        = 8;   // strands never fully collapse — tight cluster at zip point
    const r = (minR + (radius - minR) * zipEased) + organic * zipEased;

    const totalH = H * 3.2;
    const yFlat  = (cy - totalH / 2) + frac * totalH;  // un-projected y
    const xFlat  = cx + Math.cos(angle) * r;            // un-projected x

    // Rotation Z (side-to-side depth from helix spin)
    const zRot  = Math.sin(angle) * RADIUS * 0.5;
    // Tilt Z: top is closer (+), bottom is farther (-)
    const zTilt = (0.5 - frac) * 2.0 * TILT_AMOUNT;
    const zTotal = zRot + zTilt;

    // Perspective scale: closer points spread outward, farther points contract
    const scale = FOCAL / Math.max(1, FOCAL - zTotal);
    const x     = cx + (xFlat - cx) * scale;
    const y     = cy + (yFlat - cy) * scale;

    // Depth: 0 = far back, 1 = close front — drives blur, brightness, colour
    const depth = clamp01((zTotal + RADIUS * 0.5 + TILT_AMOUNT) / (RADIUS + TILT_AMOUNT * 2));

    // ── Bottom-up assembly alpha — matches zip direction ─────────────────────
    // frac=1 (bottom) unlocks first, frac=0 (top) unlocks last.
    // threshold = 1 - frac: bottom points have threshold=0 (appear immediately),
    // top points have threshold=1 (appear last).
    const asmThreshold = 1 - frac;
    const asmAlpha     = Math.max(0, Math.min(1,
      (assemblyProgress - (1 - asmThreshold)) / 0.20
    ));
    // ─────────────────────────────────────────────────────────────────────────

    return { x, y, depth, alpha: asmAlpha, frac };
  }

  // ── Connectors ────────────────────────────────────────────────────────────────
  // ── Draw a single glowing spot (no 3D — pure bioluminescent glow) ────────────
  // blur: 0 = sharp (foreground), 1 = fully blurred (background)
  function drawSpot(x, y, radius, r, g, b, alpha, blur) {
    if (alpha < 0.01) return;

    // Blur is simulated by expanding the outer glow and softening the core.
    // A sharp foreground spot has a tight bright centre.
    // A blurred background spot has a wider, dimmer, diffuse haze — no hard core.
    const sharpness = 1 - blur;   // 1 = sharp, 0 = fully blurred

    // Outermost haze — always present, expands with blur
    const hazeR = radius * (4.0 + blur * 3.3);
    ctx.beginPath(); ctx.arc(x, y, hazeR, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(${r},${g},${b},${alpha * (0.03 + blur * 0.025)})`; ctx.fill();

    // Mid glow
    ctx.beginPath(); ctx.arc(x, y, radius * (2.2 + blur * 1.3), 0, Math.PI * 2);
    ctx.fillStyle = `rgba(${r},${g},${b},${alpha * (0.12 - blur * 0.05)})`; ctx.fill();

    // Inner glow — shrinks and dims as blur increases
    ctx.beginPath(); ctx.arc(x, y, radius * (1.3 + blur * 0.25), 0, Math.PI * 2);
    ctx.fillStyle = `rgba(${r},${g},${b},${alpha * (0.30 - blur * 0.18)})`; ctx.fill();

    // Core — only visible when sharp; fades out as blur increases
    if (sharpness > 0.05) {
      ctx.beginPath(); ctx.arc(x, y, radius * sharpness, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(${r},${g},${b},${alpha * sharpness * 0.90})`; ctx.fill();
    }
  }

  // ── Pre-baked spot size variation ────────────────────────────────────────────
  const strandSizeA = Float32Array.from({length: POINTS * 2 + 100}, () => Math.random());
  const strandSizeB = Float32Array.from({length: POINTS * 2 + 100}, () => Math.random());
  const strandSizeC  = Float32Array.from({length: POINTS * 2 + 100}, () => Math.random());
  // 0 = teal (#00FFD1 = 0,255,209), 1 = electric blue (#0066FF = 0,102,255)
  // Random per-spot — both colours appear in foreground and background
  const strandColour = Float32Array.from({length: POINTS * 6 + 100}, () => Math.random());

  function drawStrand(strand, radius, flyDepth) {
    for (let i = 0; i < POINTS; i++) {
      const p = getPoint(strand, i, radius, flyDepth);
      if (p.alpha < 0.01) continue;

      const depthAlpha = Math.max(0.5, p.depth);
      const a          = p.alpha * depthAlpha;
      const blur       = 1 - Math.max(0.0, p.depth);
      const ni         = strand * POINTS + i;

      // Each spot picks teal or blue independently of depth
      const pickColour = (seed) => seed < 0.5
        ? { r:0, g:255, b:209 }   // teal
        : { r:0, g:102, b:255 };  // electric blue

      const sizeNoise = strandSizeA[ni % strandSizeA.length];
      const c1 = pickColour(strandColour[ni % strandColour.length]);
      const spotR = 0.8 + sizeNoise * 9.0 + Math.max(0.5, p.depth) * 1.5;
      drawSpot(p.x, p.y, spotR, c1.r, c1.g, c1.b, a, blur);

      const sNoise2  = strandSizeB[ni % strandSizeB.length];
      const c2 = pickColour(strandColour[(ni + 1) % strandColour.length]);
      const spotR2   = 0.4 + sNoise2 * 5.5;
      const jitter2  = (sNoise2 - 0.5) * 7;
      const angle2   = p.frac * Math.PI * 2 * 3.0 + t * 0.55 + Math.PI * 0.5;
      drawSpot(p.x + Math.cos(angle2) * jitter2, p.y + Math.sin(angle2) * jitter2 * 0.4,
               spotR2, c2.r, c2.g, c2.b, a * 0.70, blur);

      if (i % 2 === 0) {
        const sNoise3 = strandSizeC[ni % strandSizeC.length];
        const c3 = pickColour(strandColour[(ni + 2) % strandColour.length]);
        const spotR3  = 0.3 + sNoise3 * 4.0;
        const jitter3 = (sNoise3 - 0.5) * 5;
        drawSpot(p.x + Math.cos(angle2 + Math.PI) * jitter3,
                 p.y + Math.sin(angle2 + Math.PI) * jitter3 * 0.4,
                 spotR3, c3.r, c3.g, c3.b, a * 0.55, blur);
      }
    }
  }

  // Pre-baked connector spot size variation
  const connSizeA  = Float32Array.from({length: POINTS * 20 + 200}, () => Math.random());
  const connSizeB  = Float32Array.from({length: POINTS * 20 + 200}, () => Math.random());
  const connSizeC  = Float32Array.from({length: POINTS * 20 + 200}, () => Math.random());
  const connJitX   = Float32Array.from({length: POINTS * 20 + 200}, () => Math.random() - 0.5);
  const connJitY   = Float32Array.from({length: POINTS * 20 + 200}, () => Math.random() - 0.5);
  // Same random teal/blue choice as strands
  const connColour = Float32Array.from({length: POINTS * 20 + 200}, () => Math.random());

  // ── Connectors: cluster of spots matching strand organic style ─────────────────
  function drawConnectors(radius, flyDepth) {
    const RUNG_STEP    = 12;
    const BRIDGE_SPOTS = 24;

    const pickC = (seed) => seed < 0.5
      ? { r:0, g:255, b:209 }   // teal
      : { r:0, g:102, b:255 };  // electric blue

    for (let i = 0; i < POINTS; i += RUNG_STEP) {
      const p0 = getPoint(0, i, radius, flyDepth);
      const p1 = getPoint(1, i, radius, flyDepth);
      const baseAlpha = Math.min(p0.alpha, p1.alpha) * 0.95;
      if (baseAlpha < 0.01) continue;

      for (let s = 0; s <= BRIDGE_SPOTS; s++) {
        const frac = s / BRIDGE_SPOTS;

        // Both x AND y arc so spots don't line up in a single row
        const bowX = Math.sin(frac * Math.PI) * Math.sin(t * 0.7 + i * 0.22) * 14;
        const bowY = Math.sin(frac * Math.PI) * Math.cos(t * 0.5 + i * 0.18) * 6;
        const baseBX = p0.x + (p1.x - p0.x) * frac + bowX;
        const baseBY = p0.y + (p1.y - p0.y) * frac + bowY;

        const depth = p0.depth + (p1.depth - p0.depth) * frac;
        const blur  = 1 - Math.max(0.0, depth);

        // Foreground brightness boost — same principle as strand depthAlpha
        const depthBoost = Math.max(0.5, depth);

        const ni = i * BRIDGE_SPOTS + s;

        // ── Primary spot — full 2D scatter, varied size ──
        const sNoise1    = connSizeA[ni % connSizeA.length];
        const jx1        = connJitX[ni % connJitX.length] * 12;
        const jy1        = connJitY[ni % connJitY.length] * 12;
        const spotR1     = 0.6 + sNoise1 * 8.5;
        const c1         = pickC(connColour[ni % connColour.length]);
        const alpha1     = baseAlpha * depthBoost;
        drawSpot(baseBX + jx1, baseBY + jy1, spotR1, c1.r, c1.g, c1.b, alpha1, blur);

        // ── Secondary spot — every point, independent scatter ──
        const sNoise2    = connSizeB[ni % connSizeB.length];
        const jx2        = connJitX[(ni + 40) % connJitX.length] * 10;
        const jy2        = connJitY[(ni + 40) % connJitY.length] * 10;
        const spotR2     = 0.4 + sNoise2 * 5.8;
        const c2         = pickC(connColour[(ni + 1) % connColour.length]);
        const alpha2     = baseAlpha * depthBoost * 0.75;
        drawSpot(baseBX + jx2, baseBY + jy2, spotR2, c2.r, c2.g, c2.b, alpha2, blur);

        // ── Tertiary small filler — every other point ──
        if (s % 2 === 0) {
          const sNoise3  = connSizeC[ni % connSizeC.length];
          const jx3      = connJitX[(ni + 80) % connJitX.length] * 8;
          const jy3      = connJitY[(ni + 80) % connJitY.length] * 8;
          const spotR3   = 0.3 + sNoise3 * 3.8;
          const c3       = pickC(connColour[(ni + 2) % connColour.length]);
          const alpha3   = baseAlpha * depthBoost * 0.50;
          drawSpot(baseBX + jx3, baseBY + jy3, spotR3, c3.r, c3.g, c3.b, alpha3, blur);
        }
      }
    }
  }



  // ── Vignette ──────────────────────────────────────────────────────────────────
  function drawVignette(flyDepth) {
    const s = 0.42 + flyDepth * 0.58;
    const g = ctx.createRadialGradient(cx, cy, H * 0.04, cx, cy, H * 0.96);
    g.addColorStop(0, 'rgba(10,10,26,0)');
    g.addColorStop(1, `rgba(10,10,26,${s})`);
    ctx.fillStyle = g; ctx.fillRect(0, 0, W, H);
  }

  // ── Particles ─────────────────────────────────────────────────────────────────
  const PX_COLS = [[0,212,170],[0,102,255],[0,255,209]];

  // ── Ambient spots — organic texture across the full viewport ──────────────────
  // Two layers: far (small, blurred, slow) and near (larger, sharp, faster)
  // Each spot drifts on a unique Lissajous-style path so motion feels organic
  const AMBIENT_SPOTS = Array.from({length: 400}, () => {
    const depth = Math.random();           // 0=far, 1=near
    const isFar = depth < 0.5;
    return {
      // Starting position — spread across full viewport including edges
      x:    Math.random(),
      y:    Math.random(),
      // Drift: each spot has its own x and y frequency/amplitude for wandering
      ax:   (Math.random() - 0.5) * 0.018,   // x drift amplitude (fraction of W)
      ay:   (Math.random() - 0.5) * 0.022,   // y drift amplitude (fraction of H)
      fx:   Math.random() * 0.4 + 0.1,       // x drift frequency
      fy:   Math.random() * 0.4 + 0.1,       // y drift frequency
      phx:  Math.random() * Math.PI * 2,     // x phase offset
      phy:  Math.random() * Math.PI * 2,     // y phase offset
      // Visual properties
      depth,
      r:    isFar
              ? Math.random() * 2.5 + 0.8    // far: small
              : Math.random() * 5.5 + 1.5,   // near: larger
      o:    isFar
              ? Math.random() * 0.234 + 0.059  // far: +50% brighter
              : Math.random() * 0.410 + 0.117, // near: +50% brighter
      col:  Math.floor(Math.random() * 3),
      ph:   Math.random() * Math.PI * 2,     // pulse phase
    };
  });

  function drawParticles(flyDepth) {
    // Spots appear only after helix animation completes (assembly + zip)
    const helixDone = ASM_COMPLETE + ZIP_DUR;  // ~7.9s
    const spotFadeIn = Math.min(1, Math.max(0, (age - 3.0) / 1.5)); // fade in over 1.5s after 3s
    if (spotFadeIn < 0.01) return;

    const sceneFade = spotFadeIn * Math.max(0.3, 1 - flyDepth * 0.5);

    AMBIENT_SPOTS.forEach(p => {
      // Wandering position — Lissajous drift around home position
      const wx = (p.x + p.ax * Math.sin(age * p.fx + p.phx)) * W;
      const wy = (p.y + p.ay * Math.sin(age * p.fy + p.phy)) * H;

      // Gentle pulse
      const pulse = Math.sin(age * 0.6 + p.ph) * 0.18 + 0.82;

      const [pr, pg, pb] = PX_COLS[p.col];
      const blur  = 1 - Math.max(0.0, p.depth);   // far spots blurred, near spots sharp
      const alpha = p.o * pulse * sceneFade;

      drawSpot(wx, wy, p.r, pr, pg, pb, alpha, blur);
    });
  }

  // ── Triggers ──────────────────────────────────────────────────────────────────
  function revealText() {
    // Section 1: stagger each word by 120ms
    const words = document.querySelectorAll('.s1-word');
    const wordDelay = 120; // ms between each word
    words.forEach((w, i) => {
      setTimeout(() => w.classList.add('show'), i * wordDelay);
    });
    const s1Duration = words.length * wordDelay + 450; // time until last word fully visible
    const PAUSE      = 250;                              // 0.25s pause after Section 1

    // Section 2 + bloom/flash: 1s after Section 1 finishes
    setTimeout(() => {
      document.getElementById('s2').classList.add('show');
    }, s1Duration + PAUSE);

    // Section 4: 0.5s after Section 2 appears
    const s4Start = s1Duration + PAUSE + 650; // s2 transition ~0.55s + 0.5s gap

    // Section 3: now appears where s4 used to start
    setTimeout(() => {
      document.getElementById('s3').classList.add('show');
    }, s4Start);

    // Scroll indicator after everything has settled
    setTimeout(() => {
      document.getElementById('scroll-indicator').style.opacity = '1';
    }, s4Start + 2000);
  }
  function revealNav() {
    document.getElementById('nav').style.opacity           = '1';
    document.getElementById('progress-dots').style.opacity = '1';
  }

  // ── Main loop ─────────────────────────────────────────────────────────────────
  function loop() {
    age += 0.016;
    t   += 0.011;

    // Assembly: linear increment, identical to v3's assemblyProgress += 0.0038
    assemblyProgress = Math.min(1, assemblyProgress + ASM_RATE);

    // Advance zipper pull from bottom to top after assembly completes
    if (age > ZIP_START) {
      zipProgress = Math.min(1, (age - ZIP_START) / ZIP_DUR);
    }

    ctx.fillStyle = '#0A0A1A';
    ctx.fillRect(0, 0, W, H);

    const radius   = getRadius();
    const flyDepth = getFlyDepth();

    if (!textDone && age >= T_NAV - 1.5) {
      textDone = true;
      revealText();
    }
    if (!navDone && age >= T_NAV) {
      navDone = true;
      revealNav();
    }

    drawParticles(flyDepth);
    drawConnectors(radius, flyDepth);

    // Depth sort: sample midpoint of each strand — tilt-aware
    const mid0 = getPoint(0, Math.floor(POINTS / 2), radius, flyDepth);
    const mid1 = getPoint(1, Math.floor(POINTS / 2), radius, flyDepth);
    if (mid1.depth < mid0.depth) { drawStrand(1, radius, flyDepth); drawStrand(0, radius, flyDepth); }
    else                         { drawStrand(0, radius, flyDepth); drawStrand(1, radius, flyDepth); }

    drawVignette(flyDepth);

    requestAnimationFrame(loop);
  }

  loop();

  // ══════════════════════════════════════════
  // SCENE 2 — THE PROBLEM
  // ══════════════════════════════════════════
  const messages = [
    { lines: ['47 slides.',        'Nobody read them.'                        ], classes: ['', 'dim']              },
    { lines: ['The data arrived.', 'Three weeks late.',   'With errors.'      ], classes: ['', 'dim', 'highlight'] },
    { lines: ['Your team built',   'the report.',         'Manually. Again.'  ], classes: ['', 'dim', 'dim']       },
    { lines: ['Is your competitive','advantage still',    'competitive?'      ], classes: ['', '', 'highlight']    },
  ];

  const noiseTexts = [
    { t: 'Q3_Revenue_FINAL_v3.xlsx',            sz: 0.7  },
    { t: 'Board_Pack_Draft_18.pptx',            sz: 0.8  },
    { t: '=VLOOKUP(A2,Sheet3!$B:$F,4,0)',       sz: 0.65 },
    { t: 'RE: RE: RE: FWD: Urgent — Numbers',   sz: 0.9  },
    { t: '#REF!',                               sz: 2.2  },
    { t: 'Waiting on finance...',               sz: 0.8  },
    { t: '47 unread',                           sz: 1.4  },
    { t: 'OVERDUE',                             sz: 2.6  },
    { t: '=SUM(B2:B847)',                       sz: 0.7  },
    { t: 'tmp_DO_NOT_USE.csv',                  sz: 0.75 },
    { t: 'TODO: fix before board',              sz: 0.85 },
    { t: '#N/A',                                sz: 2.0  },
    { t: 'syncing...',                          sz: 0.7  },
    { t: 'Last saved: 3 days ago',              sz: 0.85 },
    { t: 'Johan — can you check this?',         sz: 0.9  },
    { t: 'ERR: circular reference',             sz: 0.75 },
    { t: '99+',                                 sz: 3.2  },
    { t: 'Meeting_Notes_Oct14.docx',            sz: 0.7  },
    { t: 'MISSING DATA',                        sz: 1.6  },
    { t: 'Version mismatch — which is master?', sz: 0.8  },
    { t: '=IFERROR(...)',                        sz: 0.7  },
    { t: '12 new comments',                     sz: 0.9  },
    { t: 'FINAL_FINAL_v2.xlsx',                 sz: 1.0  },
    { t: 'Deadline: TOMORROW',                  sz: 1.3  },
  ];

  const NOISE_LEAD = 0;
  const VH_PER_MSG = 120;
  const PAUSE_FRAC = 0.30;
  const TAIL_VH    = 80;

  const scene2El = document.getElementById('scene2');
  scene2El.style.height = (VH_PER_MSG * messages.length + TAIL_VH) + 'vh';

  const msgContainer = document.getElementById('messages-container');
  const msgEls = [];
  messages.forEach(msg => {
    const wrap = document.createElement('div');
    wrap.className = 'problem-msg';
    msg.lines.forEach((line, li) => {
      const span = document.createElement('span');
      span.className = 'line ' + (msg.classes[li] || '');
      span.textContent = line;
      wrap.appendChild(span);
    });
    msgContainer.appendChild(wrap);
    msgEls.push(wrap);
  });

  const noiseLayer  = document.getElementById('noise-layer');
  const noiseItems  = [];
  const noiseColors = [
    'rgba(180,180,200,0.55)', 'rgba(0,212,170,0.5)',
    'rgba(255,184,0,0.55)',   'rgba(255,68,85,0.5)',
    'rgba(100,160,255,0.45)',
  ];
  noiseTexts.forEach((item, i) => {
    const el = document.createElement('div');
    el.className   = 'noise-item';
    el.textContent = item.t;
    el.style.fontSize  = item.sz + 'rem';
    el.style.left      = (3 + Math.random() * 88) + '%';
    el.style.top       = (3 + Math.random() * 88) + '%';
    el.style.transform = `rotate(${(Math.random()-0.5)*12}deg)`;
    el.style.color     = noiseColors[Math.floor(Math.random() * noiseColors.length)];
    noiseLayer.appendChild(el);
    noiseItems.push(el);
  });

  ScrollTrigger.create({
    trigger: '#scene2-wrapper', start: 'top top', end: 'bottom bottom', scrub: true,
    onUpdate: (self) => {
      const p = self.progress;
      noiseItems.forEach((el, i) => {
        const stagger  = (i / noiseItems.length) * 0.8;
        const cycle    = 0.35;
        const localP   = Math.min(((p + stagger) % 1) / cycle, 1);
        const scale    = 1 + localP * 5;
        const opacity  = localP < 0.3 ? 0.2 + (localP / 0.3) * 0.6
                       : localP < 0.7 ? 0.8
                       : 0.8 - ((localP - 0.7) / 0.3) * 0.6;
        el.style.opacity   = opacity;
        el.style.transform = `rotate(${(i%2===0?1:-1)*(i%7)*0.8}deg) scale(${scale})`;
      });
      const msgRange = 1 - NOISE_LEAD;
      const segSize  = msgRange / messages.length;
      msgEls.forEach((el, i) => {
        const segStart = NOISE_LEAD + i * segSize;
        const flyInEnd = segStart + segSize * 0.25;
        const pauseEnd = segStart + segSize * (0.25 + PAUSE_FRAC);
        const segEnd   = segStart + segSize;
        let localP;
        if      (p < segStart)  { localP = -1; }
        else if (p < flyInEnd)  { localP = (p - segStart) / (flyInEnd - segStart); }
        else if (p < pauseEnd)  { localP = 1; }
        else                    { localP = 1 + (p - pauseEnd) / (segEnd - pauseEnd); }
        if (localP < 0) {
          el.style.opacity = 0; el.style.transform = 'translateZ(-800px) scale(0.05)';
        } else if (localP <= 1) {
          const eased = localP < 0.5 ? 4*localP*localP*localP : 1 - Math.pow(-2*localP+2,3)/2;
          el.style.opacity   = Math.min(localP * 3, 1);
          el.style.transform = `translateZ(${-800+eased*800}px) scale(${0.05+eased*0.95})`;
        } else {
          const ov = localP - 1;
          el.style.opacity   = Math.max(1 - ov*2.5, 0);
          el.style.transform = `translateZ(${ov*400}px) scale(${1+ov*2.5})`;
        }
      });
    }
  });

  // Pre-warm noise so it's visible immediately
  (function preWarmNoise() {
    noiseItems.forEach((el, i) => {
      const stagger = (i / noiseItems.length) * 0.8;
      const localP  = Math.min((stagger % 1) / 0.35, 1);
      const scale   = 1 + localP * 5;
      const opacity = localP < 0.3 ? 0.2 + (localP/0.3)*0.6 : localP < 0.7 ? 0.8 : 0.8-((localP-0.7)/0.3)*0.6;
      el.style.opacity   = opacity;
      el.style.transform = `rotate(${(i%2===0?1:-1)*(i%7)*0.8}deg) scale(${scale})`;
    });
  })();

  // ══════════════════════════════════════════
  // SCENE 3 — THE TRANSFORMATION
  // ══════════════════════════════════════════
  document.getElementById('scene3-s1').style.height = '280vh';

  ScrollTrigger.create({
    trigger: '#scene3-s1-wrapper', start: 'top 80%',
    onEnter: () => gsap.to('#transform-header', { opacity: 1, duration: 0.8, ease: 'power3.out' })
  });

  let transformFired = false;

  function resetToSpreadsheet() {
    gsap.killTweensOf(['#before-state','#after-state','#transition-overlay',
      '#mc1','#mc2','#mc3','#mc4','#cc1','#cc2','#ai-insight']);
    gsap.set('#before-state', { opacity: 1 });
    gsap.set('#after-state',  { opacity: 0 });
    gsap.set('#transition-overlay', { opacity: 0 });
    gsap.set('#mc1,#mc2,#mc3,#mc4', { opacity: 0, y: 12 });
    gsap.set('#cc1,#cc2',   { opacity: 0 });
    gsap.set('#ai-insight', { opacity: 0, x: -12 });
    document.querySelectorAll('#before-state td.rc').forEach(c => gsap.set(c, { opacity: 1 }));
    [1,2,3,4,5,6,7,8,9].forEach(i => { const b = document.getElementById('b'+i); if(b) b.style.height='0px'; });
    ['donut1','donut2','donut3'].forEach(id => { const el=document.getElementById(id); if(el) el.setAttribute('stroke-dasharray','0 175.9'); });
    transformFired = false;
  }

  ScrollTrigger.create({
    trigger: '#scene3-s1-wrapper', start: 'top top', end: 'bottom bottom',
    onUpdate: (self) => { if (self.progress > 0.25 && !transformFired) { transformFired = true; fireTransformation(); } },
    onLeaveBack: () => { gsap.to('#transform-header', { opacity: 0, duration: 0.4 }); resetToSpreadsheet(); }
  });

  function fireTransformation() {
    gsap.to('#transition-overlay', { opacity: 1, backgroundColor: 'rgba(0,212,170,0.12)', duration: 0.3, ease: 'power2.out',
      onComplete: () => gsap.to('#transition-overlay', { opacity: 0, duration: 0.6 }) });
    document.querySelectorAll('#before-state td.rc').forEach(cell => {
      const delay = Math.random() * 0.6;
      const orig  = cell.textContent;
      const iv    = setInterval(() => { cell.textContent = Math.random()>0.5 ? Math.floor(Math.random()*999999).toString() : ['#REF!','...','→','✓'][Math.floor(Math.random()*4)]; }, 60);
      setTimeout(() => { clearInterval(iv); cell.textContent = orig; gsap.to(cell, { opacity: 0, duration: 0.4, delay }); }, delay*1000+200);
    });
    gsap.to('#before-state',  { opacity: 0, duration: 0.8, delay: 0.9, ease: 'power2.inOut' });
    gsap.to('#after-state',   { opacity: 1, duration: 1.2, delay: 1.0, ease: 'power3.out' });
    ['#mc1','#mc2','#mc3','#mc4'].forEach((id, i) => gsap.to(id, { opacity: 1, y: 0, duration: 0.6, delay: 1.4+i*0.12, ease: 'power3.out' }));
    setTimeout(() => {
      animCount('mv-revenue', 0, 9849629, 1600, v => 'R'+(v/1e6).toFixed(1)+'M');
      animCount('mv-gp',      0, 50,      1400, v => v.toFixed(0)+'%');
      animCount('mv-units',   0, 4568,    1200, v => Math.round(v).toLocaleString());
      document.getElementById('md-rev').textContent = '↑ 11% vs target';
    }, 1500);
    setTimeout(() => { [66,47,23,73,58,21,80,71,27].forEach((h,i)=>{ const b=document.getElementById('b'+(i+1)); if(b) b.style.height=h+'px'; }); }, 1800);
    setTimeout(() => {
      const C=2*Math.PI*28, s1=C*0.52, s2=C*0.35, s3=C*0.13;
      const d1=document.getElementById('donut1'), d2=document.getElementById('donut2'), d3=document.getElementById('donut3');
      d1.setAttribute('stroke-dasharray',s1+' '+(C-s1)); d1.setAttribute('stroke-dashoffset',C*0.25);
      setTimeout(()=>{d2.setAttribute('stroke-dasharray',s2+' '+(C-s2));d2.setAttribute('stroke-dashoffset',C*0.25-s1);},200);
      setTimeout(()=>{d3.setAttribute('stroke-dasharray',s3+' '+(C-s3));d3.setAttribute('stroke-dashoffset',C*0.25-s1-s2);},400);
    }, 2000);
    gsap.to('#cc1',       { opacity: 1, duration: 0.7, delay: 2.1 });
    gsap.to('#cc2',       { opacity: 1, duration: 0.7, delay: 2.3 });
    gsap.to('#ai-insight',{ opacity: 1, x: 0, duration: 0.9, delay: 2.7, ease: 'power3.out' });
  }

  function animCount(id, from, to, ms, fmt) {
    const el = document.getElementById(id), start = performance.now();
    function tick(now) {
      const t = Math.min((now-start)/ms,1), eased = 1-Math.pow(1-t,3);
      el.textContent = fmt(from+(to-from)*eased);
      if(t<1) requestAnimationFrame(tick);
    }
    requestAnimationFrame(tick);
  }

  // ══════════════════════════════════════════
  // SCENE 3 — SHOWCASE STUBS (5)
  // Showcases 1, 2, 3, 4 are full builds — see blocks above/below
  // ══════════════════════════════════════════
  [
    '#scene3-s5-wrapper',
  ].forEach(function(sel) {
    var wrapper = document.querySelector(sel);
    if (!wrapper) return;
    var inner = wrapper.querySelector('.showcase-stub-inner');
    ScrollTrigger.create({
      trigger: wrapper,
      start: 'top 70%',
      onEnter: function() { if (inner) inner.classList.add('visible'); }
    });
  });

  // ══════════════════════════════════════════
  // SCENE 3 — SHOWCASE 2: SALES PIPELINE
  // Two-phase scroll:
  //   Phase 1 — heading fades in on entry
  //   Phase 2 — sticky locks, dashboard track
  //             scrubs upward via translateY,
  //             wave animations fire on progress
  // ══════════════════════════════════════════

  var CARD_GAP   = 24;
  var BOTTOM_PAD = 20;

  var s2HeaderEl = document.getElementById('s2-transform-header');
  var s2TrackEl  = document.getElementById('s2-dashboard-track');
  var s2CardEl   = document.getElementById('s2-dashboard-card');

  function s2GetMetrics() {
    var vh           = window.innerHeight;
    var headerBottom = s2HeaderEl.offsetTop + s2HeaderEl.offsetHeight;
    var startY       = headerBottom + CARD_GAP;
    var cardH        = s2CardEl.offsetHeight;
    var spaceBelow   = vh - startY - BOTTOM_PAD;
    var travelY      = Math.max(0, cardH - spaceBelow);
    var scrollH      = vh + travelY + (vh * 0.5);
    return { startY: startY, travelY: travelY, scrollH: scrollH };
  }

  var s2m = s2GetMetrics();
  document.getElementById('scene3-s2').style.height = s2m.scrollH + 'px';
  s2TrackEl.style.top = s2m.startY + 'px';
  gsap.set(s2TrackEl, { y: 0 });

  // Phase 1 — header fade in
  ScrollTrigger.create({
    trigger: '#scene3-s2-wrapper',
    start: 'top 80%',
    onEnter: function() {
      gsap.to('#s2-transform-header', { opacity: 1, duration: 0.8, ease: 'power3.out' });
    }
  });

  // Phase 2 — sticky lock + scrub
  var s2Wave1 = false, s2Wave2 = false, s2Wave3 = false,
      s2Wave4 = false, s2Wave5 = false;

  var s2ScrubTween = gsap.to(s2TrackEl, {
    y: -s2m.travelY, ease: 'none', paused: true
  });

  ScrollTrigger.create({
    trigger: '#scene3-s2-wrapper',
    start: 'top top',
    end: 'bottom bottom',
    scrub: true,
    animation: s2ScrubTween,
    onUpdate: function(self) { s2FireWaves(self.progress); },
    onLeaveBack: function() {
      s2Reset();
      gsap.to('#s2-transform-header', { opacity: 0, duration: 0.3 });
    }
  });

  function s2FireWaves(p) {
    if (p > 0.04 && !s2Wave1) {
      s2Wave1 = true;
      gsap.to('#s2-dashboard-card', { opacity: 1, duration: 0.5, ease: 'power2.out' });
      gsap.to('#s2-board-header', { opacity: 1, y: 0, duration: 0.7, ease: 'power3.out', delay: 0.15 });
      gsap.to(['#s2-mc1','#s2-mc2','#s2-mc3','#s2-mc4'], {
        opacity: 1, y: 0, duration: 0.6, ease: 'power3.out', stagger: 0.1, delay: 0.3
      });
    }
    if (p > 0.22 && !s2Wave2) {
      s2Wave2 = true;
      gsap.to('#s2-funnel-card', { opacity: 1, y: 0, duration: 0.6, ease: 'power3.out' });
      var stages = [
        { id: 'fs1', pct: 43.5 }, { id: 'fs2', pct: 26.5 },
        { id: 'fs3', pct: 18.4 }, { id: 'fs4', pct: 7.3 }, { id: 'fs5', pct: 4.3 }
      ];
      stages.forEach(function(s, i) {
        var el = document.getElementById(s.id);
        if (!el) return;
        setTimeout(function() {
          el.style.transition = 'width 1.1s cubic-bezier(0.16,1,0.3,1)';
          el.style.width = s.pct + '%';
          el.classList.add('visible');
        }, i * 120 + 200);
      });
    }
    if (p > 0.42 && !s2Wave3) {
      s2Wave3 = true;
      gsap.to('#s2-bar-card', { opacity: 1, duration: 0.6, ease: 'power3.out' });
      var bars = [
        { id: 's2b1', h: '82%' }, { id: 's2b2', h: '55%' },
        { id: 's2b3', h: '38%' }, { id: 's2b4', h: '22%' }, { id: 's2b5', h: '14%' }
      ];
      bars.forEach(function(b, i) {
        var el = document.getElementById(b.id);
        if (!el) return;
        setTimeout(function() { el.style.height = b.h; }, i * 90 + 200);
      });
    }
    if (p > 0.58 && !s2Wave4) {
      s2Wave4 = true;
      gsap.to('#s2-deal-table-card', { opacity: 1, duration: 0.5, ease: 'power3.out' });
      ['s2-deal-1','s2-deal-2','s2-deal-3','s2-deal-4','s2-deal-5'].forEach(function(id, i) {
        gsap.to('#' + id, { opacity: 1, x: 0, duration: 0.5, ease: 'power2.out', delay: i * 0.1 + 0.2 });
      });
    }
    if (p > 0.78 && !s2Wave5) {
      s2Wave5 = true;
      gsap.to('#s2-ai-insight', { opacity: 1, x: 0, duration: 0.8, ease: 'power3.out' });
    }
  }

  function s2Reset() {
    s2Wave1 = false; s2Wave2 = false; s2Wave3 = false;
    s2Wave4 = false; s2Wave5 = false;
    gsap.set(s2TrackEl, { y: 0 });
    gsap.set('#s2-dashboard-card', { opacity: 0 });
    gsap.set('#s2-board-header', { opacity: 0, y: 12 });
    gsap.set(['#s2-mc1','#s2-mc2','#s2-mc3','#s2-mc4'], { opacity: 0, y: 12 });
    gsap.set('#s2-funnel-card', { opacity: 0, y: 12 });
    ['fs1','fs2','fs3','fs4','fs5'].forEach(function(id) {
      var el = document.getElementById(id);
      if (!el) return;
      el.style.transition = 'none';
      el.style.width = '0';
      el.classList.remove('visible');
    });
    gsap.set('#s2-bar-card', { opacity: 0 });
    ['s2b1','s2b2','s2b3','s2b4','s2b5'].forEach(function(id) {
      var el = document.getElementById(id);
      if (el) el.style.height = '0';
    });
    gsap.set('#s2-deal-table-card', { opacity: 0 });
    ['s2-deal-1','s2-deal-2','s2-deal-3','s2-deal-4','s2-deal-5'].forEach(function(id) {
      gsap.set('#' + id, { opacity: 0, x: -12 });
    });
    gsap.set('#s2-ai-insight', { opacity: 0, x: -12 });
  }

  // Resize handler
  var s2ResizeTimer;
  window.addEventListener('resize', function() {
    clearTimeout(s2ResizeTimer);
    s2ResizeTimer = setTimeout(function() {
      s2m = s2GetMetrics();
      document.getElementById('scene3-s2').style.height = s2m.scrollH + 'px';
      s2TrackEl.style.top = s2m.startY + 'px';
      s2ScrubTween.vars.y = -s2m.travelY;
      gsap.set(s2TrackEl, { y: 0 });
      ScrollTrigger.refresh();
    }, 150);
  });

  // ══════════════════════════════════════════
  // SCENE 3 — SHOWCASE 3: BUSINESS RISK
  // Animated after-state build — 5-wave scroll
  // choreography with reverse-on-scroll-back.
  // ══════════════════════════════════════════
  (function initShowcase3() {
    var wrapper = document.querySelector('#scene3-s3-wrapper');
    if (!wrapper) return;

    /* ───────── initial (hidden) states ───────── */
    gsap.set('.risk-header', { opacity: 0, y: 30 });
    gsap.set('.risk-matrix-card', { opacity: 0, y: 20 });
    gsap.set('.risk-matrix-cell', { opacity: 0, scale: 0.6 });
    gsap.set('.risk-dot', { opacity: 0, scale: 0 });
    gsap.set('.risk-side .risk-card:first-child', { opacity: 0, y: 20 });
    gsap.set('.risk-register-item', { opacity: 0, x: 40 });
    gsap.set('.risk-trend-card', { opacity: 0, y: 20 });
    gsap.set('.risk-insight', { opacity: 0, y: 20 });
    gsap.set('.risk-trend-line', { strokeDasharray: 460, strokeDashoffset: 460 });
    gsap.set('.risk-trend-area', { opacity: 0 });
    gsap.set('.risk-trend-point', { opacity: 0 });

    var thresholds = [0.04, 0.22, 0.42, 0.58, 0.78];
    var activeWave = -1;

    function playWave(i) {
      switch (i) {
        case 0:
          gsap.to('.risk-header', { opacity: 1, y: 0, duration: 0.7, ease: 'power3.out' });
          break;
        case 1:
          gsap.to('.risk-matrix-card', { opacity: 1, y: 0, duration: 0.6, ease: 'power3.out' });
          gsap.to('.risk-matrix-cell', { opacity: 1, scale: 1, duration: 0.4, stagger: 0.018, ease: 'power3.out' });
          break;
        case 2:
          gsap.to('.risk-dot', { opacity: 1, scale: 1, duration: 0.45, stagger: 0.07, ease: 'back.out(1.7)' });
          break;
        case 3:
          gsap.to('.risk-side .risk-card:first-child', { opacity: 1, y: 0, duration: 0.5, ease: 'power3.out' });
          gsap.to('.risk-register-item', { opacity: 1, x: 0, duration: 0.5, stagger: 0.08, ease: 'power3.out' });
          break;
        case 4:
          gsap.to('.risk-trend-card', { opacity: 1, y: 0, duration: 0.6, ease: 'power3.out' });
          gsap.to('.risk-trend-line', { strokeDashoffset: 0, duration: 1.2, ease: 'power2.inOut' });
          gsap.to('.risk-trend-area', { opacity: 1, duration: 0.8, delay: 0.5, ease: 'power2.inOut' });
          gsap.to('.risk-trend-point', { opacity: 1, duration: 0.3, stagger: 0.06, delay: 0.3 });
          gsap.to('.risk-insight', { opacity: 1, y: 0, duration: 0.6, delay: 0.35, ease: 'power3.out' });
          break;
      }
    }

    function reverseWave(i) {
      switch (i) {
        case 0:
          gsap.to('.risk-header', { opacity: 0, y: 30, duration: 0.4, ease: 'power2.inOut' });
          break;
        case 1:
          gsap.to('.risk-matrix-card', { opacity: 0, y: 20, duration: 0.4, ease: 'power2.inOut' });
          gsap.to('.risk-matrix-cell', { opacity: 0, scale: 0.6, duration: 0.3, ease: 'power2.inOut' });
          break;
        case 2:
          gsap.to('.risk-dot', { opacity: 0, scale: 0, duration: 0.3, ease: 'power2.inOut' });
          break;
        case 3:
          gsap.to('.risk-side .risk-card:first-child', { opacity: 0, y: 20, duration: 0.4, ease: 'power2.inOut' });
          gsap.to('.risk-register-item', { opacity: 0, x: 40, duration: 0.4, ease: 'power2.inOut' });
          break;
        case 4:
          gsap.to('.risk-trend-card', { opacity: 0, y: 20, duration: 0.4, ease: 'power2.inOut' });
          gsap.to('.risk-trend-line', { strokeDashoffset: 460, duration: 0.6, ease: 'power2.inOut' });
          gsap.to('.risk-trend-area', { opacity: 0, duration: 0.4, ease: 'power2.inOut' });
          gsap.to('.risk-trend-point', { opacity: 0, duration: 0.2 });
          gsap.to('.risk-insight', { opacity: 0, y: 20, duration: 0.4, ease: 'power2.inOut' });
          break;
      }
    }

    ScrollTrigger.create({
      trigger: '#scene3-s3-wrapper',
      start: 'top top',
      end: 'bottom bottom',
      scrub: true,
      onUpdate: function (self) {
        var p = self.progress;
        var target = -1;
        for (var i = 0; i < thresholds.length; i++) {
          if (p >= thresholds[i]) target = i;
        }
        if (target > activeWave) {
          for (var f = activeWave + 1; f <= target; f++) playWave(f);
        } else if (target < activeWave) {
          for (var r = activeWave; r > target; r--) reverseWave(r);
        }
        activeWave = target;
      }
    });
  })();

  // ══════════════════════════════════════════
  // SCENE 3 — SHOWCASE 4: MARKET INTELLIGENCE
  // ══════════════════════════════════════════
  (function initShowcase4() {
        var wrapper = document.querySelector('#scene3-s4-wrapper');
        if (!wrapper) return;
  
        /* ── Initial hidden states ──────────────────────────────── */
        gsap.set('#sc-header-4',       { opacity: 0, y: 30 });
        gsap.set('.mi-tile',           { opacity: 0, y: 22 });
        gsap.set('.mi-signals-wrap',   { opacity: 0 });
        gsap.set('.mi-signal',         { opacity: 0, x: -14 });
        gsap.set('.mi-chart-wrap',     { opacity: 0, y: 14 });
        gsap.set('.mi-chart-bar-fill', { scaleX: 0 });
        gsap.set('.mi-chart-pct',      { opacity: 0 });
        gsap.set('.mi-news-card',      { opacity: 0, y: 18 });
        gsap.set('.mi-insight',        { opacity: 0, y: 10 });
  
        /* ── Counter helpers ─────────────────────────────────────── */
        var counterTweens = [];
  
        function startCounters() {
          counterTweens.forEach(function(t) { t.kill(); });
          counterTweens = [];
          document.querySelectorAll('.mi-tile-num-val').forEach(function(el) {
            var target = parseInt(el.getAttribute('data-target'), 10);
            var obj    = { val: 0 };
            var tween  = gsap.to(obj, {
              val:        target,
              duration:   1.3,
              ease:       'power3.out',
              onUpdate:   function() { el.textContent = Math.round(obj.val); }
            });
            counterTweens.push(tween);
          });
        }
  
        function resetCounters() {
          counterTweens.forEach(function(t) { t.kill(); });
          counterTweens = [];
          document.querySelectorAll('.mi-tile-num-val').forEach(function(el) {
            el.textContent = '0';
          });
        }
  
        /* ── Wave thresholds ─────────────────────────────────────── */
        var thresholds = [0.04, 0.22, 0.42, 0.58, 0.78];
        var activeWave = -1;
  
        /* ── Play waves (forward) ────────────────────────────────── */
        function playWave(i) {
          switch (i) {
  
            case 0: /* Header */
              gsap.to('#sc-header-4', {
                opacity: 1, y: 0,
                duration: 0.7, ease: 'power3.out'
              });
              break;
  
            case 1: /* Metric tiles + counter animation */
              gsap.to('.mi-tile', {
                opacity: 1, y: 0,
                duration: 0.65, stagger: 0.09, ease: 'power3.out'
              });
              startCounters();
              break;
  
            case 2: /* Signals feed */
              gsap.to('.mi-signals-wrap', {
                opacity: 1,
                duration: 0.45, ease: 'power3.out'
              });
              gsap.to('.mi-signal', {
                opacity: 1, x: 0,
                duration: 0.5, stagger: 0.08, ease: 'power3.out',
                delay: 0.12
              });
              break;
  
            case 3: /* Share-of-voice chart */
              gsap.to('.mi-chart-wrap', {
                opacity: 1, y: 0,
                duration: 0.5, ease: 'power3.out'
              });
              gsap.to('.mi-chart-bar-fill', {
                scaleX: 1,
                duration: 0.75, stagger: 0.09, ease: 'power3.out',
                delay: 0.2
              });
              gsap.to('.mi-chart-pct', {
                opacity: 1,
                duration: 0.4, stagger: 0.09,
                delay: 0.62
              });
              break;
  
            case 4: /* News cards + AI insight */
              gsap.to('.mi-news-card', {
                opacity: 1, y: 0,
                duration: 0.5, stagger: 0.09, ease: 'power3.out'
              });
              gsap.to('.mi-insight', {
                opacity: 1, y: 0,
                duration: 0.55, ease: 'power3.out',
                delay: 0.3
              });
              break;
          }
        }
  
        /* ── Reverse waves (scroll back) ─────────────────────────── */
        function reverseWave(i) {
          switch (i) {
  
            case 0: /* Header */
              gsap.to('#sc-header-4', {
                opacity: 0, y: 30,
                duration: 0.4, ease: 'power2.inOut'
              });
              break;
  
            case 1: /* Metric tiles */
              gsap.to('.mi-tile', {
                opacity: 0, y: 22,
                duration: 0.4, stagger: { each: 0.06, from: 'end' }, ease: 'power2.inOut'
              });
              resetCounters();
              break;
  
            case 2: /* Signals feed */
              gsap.to('.mi-signal', {
                opacity: 0, x: -14,
                duration: 0.3, stagger: { each: 0.05, from: 'end' }, ease: 'power2.inOut'
              });
              gsap.to('.mi-signals-wrap', {
                opacity: 0,
                duration: 0.35, ease: 'power2.inOut',
                delay: 0.22
              });
              break;
  
            case 3: /* Chart */
              gsap.to('.mi-chart-pct', {
                opacity: 0,
                duration: 0.2
              });
              gsap.to('.mi-chart-bar-fill', {
                scaleX: 0,
                duration: 0.35, stagger: { each: 0.05, from: 'end' }, ease: 'power2.inOut',
                delay: 0.05
              });
              gsap.to('.mi-chart-wrap', {
                opacity: 0, y: 14,
                duration: 0.4, ease: 'power2.inOut',
                delay: 0.22
              });
              break;
  
            case 4: /* News cards + insight */
              gsap.to('.mi-insight', {
                opacity: 0, y: 10,
                duration: 0.3, ease: 'power2.inOut'
              });
              gsap.to('.mi-news-card', {
                opacity: 0, y: 18,
                duration: 0.35, stagger: { each: 0.06, from: 'end' }, ease: 'power2.inOut',
                delay: 0.1
              });
              break;
          }
        }
  
        /* ── ScrollTrigger ───────────────────────────────────────── */
        ScrollTrigger.create({
          trigger: '#scene3-s4-wrapper',
          start:   'top top',
          end:     'bottom bottom',
          scrub:   true,
          onUpdate: function(self) {
            var p      = self.progress;
            var target = -1;
            for (var i = 0; i < thresholds.length; i++) {
              if (p >= thresholds[i]) target = i;
            }
            if (target > activeWave) {
              for (var f = activeWave + 1; f <= target; f++) playWave(f);
            } else if (target < activeWave) {
              for (var r = activeWave; r > target; r--) reverseWave(r);
            }
            activeWave = target;
          }
        });
  
      })();

  // ══════════════════════════════════════════
  // SCENE 4 — THE PROOF
  // ══════════════════════════════════════════

  // Header fade-in
  ScrollTrigger.create({
    trigger: '#scene4-header',
    start: 'top 75%',
    onEnter: () => {
      const el = document.getElementById('scene4-header');
      if (el) el.classList.add('visible');
    }
  });

  // Stat cards stagger in
  document.querySelectorAll('.stat-card').forEach((card, i) => {
    ScrollTrigger.create({
      trigger: card,
      start: 'top 80%',
      onEnter: () => {
        setTimeout(() => card.classList.add('visible'), i * 150);
      }
    });
  });

  // Case study cards stagger in
  document.querySelectorAll('.case-card').forEach((card, i) => {
    ScrollTrigger.create({
      trigger: card,
      start: 'top 80%',
      onEnter: () => {
        setTimeout(() => card.classList.add('visible'), i * 200);
      }
    });
  });

  // Partner strip fades in
  ScrollTrigger.create({
    trigger: '#partner-strip',
    start: 'top 85%',
    onEnter: () => {
      const el = document.getElementById('partner-strip');
      if (el) el.classList.add('visible');
    }
  });

  // ══════════════════════════════════════════
  // SCENE 5 — THE CLOSE
  // ══════════════════════════════════════════

  // Main content fades in
  ScrollTrigger.create({
    trigger: '#scene5',
    start: 'top 65%',
    onEnter: () => {
      const content = document.querySelector('.scene5-content');
      if (content) content.classList.add('visible');
      const footer = document.getElementById('site-footer');
      if (footer) footer.classList.add('visible');
    }
  });

  // ══════════════════════════════════════════
  // PROGRESS DOTS
  // ══════════════════════════════════════════
  ['#scene1','#scene2-wrapper','#scene3-s1-wrapper','#scene4-wrapper','#scene5-wrapper'].forEach((sel, i) => {
    ScrollTrigger.create({
      trigger: sel, start: 'top center', end: 'bottom center',
      onToggle: self => {
        if (self.isActive) {
          document.querySelectorAll('.dot').forEach(d => d.classList.remove('active'));
          const dots = document.querySelectorAll('.dot');
          if (dots[i]) dots[i].classList.add('active');
        }
      }
    });
  });

  }, 300); // end setTimeout
}); // end window.load
</script>

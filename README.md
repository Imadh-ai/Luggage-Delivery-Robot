<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart Airport Luggage Robot</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #040a14;
    --bg2: #071220;
    --panel: #0a1a2e;
    --border: #0e3a6e;
    --accent: #00c8ff;
    --accent2: #00ffb3;
    --accent3: #ff6b35;
    --warn: #ffd700;
    --text: #c8e0f4;
    --text-dim: #5a7a9a;
    --glow: 0 0 20px rgba(0,200,255,0.4);
    --glow2: 0 0 20px rgba(0,255,179,0.3);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    font-size: 16px;
    line-height: 1.6;
    overflow-x: hidden;
  }

  /* ── GRID OVERLAY ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,200,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,200,255,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    pointer-events: none;
    z-index: 0;
  }

  /* ── SCANLINE ── */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 1;
  }

  .container {
    max-width: 1100px;
    margin: 0 auto;
    padding: 0 24px;
    position: relative;
    z-index: 2;
  }

  /* ── HERO ── */
  .hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    position: relative;
    padding: 80px 24px 60px;
    overflow: hidden;
  }

  .hero-bg-ring {
    position: absolute;
    border-radius: 50%;
    border: 1px solid rgba(0,200,255,0.07);
    animation: pulse-ring 6s ease-in-out infinite;
  }
  .hero-bg-ring:nth-child(1) { width: 600px; height: 600px; animation-delay: 0s; }
  .hero-bg-ring:nth-child(2) { width: 900px; height: 900px; animation-delay: 1.5s; border-color: rgba(0,255,179,0.05); }
  .hero-bg-ring:nth-child(3) { width: 1200px; height: 1200px; animation-delay: 3s; border-color: rgba(0,200,255,0.03); }

  @keyframes pulse-ring {
    0%, 100% { transform: scale(1); opacity: 0.5; }
    50% { transform: scale(1.03); opacity: 1; }
  }

  .badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    border: 1px solid var(--accent);
    color: var(--accent);
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 3px;
    padding: 6px 16px;
    border-radius: 2px;
    margin-bottom: 32px;
    animation: fadeSlideDown 0.8s ease both;
    position: relative;
  }
  .badge::before {
    content: '';
    position: absolute;
    inset: -1px;
    background: linear-gradient(90deg, var(--accent), var(--accent2), var(--accent));
    background-size: 200% 100%;
    border-radius: 2px;
    z-index: -1;
    opacity: 0.2;
    animation: shimmer 3s linear infinite;
  }
  @keyframes shimmer { to { background-position: -200% 0; } }

  .hero-icon {
    font-size: 72px;
    margin-bottom: 24px;
    animation: fadeSlideDown 0.9s ease both 0.1s;
    filter: drop-shadow(0 0 30px rgba(0,200,255,0.5));
  }

  .hero h1 {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(26px, 5vw, 52px);
    font-weight: 900;
    line-height: 1.1;
    letter-spacing: 2px;
    animation: fadeSlideDown 1s ease both 0.2s;
    background: linear-gradient(135deg, #fff 0%, var(--accent) 50%, var(--accent2) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 8px;
  }

  .hero-subtitle {
    font-size: 18px;
    color: var(--text-dim);
    max-width: 680px;
    margin: 16px auto 48px;
    animation: fadeSlideDown 1s ease both 0.35s;
    font-weight: 300;
    letter-spacing: 1px;
  }

  .hero-chips {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
    animation: fadeSlideDown 1s ease both 0.5s;
  }

  .chip {
    background: rgba(0,200,255,0.07);
    border: 1px solid rgba(0,200,255,0.25);
    color: var(--accent);
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 1.5px;
    padding: 5px 12px;
    border-radius: 2px;
    transition: all 0.2s;
  }
  .chip:hover {
    background: rgba(0,200,255,0.15);
    border-color: var(--accent);
    box-shadow: var(--glow);
  }

  .scroll-indicator {
    position: absolute;
    bottom: 32px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    color: var(--text-dim);
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    letter-spacing: 2px;
    animation: fadeIn 2s ease both 1.5s;
  }
  .scroll-line {
    width: 1px;
    height: 40px;
    background: linear-gradient(to bottom, var(--accent), transparent);
    animation: scrollPulse 2s ease-in-out infinite;
  }
  @keyframes scrollPulse {
    0%, 100% { opacity: 0.3; transform: scaleY(1); }
    50% { opacity: 1; transform: scaleY(1.1); }
  }

  @keyframes fadeSlideDown {
    from { opacity: 0; transform: translateY(-20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  /* ── SECTION COMMON ── */
  section {
    padding: 80px 0;
    position: relative;
    z-index: 2;
  }

  .section-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 4px;
    color: var(--accent2);
    text-transform: uppercase;
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .section-label::after {
    content: '';
    flex: 1;
    max-width: 60px;
    height: 1px;
    background: var(--accent2);
    opacity: 0.4;
  }

  .section-title {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(20px, 3.5vw, 32px);
    font-weight: 700;
    letter-spacing: 2px;
    margin-bottom: 48px;
    color: #fff;
  }

  /* ── DIVIDER ── */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border), transparent);
    margin: 0;
    position: relative;
    z-index: 2;
  }

  /* ── OVERVIEW ── */
  .overview-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
  }
  @media (max-width: 700px) { .overview-grid { grid-template-columns: 1fr; } }

  .overview-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 32px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s, box-shadow 0.3s;
  }
  .overview-card:hover {
    border-color: var(--accent);
    box-shadow: var(--glow);
  }
  .overview-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    opacity: 0;
    transition: opacity 0.3s;
  }
  .overview-card:hover::before { opacity: 1; }

  .overview-card h3 {
    font-family: 'Orbitron', sans-serif;
    font-size: 14px;
    letter-spacing: 2px;
    color: var(--accent);
    margin-bottom: 12px;
  }
  .overview-card p {
    color: var(--text-dim);
    font-size: 15px;
    line-height: 1.7;
  }
  .card-icon {
    font-size: 36px;
    margin-bottom: 16px;
    filter: drop-shadow(0 0 10px rgba(0,200,255,0.4));
  }

  /* ── WORKFLOW ── */
  .workflow {
    display: flex;
    flex-direction: column;
    gap: 0;
  }

  .workflow-step {
    display: flex;
    gap: 24px;
    align-items: flex-start;
    position: relative;
  }

  .workflow-left {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex-shrink: 0;
    width: 48px;
  }

  .step-num {
    width: 48px;
    height: 48px;
    border: 2px solid var(--accent);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Orbitron', sans-serif;
    font-size: 14px;
    font-weight: 700;
    color: var(--accent);
    background: var(--panel);
    box-shadow: var(--glow);
    flex-shrink: 0;
    z-index: 1;
    transition: background 0.3s, color 0.3s;
  }

  .workflow-step:hover .step-num {
    background: var(--accent);
    color: var(--bg);
  }

  .step-line {
    width: 2px;
    flex: 1;
    min-height: 32px;
    background: linear-gradient(to bottom, var(--accent), rgba(0,200,255,0.1));
    margin: 0 auto;
  }

  .workflow-step:last-child .step-line { display: none; }

  .step-content {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 20px 24px;
    margin-bottom: 16px;
    flex: 1;
    transition: border-color 0.3s, box-shadow 0.3s;
  }
  .workflow-step:hover .step-content {
    border-color: var(--accent);
    box-shadow: 0 0 15px rgba(0,200,255,0.15);
  }
  .step-content h4 {
    font-family: 'Orbitron', sans-serif;
    font-size: 12px;
    letter-spacing: 2px;
    color: var(--accent);
    margin-bottom: 6px;
  }
  .step-content p {
    color: var(--text-dim);
    font-size: 14px;
  }

  /* ── HARDWARE GRID ── */
  .hw-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 16px;
  }

  .hw-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 8px;
    transition: all 0.3s;
    cursor: default;
    position: relative;
    overflow: hidden;
  }
  .hw-card::after {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,200,255,0.05), transparent);
    opacity: 0;
    transition: opacity 0.3s;
  }
  .hw-card:hover {
    border-color: var(--accent2);
    box-shadow: var(--glow2);
    transform: translateY(-2px);
  }
  .hw-card:hover::after { opacity: 1; }

  .hw-icon { font-size: 28px; }
  .hw-name {
    font-family: 'Orbitron', sans-serif;
    font-size: 11px;
    letter-spacing: 1px;
    color: var(--accent2);
    line-height: 1.3;
  }
  .hw-role {
    font-size: 12px;
    color: var(--text-dim);
  }

  /* ── ARCHITECTURE ── */
  .arch-diagram {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 40px;
    position: relative;
    overflow: hidden;
  }
  .arch-diagram::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at center, rgba(0,200,255,0.04) 0%, transparent 70%);
  }

  .arch-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
    position: relative;
    z-index: 1;
  }
  @media (max-width: 700px) {
    .arch-grid { grid-template-columns: repeat(2, 1fr); }
  }

  .arch-unit {
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 20px 12px;
    text-align: center;
    position: relative;
    transition: all 0.3s;
  }
  .arch-unit:hover {
    border-color: var(--accent);
    box-shadow: var(--glow);
  }

  .arch-unit-icon { font-size: 32px; margin-bottom: 10px; }
  .arch-unit-label {
    font-family: 'Orbitron', sans-serif;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--accent);
    display: block;
    margin-bottom: 8px;
    text-transform: uppercase;
  }
  .arch-unit-detail {
    font-size: 11px;
    color: var(--text-dim);
    line-height: 1.5;
  }

  .arch-connector {
    position: absolute;
    top: 50%;
    right: -20px;
    width: 20px;
    height: 2px;
    background: var(--accent);
    opacity: 0.3;
  }

  /* ── FEATURES ── */
  .features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
  }

  .feature-item {
    display: flex;
    gap: 16px;
    align-items: flex-start;
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 20px;
    transition: all 0.3s;
  }
  .feature-item:hover {
    border-color: var(--accent2);
    box-shadow: var(--glow2);
  }

  .feature-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: var(--accent2);
    margin-top: 6px;
    flex-shrink: 0;
    box-shadow: 0 0 8px var(--accent2);
    animation: dotPulse 2s ease-in-out infinite;
  }
  @keyframes dotPulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.6; transform: scale(0.8); }
  }
  .feature-text {
    font-size: 14px;
    color: var(--text);
    font-weight: 400;
  }

  /* ── OBJECTIVES ── */
  .objectives-list {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }
  @media (max-width: 600px) { .objectives-list { grid-template-columns: 1fr; } }

  .obj-item {
    background: var(--panel);
    border: 1px solid var(--border);
    border-left: 3px solid var(--accent);
    border-radius: 0 4px 4px 0;
    padding: 16px 20px;
    font-size: 14px;
    color: var(--text-dim);
    transition: all 0.3s;
  }
  .obj-item:hover {
    border-left-color: var(--accent2);
    color: var(--text);
    box-shadow: -4px 0 16px rgba(0,255,179,0.15);
  }

  /* ── FUTURE ── */
  .future-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 16px;
  }

  .future-card {
    background: rgba(255,107,53,0.05);
    border: 1px solid rgba(255,107,53,0.2);
    border-radius: 4px;
    padding: 20px;
    display: flex;
    gap: 14px;
    align-items: flex-start;
    transition: all 0.3s;
  }
  .future-card:hover {
    background: rgba(255,107,53,0.1);
    border-color: var(--accent3);
    box-shadow: 0 0 16px rgba(255,107,53,0.2);
  }
  .future-arrow {
    color: var(--accent3);
    font-size: 18px;
    flex-shrink: 0;
    margin-top: 2px;
  }
  .future-text {
    font-size: 14px;
    color: var(--text-dim);
  }

  /* ── SOFTWARE ── */
  .tech-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 32px;
  }

  .tech-pill {
    background: rgba(0,200,255,0.06);
    border: 1px solid rgba(0,200,255,0.2);
    border-radius: 40px;
    padding: 8px 20px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    color: var(--accent);
    letter-spacing: 1px;
    transition: all 0.3s;
  }
  .tech-pill:hover {
    background: rgba(0,200,255,0.15);
    box-shadow: var(--glow);
  }

  /* ── IMAGES ── */
  .images-section {
    background: var(--bg2);
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
  }

  .images-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
  }
  @media (max-width: 600px) { .images-grid { grid-template-columns: 1fr; } }

  .img-frame {
    border: 1px solid var(--border);
    border-radius: 4px;
    overflow: hidden;
    position: relative;
    background: var(--panel);
    aspect-ratio: 16/9;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: border-color 0.3s, box-shadow 0.3s;
  }
  .img-frame:hover {
    border-color: var(--accent);
    box-shadow: var(--glow);
  }
  .img-frame img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }
  .img-placeholder {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;
    color: var(--text-dim);
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    letter-spacing: 2px;
  }
  .img-placeholder-icon { font-size: 40px; opacity: 0.3; }

  .img-label {
    position: absolute;
    bottom: 12px;
    left: 12px;
    background: rgba(4,10,20,0.85);
    border: 1px solid var(--border);
    border-radius: 2px;
    padding: 4px 10px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--accent);
    letter-spacing: 2px;
  }

  /* ── KEYWORDS ── */
  .keywords {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }

  .keyword-tag {
    background: rgba(0,255,179,0.05);
    border: 1px solid rgba(0,255,179,0.2);
    color: var(--accent2);
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    padding: 5px 12px;
    border-radius: 2px;
    letter-spacing: 1px;
    transition: all 0.3s;
  }
  .keyword-tag::before { content: '#'; opacity: 0.5; }
  .keyword-tag:hover {
    background: rgba(0,255,179,0.12);
    border-color: var(--accent2);
    box-shadow: var(--glow2);
  }

  /* ── FOOTER ── */
  footer {
    padding: 60px 0 40px;
    text-align: center;
    position: relative;
    z-index: 2;
    border-top: 1px solid var(--border);
  }

  .footer-logo {
    font-family: 'Orbitron', sans-serif;
    font-size: 20px;
    font-weight: 900;
    letter-spacing: 4px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 12px;
  }

  .footer-sub {
    color: var(--text-dim);
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 3px;
    margin-bottom: 32px;
  }

  .footer-line {
    width: 80px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
    margin: 0 auto 24px;
  }

  .footer-credit {
    font-size: 13px;
    color: var(--text-dim);
  }

  /* ── ANIMATED CORNER ACCENT ── */
  .corner-accent {
    position: absolute;
    width: 24px;
    height: 24px;
  }
  .corner-accent.tl { top: 0; left: 0; border-top: 2px solid var(--accent); border-left: 2px solid var(--accent); }
  .corner-accent.tr { top: 0; right: 0; border-top: 2px solid var(--accent); border-right: 2px solid var(--accent); }
  .corner-accent.bl { bottom: 0; left: 0; border-bottom: 2px solid var(--accent); border-left: 2px solid var(--accent); }
  .corner-accent.br { bottom: 0; right: 0; border-bottom: 2px solid var(--accent); border-right: 2px solid var(--accent); }

  /* ── STATUS BAR ── */
  .status-bar {
    background: rgba(0,200,255,0.05);
    border-bottom: 1px solid var(--border);
    padding: 8px 0;
    position: sticky;
    top: 0;
    z-index: 100;
    backdrop-filter: blur(12px);
  }
  .status-bar-inner {
    display: flex;
    align-items: center;
    gap: 24px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    letter-spacing: 2px;
    color: var(--text-dim);
    overflow-x: auto;
    white-space: nowrap;
  }
  .status-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--accent2);
    box-shadow: 0 0 6px var(--accent2);
    flex-shrink: 0;
    animation: dotPulse 1.5s ease-in-out infinite;
  }
  .status-sep { opacity: 0.2; }

  /* ── REVEAL ANIMATION ── */
  .reveal {
    opacity: 0;
    transform: translateY(24px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* ── SCROLL PROGRESS ── */
  #progress-bar {
    position: fixed;
    top: 0; left: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    z-index: 1000;
    width: 0%;
    transition: width 0.1s linear;
    box-shadow: 0 0 8px var(--accent);
  }
</style>
</head>
<body>

<div id="progress-bar"></div>

<!-- STATUS BAR -->
<div class="status-bar">
  <div class="container">
    <div class="status-bar-inner">
      <div class="status-dot"></div>
      <span>SYSTEM ONLINE</span>
      <span class="status-sep">│</span>
      <span>ARDUINO UNO + ESP8266</span>
      <span class="status-sep">│</span>
      <span>IR NAVIGATION ACTIVE</span>
      <span class="status-sep">│</span>
      <span>RFID MODULE READY</span>
      <span class="status-sep">│</span>
      <span>LOAD CELL CALIBRATED</span>
      <span class="status-sep">│</span>
      <span>✈ AIRPORT AUTOMATION PROTOTYPE</span>
    </div>
  </div>
</div>

<!-- HERO -->
<section class="hero">
  <div class="hero-bg-ring"></div>
  <div class="hero-bg-ring"></div>
  <div class="hero-bg-ring"></div>

  <div class="badge">✈ EMBEDDED SYSTEMS & ROBOTICS</div>
  <div class="hero-icon">🤖</div>
  <h1>Smart Autonomous<br>Luggage Robot</h1>
  <p class="hero-subtitle">
    An intelligent embedded robotic system for autonomous luggage transport and guided navigation inside airport environments. IR line-following · RFID destination selection · Real-time load monitoring.
  </p>
  <div class="hero-chips">
    <span class="chip">ARDUINO UNO</span>
    <span class="chip">ESP8266</span>
    <span class="chip">RFID RC522</span>
    <span class="chip">HX711 LOAD CELL</span>
    <span class="chip">L298N MOTOR DRIVER</span>
    <span class="chip">IR SENSOR ARRAY</span>
    <span class="chip">EMBEDDED C/C++</span>
  </div>

  <div class="scroll-indicator">
    <div class="scroll-line"></div>
    SCROLL
  </div>
</section>

<div class="divider"></div>

<!-- OVERVIEW -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">01 // OVERVIEW</div>
      <h2 class="section-title">SYSTEM OVERVIEW</h2>
    </div>
    <div class="overview-grid reveal">
      <div class="overview-card">
        <div class="corner-accent tl"></div>
        <div class="corner-accent br"></div>
        <div class="card-icon">🛣️</div>
        <h3>AUTONOMOUS NAVIGATION</h3>
        <p>Follows predefined airport paths using a precision IR sensor array capable of detecting and tracking black-line routes with real-time correction logic.</p>
      </div>
      <div class="overview-card">
        <div class="corner-accent tl"></div>
        <div class="corner-accent br"></div>
        <div class="card-icon">🪪</div>
        <h3>RFID DESTINATION SYSTEM</h3>
        <p>Passengers scan RFID cards to select their terminal or gate destination. The system maps RFID tags to navigation routes and executes the path autonomously.</p>
      </div>
      <div class="overview-card">
        <div class="corner-accent tl"></div>
        <div class="corner-accent br"></div>
        <div class="card-icon">⚖️</div>
        <h3>REAL-TIME WEIGHT SENSING</h3>
        <p>A 1kg Load Cell paired with the HX711 amplifier measures luggage weight precisely before route execution, ensuring load-safe travel.</p>
      </div>
      <div class="overview-card">
        <div class="corner-accent tl"></div>
        <div class="corner-accent br"></div>
        <div class="card-icon">📡</div>
        <h3>IoT EXPANSION READY</h3>
        <p>NodeMCU ESP8266 module enables optional cloud connectivity via Firebase, remote monitoring, and future mobile app integration capabilities.</p>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- WORKFLOW -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">02 // OPERATION</div>
      <h2 class="section-title">SYSTEM WORKFLOW</h2>
    </div>
    <div class="workflow reveal">

      <div class="workflow-step">
        <div class="workflow-left">
          <div class="step-num">01</div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <h4>SYSTEM INITIALIZATION</h4>
          <p>All sensors, motor drivers, RFID module, and load cell system boot and run self-diagnostics. UART communication established between Arduino and ESP8266.</p>
        </div>
      </div>

      <div class="workflow-step">
        <div class="workflow-left">
          <div class="step-num">02</div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <h4>LUGGAGE PLACEMENT</h4>
          <p>Passenger places their luggage on the robot platform. The system awaits a stable weight reading before proceeding to the next step.</p>
        </div>
      </div>

      <div class="workflow-step">
        <div class="workflow-left">
          <div class="step-num">03</div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <h4>WEIGHT MEASUREMENT</h4>
          <p>Load Cell + HX711 module samples and filters weight data. Readings are processed and displayed. Overload conditions trigger safety halt.</p>
        </div>
      </div>

      <div class="workflow-step">
        <div class="workflow-left">
          <div class="step-num">04</div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <h4>RFID DESTINATION SCAN</h4>
          <p>Passenger taps their RFID card to the RC522 module. The UID is validated and matched to a predefined route in memory — e.g., Gate A, Gate B, Check-In Counter.</p>
        </div>
      </div>

      <div class="workflow-step">
        <div class="workflow-left">
          <div class="step-num">05</div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <h4>ROUTE PROCESSING</h4>
          <p>The Arduino processes the selected destination and loads the corresponding navigation instruction set. Motor direction and intersection decision logic is prepared.</p>
        </div>
      </div>

      <div class="workflow-step">
        <div class="workflow-left">
          <div class="step-num">06</div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <h4>LINE-FOLLOWING NAVIGATION</h4>
          <p>The IR sensor array continuously scans the track. PID-based corrections drive the L298N motor driver to keep the robot precisely on the line path.</p>
        </div>
      </div>

      <div class="workflow-step">
        <div class="workflow-left">
          <div class="step-num">07</div>
        </div>
        <div class="step-content">
          <h4>DESTINATION REACHED</h4>
          <p>Robot halts at the target destination node. A completion signal is issued. The system resets and enters standby mode, awaiting the next passenger interaction.</p>
        </div>
      </div>

    </div>
  </div>
</section>

<div class="divider"></div>

<!-- HARDWARE -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">03 // HARDWARE</div>
      <h2 class="section-title">HARDWARE COMPONENTS</h2>
    </div>
    <div class="hw-grid reveal">
      <div class="hw-card">
        <div class="hw-icon">🧠</div>
        <div class="hw-name">ARDUINO UNO</div>
        <div class="hw-role">Main controller — logic, motor control & sensor management</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">📶</div>
        <div class="hw-name">NodeMCU ESP8266</div>
        <div class="hw-role">IoT communication — WiFi bridge & cloud expansion</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">⚡</div>
        <div class="hw-name">L298N Motor Driver</div>
        <div class="hw-role">Dual H-bridge — controls direction and speed of DC motors</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">👁️</div>
        <div class="hw-name">IR Sensor Array</div>
        <div class="hw-role">Multi-channel infrared — line detection for navigation</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">🪪</div>
        <div class="hw-name">RFID RC522</div>
        <div class="hw-role">13.56 MHz RFID reader — passenger destination authentication</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">⚖️</div>
        <div class="hw-name">Load Cell + HX711</div>
        <div class="hw-role">1kg precision sensing — real-time luggage weight measurement</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">🔧</div>
        <div class="hw-name">DC Geared Motors</div>
        <div class="hw-role">High-torque drive motors — platform movement and maneuvering</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">🔋</div>
        <div class="hw-name">Li-ion Battery + BMS</div>
        <div class="hw-role">Portable power — battery management system for safe operation</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">🔌</div>
        <div class="hw-name">DC-DC Buck Converter</div>
        <div class="hw-role">Voltage regulation — stable power supply for all subsystems</div>
      </div>
      <div class="hw-card">
        <div class="hw-icon">🏗️</div>
        <div class="hw-name">Robot Chassis</div>
        <div class="hw-role">Modular platform — structural base with luggage tray mount</div>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- ARCHITECTURE -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">04 // ARCHITECTURE</div>
      <h2 class="section-title">SYSTEM ARCHITECTURE</h2>
    </div>
    <div class="arch-diagram reveal">
      <div class="arch-grid">
        <div class="arch-unit">
          <div class="arch-unit-icon">🧠</div>
          <span class="arch-unit-label">Controller Unit</span>
          <div class="arch-unit-detail">Arduino UNO<br>Main Logic &<br>Motor Control</div>
        </div>
        <div class="arch-unit">
          <div class="arch-unit-icon">📡</div>
          <span class="arch-unit-label">Communication</span>
          <div class="arch-unit-detail">NodeMCU ESP8266<br>UART + WiFi<br>IoT Expansion</div>
        </div>
        <div class="arch-unit">
          <div class="arch-unit-icon">🔬</div>
          <span class="arch-unit-label">Sensing Unit</span>
          <div class="arch-unit-detail">IR Array<br>Load Cell HX711<br>RFID RC522</div>
        </div>
        <div class="arch-unit">
          <div class="arch-unit-icon">⚙️</div>
          <span class="arch-unit-label">Actuation Unit</span>
          <div class="arch-unit-detail">L298N Driver<br>DC Geared Motors<br>Differential Drive</div>
        </div>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- KEY FEATURES -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">05 // FEATURES</div>
      <h2 class="section-title">KEY FEATURES</h2>
    </div>
    <div class="features-grid reveal">
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">Autonomous line-following navigation system using multi-channel IR sensing</div></div>
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">RFID-based destination selection with UID-to-route mapping logic</div></div>
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">Real-time luggage weight measurement and overload safety detection</div></div>
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">Load Cell + HX711 24-bit ADC weight sensing module integration</div></div>
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">Predefined multi-destination route guidance system</div></div>
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">Battery-powered portable robotic platform with BMS protection</div></div>
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">Modular embedded system design — easy to expand and maintain</div></div>
      <div class="feature-item"><div class="feature-dot"></div><div class="feature-text">Low-cost airport automation prototype — scalable for real-world deployment</div></div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- SOFTWARE -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">06 // SOFTWARE</div>
      <h2 class="section-title">SOFTWARE & TECHNOLOGIES</h2>
    </div>
    <div class="tech-pills reveal">
      <span class="tech-pill">Embedded C / C++</span>
      <span class="tech-pill">Arduino IDE</span>
      <span class="tech-pill">UART Communication</span>
      <span class="tech-pill">RFID System Integration</span>
      <span class="tech-pill">Sensor Fusion Logic</span>
      <span class="tech-pill">Autonomous Navigation Algorithm</span>
      <span class="tech-pill">HX711 ADC Library</span>
      <span class="tech-pill">MFRC522 RFID Library</span>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- PROJECT IMAGES -->
<section class="images-section">
  <div class="container">
    <div class="reveal">
      <div class="section-label">07 // PROTOTYPE</div>
      <h2 class="section-title">PROJECT IMAGES</h2>
    </div>
    <div class="images-grid reveal">
      <div class="img-frame" id="frame1">
        <div class="img-placeholder">
          <div class="img-placeholder-icon">📷</div>
          <span>ROBOT IMAGE 01</span>
          <span style="opacity:0.5;font-size:10px">Replace with actual image</span>
        </div>
        <div class="img-label">PROTOTYPE · VIEW 01</div>
      </div>
      <div class="img-frame" id="frame2">
        <div class="img-placeholder">
          <div class="img-placeholder-icon">📷</div>
          <span>ROBOT IMAGE 02</span>
          <span style="opacity:0.5;font-size:10px">Replace with actual image</span>
        </div>
        <div class="img-label">PROTOTYPE · VIEW 02</div>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- OBJECTIVES -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">08 // OBJECTIVES</div>
      <h2 class="section-title">PROJECT OBJECTIVES</h2>
    </div>
    <div class="objectives-list reveal">
      <div class="obj-item">Reduce physical effort of passengers navigating airport environments</div>
      <div class="obj-item">Provide reliable autonomous indoor navigation assistance</div>
      <div class="obj-item">Develop a low-cost replicable smart robotic prototype</div>
      <div class="obj-item">Demonstrate embedded robotics in real-world public environments</div>
      <div class="obj-item">Improve automation efficiency in public transport facilities</div>
      <div class="obj-item">Establish a foundation for AI-enhanced mobility assistance systems</div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- FUTURE -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">09 // ROADMAP</div>
      <h2 class="section-title">FUTURE IMPROVEMENTS</h2>
    </div>
    <div class="future-grid reveal">
      <div class="future-card">
        <div class="future-arrow">▶</div>
        <div class="future-text">Ultrasonic obstacle avoidance system for real-time collision prevention</div>
      </div>
      <div class="future-card">
        <div class="future-arrow">▶</div>
        <div class="future-text">LCD / OLED display module for live navigation status feedback</div>
      </div>
      <div class="future-card">
        <div class="future-arrow">▶</div>
        <div class="future-text">Firebase IoT cloud monitoring for fleet management dashboard</div>
      </div>
      <div class="future-card">
        <div class="future-arrow">▶</div>
        <div class="future-text">Mobile application integration for remote control and status tracking</div>
      </div>
      <div class="future-card">
        <div class="future-arrow">▶</div>
        <div class="future-text">Voice-based navigation system using onboard speech recognition</div>
      </div>
      <div class="future-card">
        <div class="future-arrow">▶</div>
        <div class="future-text">AI-based human-following capability using computer vision and deep learning</div>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- KEYWORDS -->
<section>
  <div class="container">
    <div class="reveal">
      <div class="section-label">10 // TAGS</div>
      <h2 class="section-title">KEYWORDS</h2>
    </div>
    <div class="keywords reveal">
      <span class="keyword-tag">Arduino</span>
      <span class="keyword-tag">ESP8266</span>
      <span class="keyword-tag">RFID</span>
      <span class="keyword-tag">LineFollowingRobot</span>
      <span class="keyword-tag">EmbeddedSystems</span>
      <span class="keyword-tag">IoT</span>
      <span class="keyword-tag">Robotics</span>
      <span class="keyword-tag">AutonomousNavigation</span>
      <span class="keyword-tag">SensorFusion</span>
      <span class="keyword-tag">SmartAirport</span>
      <span class="keyword-tag">Automation</span>
      <span class="keyword-tag">HX711</span>
      <span class="keyword-tag">L298N</span>
      <span class="keyword-tag">MFRC522</span>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="container">
    <div class="footer-logo">SMART AIRPORT ROBOT</div>
    <div class="footer-sub">EMBEDDED SYSTEMS & ROBOTICS PROJECT</div>
    <div class="footer-line"></div>
    <div class="footer-credit">
      Autonomous Navigation · RFID Destination Selection · Load Monitoring<br>
      <span style="color:var(--text-dim);font-size:12px;font-family:'Share Tech Mono',monospace;letter-spacing:2px;">
        Built with Arduino UNO + NodeMCU ESP8266 · Embedded C/C++
      </span>
    </div>
  </div>
</footer>

<script>
  // ── SCROLL PROGRESS ──
  window.addEventListener('scroll', () => {
    const scrollTop = window.scrollY;
    const docHeight = document.documentElement.scrollHeight - window.innerHeight;
    const pct = (scrollTop / docHeight) * 100;
    document.getElementById('progress-bar').style.width = pct + '%';
  });

  // ── REVEAL ON SCROLL ──
  const revealEls = document.querySelectorAll('.reveal');
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry, i) => {
      if (entry.isIntersecting) {
        setTimeout(() => entry.target.classList.add('visible'), 80);
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.1 });
  revealEls.forEach(el => observer.observe(el));

  // ── STATUS BAR TICKER ──
  const statusItems = document.querySelectorAll('.status-bar-inner span:not(.status-sep)');
  let si = 0;
  setInterval(() => {
    statusItems.forEach(s => s.style.color = '');
    statusItems[si % statusItems.length].style.color = 'var(--accent)';
    si++;
  }, 2000);
</script>
</body>
</html>

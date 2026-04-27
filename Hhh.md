<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BSc Physics CGPA Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Space+Mono:ital,wght@0,400;0,700;1,400&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #05050f;
    --surface: #0d0d1f;
    --surface2: #13132a;
    --border: #1e1e3f;
    --accent: #7c6af7;
    --accent2: #f76a8c;
    --accent3: #6af7c4;
    --text: #e8e8f0;
    --muted: #6b6b8f;
    --fail: #f76a6a;
    --pass: #6af7a0;
    --gold: #f7c36a;
  }
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* NOISE OVERLAY */
  body::before {
    content: '';
    position: fixed; inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none; z-index: 9999; opacity: 0.4;
  }

  /* GRID BACKGROUND */
  body::after {
    content: '';
    position: fixed; inset: 0;
    background-image: linear-gradient(var(--border) 1px, transparent 1px),
                      linear-gradient(90deg, var(--border) 1px, transparent 1px);
    background-size: 48px 48px;
    pointer-events: none; z-index: 0; opacity: 0.4;
  }

  .container { position: relative; z-index: 1; max-width: 900px; margin: 0 auto; padding: 0 20px 80px; }

  /* HEADER */
  header {
    padding: 60px 0 40px;
    text-align: center;
    position: relative;
  }
  .brand-tag {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    letter-spacing: 4px;
    text-transform: uppercase;
    margin-bottom: 12px;
    display: block;
  }
  .brand-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(52px, 10vw, 96px);
    letter-spacing: 2px;
    line-height: 0.9;
    background: linear-gradient(135deg, #fff 0%, var(--accent) 50%, var(--accent2) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .brand-sub {
    font-family: 'Space Mono', monospace;
    font-size: 12px;
    color: var(--muted);
    margin-top: 14px;
    letter-spacing: 2px;
  }
  .disclaimer {
    display: inline-block;
    margin-top: 18px;
    background: rgba(247, 106, 140, 0.1);
    border: 1px solid rgba(247, 106, 140, 0.3);
    border-radius: 6px;
    padding: 8px 16px;
    font-size: 12px;
    color: var(--accent2);
    font-family: 'Space Mono', monospace;
  }

  /* GRADE SCALE */
  .grade-scale {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    margin: 32px 0;
  }
  .grade-chip {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 6px 12px;
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    transition: all 0.2s;
  }
  .grade-chip span { color: var(--accent); font-weight: 700; }

  /* SECTION CARDS */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 28px;
    margin-bottom: 20px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s;
  }
  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    opacity: 0;
    transition: opacity 0.3s;
  }
  .card:hover { border-color: rgba(124, 106, 247, 0.4); }
  .card:hover::before { opacity: 1; }

  .card-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 22px;
    letter-spacing: 1px;
    color: var(--accent);
    margin-bottom: 6px;
  }
  .card-desc {
    font-size: 13px;
    color: var(--muted);
    margin-bottom: 20px;
    font-family: 'Space Mono', monospace;
  }

  /* SEM SELECTOR */
  .sem-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin-bottom: 8px;
  }
  @media(max-width: 600px) { .sem-grid { grid-template-columns: repeat(2, 1fr); } }
  .sem-btn {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px 8px;
    cursor: pointer;
    text-align: center;
    transition: all 0.25s;
    color: var(--muted);
    font-family: 'Space Mono', monospace;
    font-size: 11px;
  }
  .sem-btn .sem-num {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 28px;
    color: var(--text);
    display: block;
    line-height: 1;
  }
  .sem-btn:hover { border-color: var(--accent); background: rgba(124,106,247,0.08); }
  .sem-btn.active {
    border-color: var(--accent);
    background: rgba(124,106,247,0.15);
    color: var(--accent);
  }
  .sem-btn.active .sem-num { color: var(--accent); }

  /* PREVIOUS SGPA */
  .prev-sgpa-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 12px;
    margin-top: 12px;
  }
  .prev-sgpa-item label {
    font-size: 11px;
    font-family: 'Space Mono', monospace;
    color: var(--muted);
    display: block;
    margin-bottom: 6px;
  }
  .prev-sgpa-item input {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px 14px;
    color: var(--text);
    font-family: 'Space Mono', monospace;
    font-size: 14px;
    outline: none;
    transition: border-color 0.2s;
  }
  .prev-sgpa-item input:focus { border-color: var(--accent); }

  /* SUBJECT TABLE */
  .subjects-section { margin-top: 20px; }
  .subjects-header {
    display: grid;
    grid-template-columns: 1fr 120px 90px 100px;
    gap: 10px;
    padding: 0 12px 8px;
    font-size: 10px;
    font-family: 'Space Mono', monospace;
    color: var(--muted);
    letter-spacing: 1px;
    text-transform: uppercase;
  }
  .subject-row {
    display: grid;
    grid-template-columns: 1fr 120px 90px 100px;
    gap: 10px;
    padding: 12px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    margin-bottom: 8px;
    align-items: center;
    transition: all 0.2s;
    position: relative;
  }
  .subject-row.failing {
    border-color: rgba(247, 106, 106, 0.4);
    background: rgba(247, 106, 106, 0.05);
  }
  .subject-row.failing .sub-name { text-decoration: line-through; color: var(--fail); opacity: 0.7; }
  .sub-name {
    font-size: 13px;
    color: var(--text);
    font-weight: 500;
  }
  .sub-type {
    font-size: 10px;
    font-family: 'Space Mono', monospace;
    color: var(--muted);
    margin-top: 2px;
  }

  select, .grade-select {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 8px;

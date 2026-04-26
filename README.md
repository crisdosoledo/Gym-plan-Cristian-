<!DOCTYPE html>

<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="GymCoach">
<meta name="theme-color" content="#000000">
<meta name="mobile-web-app-capable" content="yes">
<title>GymCoach — Il tuo allenatore</title>

<!-- Inline manifest as data URL for self-contained install -->

<link rel="manifest" href='data:application/manifest+json,{"name":"GymCoach","short_name":"GymCoach","start_url":"./","display":"standalone","background_color":"%23000000","theme_color":"%23000000","orientation":"portrait"}'>

<!-- App icon as embedded SVG (dumbbell) -->

<link rel="apple-touch-icon" href='data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 180 180"><rect width="180" height="180" rx="40" fill="black"/><g fill="%2330d158"><rect x="30" y="75" width="14" height="30" rx="3"/><rect x="46" y="65" width="10" height="50" rx="2"/><rect x="58" y="86" width="64" height="8" rx="2"/><rect x="124" y="65" width="10" height="50" rx="2"/><rect x="136" y="75" width="14" height="30" rx="3"/></g></svg>'>

<style>
/* ─────────────────────────────────────────────────────────────
   GymCoach — Apple-inspired iOS design language
   System font (SF Pro on Apple devices), OLED-black background,
   refined minimalism with fitness green accent.
   ───────────────────────────────────────────────────────────── */

* { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
:root {
  --bg: #000000;
  --surface-1: #1c1c1e;
  --surface-2: #2c2c2e;
  --surface-3: #3a3a3c;
  --border: rgba(255,255,255,0.08);
  --border-strong: rgba(255,255,255,0.16);
  --text: #ffffff;
  --text-secondary: #98989f;
  --text-tertiary: #6c6c70;
  --accent: #30d158;       /* iOS system green - fitness vibe */
  --accent-dim: #248a3d;
  --warning: #ff9f0a;
  --danger: #ff453a;
  --info: #0a84ff;
  --safe-top: env(safe-area-inset-top, 0);
  --safe-bottom: env(safe-area-inset-bottom, 0);
  --tab-h: 84px;
}

html, body {
  background: var(--bg);
  color: var(--text);
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', system-ui, sans-serif;
  font-size: 17px;
  line-height: 1.4;
  -webkit-font-smoothing: antialiased;
  overflow-x: hidden;
  overscroll-behavior-y: none;
  height: 100%;
  width: 100%;
}
body { position: fixed; inset: 0; }

#app {
  position: absolute; inset: 0;
  display: flex; flex-direction: column;
  padding-top: var(--safe-top);
}

/* ── Top bar ───────────────────────────────────────── */
.topbar {
  display: flex; align-items: center; justify-content: space-between;
  padding: 14px 20px 8px;
  background: rgba(0,0,0,0.6);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  border-bottom: 0.5px solid var(--border);
  position: relative; z-index: 10;
}
.topbar .title { font-size: 17px; font-weight: 600; letter-spacing: -0.2px; }
.topbar .greeting { font-size: 13px; color: var(--text-secondary); font-weight: 500; }
.topbar .stack { display: flex; flex-direction: column; gap: 1px; }
.topbar .icon-btn {
  width: 36px; height: 36px; border-radius: 50%;
  background: var(--surface-1);
  display: flex; align-items: center; justify-content: center;
  border: none; color: var(--text); cursor: pointer;
  transition: transform 0.15s, background 0.2s;
}
.topbar .icon-btn:active { transform: scale(0.92); background: var(--surface-2); }

/* ── Main scroll container ───────────────────────────── */
.view {
  flex: 1; overflow-y: auto; overflow-x: hidden;
  -webkit-overflow-scrolling: touch;
  padding-bottom: calc(var(--tab-h) + var(--safe-bottom) + 20px);
  display: none;
}
.view.active { display: block; animation: fadeIn 0.25s ease; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

/* ── Bottom tab bar ────────────────────────────────── */
.tabbar {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  display: flex;
  height: var(--tab-h);
  padding: 8px 0 calc(var(--safe-bottom) + 4px);
  background: rgba(20,20,22,0.85);
  backdrop-filter: saturate(180%) blur(28px);
  -webkit-backdrop-filter: saturate(180%) blur(28px);
  border-top: 0.5px solid var(--border);
  z-index: 50;
}
.tab {
  flex: 1; background: none; border: none; color: var(--text-tertiary);
  display: flex; flex-direction: column; align-items: center; gap: 3px;
  font-size: 10px; font-weight: 500; letter-spacing: 0.1px;
  cursor: pointer; transition: color 0.15s;
  padding: 4px 0;
}
.tab svg { width: 26px; height: 26px; transition: transform 0.15s; }
.tab.active { color: var(--accent); }
.tab:active svg { transform: scale(0.9); }

/* ── Cards ─────────────────────────────────────────── */
.section { padding: 0 20px; }
.section-title {
  font-size: 13px; font-weight: 600; color: var(--text-secondary);
  text-transform: uppercase; letter-spacing: 0.6px;
  margin: 28px 0 10px; padding: 0 4px;
}
.card {
  background: var(--surface-1);
  border-radius: 16px;
  border: 0.5px solid var(--border);
  overflow: hidden;
}
.card + .card { margin-top: 12px; }

.hero-card {
  background: linear-gradient(135deg, #1a3a2a 0%, #0f2418 100%);
  border: 1px solid rgba(48,209,88,0.25);
  border-radius: 20px;
  padding: 22px;
  position: relative; overflow: hidden;
}
.hero-card::before {
  content: ''; position: absolute; top: -40%; right: -20%;
  width: 280px; height: 280px;
  background: radial-gradient(circle, rgba(48,209,88,0.18) 0%, transparent 60%);
  pointer-events: none;
}
.hero-eyebrow { font-size: 12px; font-weight: 600; color: var(--accent); text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 6px; }
.hero-title { font-size: 28px; font-weight: 700; letter-spacing: -0.6px; margin-bottom: 4px; }
.hero-subtitle { font-size: 15px; color: var(--text-secondary); margin-bottom: 18px; }
.hero-meta { display: flex; gap: 18px; margin-bottom: 18px; flex-wrap: wrap; }
.hero-meta-item { display: flex; flex-direction: column; }
.hero-meta-item .lbl { font-size: 11px; color: var(--text-tertiary); text-transform: uppercase; letter-spacing: 0.5px; }
.hero-meta-item .val { font-size: 17px; font-weight: 600; margin-top: 2px; }

.btn {
  display: inline-flex; align-items: center; justify-content: center; gap: 8px;
  background: var(--accent); color: #001a09;
  border: none; border-radius: 12px;
  padding: 14px 22px; font-size: 16px; font-weight: 600;
  cursor: pointer; width: 100%;
  transition: transform 0.1s, opacity 0.15s, background 0.2s;
  font-family: inherit;
  letter-spacing: -0.2px;
}
.btn:active { transform: scale(0.98); opacity: 0.9; }
.btn.secondary { background: var(--surface-2); color: var(--text); }
.btn.ghost { background: transparent; border: 1px solid var(--border-strong); color: var(--text); }
.btn.danger { background: var(--danger); color: white; }
.btn.compact { padding: 10px 16px; font-size: 14px; width: auto; }

/* Stats grid */
.stats-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 12px;
  margin-top: 14px;
}
.stat-card {
  background: var(--surface-1);
  border-radius: 14px;
  padding: 16px;
  border: 0.5px solid var(--border);
}
.stat-card .ic {
  width: 32px; height: 32px; border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  margin-bottom: 10px;
}
.stat-card .lbl { font-size: 12px; color: var(--text-secondary); font-weight: 500; }
.stat-card .val { font-size: 22px; font-weight: 700; margin-top: 2px; letter-spacing: -0.5px; }
.stat-card .delta { font-size: 12px; margin-top: 4px; font-weight: 500; }
.delta.up { color: var(--accent); }
.delta.down { color: var(--danger); }

/* List rows (iOS Settings style) */
.list { background: var(--surface-1); border-radius: 14px; overflow: hidden; border: 0.5px solid var(--border); }
.row {
  display: flex; align-items: center; gap: 12px;
  padding: 14px 16px;
  border-bottom: 0.5px solid var(--border);
  cursor: pointer;
  transition: background 0.15s;
  background: transparent;
  border-left: none; border-right: none; border-top: none;
  width: 100%; color: var(--text); font-family: inherit; font-size: 16px;
  text-align: left;
}
.row:last-child { border-bottom: none; }
.row:active { background: var(--surface-2); }
.row .row-icon {
  width: 30px; height: 30px; border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.row .row-main { flex: 1; min-width: 0; }
.row .row-title { font-size: 16px; font-weight: 500; }
.row .row-sub { font-size: 13px; color: var(--text-secondary); margin-top: 2px; }
.row .row-trail { color: var(--text-tertiary); font-size: 14px; display: flex; align-items: center; gap: 4px; }
.row .chev { color: var(--text-tertiary); }

/* Day cards (program) */
.day-card {
  background: var(--surface-1);
  border-radius: 18px;
  padding: 18px;
  border: 0.5px solid var(--border);
  cursor: pointer;
  transition: transform 0.1s, border-color 0.2s;
  position: relative;
  overflow: hidden;
}
.day-card:active { transform: scale(0.99); }
.day-card.today { border-color: var(--accent); background: linear-gradient(135deg, rgba(48,209,88,0.08), rgba(48,209,88,0.02)); }
.day-card.today::after {
  content: 'OGGI';
  position: absolute; top: 16px; right: 16px;
  font-size: 10px; font-weight: 700; letter-spacing: 0.6px;
  background: var(--accent); color: #001a09;
  padding: 3px 8px; border-radius: 6px;
}
.day-card .dow { font-size: 12px; color: var(--accent); font-weight: 600; text-transform: uppercase; letter-spacing: 0.6px; }
.day-card .name { font-size: 20px; font-weight: 700; margin-top: 4px; letter-spacing: -0.4px; }
.day-card .focus { font-size: 14px; color: var(--text-secondary); margin-top: 4px; }
.day-card .meta {
  display: flex; gap: 14px; margin-top: 14px; padding-top: 14px;
  border-top: 0.5px solid var(--border);
  font-size: 13px; color: var(--text-secondary);
}
.day-card .meta-item { display: flex; align-items: center; gap: 5px; }

.rest-day {
  background: var(--surface-1);
  border: 1px dashed var(--border-strong);
  border-radius: 16px;
  padding: 18px;
  text-align: center;
  color: var(--text-secondary);
  font-size: 14px;
}

/* Exercise list (workout detail / active) */
.exercise-item {
  background: var(--surface-1);
  border: 0.5px solid var(--border);
  border-radius: 14px;
  padding: 16px;
  margin-bottom: 10px;
}
.exercise-item.done { border-color: var(--accent); background: linear-gradient(135deg, rgba(48,209,88,0.06), rgba(48,209,88,0.01)); }
.ex-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 12px; }
.ex-name { font-size: 17px; font-weight: 600; letter-spacing: -0.2px; }
.ex-target { font-size: 13px; color: var(--text-secondary); margin-top: 4px; }
.ex-tip { font-size: 12px; color: var(--accent); margin-top: 6px; font-style: italic; }
.ex-num {
  width: 26px; height: 26px; border-radius: 50%;
  background: var(--surface-2);
  display: flex; align-items: center; justify-content: center;
  font-size: 12px; font-weight: 600; color: var(--text-secondary);
  flex-shrink: 0;
}
.exercise-item.done .ex-num { background: var(--accent); color: #001a09; }

/* Set logger (active workout) */
.sets-grid { margin-top: 14px; display: flex; flex-direction: column; gap: 8px; }
.set-row {
  display: grid;
  grid-template-columns: 36px 1fr 1fr 44px;
  gap: 8px;
  align-items: center;
  padding: 6px 0;
}
.set-row .set-num { font-size: 13px; font-weight: 600; color: var(--text-secondary); text-align: center; }
.set-row input {
  background: var(--surface-2);
  border: 1px solid var(--border);
  color: var(--text);
  font-family: inherit;
  font-size: 16px;
  font-weight: 500;
  padding: 10px 12px;
  border-radius: 10px;
  width: 100%;
  text-align: center;
  -moz-appearance: textfield;
}
.set-row input:focus { outline: none; border-color: var(--accent); }
.set-row input::-webkit-outer-spin-button, .set-row input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
.set-row .set-toggle {
  width: 40px; height: 40px; border-radius: 10px;
  background: var(--surface-2); border: 1px solid var(--border);
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; color: var(--text-tertiary);
}
.set-row.completed .set-toggle { background: var(--accent); color: #001a09; border-color: var(--accent); }
.set-row.completed input { color: var(--text); background: rgba(48,209,88,0.08); border-color: rgba(48,209,88,0.3); }
.set-row .placeholder { color: var(--text-tertiary); font-weight: 400; }
.sets-header {
  display: grid;
  grid-template-columns: 36px 1fr 1fr 44px;
  gap: 8px;
  font-size: 11px; color: var(--text-tertiary); text-transform: uppercase; letter-spacing: 0.5px;
  font-weight: 600;
  margin-bottom: 4px;
}
.sets-header div { text-align: center; }

/* Rest timer overlay */
.rest-timer {
  position: fixed;
  bottom: calc(var(--tab-h) + 14px + var(--safe-bottom));
  left: 16px; right: 16px;
  background: var(--surface-2);
  border: 1px solid var(--border-strong);
  border-radius: 14px;
  padding: 12px 16px;
  display: flex; align-items: center; gap: 12px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.5);
  z-index: 100;
  animation: slideUp 0.3s ease;
}
@keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
.rest-timer .ring {
  width: 38px; height: 38px;
  border-radius: 50%;
  background: var(--accent);
  display: flex; align-items: center; justify-content: center;
  color: #001a09;
}
.rest-timer .info { flex: 1; }
.rest-timer .info .label { font-size: 11px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600; }
.rest-timer .info .time { font-size: 22px; font-weight: 700; letter-spacing: -0.5px; font-variant-numeric: tabular-nums; }
.rest-timer .skip {
  background: var(--surface-3); border: none; color: var(--text);
  padding: 8px 14px; border-radius: 10px;
  font-family: inherit; font-size: 13px; font-weight: 600;
  cursor: pointer;
}

/* Modal / sheet */
.sheet-backdrop {
  position: fixed; inset: 0; background: rgba(0,0,0,0.5);
  z-index: 200;
  opacity: 0; pointer-events: none;
  transition: opacity 0.25s;
  backdrop-filter: blur(4px);
}
.sheet-backdrop.open { opacity: 1; pointer-events: auto; }
.sheet {
  position: fixed; bottom: 0; left: 0; right: 0;
  background: var(--surface-1);
  border-radius: 20px 20px 0 0;
  z-index: 201;
  transform: translateY(100%);
  transition: transform 0.3s cubic-bezier(0.32, 0.72, 0, 1);
  max-height: 90vh;
  display: flex; flex-direction: column;
  padding-bottom: var(--safe-bottom);
}
.sheet.open { transform: translateY(0); }
.sheet-handle { width: 36px; height: 5px; background: var(--surface-3); border-radius: 3px; margin: 8px auto 4px; flex-shrink: 0; }
.sheet-header { padding: 12px 20px 16px; display: flex; align-items: center; justify-content: space-between; flex-shrink: 0; border-bottom: 0.5px solid var(--border); }
.sheet-title { font-size: 18px; font-weight: 700; letter-spacing: -0.3px; }
.sheet-close { background: var(--surface-2); border: none; color: var(--text); width: 30px; height: 30px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; }
.sheet-body { padding: 20px; overflow-y: auto; flex: 1; -webkit-overflow-scrolling: touch; }

/* Active workout (full-screen overlay) */
.workout-overlay {
  position: fixed; inset: 0;
  background: var(--bg);
  z-index: 300;
  display: none; flex-direction: column;
  padding-top: var(--safe-top);
}
.workout-overlay.open { display: flex; }
.workout-topbar {
  display: flex; align-items: center; gap: 12px;
  padding: 12px 16px;
  border-bottom: 0.5px solid var(--border);
  background: rgba(0,0,0,0.6);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
}
.workout-topbar .back-btn { background: var(--surface-1); border: none; color: var(--text); width: 34px; height: 34px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; flex-shrink: 0; }
.workout-topbar .info { flex: 1; min-width: 0; }
.workout-topbar .info .name { font-size: 16px; font-weight: 700; letter-spacing: -0.3px; }
.workout-topbar .info .meta { font-size: 12px; color: var(--text-secondary); margin-top: 1px; font-variant-numeric: tabular-nums; }
.workout-topbar .end-btn { background: var(--danger); color: white; border: none; padding: 8px 14px; border-radius: 10px; font-family: inherit; font-size: 13px; font-weight: 600; cursor: pointer; }

.progress-bar {
  height: 3px; background: var(--surface-2); position: relative;
}
.progress-bar .fill { height: 100%; background: var(--accent); transition: width 0.3s; }

.workout-body {
  flex: 1; overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  padding: 16px 16px 140px;
}

/* Forms */
.form-group { margin-bottom: 16px; }
.form-label { display: block; font-size: 13px; color: var(--text-secondary); margin-bottom: 6px; font-weight: 500; padding: 0 4px; }
.form-input, .form-select, .form-textarea {
  width: 100%;
  background: var(--surface-2);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 13px 14px;
  color: var(--text);
  font-family: inherit;
  font-size: 16px;
}
.form-textarea { min-height: 90px; resize: vertical; font-family: inherit; }
.form-input:focus, .form-select:focus, .form-textarea:focus { outline: none; border-color: var(--accent); }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }

/* Chart */
.chart-card { background: var(--surface-1); border-radius: 16px; border: 0.5px solid var(--border); padding: 18px; }
.chart-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 14px; }
.chart-current { font-size: 32px; font-weight: 700; letter-spacing: -0.8px; }
.chart-current .unit { font-size: 18px; color: var(--text-secondary); font-weight: 500; }
.chart-delta { font-size: 13px; margin-top: 2px; font-weight: 500; }
canvas.chart { width: 100%; height: 160px; display: block; }

/* Photos */
.photo-grid {
  display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px;
}
.photo-tile {
  aspect-ratio: 3/4;
  background: var(--surface-1);
  border: 0.5px solid var(--border);
  border-radius: 12px;
  overflow: hidden; position: relative;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer;
}
.photo-tile img { width: 100%; height: 100%; object-fit: cover; }
.photo-tile .add-icon { color: var(--text-tertiary); }
.photo-tile .date-overlay {
  position: absolute; bottom: 0; left: 0; right: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.85), transparent);
  color: white;
  font-size: 10px; font-weight: 600;
  padding: 14px 6px 5px; text-align: center;
  letter-spacing: 0.3px;
}
.photo-tile .pose-tag {
  position: absolute; top: 6px; left: 6px;
  background: rgba(0,0,0,0.7);
  color: white; font-size: 9px; font-weight: 700; letter-spacing: 0.5px;
  padding: 2px 6px; border-radius: 4px; text-transform: uppercase;
  backdrop-filter: blur(8px);
}

.photo-section-title { display: flex; justify-content: space-between; align-items: center; margin: 22px 4px 10px; }
.photo-section-title .name { font-size: 14px; font-weight: 600; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.5px; }
.photo-section-title .add { background: none; border: none; color: var(--accent); font-family: inherit; font-size: 14px; font-weight: 600; cursor: pointer; }

/* Photo viewer overlay */
.photo-viewer {
  position: fixed; inset: 0; background: rgba(0,0,0,0.95);
  z-index: 400; display: none;
  align-items: center; justify-content: center;
  flex-direction: column;
}
.photo-viewer.open { display: flex; }
.photo-viewer img { max-width: 100%; max-height: 80vh; object-fit: contain; }
.photo-viewer .photo-meta { color: white; font-size: 14px; margin-top: 16px; text-align: center; }
.photo-viewer .photo-meta .d { font-weight: 600; }
.photo-viewer .photo-meta .p { color: var(--text-secondary); font-size: 12px; margin-top: 2px; text-transform: uppercase; letter-spacing: 0.6px; }
.photo-viewer .controls {
  position: absolute; top: calc(var(--safe-top) + 12px); right: 16px; display: flex; gap: 8px;
}
.photo-viewer .controls button {
  background: rgba(255,255,255,0.12); border: none; color: white;
  width: 38px; height: 38px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; backdrop-filter: blur(20px);
}

/* Coach card / tips */
.coach-card {
  background: linear-gradient(135deg, #1c2c1f 0%, #131816 100%);
  border: 1px solid rgba(48,209,88,0.2);
  border-radius: 16px;
  padding: 18px;
  margin-top: 14px;
}
.coach-card .who {
  display: flex; align-items: center; gap: 10px; margin-bottom: 10px;
}
.coach-card .avatar {
  width: 36px; height: 36px; border-radius: 50%;
  background: var(--accent); color: #001a09;
  display: flex; align-items: center; justify-content: center;
  font-weight: 700; font-size: 14px;
}
.coach-card .role { font-size: 13px; font-weight: 600; }
.coach-card .role-sub { font-size: 11px; color: var(--text-secondary); }
.coach-card .quote { font-size: 14px; line-height: 1.5; color: rgba(255,255,255,0.92); font-style: italic; }

/* PR badge */
.pr-badge { background: linear-gradient(135deg, #ff9f0a, #ff6b00); color: #1a0e00; font-size: 10px; font-weight: 700; padding: 2px 6px; border-radius: 5px; text-transform: uppercase; letter-spacing: 0.4px; }

/* Empty state */
.empty {
  text-align: center; padding: 40px 20px; color: var(--text-secondary);
}
.empty svg { color: var(--text-tertiary); margin-bottom: 12px; }
.empty .t { font-size: 16px; font-weight: 600; color: var(--text); margin-bottom: 4px; }
.empty .s { font-size: 14px; }

/* Toast */
.toast {
  position: fixed; bottom: calc(var(--tab-h) + 30px + var(--safe-bottom));
  left: 50%; transform: translateX(-50%) translateY(20px);
  background: var(--surface-3);
  color: var(--text);
  padding: 10px 18px;
  border-radius: 22px;
  font-size: 14px;
  font-weight: 500;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.25s, transform 0.25s;
  z-index: 1000;
  border: 0.5px solid var(--border-strong);
  box-shadow: 0 8px 24px rgba(0,0,0,0.4);
  max-width: 84%;
  text-align: center;
}
.toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

/* Workout history list */
.history-row {
  display: flex; align-items: center; padding: 14px 16px;
  border-bottom: 0.5px solid var(--border);
  gap: 12px;
}
.history-row:last-child { border-bottom: none; }
.history-row .day-pill {
  width: 44px; height: 44px; border-radius: 10px;
  background: var(--surface-2);
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.history-row .day-pill .d { font-size: 16px; font-weight: 700; line-height: 1; }
.history-row .day-pill .m { font-size: 9px; color: var(--text-secondary); margin-top: 2px; text-transform: uppercase; letter-spacing: 0.5px; }
.history-row .info { flex: 1; min-width: 0; }
.history-row .name { font-size: 15px; font-weight: 600; }
.history-row .sub { font-size: 12px; color: var(--text-secondary); margin-top: 2px; }

/* Misc */
.text-secondary { color: var(--text-secondary); }
.text-accent { color: var(--accent); }
.divider-thin { height: 0.5px; background: var(--border); margin: 14px 0; }
.spacer-sm { height: 8px; }
.spacer { height: 16px; }
.spacer-lg { height: 24px; }

/* Hide arrows on number inputs (mobile) */
input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; margin: 0; }
input[type=number] { -moz-appearance: textfield; }

/* ── Calendar ──────────────────────────────────────── */
.cal-strip {
  display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px;
  margin-top: 14px;
}
.cal-strip-cell {
  aspect-ratio: 1;
  background: var(--surface-1);
  border-radius: 10px;
  border: 0.5px solid var(--border);
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  position: relative;
  cursor: pointer;
}
.cal-strip-cell .dow { font-size: 9px; color: var(--text-tertiary); text-transform: uppercase; letter-spacing: 0.4px; font-weight: 600; }
.cal-strip-cell .dn { font-size: 14px; font-weight: 600; margin-top: 1px; }
.cal-strip-cell.today { border-color: var(--accent); background: rgba(48,209,88,0.08); }
.cal-strip-cell.has-session { background: var(--accent); border-color: var(--accent); color: #001a09; }
.cal-strip-cell.has-session .dow { color: rgba(0,26,9,0.6); }
.cal-strip-cell.has-session.today { box-shadow: 0 0 0 2px rgba(255,255,255,0.3); }
.cal-strip-cell.future { opacity: 0.45; cursor: default; }

.cal-month {
  background: var(--surface-1);
  border-radius: 16px;
  padding: 14px;
  border: 0.5px solid var(--border);
}
.cal-month-header {
  display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;
}
.cal-month-title { font-size: 16px; font-weight: 700; letter-spacing: -0.2px; text-transform: capitalize; }
.cal-nav { display: flex; gap: 6px; }
.cal-nav button {
  width: 30px; height: 30px; border-radius: 50%;
  background: var(--surface-2); border: none; color: var(--text);
  display: flex; align-items: center; justify-content: center; cursor: pointer;
}
.cal-grid {
  display: grid; grid-template-columns: repeat(7, 1fr); gap: 4px;
}
.cal-dow {
  font-size: 10px; font-weight: 700; text-align: center; color: var(--text-tertiary);
  text-transform: uppercase; letter-spacing: 0.5px; padding: 4px 0;
}
.cal-cell {
  aspect-ratio: 1;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  font-size: 13px; font-weight: 500;
  border-radius: 8px;
  position: relative;
  cursor: pointer;
}
.cal-cell.empty { opacity: 0; cursor: default; }
.cal-cell.today { background: var(--surface-2); color: var(--accent); font-weight: 700; }
.cal-cell.has-session::after {
  content: ''; position: absolute; bottom: 4px; width: 5px; height: 5px;
  border-radius: 50%; background: var(--accent);
}
.cal-cell.today.has-session::after { background: var(--accent); box-shadow: 0 0 0 1px rgba(48,209,88,0.3); }
.cal-cell.future { color: var(--text-tertiary); }

/* ── Suggestion pill ────────────────────────────────── */
.suggest-pill {
  display: inline-flex; align-items: center; gap: 4px;
  font-size: 11px; font-weight: 700; padding: 3px 7px;
  border-radius: 5px; letter-spacing: 0.3px;
  text-transform: uppercase;
}
.suggest-pill.up { background: rgba(48,209,88,0.18); color: var(--accent); }
.suggest-pill.hold { background: rgba(152,152,159,0.15); color: var(--text-secondary); }
.suggest-pill.down { background: rgba(255,159,10,0.18); color: var(--warning); }

/* ── Exercise detail chart ──────────────────────────── */
.ex-chart-card {
  background: var(--surface-1);
  border-radius: 14px;
  padding: 16px;
  border: 0.5px solid var(--border);
  margin-bottom: 14px;
}
canvas.ex-chart { width: 100%; height: 130px; display: block; margin-top: 10px; }

/* ── Measurements ──────────────────────────────────── */
.measure-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 10px;
}
.measure-card {
  background: var(--surface-1);
  border: 0.5px solid var(--border);
  border-radius: 12px;
  padding: 14px;
  cursor: pointer;
}
.measure-card .lbl { font-size: 11px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600; }
.measure-card .val { font-size: 22px; font-weight: 700; margin-top: 4px; letter-spacing: -0.5px; }
.measure-card .val .u { font-size: 13px; color: var(--text-secondary); font-weight: 500; margin-left: 2px; }
.measure-card .delta { font-size: 11px; margin-top: 2px; font-weight: 500; }

/* ── Exercise actions menu (during workout) ────────── */
.ex-actions {
  display: flex; gap: 6px; margin-top: 10px; padding-top: 10px;
  border-top: 0.5px solid var(--border);
}
.ex-action-btn {
  flex: 1;
  background: var(--surface-2); border: 1px solid var(--border);
  color: var(--text);
  font-family: inherit; font-size: 12px; font-weight: 600;
  padding: 8px 10px; border-radius: 8px;
  cursor: pointer;
  display: flex; align-items: center; justify-content: center; gap: 4px;
}
.ex-action-btn:active { background: var(--surface-3); }

/* ── Add exercise floating button ──────────────────── */
.add-extra-btn {
  width: 100%;
  background: transparent;
  border: 1px dashed var(--border-strong);
  color: var(--text-secondary);
  padding: 14px;
  border-radius: 14px;
  font-family: inherit; font-size: 14px; font-weight: 600;
  cursor: pointer;
  margin-top: 10px;
  display: flex; align-items: center; justify-content: center; gap: 6px;
}

/* ── Program editor ──────────────────────────────── */
.editor-ex-row {
  display: flex; align-items: center; gap: 10px;
  background: var(--surface-1);
  border: 0.5px solid var(--border);
  border-radius: 12px;
  padding: 12px 14px;
  margin-bottom: 8px;
}
.editor-ex-row .ex-info { flex: 1; min-width: 0; }
.editor-ex-row .ex-info .nm { font-size: 15px; font-weight: 600; }
.editor-ex-row .ex-info .det { font-size: 12px; color: var(--text-secondary); margin-top: 2px; }
.editor-ex-row .reorder { display: flex; flex-direction: column; gap: 2px; flex-shrink: 0; }
.editor-ex-row .reorder button {
  width: 30px; height: 22px;
  background: var(--surface-2); border: 1px solid var(--border);
  color: var(--text); border-radius: 6px;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; padding: 0;
}
.editor-ex-row .reorder button:disabled { opacity: 0.3; cursor: not-allowed; }
.editor-ex-row .reorder button:active:not(:disabled) { background: var(--surface-3); }
.editor-ex-row .actions { display: flex; gap: 4px; flex-shrink: 0; }
.editor-ex-row .actions button {
  width: 30px; height: 30px;
  border-radius: 8px; border: 1px solid var(--border);
  background: var(--surface-2); color: var(--text);
  display: flex; align-items: center; justify-content: center;
  cursor: pointer;
}
.editor-ex-row .actions button.del { color: var(--danger); }
.editor-ex-row .actions button:active { background: var(--surface-3); }

.day-tabs {
  display: flex; gap: 6px; overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  margin-bottom: 14px; padding: 2px;
}
.day-tab {
  flex-shrink: 0;
  background: var(--surface-2); border: 1px solid var(--border);
  color: var(--text);
  padding: 8px 14px; border-radius: 18px;
  font-family: inherit; font-size: 13px; font-weight: 600;
  cursor: pointer;
  white-space: nowrap;
}
.day-tab.active { background: var(--accent); color: #001a09; border-color: var(--accent); }

/* ── Protein ring & cardio cards ──────────────────── */
.daily-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 14px;
}
.protein-card, .cardio-card {
  background: var(--surface-1);
  border: 0.5px solid var(--border);
  border-radius: 14px;
  padding: 14px;
  cursor: pointer;
}
.protein-card { display: flex; align-items: center; gap: 12px; }
.protein-ring { position: relative; width: 60px; height: 60px; flex-shrink: 0; }
.protein-ring svg { width: 100%; height: 100%; transform: rotate(-90deg); }
.protein-ring .ring-bg { stroke: var(--surface-3); fill: none; }
.protein-ring .ring-fg { stroke: var(--info); fill: none; stroke-linecap: round; transition: stroke-dashoffset 0.4s; }
.protein-ring.complete .ring-fg { stroke: var(--accent); }
.protein-ring .pct { position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; font-size: 14px; font-weight: 700; letter-spacing: -0.3px; }
.protein-card .info { flex: 1; min-width: 0; }
.protein-card .lbl { font-size: 11px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600; }
.protein-card .val { font-size: 17px; font-weight: 700; margin-top: 2px; letter-spacing: -0.3px; }
.protein-card .target { font-size: 12px; color: var(--text-secondary); margin-top: 1px; }

.cardio-card { display: flex; flex-direction: column; }
.cardio-card .ic {
  width: 36px; height: 36px; border-radius: 10px;
  background: rgba(255,159,10,0.15); color: var(--warning);
  display: flex; align-items: center; justify-content: center;
  margin-bottom: 8px;
}
.cardio-card .lbl { font-size: 11px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600; }
.cardio-card .val { font-size: 17px; font-weight: 700; margin-top: 2px; letter-spacing: -0.3px; }
.cardio-card .sub { font-size: 12px; color: var(--text-secondary); margin-top: 1px; }

/* Quick add chips */
.chips { display: flex; gap: 8px; flex-wrap: wrap; }
.chip {
  background: var(--surface-2);
  border: 1px solid var(--border);
  color: var(--text);
  padding: 9px 14px;
  border-radius: 18px;
  font-family: inherit; font-size: 14px; font-weight: 600;
  cursor: pointer;
}
.chip:active { background: var(--surface-3); }
.chip.selected { background: var(--accent); color: #001a09; border-color: var(--accent); }

/* ── Warmup checklist ─────────────────────────────── */
.warmup-item {
  display: flex; align-items: center; gap: 12px;
  background: var(--surface-1);
  border: 0.5px solid var(--border);
  border-radius: 12px;
  padding: 13px 14px;
  margin-bottom: 8px;
  cursor: pointer;
  transition: background 0.15s;
}
.warmup-item.done { background: linear-gradient(135deg, rgba(48,209,88,0.08), rgba(48,209,88,0.02)); border-color: rgba(48,209,88,0.3); }
.warmup-check {
  width: 26px; height: 26px; border-radius: 50%;
  border: 1.5px solid var(--border-strong);
  display: flex; align-items: center; justify-content: center;
  color: transparent;
  flex-shrink: 0;
  transition: all 0.15s;
}
.warmup-item.done .warmup-check { background: var(--accent); border-color: var(--accent); color: #001a09; }
.warmup-info { flex: 1; min-width: 0; }
.warmup-name { font-size: 15px; font-weight: 600; }
.warmup-detail { font-size: 12px; color: var(--text-secondary); margin-top: 2px; }

/* ── Photo comparison ─────────────────────────────── */
.compare-overlay {
  position: fixed; inset: 0;
  background: var(--bg);
  z-index: 350;
  display: none; flex-direction: column;
  padding-top: var(--safe-top);
}
.compare-overlay.open { display: flex; }
.compare-topbar {
  display: flex; align-items: center; gap: 12px;
  padding: 12px 16px;
  border-bottom: 0.5px solid var(--border);
}
.compare-topbar .title { flex: 1; font-size: 16px; font-weight: 700; }
.compare-topbar button {
  background: var(--surface-1); border: none; color: var(--text);
  width: 34px; height: 34px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer;
}
.compare-body {
  flex: 1; overflow-y: auto;
  padding: 12px;
  -webkit-overflow-scrolling: touch;
}
.compare-pair {
  display: grid; grid-template-columns: 1fr 1fr; gap: 8px;
}
.compare-pane {
  background: var(--surface-1);
  border-radius: 12px;
  border: 0.5px solid var(--border);
  overflow: hidden;
  display: flex; flex-direction: column;
}
.compare-pane img {
  width: 100%; aspect-ratio: 3/4;
  object-fit: cover;
  background: var(--surface-2);
}
.compare-pane .meta {
  padding: 8px 10px;
  font-size: 12px;
}
.compare-pane .meta .d { font-weight: 700; }
.compare-pane .meta .p { color: var(--text-secondary); font-size: 10px; text-transform: uppercase; letter-spacing: 0.5px; margin-top: 1px; }
.compare-delta {
  background: var(--surface-1);
  border: 0.5px solid var(--border);
  border-radius: 12px;
  padding: 14px;
  margin-top: 12px;
  text-align: center;
}
.compare-delta .big { font-size: 20px; font-weight: 700; letter-spacing: -0.3px; }
.compare-delta .sub { font-size: 12px; color: var(--text-secondary); margin-top: 4px; text-transform: uppercase; letter-spacing: 0.5px; }
.compare-thumbs {
  display: flex; gap: 6px; overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  padding: 4px 2px;
  margin-top: 12px;
}
.compare-thumb {
  flex-shrink: 0;
  width: 56px; height: 75px;
  border-radius: 8px;
  overflow: hidden;
  border: 2px solid transparent;
  cursor: pointer;
  position: relative;
}
.compare-thumb img { width: 100%; height: 100%; object-fit: cover; }
.compare-thumb.selected { border-color: var(--accent); }
.compare-thumb .lbl { position: absolute; top: 4px; left: 4px; background: rgba(0,0,0,0.7); color: white; font-size: 9px; font-weight: 700; padding: 1px 5px; border-radius: 3px; }

/* Calendar cardio dot */
.cal-cell.has-cardio::after {
  content: ''; position: absolute; bottom: 4px; width: 5px; height: 5px;
  border-radius: 50%; background: var(--warning);
}
.cal-cell.has-session.has-cardio::after {
  content: '';
  background: linear-gradient(90deg, var(--accent) 50%, var(--warning) 50%);
  width: 8px;
}
.cal-strip-cell.has-cardio:not(.has-session) {
  background: var(--warning);
  border-color: var(--warning);
  color: #2a1900;
}
.cal-strip-cell.has-cardio:not(.has-session) .dow { color: rgba(42,25,0,0.6); }
.cal-strip-cell.has-session.has-cardio { background: linear-gradient(135deg, var(--accent), var(--warning)); border-color: transparent; color: #001a09; }

</style>

</head>
<body>

<div id="app">

  <!-- Top bar -->

  <header class="topbar" id="topbar">
    <div class="stack">
      <span class="greeting" id="greeting">Buongiorno</span>
      <span class="title" id="topbar-title">Oggi</span>
    </div>
    <button class="icon-btn" id="topbar-action" aria-label="Azione">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="12" cy="12" r="9"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
    </button>
  </header>

  <!-- Today view -->

  <main class="view active" id="view-today" data-view="today"></main>
  <!-- Program view -->
  <main class="view" id="view-program" data-view="program"></main>
  <!-- Progress view -->
  <main class="view" id="view-progress" data-view="progress"></main>
  <!-- Profile view -->
  <main class="view" id="view-profile" data-view="profile"></main>

  <!-- Bottom tab bar -->

  <nav class="tabbar">
    <button class="tab active" data-tab="today">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12l9-9 9 9"/><path d="M5 10v10h14V10"/></svg>
      Oggi
    </button>
    <button class="tab" data-tab="program">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="6" y="10" width="3" height="4" rx="1"/><rect x="15" y="10" width="3" height="4" rx="1"/><line x1="9" y1="12" x2="15" y2="12"/><line x1="3" y1="12" x2="6" y2="12"/><line x1="18" y1="12" x2="21" y2="12"/></svg>
      Programma
    </button>
    <button class="tab" data-tab="progress">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polyline points="3,17 9,11 13,15 21,7"/><polyline points="14,7 21,7 21,14"/></svg>
      Progressi
    </button>
    <button class="tab" data-tab="profile">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 21v-1a7 7 0 0 1 14 0v1"/></svg>
      Profilo
    </button>
  </nav>

</div>

<!-- Sheet (modal) -->

<div class="sheet-backdrop" id="sheet-backdrop"></div>
<div class="sheet" id="sheet">
  <div class="sheet-handle"></div>
  <div class="sheet-header">
    <span class="sheet-title" id="sheet-title">Titolo</span>
    <button class="sheet-close" id="sheet-close" aria-label="Chiudi">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><line x1="6" y1="6" x2="18" y2="18"/><line x1="6" y1="18" x2="18" y2="6"/></svg>
    </button>
  </div>
  <div class="sheet-body" id="sheet-body"></div>
</div>

<!-- Active workout overlay -->

<div class="workout-overlay" id="workout-overlay">
  <div class="workout-topbar">
    <button class="back-btn" id="workout-back" aria-label="Indietro">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><polyline points="15,18 9,12 15,6"/></svg>
    </button>
    <div class="info">
      <div class="name" id="workout-name">Allenamento</div>
      <div class="meta" id="workout-meta">0:00 · 0/0 esercizi</div>
    </div>
    <button class="end-btn" id="workout-end">Fine</button>
  </div>
  <div class="progress-bar"><div class="fill" id="workout-progress" style="width: 0%"></div></div>
  <div class="workout-body" id="workout-body"></div>
</div>

<!-- Photo comparison overlay -->

<div class="compare-overlay" id="compare-overlay">
  <div class="compare-topbar">
    <button id="compare-close" aria-label="Chiudi">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><line x1="6" y1="6" x2="18" y2="18"/><line x1="6" y1="18" x2="18" y2="6"/></svg>
    </button>
    <div class="title">Confronto foto</div>
  </div>
  <div class="compare-body" id="compare-body"></div>
</div>

<!-- Photo viewer -->

<div class="photo-viewer" id="photo-viewer">
  <div class="controls">
    <button id="photo-delete" aria-label="Elimina">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><polyline points="3,6 5,6 21,6"/><path d="M19 6l-2 14H7L5 6"/><line x1="10" y1="11" x2="10" y2="17"/><line x1="14" y1="11" x2="14" y2="17"/></svg>
    </button>
    <button id="photo-close" aria-label="Chiudi">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><line x1="6" y1="6" x2="18" y2="18"/><line x1="6" y1="18" x2="18" y2="6"/></svg>
    </button>
  </div>
  <img id="photo-viewer-img" src="" alt="">
  <div class="photo-meta" id="photo-viewer-meta"></div>
</div>

<!-- Toast -->

<div class="toast" id="toast"></div>

<!-- Hidden file input for photo upload -->

<input type="file" id="photo-input" accept="image/*" capture="environment" style="display:none">

<script>
/* ════════════════════════════════════════════════════════════════════
   GymCoach — Logica applicazione
   Costruita come PWA self-contained con persistenza locale.
   ════════════════════════════════════════════════════════════════════ */

// ── Programma di allenamento di DEFAULT (4 giorni, Upper/Lower x2) ──
// Questo è il SEME del programma. Viene copiato in State.data.userProgram
// alla prima apertura e da quel momento è completamente editabile.
// L'utente può aggiungere, togliere, sostituire, riordinare e modificare ogni esercizio.
const DEFAULT_PROGRAM = {
  // Giorno 1 — Lunedì — Upper A (Petto + Schiena focus)
  day1: {
    id: 'day1',
    name: 'Upper A',
    focus: 'Petto · Schiena · Spalle · Braccia',
    dow: 1, // Lunedì
    duration: 60,
    exercises: [
      { id: 'd1e1', name: 'Panca piana con bilanciere', target: '4 × 6-8', sets: 4, reps: '6-8', muscle: 'Petto', tip: 'Scapole retratte, gomiti a 45°. Movimento controllato.' },
      { id: 'd1e2', name: 'Rematore con bilanciere', target: '4 × 6-8', sets: 4, reps: '6-8', muscle: 'Schiena', tip: 'Busto a 45°, tira verso l\'ombelico. Stringi le scapole.' },
      { id: 'd1e3', name: 'Spinte con manubri panca inclinata 30°', target: '3 × 8-10', sets: 3, reps: '8-10', muscle: 'Petto alto', tip: 'Scendi fino al pari del petto, spingi verso l\'interno.' },
      { id: 'd1e4', name: 'Lat machine presa larga', target: '3 × 8-10', sets: 3, reps: '8-10', muscle: 'Dorsali', tip: 'Tira al petto, non dietro la nuca. Petto in fuori.' },
      { id: 'd1e5', name: 'Alzate laterali con manubri', target: '3 × 12-15', sets: 3, reps: '12-15', muscle: 'Spalle', tip: 'Gomiti leggermente piegati, polsi neutri. Fai parlare il deltoide.' },
      { id: 'd1e6', name: 'Curl bicipiti con bilanciere', target: '3 × 10-12', sets: 3, reps: '10-12', muscle: 'Bicipiti', tip: 'Gomiti fermi al fianco, niente slancio del busto.' },
      { id: 'd1e7', name: 'Pushdown ai cavi (corda)', target: '3 × 10-12', sets: 3, reps: '10-12', muscle: 'Tricipiti', tip: 'Gomiti incollati al busto, separa la corda in fondo.' }
    ]
  },
  // Giorno 2 — Martedì — Lower A (Quadricipiti focus + Core)
  day2: {
    id: 'day2',
    name: 'Lower A',
    focus: 'Quadricipiti · Posteriori · Core',
    dow: 2,
    duration: 60,
    exercises: [
      { id: 'd2e1', name: 'Squat con bilanciere (back squat)', target: '4 × 6-8', sets: 4, reps: '6-8', muscle: 'Quadricipiti', tip: 'Scendi sotto il parallelo, schiena neutra, ginocchia in linea con i piedi.' },
      { id: 'd2e2', name: 'Stacco rumeno (RDL)', target: '3 × 8-10', sets: 3, reps: '8-10', muscle: 'Femorali', tip: 'Hip hinge: spingi il bacino indietro, schiena dritta, sentire stretch sui femorali.' },
      { id: 'd2e3', name: 'Pressa orizzontale 45°', target: '3 × 10-12', sets: 3, reps: '10-12', muscle: 'Quadricipiti', tip: 'Piedi al centro pedana, ROM completo, niente blocco delle ginocchia.' },
      { id: 'd2e4', name: 'Leg curl seduto', target: '3 × 10-12', sets: 3, reps: '10-12', muscle: 'Femorali', tip: 'Contrazione di picco, ritorno controllato in 3 secondi.' },
      { id: 'd2e5', name: 'Calf in piedi alla macchina', target: '4 × 12-15', sets: 4, reps: '12-15', muscle: 'Polpacci', tip: 'ROM completo: stretch in basso, picco in alto. Pausa 1 sec in cima.' },
      { id: 'd2e6', name: 'Plank addominale', target: '3 × 45 sec', sets: 3, reps: '45s', muscle: 'Core', tip: 'Glutei contratti, addome attivo. Linea retta dalla testa ai talloni.' },
      { id: 'd2e7', name: 'Sollevamento gambe sospeso', target: '3 × 12-15', sets: 3, reps: '12-15', muscle: 'Addominali bassi', tip: 'Inclina il bacino, niente dondolio. Movimento solo dell\'addome.' }
    ]
  },
  // Giorno 3 — Giovedì — Upper B (Schiena + Spalle focus)
  day3: {
    id: 'day3',
    name: 'Upper B',
    focus: 'Schiena · Spalle · Petto · Braccia',
    dow: 4,
    duration: 60,
    exercises: [
      { id: 'd3e1', name: 'Trazioni alla sbarra (assistite se serve)', target: '4 × 6-8', sets: 4, reps: '6-8', muscle: 'Dorsali', tip: 'Presa prona larga. Scapole prima, poi braccia. Petto verso la sbarra.' },
      { id: 'd3e2', name: 'Military press in piedi (lento avanti)', target: '4 × 6-8', sets: 4, reps: '6-8', muscle: 'Spalle', tip: 'Glutei contratti, non inarcare la schiena. Spingi sopra la testa, non davanti.' },
      { id: 'd3e3', name: 'Pulley basso al cavo (presa stretta)', target: '3 × 8-10', sets: 3, reps: '8-10', muscle: 'Schiena media', tip: 'Stringi le scapole in fondo, gomiti vicini al corpo.' },
      { id: 'd3e4', name: 'Spinte con manubri panca piana', target: '3 × 8-10', sets: 3, reps: '8-10', muscle: 'Petto', tip: 'Polsi neutri, ROM ampio. Stretch profondo in basso.' },
      { id: 'd3e5', name: 'Face pulls al cavo', target: '3 × 12-15', sets: 3, reps: '12-15', muscle: 'Spalle posteriori', tip: 'Tira verso la fronte, gomiti alti. Postura — ottimo per le spalle.' },
      { id: 'd3e6', name: 'Curl a martello con manubri', target: '3 × 10-12', sets: 3, reps: '10-12', muscle: 'Bicipiti / Brachiale', tip: 'Polsi neutri, alterna se preferisci. Costruisce spessore al braccio.' },
      { id: 'd3e7', name: 'French press con manubri (overhead)', target: '3 × 10-12', sets: 3, reps: '10-12', muscle: 'Tricipiti capo lungo', tip: 'Gomiti fermi e vicini, scendi dietro la nuca. Stretch attivo.' }
    ]
  },
  // Giorno 4 — Venerdì — Lower B (Catena posteriore + Core)
  day4: {
    id: 'day4',
    name: 'Lower B',
    focus: 'Glutei · Femorali · Quadricipiti · Core',
    dow: 5,
    duration: 60,
    exercises: [
      { id: 'd4e1', name: 'Stacco da terra convenzionale', target: '4 × 5-6', sets: 4, reps: '5-6', muscle: 'Catena posteriore', tip: 'Bilanciere vicino alle tibie. Schiena neutra, spingi il pavimento via dai piedi.' },
      { id: 'd4e2', name: 'Front squat con bilanciere', target: '3 × 8-10', sets: 3, reps: '8-10', muscle: 'Quadricipiti', tip: 'Gomiti alti, busto verticale. Carico minore del back squat — fai il movimento.' },
      { id: 'd4e3', name: 'Hip thrust con bilanciere', target: '3 × 10-12', sets: 3, reps: '10-12', muscle: 'Glutei', tip: 'Mento verso il petto, contrai i glutei in cima e tieni 1 secondo.' },
      { id: 'd4e4', name: 'Affondi camminando con manubri', target: '3 × 12 (per gamba)', sets: 3, reps: '12/lato', muscle: 'Quadricipiti / Glutei', tip: 'Passi lunghi, ginocchio posteriore quasi a terra. Busto eretto.' },
      { id: 'd4e5', name: 'Calf seduto alla macchina', target: '4 × 15-20', sets: 4, reps: '15-20', muscle: 'Polpacci (soleo)', tip: 'Range completo, niente molleggio. Brucia di brutto — è normale.' },
      { id: 'd4e6', name: 'Cable woodchopper (alto-basso)', target: '3 × 12 (per lato)', sets: 3, reps: '12/lato', muscle: 'Obliqui / Core', tip: 'Ruota dal busto, non solo dalle braccia. Movimento deciso.' },
      { id: 'd4e7', name: 'Russian twist con disco', target: '3 × 20 (10 per lato)', sets: 3, reps: '20', muscle: 'Obliqui', tip: 'Piedi sollevati per maggiore intensità, busto inclinato indietro a 45°.' }
    ]
  }
};

const DAY_ORDER = ['day1','day2','day3','day4'];
const DOW_NAMES = ['Domenica','Lunedì','Martedì','Mercoledì','Giovedì','Venerdì','Sabato'];
const DOW_SHORT = ['Dom','Lun','Mar','Mer','Gio','Ven','Sab'];
const DOW_INITIAL = ['D','L','M','M','G','V','S'];
const MONTHS_SHORT = ['Gen','Feb','Mar','Apr','Mag','Giu','Lug','Ago','Set','Ott','Nov','Dic'];
const MONTHS = ['Gennaio','Febbraio','Marzo','Aprile','Maggio','Giugno','Luglio','Agosto','Settembre','Ottobre','Novembre','Dicembre'];

// ── Library di esercizi (per sostituzioni e aggiunte) ───────────────
// Catalogo organizzato per gruppo muscolare. Usato per: swap durante
// sessione, aggiunta esercizi extra, suggerimenti per esercizi custom.
const EXERCISE_LIBRARY = {
  Petto: [
    { name: 'Panca piana con bilanciere', sets: 4, reps: '6-8' },
    { name: 'Panca piana con manubri', sets: 4, reps: '8-10' },
    { name: 'Panca inclinata con bilanciere', sets: 4, reps: '6-8' },
    { name: 'Spinte inclinate con manubri', sets: 3, reps: '8-10' },
    { name: 'Panca declinata con manubri', sets: 3, reps: '8-10' },
    { name: 'Croci ai cavi alti', sets: 3, reps: '12-15' },
    { name: 'Croci ai cavi bassi', sets: 3, reps: '12-15' },
    { name: 'Pectoral machine (peck deck)', sets: 3, reps: '12-15' },
    { name: 'Piegamenti sulle braccia', sets: 3, reps: 'max' },
    { name: 'Dip alle parallele', sets: 3, reps: '6-10' },
    { name: 'Spinte con manubri presa neutra', sets: 3, reps: '8-10' }
  ],
  Schiena: [
    { name: 'Trazioni alla sbarra', sets: 4, reps: '6-8' },
    { name: 'Trazioni assistite alla macchina', sets: 4, reps: '8-10' },
    { name: 'Lat machine presa larga', sets: 3, reps: '8-10' },
    { name: 'Lat machine presa stretta', sets: 3, reps: '10-12' },
    { name: 'Rematore con bilanciere', sets: 4, reps: '6-8' },
    { name: 'Rematore con manubrio', sets: 3, reps: '8-10' },
    { name: 'Rematore Pendlay', sets: 4, reps: '5-6' },
    { name: 'T-bar row', sets: 3, reps: '8-10' },
    { name: 'Pulley basso al cavo', sets: 3, reps: '10-12' },
    { name: 'Hyperextensions (lombari)', sets: 3, reps: '12-15' },
    { name: 'Pull-over con manubrio', sets: 3, reps: '10-12' }
  ],
  Spalle: [
    { name: 'Military press in piedi', sets: 4, reps: '6-8' },
    { name: 'Lento avanti seduto con manubri', sets: 4, reps: '8-10' },
    { name: 'Arnold press', sets: 3, reps: '8-10' },
    { name: 'Spinte alla Smith machine', sets: 3, reps: '8-10' },
    { name: 'Alzate laterali con manubri', sets: 4, reps: '12-15' },
    { name: 'Alzate laterali al cavo', sets: 3, reps: '12-15' },
    { name: 'Alzate frontali con manubri', sets: 3, reps: '10-12' },
    { name: 'Alzate posteriori (rear delt fly)', sets: 3, reps: '12-15' },
    { name: 'Face pulls al cavo', sets: 3, reps: '12-15' },
    { name: 'Upright row', sets: 3, reps: '10-12' }
  ],
  Bicipiti: [
    { name: 'Curl con bilanciere', sets: 3, reps: '8-10' },
    { name: 'Curl con manubri (alternato)', sets: 3, reps: '10-12' },
    { name: 'Curl a martello', sets: 3, reps: '10-12' },
    { name: 'Curl panca Scott', sets: 3, reps: '10-12' },
    { name: 'Curl al cavo basso', sets: 3, reps: '12-15' },
    { name: 'Curl 21', sets: 3, reps: '21' },
    { name: 'Curl con bilanciere EZ', sets: 3, reps: '10-12' },
    { name: 'Spider curl', sets: 3, reps: '10-12' }
  ],
  Tricipiti: [
    { name: 'Pushdown ai cavi (corda)', sets: 3, reps: '10-12' },
    { name: 'Pushdown ai cavi (V-bar)', sets: 3, reps: '10-12' },
    { name: 'French press con bilanciere', sets: 3, reps: '8-10' },
    { name: 'French press con manubri (overhead)', sets: 3, reps: '10-12' },
    { name: 'Estensioni dietro la nuca al cavo', sets: 3, reps: '10-12' },
    { name: 'Dip alla panca', sets: 3, reps: '10-15' },
    { name: 'Panca presa stretta', sets: 3, reps: '6-8' },
    { name: 'Kickback con manubrio', sets: 3, reps: '12-15' }
  ],
  Quadricipiti: [
    { name: 'Squat con bilanciere (back squat)', sets: 4, reps: '6-8' },
    { name: 'Front squat', sets: 3, reps: '8-10' },
    { name: 'Squat alla Smith machine', sets: 3, reps: '8-10' },
    { name: 'Bulgarian split squat', sets: 3, reps: '10/lato' },
    { name: 'Pressa orizzontale 45°', sets: 3, reps: '10-12' },
    { name: 'Hack squat', sets: 3, reps: '8-10' },
    { name: 'Leg extension', sets: 3, reps: '12-15' },
    { name: 'Goblet squat', sets: 3, reps: '10-12' },
    { name: 'Affondi camminando', sets: 3, reps: '12/lato' },
    { name: 'Step-up con manubri', sets: 3, reps: '10/lato' }
  ],
  Femorali: [
    { name: 'Stacco rumeno (RDL) con bilanciere', sets: 3, reps: '8-10' },
    { name: 'Stacco rumeno con manubri', sets: 3, reps: '10-12' },
    { name: 'Leg curl seduto', sets: 3, reps: '10-12' },
    { name: 'Leg curl in piedi', sets: 3, reps: '10-12' },
    { name: 'Leg curl prono', sets: 3, reps: '10-12' },
    { name: 'Stacco gambe tese', sets: 3, reps: '8-10' },
    { name: 'Glute-ham raise', sets: 3, reps: '8-10' },
    { name: 'Nordic curl', sets: 3, reps: '6-8' }
  ],
  Glutei: [
    { name: 'Hip thrust con bilanciere', sets: 3, reps: '10-12' },
    { name: 'Glute bridge', sets: 3, reps: '12-15' },
    { name: 'Hip thrust ad una gamba', sets: 3, reps: '10/lato' },
    { name: 'Cable kickback', sets: 3, reps: '12-15/lato' },
    { name: 'Step-up alti', sets: 3, reps: '10/lato' },
    { name: 'Sumo squat con kettlebell', sets: 3, reps: '12-15' },
    { name: 'Stacco sumo', sets: 3, reps: '6-8' }
  ],
  Polpacci: [
    { name: 'Calf in piedi alla macchina', sets: 4, reps: '12-15' },
    { name: 'Calf seduto alla macchina', sets: 4, reps: '15-20' },
    { name: 'Calf alla pressa', sets: 4, reps: '12-15' },
    { name: 'Calf su gradino con manubrio', sets: 4, reps: '15-20' },
    { name: 'Donkey calf raise', sets: 4, reps: '12-15' }
  ],
  Core: [
    { name: 'Plank', sets: 3, reps: '45s' },
    { name: 'Plank laterale', sets: 3, reps: '30s/lato' },
    { name: 'Sollevamento gambe sospeso', sets: 3, reps: '12-15' },
    { name: 'Crunch al cavo (preghiera)', sets: 3, reps: '12-15' },
    { name: 'Russian twist', sets: 3, reps: '20' },
    { name: 'Cable woodchopper', sets: 3, reps: '12/lato' },
    { name: 'Hollow hold', sets: 3, reps: '30s' },
    { name: 'Dead bug', sets: 3, reps: '10/lato' },
    { name: 'Crunch addominale', sets: 3, reps: '15-20' },
    { name: 'Mountain climbers', sets: 3, reps: '20/lato' },
    { name: 'Ab wheel rollout', sets: 3, reps: '8-12' }
  ]
};

// ── Routine di riscaldamento ────────────────────────────────────────
// Diversificate per Upper / Lower. 5-7 minuti totali, mobilità + attivazione.
const WARMUP_ROUTINES = {
  upper: {
    name: 'Riscaldamento Upper',
    duration: '5-7 min',
    items: [
      { name: 'Cardio leggero', detail: '3-5 min · cyclette, vogatore o corda', timer: 240 },
      { name: 'Cat-camel', detail: '10 ripetizioni · mobilità colonna', timer: 30 },
      { name: 'Rotazione toracica quadrupedia', detail: '8 per lato', timer: 45 },
      { name: 'Wall slides', detail: '10 ripetizioni · scapole + spalle', timer: 30 },
      { name: 'Band pull-aparts', detail: '15 ripetizioni · attivazione dorsali', timer: 30 },
      { name: 'Scapular push-ups', detail: '10 ripetizioni · attivazione serrato', timer: 30 },
      { name: 'Serie di approccio', detail: 'Bilanciere vuoto × 10-12 sul primo composto', timer: null }
    ]
  },
  lower: {
    name: 'Riscaldamento Lower',
    duration: '6-8 min',
    items: [
      { name: 'Cardio leggero', detail: '3-5 min · cyclette o tapis roulant', timer: 240 },
      { name: 'Hip circles', detail: '10 per direzione, ogni gamba', timer: 45 },
      { name: 'Squat a corpo libero', detail: '15 ripetizioni · ROM completo', timer: 30 },
      { name: 'Glute bridge', detail: '12 ripetizioni · attivazione glutei', timer: 30 },
      { name: 'Affondi camminando', detail: '8 per gamba · senza peso', timer: 60 },
      { name: 'Cossack squat', detail: '6 per lato · mobilità anche', timer: 45 },
      { name: 'Serie di approccio', detail: 'Bilanciere vuoto × 10 sul primo composto', timer: null }
    ]
  }
};

function getWarmupForWorkout(workoutId) {
  const w = PROGRAM[workoutId];
  if (!w) return WARMUP_ROUTINES.upper;
  const isLower = w.name.toLowerCase().includes('lower');
  return isLower ? WARMUP_ROUTINES.lower : WARMUP_ROUTINES.upper;
}


function mapMuscleToCategory(muscle) {
  const m = (muscle || '').toLowerCase();
  if (m.includes('petto')) return 'Petto';
  if (m.includes('dorsali') || m.includes('schiena')) return 'Schiena';
  if (m.includes('spalle')) return 'Spalle';
  if (m.includes('bicipit') || m.includes('brachiale')) return 'Bicipiti';
  if (m.includes('tricipit')) return 'Tricipiti';
  if (m.includes('quadricipit')) return 'Quadricipiti';
  if (m.includes('femoral') || m.includes('catena')) return 'Femorali';
  if (m.includes('glute')) return 'Glutei';
  if (m.includes('polpac')) return 'Polpacci';
  if (m.includes('core') || m.includes('addominal') || m.includes('obliqu')) return 'Core';
  return null;
}

// ── Algoritmo sovraccarico progressivo ──────────────────────────────
// Logica: se hai chiuso TUTTE le serie al massimo del range reps,
// prossima volta +2.5kg. Se hai fallito spesso sotto il minimo, -2.5kg.
// Altrimenti mantieni il peso e prova ad aggiungere reps.
function suggestNextWeight(exId, exDef) {
  const log = State.data.exerciseLog[exId];
  if (!log || !log.length || !exDef) return null;
  const last = log[log.length - 1];
  if (!last.sets || !last.sets.length) return null;

  const m = (exDef.reps || '').match(/(\d+)\s*-\s*(\d+)/);
  if (!m) return null;
  const minR = parseInt(m[1]), maxR = parseInt(m[2]);

  const lastWeight = last.topSet || Math.max(0, ...last.sets.map(s => s.weight || 0));
  if (!lastWeight) return null;

  const targetSets = exDef.sets || last.sets.length;
  const setsAtTopWeight = last.sets.filter(s => Math.abs((s.weight||0) - lastWeight) < 0.5);
  const allAtMaxReps = setsAtTopWeight.length >= targetSets && setsAtTopWeight.every(s => (s.reps||0) >= maxR);
  const failures = last.sets.filter(s => (s.reps||0) > 0 && (s.reps||0) < minR).length;

  if (allAtMaxReps) {
    return { weight: +(lastWeight + 2.5).toFixed(1), action: 'up', label: '+2.5kg' };
  }
  if (failures >= Math.ceil(targetSets / 2)) {
    return { weight: Math.max(0, +(lastWeight - 2.5).toFixed(1)), action: 'down', label: 'scarica -2.5kg' };
  }
  return { weight: lastWeight, action: 'hold', label: 'mantieni' };
}

// Tip dell'allenatore (rotazione casuale)
const COACH_TIPS = [
  "Il sovraccarico progressivo è la regola d'oro. Aggiungi 1-2.5kg appena completi tutte le serie nel range alto delle ripetizioni.",
  "Il sonno è il tuo terzo allenamento. Punta a 7-8 ore — è quando i muscoli crescono davvero.",
  "Mangia proteine: 1.6-2.0g per kg di peso corporeo. A 80kg parliamo di 130-160g al giorno.",
  "Il riscaldamento non è facoltativo. 5 minuti di cardio + 1-2 serie leggere prima di ogni esercizio composto.",
  "Tecnica prima del peso. Una ripetizione perfetta vale più di tre fatte male.",
  "L'addominale si scolpisce in cucina, non in palestra. Deficit calorico 300-500 kcal per perdere grasso.",
  "Il riposo tra le serie conta: 90-120 sec sui composti, 60-90 sec sugli isolamenti.",
  "Bevi acqua. 35-40 ml per kg di peso corporeo, di più nei giorni di allenamento.",
  "I cosiddetti 'plateau' sono spesso problemi di recupero, non di stimolo. Considera una settimana di scarico ogni 6-8 settimane.",
  "Cardio leggero (20-30 min) nei giorni off aiuta il recupero e il deficit calorico, senza compromettere la massa.",
  "Tieni un quaderno (o questa app). Quello che non si misura, non si migliora.",
  "Le caviglie e i polsi flessibili migliorano squat e panca. 5 minuti di mobilità prima di iniziare."
];

// ── Storage layer ────────────────────────────────────────────────────
const KEY = 'gymcoach.v1';
const State = {
  data: null,
  load() {
    try {
      const raw = localStorage.getItem(KEY);
      if (raw) this.data = JSON.parse(raw);
      else this.data = this.defaults();
    } catch(e) { console.error(e); this.data = this.defaults(); }
    // Migration: ensure required fields exist
    if (!this.data.profile) this.data.profile = this.defaults().profile;
    if (!this.data.weightLog) this.data.weightLog = [];
    if (!this.data.workoutHistory) this.data.workoutHistory = [];
    if (!this.data.exerciseLog) this.data.exerciseLog = {};
    if (!this.data.startDate) this.data.startDate = new Date().toISOString();
    if (!this.data.measurements) this.data.measurements = [];
    if (!this.data.customExercises) this.data.customExercises = [];
    if (!this.data.workoutNotes) this.data.workoutNotes = {};
    if (!this.data.cardioLog) this.data.cardioLog = [];
    if (!this.data.proteinLog) this.data.proteinLog = {};
    if (!this.data.userProgram) {
      // First-time migration: deep clone the default program into editable state
      this.data.userProgram = JSON.parse(JSON.stringify(DEFAULT_PROGRAM));
    }
    if (this.data.profile && !this.data.profile.proteinTarget) {
      // Default 1.8g/kg of bodyweight
      this.data.profile.proteinTarget = Math.round((this.data.profile.startWeight||80) * 1.8);
    }
    if (this.data.profile && this.data.profile.warmupEnabled === undefined) {
      this.data.profile.warmupEnabled = true;
    }
  },
  save() { localStorage.setItem(KEY, JSON.stringify(this.data)); },
  defaults() {
    return {
      profile: {
        name: 'Cristian',
        startWeight: 80,
        currentWeight: 80,
        height: 180,
        age: null,
        goal: 'Ricomposizione corporea: ridurre grasso addominale e costruire massa magra.',
        proteinTarget: 144,
        warmupEnabled: true
      },
      weightLog: [{ date: new Date().toISOString(), value: 80 }],
      workoutHistory: [],
      exerciseLog: {}, // exerciseId → [{date, sets:[{weight,reps}], topSet}]
      startDate: new Date().toISOString(),
      measurements: [],
      customExercises: [],
      workoutNotes: {},
      cardioLog: [],
      proteinLog: {}
    };
  }
};

// ── PROGRAM è ora un Proxy live che riflette State.data.userProgram ──
// Tutte le letture (PROGRAM[id], Object.values(PROGRAM), ecc.) si comportano
// come prima ma puntano ai dati editabili dell'utente, non al default.
const PROGRAM = new Proxy({}, {
  get(target, prop) {
    if (!State.data || !State.data.userProgram) return undefined;
    return State.data.userProgram[prop];
  },
  has(target, prop) {
    if (!State.data || !State.data.userProgram) return false;
    return prop in State.data.userProgram;
  },
  ownKeys(target) {
    if (!State.data || !State.data.userProgram) return [];
    return Reflect.ownKeys(State.data.userProgram);
  },
  getOwnPropertyDescriptor(target, prop) {
    if (!State.data || !State.data.userProgram) return undefined;
    return Object.getOwnPropertyDescriptor(State.data.userProgram, prop);
  }
});

// ── IndexedDB for photos ────────────────────────────────────────────
const PhotoDB = {
  db: null,
  init() {
    return new Promise((resolve) => {
      const req = indexedDB.open('gymcoach-photos', 1);
      req.onupgradeneeded = (e) => {
        const db = e.target.result;
        if (!db.objectStoreNames.contains('photos')) {
          const store = db.createObjectStore('photos', { keyPath: 'id', autoIncrement: true });
          store.createIndex('pose', 'pose', { unique: false });
          store.createIndex('date', 'date', { unique: false });
        }
      };
      req.onsuccess = (e) => { this.db = e.target.result; resolve(); };
      req.onerror = (e) => { console.error('IDB error', e); resolve(); };
    });
  },
  add(photo) {
    return new Promise((resolve, reject) => {
      if (!this.db) return resolve(null);
      const tx = this.db.transaction('photos', 'readwrite');
      const req = tx.objectStore('photos').add(photo);
      req.onsuccess = () => resolve(req.result);
      req.onerror = () => reject(req.error);
    });
  },
  getAll() {
    return new Promise((resolve) => {
      if (!this.db) return resolve([]);
      const req = this.db.transaction('photos').objectStore('photos').getAll();
      req.onsuccess = () => resolve(req.result || []);
      req.onerror = () => resolve([]);
    });
  },
  delete(id) {
    return new Promise((resolve) => {
      if (!this.db) return resolve();
      const tx = this.db.transaction('photos', 'readwrite');
      tx.objectStore('photos').delete(id);
      tx.oncomplete = () => resolve();
    });
  }
};

// ── Helpers ─────────────────────────────────────────────────────────
const $ = (sel) => document.querySelector(sel);
const $$ = (sel) => document.querySelectorAll(sel);
const fmtDate = (d) => {
  const dt = (d instanceof Date) ? d : new Date(d);
  return `${dt.getDate()} ${MONTHS_SHORT[dt.getMonth()]}`;
};
const fmtDateLong = (d) => {
  const dt = (d instanceof Date) ? d : new Date(d);
  return `${DOW_NAMES[dt.getDay()]} ${dt.getDate()} ${MONTHS_SHORT[dt.getMonth()]} ${dt.getFullYear()}`;
};
const fmtTime = (sec) => {
  const m = Math.floor(sec/60), s = sec%60;
  return `${m}:${String(s).padStart(2,'0')}`;
};
const greeting = () => {
  const h = new Date().getHours();
  if (h < 6) return 'Buonanotte'; if (h < 12) return 'Buongiorno';
  if (h < 18) return 'Buon pomeriggio'; return 'Buonasera';
};
const todayKey = () => new Date().toISOString().slice(0,10);
const sameDay = (a,b) => new Date(a).toISOString().slice(0,10) === new Date(b).toISOString().slice(0,10);

// Get today's planned workout (matches PROGRAM by day-of-week)
function getTodaysWorkout() {
  const dow = new Date().getDay();
  return Object.values(PROGRAM).find(p => p.dow === dow) || null;
}
function getNextWorkout() {
  const now = new Date();
  for (let offset = 0; offset < 8; offset++) {
    const d = new Date(now); d.setDate(now.getDate()+offset);
    const w = Object.values(PROGRAM).find(p => p.dow === d.getDay());
    if (w) return { workout: w, date: d, offset };
  }
  return null;
}

// Last weight used for an exercise (top set)
function getLastWeight(exId) {
  const log = State.data.exerciseLog[exId];
  if (!log || !log.length) return null;
  const last = log[log.length-1];
  return last.topSet || null;
}

// Best ever weight (PR)
function getBestWeight(exId) {
  const log = State.data.exerciseLog[exId];
  if (!log || !log.length) return null;
  let best = 0;
  log.forEach(e => { if ((e.topSet||0) > best) best = e.topSet; });
  return best;
}

// Toast
let toastTimer;
function toast(msg) {
  const t = $('#toast'); t.textContent = msg; t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2200);
}

// ── Sheet (modal) ───────────────────────────────────────────────────
function openSheet(title, html, onOpen) {
  $('#sheet-title').textContent = title;
  $('#sheet-body').innerHTML = html;
  $('#sheet-backdrop').classList.add('open');
  $('#sheet').classList.add('open');
  if (onOpen) setTimeout(onOpen, 50);
}
function closeSheet() {
  $('#sheet-backdrop').classList.remove('open');
  $('#sheet').classList.remove('open');
}
$('#sheet-close').onclick = closeSheet;
$('#sheet-backdrop').onclick = closeSheet;

// ── Tab switching ───────────────────────────────────────────────────
function switchTab(name) {
  $$('.tab').forEach(t => t.classList.toggle('active', t.dataset.tab === name));
  $$('.view').forEach(v => v.classList.toggle('active', v.dataset.view === name));
  renderView(name);
  // Update topbar
  const titles = { today: 'Oggi', program: 'Programma', progress: 'Progressi', profile: 'Profilo' };
  $('#topbar-title').textContent = titles[name];
  $('#greeting').textContent = name === 'today' ? `${greeting()}, ${State.data.profile.name || ''}` : '';
  $('#topbar-action').style.display = (name === 'progress' || name === 'today') ? 'flex' : 'none';
}
$$('.tab').forEach(btn => btn.onclick = () => switchTab(btn.dataset.tab));

// Topbar action: context-dependent
$('#topbar-action').onclick = () => {
  const active = $('.view.active').dataset.view;
  if (active === 'progress') openWeightSheet();
  else if (active === 'today') openInfoSheet();
};


// ── View renderers ──────────────────────────────────────────────────
function renderView(name) {
  if (name === 'today') renderToday();
  if (name === 'program') renderProgram();
  if (name === 'progress') renderProgress();
  if (name === 'profile') renderProfile();
}

// ── Today view ──────────────────────────────────────────────────────
function renderToday() {
  const today = getTodaysWorkout();
  const next = getNextWorkout();
  const completedToday = State.data.workoutHistory.find(w => sameDay(w.date, new Date()));
  const totalWorkouts = State.data.workoutHistory.length;
  const startDate = new Date(State.data.startDate);
  const weeksTraining = Math.max(1, Math.floor((Date.now() - startDate.getTime()) / (7*24*60*60*1000)) + 1);
  const currentWeight = State.data.profile.currentWeight;
  const startWeight = State.data.profile.startWeight;
  const weightDelta = currentWeight - startWeight;
  const streak = computeStreak();

  // Tip: stable per day
  const tipIdx = (new Date().getDate() + new Date().getMonth()) % COACH_TIPS.length;
  const tip = COACH_TIPS[tipIdx];

  let heroHtml = '';
  if (completedToday) {
    heroHtml = `
      <div class="hero-card">
        <div class="hero-eyebrow">Sessione completata</div>
        <div class="hero-title">Lavoro fatto, ${State.data.profile.name||''} ✓</div>
        <div class="hero-subtitle">Hai completato ${completedToday.workoutName}. Recupero e idratazione adesso.</div>
        <div class="hero-meta">
          <div class="hero-meta-item"><span class="lbl">Durata</span><span class="val">${fmtTime(completedToday.duration||0)}</span></div>
          <div class="hero-meta-item"><span class="lbl">Esercizi</span><span class="val">${completedToday.exercisesCompleted}/${completedToday.exercisesTotal}</span></div>
          <div class="hero-meta-item"><span class="lbl">Volume</span><span class="val">${(completedToday.totalVolume||0).toFixed(0)} kg</span></div>
        </div>
        <button class="btn secondary" onclick="openWorkoutDetailSheet('${completedToday.workoutId}')">Rivedi sessione</button>
      </div>`;
  } else if (today) {
    const lastSession = State.data.workoutHistory.filter(w => w.workoutId === today.id).slice(-1)[0];
    const lastDate = lastSession ? fmtDate(lastSession.date) : 'mai fatto';
    heroHtml = `
      <div class="hero-card">
        <div class="hero-eyebrow">Allenamento di oggi</div>
        <div class="hero-title">${today.name}</div>
        <div class="hero-subtitle">${today.focus}</div>
        <div class="hero-meta">
          <div class="hero-meta-item"><span class="lbl">Esercizi</span><span class="val">${today.exercises.length}</span></div>
          <div class="hero-meta-item"><span class="lbl">Durata</span><span class="val">~${today.duration} min</span></div>
          <div class="hero-meta-item"><span class="lbl">Ultima volta</span><span class="val">${lastDate}</span></div>
        </div>
        <button class="btn" onclick="startWorkout('${today.id}')">Inizia allenamento</button>
      </div>`;
  } else {
    // Rest day
    const nm = next ? `${next.workout.name}` : 'prossimo allenamento';
    const offset = next?.offset || 0;
    const when = offset === 0 ? 'oggi' : offset === 1 ? 'domani' : `in ${offset} giorni (${DOW_SHORT[next.date.getDay()]})`;
    heroHtml = `
      <div class="hero-card">
        <div class="hero-eyebrow">Giorno di recupero</div>
        <div class="hero-title">Recupero attivo 🌿</div>
        <div class="hero-subtitle">Nessun allenamento programmato. Mobilità leggera, camminata 30-40 min, idratazione.</div>
        <div class="hero-meta">
          <div class="hero-meta-item"><span class="lbl">Prossima sessione</span><span class="val">${nm} · ${when}</span></div>
        </div>
        <button class="btn secondary" onclick="switchTab('program')">Vedi programma</button>
      </div>`;
  }

  const html = `
    <div class="section" style="padding-top:16px">
      ${heroHtml}

      ${renderWeekStrip()}

      ${renderDailyGrid()}

      <div class="stats-grid">
        <div class="stat-card">
          <div class="ic" style="background:rgba(48,209,88,0.15);color:var(--accent)">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><circle cx="12" cy="12" r="9"/><polyline points="12,7 12,12 16,14"/></svg>
          </div>
          <div class="lbl">Settimane attivo</div>
          <div class="val">${weeksTraining}</div>
        </div>
        <div class="stat-card">
          <div class="ic" style="background:rgba(255,159,10,0.15);color:var(--warning)">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2c-1 4-4 5-4 9a4 4 0 0 0 8 0c0-3-2-4-2-7-1 2-2 3-2 5"/></svg>
          </div>
          <div class="lbl">Streak</div>
          <div class="val">${streak} giorni</div>
        </div>
        <div class="stat-card">
          <div class="ic" style="background:rgba(10,132,255,0.15);color:var(--info)">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><rect x="6" y="10" width="3" height="4" rx="1"/><rect x="15" y="10" width="3" height="4" rx="1"/><line x1="9" y1="12" x2="15" y2="12"/></svg>
          </div>
          <div class="lbl">Sessioni totali</div>
          <div class="val">${totalWorkouts}</div>
        </div>
        <div class="stat-card">
          <div class="ic" style="background:rgba(255,69,58,0.15);color:var(--danger)">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><polyline points="3,17 9,11 13,15 21,7"/></svg>
          </div>
          <div class="lbl">Peso attuale</div>
          <div class="val">${currentWeight} kg</div>
          <div class="delta ${weightDelta>0?'up':weightDelta<0?'down':''}">${weightDelta===0?'Stabile':(weightDelta>0?'+':'')+weightDelta.toFixed(1)+' kg'}</div>
        </div>
      </div>

      <div class="section-title">Coach del giorno</div>
      <div class="coach-card">
        <div class="who">
          <div class="avatar">C</div>
          <div>
            <div class="role">Coach Cesare</div>
            <div class="role-sub">50 anni di palestra alle spalle</div>
          </div>
        </div>
        <div class="quote">"${tip}"</div>
      </div>

      ${renderRecentHistoryList()}
    </div>
  `;
  $('#view-today').innerHTML = html;
}

function computeStreak() {
  // Number of consecutive scheduled workouts completed (streak of weeks where all 4 done)
  // Simpler: total distinct workout days in last 14 days
  const now = Date.now();
  const set = new Set();
  State.data.workoutHistory.forEach(w => {
    if (now - new Date(w.date).getTime() < 21*24*60*60*1000) set.add(new Date(w.date).toISOString().slice(0,10));
  });
  return set.size;
}

function renderRecentHistoryList() {
  const recent = State.data.workoutHistory.slice(-3).reverse();
  if (!recent.length) return '';
  return `
    <div class="section-title">Ultime sessioni</div>
    <div class="list">
      ${recent.map(w => {
        const d = new Date(w.date);
        return `
        <button class="row" onclick="openWorkoutDetailSheet('${w.workoutId}', '${w.date}')">
          <div class="day-pill" style="border-radius:10px;width:44px;height:44px;background:var(--surface-2);display:flex;flex-direction:column;align-items:center;justify-content:center;">
            <div style="font-size:16px;font-weight:700;line-height:1">${d.getDate()}</div>
            <div style="font-size:9px;color:var(--text-secondary);margin-top:2px;text-transform:uppercase;letter-spacing:0.5px">${MONTHS_SHORT[d.getMonth()]}</div>
          </div>
          <div class="row-main">
            <div class="row-title">${w.workoutName}</div>
            <div class="row-sub">${fmtTime(w.duration||0)} · ${w.exercisesCompleted}/${w.exercisesTotal} esercizi · ${(w.totalVolume||0).toFixed(0)} kg vol.</div>
          </div>
          <span class="chev">›</span>
        </button>`;
      }).join('')}
    </div>
  `;
}

// Info sheet from topbar action on Today
function openInfoSheet() {
  const html = `
    <p class="text-secondary" style="font-size:14px;line-height:1.5">
      <strong style="color:var(--text)">GymCoach</strong> è il tuo personal trainer in tasca. Programma 4 giorni, ottimizzato per ipertrofia e ricomposizione.
    </p>
    <div class="spacer"></div>
    <p class="text-secondary" style="font-size:14px;line-height:1.5">
      <strong style="color:var(--text)">Filosofia di allenamento:</strong> ogni gruppo muscolare allenato 2 volte a settimana per stimolo ottimale, sessioni da ~60 minuti, sovraccarico progressivo come bussola.
    </p>
    <div class="spacer"></div>
    <p class="text-secondary" style="font-size:14px;line-height:1.5">
      I tuoi dati restano sul telefono. Niente cloud, niente account. Per esportare/cancellare tutto vai in Profilo → Dati.
    </p>
  `;
  openSheet('Informazioni', html);
}

// ── Program view ────────────────────────────────────────────────────
function renderProgram() {
  const todayDow = new Date().getDay();
  const days = DAY_ORDER.map(id => PROGRAM[id]);
  const restDays = [0,3,6]; // Domenica, Mercoledì, Sabato
  const allDays = [];
  // Show all 7 days of week starting from Monday
  for (let i=1; i<=6; i++) allDays.push(i);
  allDays.push(0);

  const html = `
    <div class="section" style="padding-top:16px">
      <div class="section-title" style="margin-top:0">Il tuo split</div>
      <div style="background:var(--surface-1);border-radius:14px;padding:14px 16px;border:0.5px solid var(--border)">
        <div style="font-size:15px;font-weight:600;margin-bottom:4px">Upper / Lower × 2</div>
        <div style="font-size:13px;color:var(--text-secondary);line-height:1.5">
          4 sessioni a settimana, ogni gruppo muscolare colpito 2 volte. Ottimale per costruire massa magra e ridurre grasso corporeo. Riposo tra serie: 90-120s sui composti, 60-90s sugli isolamenti.
        </div>
      </div>

      <div class="section-title">Settimana</div>
      <div style="display:flex;flex-direction:column;gap:10px">
        ${allDays.map(dow => {
          const w = days.find(d => d.dow === dow);
          if (!w) {
            return `<div class="rest-day">
              <div style="font-size:11px;color:var(--text-tertiary);font-weight:600;letter-spacing:0.6px;text-transform:uppercase;margin-bottom:4px">${DOW_NAMES[dow]}</div>
              Recupero · mobilità o cardio leggero
            </div>`;
          }
          const isToday = dow === todayDow;
          return `
            <button class="day-card ${isToday?'today':''}" onclick="openWorkoutSheet('${w.id}')" style="background:${isToday?'linear-gradient(135deg, rgba(48,209,88,0.08), rgba(48,209,88,0.02))':'var(--surface-1)'};text-align:left;width:100%;border:0.5px solid ${isToday?'var(--accent)':'var(--border)'};color:var(--text);font-family:inherit;cursor:pointer">
              <div class="dow">${DOW_NAMES[dow]}</div>
              <div class="name">${w.name}</div>
              <div class="focus">${w.focus}</div>
              <div class="meta">
                <div class="meta-item">
                  <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="9"/><polyline points="12,7 12,12 16,14"/></svg>
                  ~${w.duration} min
                </div>
                <div class="meta-item">
                  <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="6" width="18" height="13" rx="2"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
                  ${w.exercises.length} esercizi
                </div>
              </div>
            </button>`;
        }).join('')}
      </div>

      <div class="section-title">Personalizzazione programma</div>
      <div class="list">
        <button class="row" onclick="openCustomExercisesSheet()">
          <div class="row-icon" style="background:rgba(48,209,88,0.15);color:var(--accent)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="12" cy="12" r="9"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">I miei esercizi (libreria)</div>
            <div class="row-sub">${State.data.customExercises.length} esercizi personalizzati</div>
          </div>
          <span class="chev">›</span>
        </button>
        <button class="row" onclick="openProgramOverviewSheet()">
          <div class="row-icon" style="background:rgba(10,132,255,0.15);color:var(--info)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">Modifica programma settimanale</div>
            <div class="row-sub">Aggiungi, sposta, sostituisci esercizi</div>
          </div>
          <span class="chev">›</span>
        </button>
        <button class="row" onclick="resetProgramToDefault()">
          <div class="row-icon" style="background:rgba(255,159,10,0.15);color:var(--warning)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><polyline points="1,4 1,10 7,10"/><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">Ripristina programma predefinito</div>
            <div class="row-sub">Torna allo schema Upper/Lower originale</div>
          </div>
          <span class="chev">›</span>
        </button>
      </div>

      <div class="section-title">Suggerimenti del coach</div>
      <div class="card" style="padding:16px">
        <div style="font-size:14px;line-height:1.6;color:var(--text-secondary)">
          <p><strong style="color:var(--text)">Sovraccarico progressivo:</strong> appena completi tutte le serie nel range superiore (es. 4×8), aggiungi 1-2.5kg la sessione successiva.</p>
          <div class="divider-thin"></div>
          <p><strong style="color:var(--text)">Riscaldamento:</strong> 5 min cardio + 1-2 serie leggere prima di ogni primo esercizio composto della sessione.</p>
          <div class="divider-thin"></div>
          <p><strong style="color:var(--text)">Settimana di scarico:</strong> ogni 6-8 settimane, riduci volume e carico del 40% per 5-7 giorni. Recuperi e torni più forte.</p>
        </div>
      </div>
    </div>
  `;
  $('#view-program').innerHTML = html;
}

function openWorkoutSheet(dayId) {
  const w = PROGRAM[dayId];
  if (!w) return;
  const todayDow = new Date().getDay();
  const isToday = w.dow === todayDow;

  const html = `
    <div style="margin-bottom:6px">
      <div style="font-size:12px;color:var(--accent);font-weight:600;text-transform:uppercase;letter-spacing:0.6px;">${DOW_NAMES[w.dow]}</div>
      <div style="font-size:22px;font-weight:700;letter-spacing:-0.4px;margin-top:2px">${w.name}</div>
      <div style="font-size:14px;color:var(--text-secondary);margin-top:4px">${w.focus}</div>
    </div>
    <div class="spacer"></div>
    <div style="display:flex;gap:12px;font-size:13px;color:var(--text-secondary);margin-bottom:18px;flex-wrap:wrap">
      <span>⏱ ~${w.duration} min</span>
      <span>· ${w.exercises.length} esercizi</span>
      <span>· Riposo 60-120s</span>
    </div>
    ${w.exercises.map((ex, i) => {
      const last = getLastWeight(ex.id);
      const best = getBestWeight(ex.id);
      const isPR = last && best && last >= best;
      const suggest = suggestNextWeight(ex.id, ex);
      return `
        <div class="exercise-item" onclick="openExerciseDetailSheet('${ex.id}')" style="cursor:pointer">
          <div class="ex-header">
            <div class="ex-num">${i+1}</div>
            <div style="flex:1;min-width:0">
              <div class="ex-name">${ex.name}</div>
              <div class="ex-target">${ex.target} · ${ex.muscle}</div>
              ${ex.tip ? `<div class="ex-tip">💡 ${ex.tip}</div>` : ''}
              ${suggest ? `<div style="margin-top:8px;display:flex;align-items:center;gap:6px;flex-wrap:wrap">
                <span style="font-size:12px;color:var(--text-secondary)">Prossima volta:</span>
                <span style="font-size:13px;font-weight:700">${suggest.weight} kg</span>
                <span class="suggest-pill ${suggest.action}">${suggest.label}</span>
              </div>` : ''}
            </div>
            ${last ? `<div style="text-align:right;flex-shrink:0">
              <div style="font-size:11px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:0.4px">Ultimo</div>
              <div style="font-size:15px;font-weight:600">${last} kg ${isPR && best>0?'<span class="pr-badge">PR</span>':''}</div>
            </div>` : ''}
          </div>
        </div>`;
    }).join('')}
    <div class="spacer"></div>
    ${isToday ? `<button class="btn" onclick="closeSheet();startWorkout('${w.id}')">Inizia ora</button>` : `<button class="btn secondary" onclick="closeSheet();startWorkout('${w.id}')">Allena questo (anche se non è oggi)</button>`}
    <div class="spacer-sm"></div>
    <button class="btn ghost" onclick="openProgramDayEditSheet('${w.id}')">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
      Modifica esercizi del giorno
    </button>
    <div class="spacer-sm"></div>
    <button class="btn ghost" onclick="openQuickLogSheet('${w.id}')">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14,2 14,8 20,8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/></svg>
      Logga sessione passata
    </button>
    <p style="font-size:11px;color:var(--text-tertiary);text-align:center;margin-top:6px">Hai allenato senza l'app? Inserisci pesi e reps a posteriori</p>
    <div class="spacer-sm"></div>
  `;
  openSheet(w.name, html);
}

function openWorkoutDetailSheet(workoutId, date) {
  const w = PROGRAM[workoutId];
  if (!w) return;
  const session = date
    ? State.data.workoutHistory.find(h => h.workoutId === workoutId && h.date === date)
    : State.data.workoutHistory.filter(h => h.workoutId === workoutId).slice(-1)[0];
  if (!session) return openWorkoutSheet(workoutId);

  const html = `
    <div style="font-size:13px;color:var(--text-secondary)">${fmtDateLong(session.date)}</div>
    <div style="font-size:22px;font-weight:700;letter-spacing:-0.4px;margin-top:2px">${session.workoutName}</div>
    <div style="display:flex;gap:12px;font-size:13px;color:var(--text-secondary);margin-top:6px;flex-wrap:wrap">
      <span>⏱ ${fmtTime(session.duration||0)}</span>
      <span>· ${session.exercisesCompleted}/${session.exercisesTotal} esercizi</span>
      <span>· ${(session.totalVolume||0).toFixed(0)} kg volume</span>
    </div>
    ${session.note ? `<div class="card" style="padding:12px 14px;margin-top:14px;background:var(--surface-2)">
      <div style="font-size:11px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600">Nota di sessione</div>
      <div style="font-size:14px;margin-top:4px;font-style:italic;line-height:1.5">"${session.note}"</div>
    </div>` : ''}
    <div class="spacer"></div>
    ${session.exercises.map((ex,i) => {
      const def = w.exercises.find(e => e.id === ex.id) || {};
      return `
        <div class="exercise-item ${ex.sets.some(s=>s.completed)?'done':''}">
          <div class="ex-header">
            <div class="ex-num">${i+1}</div>
            <div style="flex:1;min-width:0">
              <div class="ex-name">${def.name||ex.name}</div>
              <div class="ex-target">${def.target||''}</div>
            </div>
          </div>
          <div style="margin-top:10px;display:flex;flex-direction:column;gap:4px">
            ${ex.sets.map((s,si) => `
              <div style="display:flex;justify-content:space-between;font-size:13px;padding:4px 0;border-bottom:0.5px solid var(--border)">
                <span class="text-secondary">Serie ${si+1}</span>
                <span style="font-weight:500">${s.weight||0} kg × ${s.reps||0} ${s.completed?'✓':''}</span>
              </div>`).join('')}
          </div>
        </div>`;
    }).join('')}
    <div class="spacer"></div>
  `;
  openSheet('Dettaglio sessione', html);
}


// ── Active workout flow ─────────────────────────────────────────────
let activeWorkout = null;
let workoutTimer = null;
let restTimer = null;
let restEndTime = null;

function startWorkout(workoutId, skipWarmup) {
  const w = PROGRAM[workoutId];
  if (!w) return;
  // Show warmup sheet first if enabled and not already shown
  if (!skipWarmup && State.data.profile.warmupEnabled !== false) {
    openWarmupSheet(workoutId);
    return;
  }
  activeWorkout = {
    id: w.id,
    name: w.name,
    workoutId: w.id,
    workoutName: w.name,
    startedAt: Date.now(),
    exercises: w.exercises.map(ex => {
      const suggest = suggestNextWeight(ex.id, ex);
      const startWeight = suggest ? suggest.weight : (getLastWeight(ex.id) || '');
      return {
        id: ex.id,
        name: ex.name,
        target: ex.target,
        muscle: ex.muscle,
        custom: false,
        sets: ex.reps.includes('s') ?
          Array.from({length: ex.sets}, () => ({ weight: 0, reps: 0, completed: false, time: parseInt(ex.reps) || 45 }))
          : Array.from({length: ex.sets}, () => ({ weight: startWeight, reps: '', completed: false }))
      };
    })
  };
  $('#workout-overlay').classList.add('open');
  $('#workout-name').textContent = w.name;
  renderActiveWorkout();
  startWorkoutClock();
}

function startWorkoutClock() {
  if (workoutTimer) clearInterval(workoutTimer);
  workoutTimer = setInterval(updateWorkoutMeta, 1000);
  updateWorkoutMeta();
}

function updateWorkoutMeta() {
  if (!activeWorkout) return;
  const elapsed = Math.floor((Date.now() - activeWorkout.startedAt)/1000);
  const total = activeWorkout.exercises.length;
  const completedEx = activeWorkout.exercises.filter(e => e.sets.length > 0 && e.sets.every(s => s.completed)).length;
  $('#workout-meta').textContent = `${fmtTime(elapsed)} · ${completedEx}/${total} esercizi`;
  $('#workout-progress').style.width = `${(completedEx / Math.max(1,total)) * 100}%`;
}

function renderActiveWorkout() {
  if (!activeWorkout) return;
  const def = PROGRAM[activeWorkout.workoutId];
  const html = activeWorkout.exercises.map((ex, exIdx) => {
    // Resolve exercise definition (from program OR from active workout itself for custom/swapped)
    let exDef = def.exercises.find(e => e.id === ex.id);
    if (!exDef) exDef = ex; // custom or swapped — use embedded data
    const allDone = ex.sets.every(s => s.completed);
    const lastBest = getBestWeight(ex.id);
    const suggest = suggestNextWeight(ex.id, exDef);
    return `
      <div class="exercise-item ${allDone?'done':''}">
        <div class="ex-header">
          <div class="ex-num">${exIdx+1}</div>
          <div style="flex:1;min-width:0">
            <div class="ex-name">${exDef.name}${ex.custom ? ' <span style="font-size:10px;background:var(--surface-3);padding:2px 6px;border-radius:4px;color:var(--text-secondary);font-weight:600;letter-spacing:0.3px;vertical-align:middle">EXTRA</span>' : ''}</div>
            <div class="ex-target">${exDef.target||''} · ${exDef.muscle||''}</div>
            ${exDef.tip ? `<div class="ex-tip">💡 ${exDef.tip}</div>` : ''}
            ${suggest ? `<div style="margin-top:6px"><span class="suggest-pill ${suggest.action}">${suggest.action==='up'?'↑':suggest.action==='down'?'↓':'='} ${suggest.weight}kg ${suggest.label}</span></div>` : ''}
          </div>
          ${lastBest ? `<div style="text-align:right;flex-shrink:0">
            <div style="font-size:10px;color:var(--text-tertiary);text-transform:uppercase;letter-spacing:0.4px">PR</div>
            <div style="font-size:14px;font-weight:600;color:var(--warning)">${lastBest} kg</div>
          </div>` : ''}
        </div>
        <div class="sets-grid">
          <div class="sets-header">
            <div>Serie</div>
            <div>Peso (kg)</div>
            <div>Reps</div>
            <div></div>
          </div>
          ${ex.sets.map((s, sIdx) => `
            <div class="set-row ${s.completed?'completed':''}" data-ex="${exIdx}" data-set="${sIdx}">
              <div class="set-num">${sIdx+1}</div>
              <input type="number" inputmode="decimal" placeholder="0" value="${s.weight||''}" data-field="weight" step="0.5">
              <input type="number" inputmode="numeric" placeholder="${(exDef.reps||'').split('-')[0]||'0'}" value="${s.reps||''}" data-field="reps">
              <button class="set-toggle" data-toggle>
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round"><polyline points="5,12 10,17 19,7"/></svg>
              </button>
            </div>
          `).join('')}
        </div>
        <div class="ex-actions">
          <button class="ex-action-btn" onclick="openSwapSheet(${exIdx})">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><polyline points="17,1 21,5 17,9"/><path d="M3 11V9a4 4 0 0 1 4-4h14"/><polyline points="7,23 3,19 7,15"/><path d="M21 13v2a4 4 0 0 1-4 4H3"/></svg>
            Sostituisci
          </button>
          <button class="ex-action-btn" onclick="skipExercise(${exIdx})">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><polygon points="5,4 15,12 5,20"/><line x1="19" y1="5" x2="19" y2="19"/></svg>
            Salta
          </button>
          <button class="ex-action-btn" onclick="openSetsConfigSheet(${exIdx})">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="15" y2="12"/><line x1="3" y1="18" x2="9" y2="18"/></svg>
            ${ex.sets.length} serie
          </button>
        </div>
      </div>`;
  }).join('') + `
    <button class="add-extra-btn" onclick="openAddExtraSheet()">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="12" cy="12" r="9"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
      Aggiungi esercizio extra
    </button>
    <div style="margin-top:14px">
      <button class="ex-action-btn" onclick="openWorkoutNoteSheet()" style="width:100%;padding:12px">
        <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14,2 14,8 20,8"/></svg>
        ${getDraftNote() ? 'Nota: '+(getDraftNote().length>30?getDraftNote().slice(0,30)+'…':getDraftNote()) : 'Aggiungi nota di sessione'}
      </button>
    </div>
  `;
  $('#workout-body').innerHTML = html;
  // Bind set events
  $$('#workout-body .set-row').forEach(row => {
    const exIdx = parseInt(row.dataset.ex), sIdx = parseInt(row.dataset.set);
    row.querySelectorAll('input').forEach(inp => {
      inp.oninput = (e) => {
        activeWorkout.exercises[exIdx].sets[sIdx][e.target.dataset.field] = parseFloat(e.target.value) || 0;
      };
    });
    row.querySelector('[data-toggle]').onclick = () => toggleSet(exIdx, sIdx);
  });
  updateWorkoutMeta();
}

let draftNote = '';
function getDraftNote() { return draftNote; }

function toggleSet(exIdx, sIdx) {
  const set = activeWorkout.exercises[exIdx].sets[sIdx];
  set.completed = !set.completed;
  if (set.completed) {
    // Auto-fill weight from previous set if empty
    if (!set.weight && sIdx > 0) {
      set.weight = activeWorkout.exercises[exIdx].sets[sIdx-1].weight;
    }
    // Start rest timer
    startRestTimer(90);
  }
  renderActiveWorkout();
}

function startRestTimer(seconds) {
  stopRestTimer();
  restEndTime = Date.now() + seconds*1000;
  showRestTimer();
  restTimer = setInterval(updateRestTimer, 200);
}
function updateRestTimer() {
  const remaining = Math.max(0, Math.ceil((restEndTime - Date.now())/1000));
  const el = document.querySelector('.rest-timer .time');
  if (el) el.textContent = fmtTime(remaining);
  if (remaining <= 0) {
    stopRestTimer();
    if (navigator.vibrate) navigator.vibrate([100,50,100]);
    toast('Riposo finito — vai!');
  }
}
function showRestTimer() {
  let el = document.querySelector('.rest-timer');
  if (!el) {
    el = document.createElement('div');
    el.className = 'rest-timer';
    el.innerHTML = `
      <div class="ring">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><circle cx="12" cy="12" r="9"/><polyline points="12,7 12,12 16,14"/></svg>
      </div>
      <div class="info">
        <div class="label">Riposo</div>
        <div class="time">1:30</div>
      </div>
      <button class="skip">Salta</button>
    `;
    document.body.appendChild(el);
    el.querySelector('.skip').onclick = () => { stopRestTimer(); };
  }
}
function stopRestTimer() {
  if (restTimer) { clearInterval(restTimer); restTimer = null; }
  const el = document.querySelector('.rest-timer');
  if (el) el.remove();
}

function endWorkout(save = true) {
  if (!activeWorkout) return;
  if (workoutTimer) { clearInterval(workoutTimer); workoutTimer = null; }
  stopRestTimer();
  if (save) {
    const duration = Math.floor((Date.now() - activeWorkout.startedAt)/1000);
    let totalVol = 0;
    let exDone = 0;
    activeWorkout.exercises.forEach(ex => {
      const completedSets = ex.sets.filter(s => s.completed);
      if (completedSets.length === ex.sets.length) exDone++;
      // Volume
      completedSets.forEach(s => { totalVol += (parseFloat(s.weight)||0) * (parseFloat(s.reps)||0); });
      // Per-exercise log
      const topSet = Math.max(0, ...completedSets.map(s => parseFloat(s.weight)||0));
      if (completedSets.length > 0) {
        if (!State.data.exerciseLog[ex.id]) State.data.exerciseLog[ex.id] = [];
        State.data.exerciseLog[ex.id].push({
          date: new Date().toISOString(),
          sets: completedSets.map(s => ({ weight: parseFloat(s.weight)||0, reps: parseFloat(s.reps)||0 })),
          topSet
        });
      }
    });
    State.data.workoutHistory.push({
      date: new Date().toISOString(),
      workoutId: activeWorkout.workoutId,
      workoutName: activeWorkout.workoutName,
      duration,
      exercisesCompleted: exDone,
      exercisesTotal: activeWorkout.exercises.length,
      totalVolume: totalVol,
      note: draftNote || '',
      exercises: activeWorkout.exercises.map(ex => ({
        id: ex.id,
        name: ex.name,
        custom: !!ex.custom,
        sets: ex.sets.map(s => ({ weight: parseFloat(s.weight)||0, reps: parseFloat(s.reps)||0, completed: !!s.completed }))
      }))
    });
    State.save();
    draftNote = '';
    if (totalVol > 0) toast(`Sessione salvata: ${(totalVol).toFixed(0)} kg di volume!`);
  }
  activeWorkout = null;
  $('#workout-overlay').classList.remove('open');
  // Re-render today and program
  renderView('today');
  renderView('program');
}

$('#workout-back').onclick = () => {
  if (confirm('Uscire dalla sessione? I dati inseriti andranno persi se non hai completato almeno una serie.')) {
    const hasData = activeWorkout && activeWorkout.exercises.some(ex => ex.sets.some(s => s.completed));
    endWorkout(hasData);
  }
};
$('#workout-end').onclick = () => {
  if (!activeWorkout) return;
  const hasData = activeWorkout.exercises.some(ex => ex.sets.some(s => s.completed));
  if (!hasData) {
    if (!confirm('Non hai completato nessuna serie. Terminare comunque?')) return;
    endWorkout(false);
  } else {
    if (confirm('Terminare e salvare la sessione?')) endWorkout(true);
  }
};


// ── Progress view (weight chart + photos) ────────────────────────────
let allPhotos = [];

async function renderProgress() {
  allPhotos = await PhotoDB.getAll();
  const log = State.data.weightLog.slice().sort((a,b) => new Date(a.date) - new Date(b.date));
  const current = log.length ? log[log.length-1].value : State.data.profile.currentWeight;
  const start = State.data.profile.startWeight;
  const delta = current - start;
  const last30 = log.filter(e => Date.now() - new Date(e.date).getTime() < 30*24*60*60*1000);
  const delta30 = last30.length >= 2 ? (last30[last30.length-1].value - last30[0].value) : 0;

  // PRs (top 5 most recent or impressive)
  const prs = [];
  Object.entries(State.data.exerciseLog).forEach(([exId, log]) => {
    if (!log.length) return;
    const def = Object.values(PROGRAM).flatMap(d => d.exercises).find(e => e.id === exId)
             || State.data.customExercises.find(e => e.id === exId);
    if (!def) return;
    let best = 0, bestDate = null;
    log.forEach(e => { if (e.topSet > best) { best = e.topSet; bestDate = e.date; } });
    if (best > 0) prs.push({ exId, name: def.name, weight: best, date: bestDate });
  });
  prs.sort((a,b) => new Date(b.date) - new Date(a.date));
  const topPRs = prs.slice(0,5);

  const html = `
    <div class="section" style="padding-top:16px">
      <div class="chart-card">
        <div class="chart-header">
          <div>
            <div style="font-size:12px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600">Peso corporeo</div>
            <div class="chart-current">${current.toFixed(1)}<span class="unit"> kg</span></div>
            <div class="chart-delta ${delta30>0?'text-accent':delta30<0?'text-secondary':''}" style="${delta30<0?'color:var(--accent)':''}">${delta30===0?'Stabile':(delta30>0?'+':'')+delta30.toFixed(1)+' kg ultimi 30g'} · da inizio: ${delta>=0?'+':''}${delta.toFixed(1)}kg</div>
          </div>
          <button class="btn compact" onclick="openWeightSheet()">+ Aggiungi</button>
        </div>
        <canvas class="chart" id="weight-chart" width="600" height="160"></canvas>
      </div>

      ${topPRs.length ? `
        <div class="section-title">I tuoi record</div>
        <div class="list">
          ${topPRs.map(pr => `
            <button class="row" onclick="openExerciseDetailSheet('${pr.exId}')">
              <div class="row-icon" style="background:rgba(255,159,10,0.15);color:var(--warning)">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2l2 6h6l-5 4 2 6-5-4-5 4 2-6-5-4h6z"/></svg>
              </div>
              <div class="row-main">
                <div class="row-title" style="font-size:14px">${pr.name}</div>
                <div class="row-sub">${fmtDateLong(pr.date)}</div>
              </div>
              <div style="font-size:16px;font-weight:700">${pr.weight} kg</div>
              <span class="chev" style="margin-left:6px">›</span>
            </button>
          `).join('')}
        </div>
      ` : ''}

      <div class="section-title">Attività cardio</div>
      ${renderCardioSection()}

      <div class="section-title">Misure circonferenze</div>
      ${renderMeasurementsSection()}

      <div class="section-title">Calendario allenamenti</div>
      <div id="cal-month-container">${renderCalendarMonth(calYear, calMonth)}</div>

      <div class="photo-section-title">
        <span class="name">Foto progressi</span>
        <div style="display:flex;gap:14px;align-items:center">
          ${allPhotos.length >= 2 ? `<button class="add" onclick="openCompareOverlay()">Confronta</button>` : ''}
          <button class="add" onclick="openPhotoUploadSheet()">+ Aggiungi foto</button>
        </div>
      </div>
      ${renderPhotoSection('front', 'Fronte')}
      ${renderPhotoSection('side', 'Lato')}
      ${renderPhotoSection('back', 'Retro')}
    </div>
  `;
  $('#view-progress').innerHTML = html;
  // Draw chart
  setTimeout(() => drawWeightChart(log), 50);
}

function renderPhotoSection(pose, label) {
  const photos = allPhotos.filter(p => p.pose === pose).sort((a,b) => new Date(b.date) - new Date(a.date));
  return `
    <div style="margin-top:14px">
      <div style="font-size:13px;color:var(--text-tertiary);font-weight:600;margin-bottom:8px;padding:0 4px;text-transform:uppercase;letter-spacing:0.5px">${label}</div>
      ${photos.length ? `
        <div class="photo-grid">
          ${photos.slice(0,6).map(p => `
            <div class="photo-tile" onclick="openPhotoViewer(${p.id})">
              <img src="${p.dataUrl}" alt="">
              <div class="pose-tag">${p.pose}</div>
              <div class="date-overlay">${fmtDate(p.date)}</div>
            </div>
          `).join('')}
        </div>
      ` : `
        <div class="photo-grid">
          <button class="photo-tile" onclick="openPhotoUploadSheet('${pose}')" style="background:var(--surface-1);border:1px dashed var(--border-strong);cursor:pointer">
            <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" class="add-icon"><circle cx="12" cy="12" r="9"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
          </button>
        </div>
      `}
    </div>
  `;
}

// Canvas chart drawing
function drawWeightChart(log) {
  const canvas = $('#weight-chart');
  if (!canvas) return;
  const ctx = canvas.getContext('2d');
  const dpr = window.devicePixelRatio || 1;
  const cssW = canvas.clientWidth;
  const cssH = 160;
  canvas.width = cssW * dpr; canvas.height = cssH * dpr;
  canvas.style.width = cssW + 'px'; canvas.style.height = cssH + 'px';
  ctx.scale(dpr, dpr);
  ctx.clearRect(0,0,cssW,cssH);

  if (log.length < 1) return;

  const padX = 8, padY = 16;
  const data = log.slice(-30); // last 30 entries
  // If only 1 point, show flat line
  if (data.length === 1) data.push({ date: new Date().toISOString(), value: data[0].value });

  const values = data.map(d => d.value);
  let min = Math.min(...values), max = Math.max(...values);
  if (max === min) { max = min + 1; min = min - 1; }
  const range = max - min;
  min -= range * 0.15; max += range * 0.15;

  const w = cssW - padX*2;
  const h = cssH - padY*2;
  const xStep = data.length > 1 ? w/(data.length-1) : w;

  // Grid lines
  ctx.strokeStyle = 'rgba(255,255,255,0.05)';
  ctx.lineWidth = 1;
  for (let i = 0; i <= 3; i++) {
    const y = padY + (h*i/3);
    ctx.beginPath(); ctx.moveTo(padX, y); ctx.lineTo(cssW-padX, y); ctx.stroke();
  }

  // Line path
  const points = data.map((d,i) => ({
    x: padX + xStep*i,
    y: padY + h - ((d.value - min)/(max-min)) * h
  }));

  // Gradient fill under line
  const grad = ctx.createLinearGradient(0, padY, 0, padY+h);
  grad.addColorStop(0, 'rgba(48,209,88,0.3)');
  grad.addColorStop(1, 'rgba(48,209,88,0)');
  ctx.fillStyle = grad;
  ctx.beginPath();
  ctx.moveTo(points[0].x, padY+h);
  points.forEach(p => ctx.lineTo(p.x, p.y));
  ctx.lineTo(points[points.length-1].x, padY+h);
  ctx.closePath(); ctx.fill();

  // Smooth line
  ctx.strokeStyle = '#30d158';
  ctx.lineWidth = 2.5;
  ctx.lineJoin = 'round'; ctx.lineCap = 'round';
  ctx.beginPath();
  points.forEach((p,i) => {
    if (i === 0) ctx.moveTo(p.x, p.y);
    else {
      const prev = points[i-1];
      const cpX = (prev.x + p.x) / 2;
      ctx.bezierCurveTo(cpX, prev.y, cpX, p.y, p.x, p.y);
    }
  });
  ctx.stroke();

  // Last point dot
  const last = points[points.length-1];
  ctx.fillStyle = '#30d158';
  ctx.beginPath(); ctx.arc(last.x, last.y, 4, 0, Math.PI*2); ctx.fill();
  ctx.fillStyle = '#001a09';
  ctx.beginPath(); ctx.arc(last.x, last.y, 1.5, 0, Math.PI*2); ctx.fill();

  // Min/Max labels
  ctx.font = '11px -apple-system, sans-serif';
  ctx.fillStyle = 'rgba(255,255,255,0.4)';
  ctx.textAlign = 'right';
  ctx.fillText(max.toFixed(1)+' kg', cssW-padX, padY+10);
  ctx.fillText(min.toFixed(1)+' kg', cssW-padX, padY+h);
}

// ── Weight log sheet ───────────────────────────────────────────────
function openWeightSheet() {
  const today = new Date();
  const todayStr = today.toISOString().slice(0,10);
  const recent = State.data.weightLog.slice().sort((a,b) => new Date(b.date) - new Date(a.date)).slice(0,15);
  const html = `
    <div class="form-group">
      <label class="form-label">Peso (kg)</label>
      <input type="number" inputmode="decimal" id="weight-input" class="form-input" placeholder="${State.data.profile.currentWeight}" step="0.1">
    </div>
    <div class="form-group">
      <label class="form-label">Data</label>
      <input type="date" id="weight-date" class="form-input" value="${todayStr}">
    </div>
    <button class="btn" id="weight-save">Salva pesata</button>
    ${recent.length ? `
      <div class="section-title" style="margin-top:24px">Storico recente</div>
      <div class="list">
        ${recent.map((e,i) => `
          <div class="row" style="cursor:default">
            <div class="row-main">
              <div class="row-title">${e.value.toFixed(1)} kg</div>
              <div class="row-sub">${fmtDateLong(e.date)}</div>
            </div>
            <button class="row-trail" onclick="deleteWeight('${e.date}')" style="background:none;border:none;color:var(--danger);cursor:pointer;font-family:inherit;font-size:13px">Elimina</button>
          </div>
        `).join('')}
      </div>
    ` : ''}
    <div class="spacer"></div>
  `;
  openSheet('Aggiungi pesata', html, () => {
    $('#weight-input').focus();
    $('#weight-save').onclick = () => {
      const v = parseFloat($('#weight-input').value);
      const d = $('#weight-date').value;
      if (isNaN(v) || v < 30 || v > 250) { toast('Inserisci un peso valido'); return; }
      // Replace if same date exists
      State.data.weightLog = State.data.weightLog.filter(e => !sameDay(e.date, d));
      State.data.weightLog.push({ date: new Date(d).toISOString(), value: v });
      State.data.profile.currentWeight = v;
      State.save();
      closeSheet();
      toast('Peso registrato');
      renderView('progress');
      renderView('today');
    };
  });
}

function deleteWeight(dateIso) {
  if (!confirm('Eliminare questa pesata?')) return;
  State.data.weightLog = State.data.weightLog.filter(e => e.date !== dateIso);
  // Update current weight to most recent
  const sorted = State.data.weightLog.slice().sort((a,b) => new Date(b.date) - new Date(a.date));
  if (sorted.length) State.data.profile.currentWeight = sorted[0].value;
  State.save();
  closeSheet();
  renderView('progress');
}

// ── Photo upload ───────────────────────────────────────────────────
let pendingPose = 'front';
function openPhotoUploadSheet(presetPose) {
  pendingPose = presetPose || 'front';
  const html = `
    <div class="form-group">
      <label class="form-label">Posa</label>
      <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px">
        ${[['front','Fronte'],['side','Lato'],['back','Retro']].map(([v,l]) => `
          <button class="btn ${v===pendingPose?'':'secondary'}" data-pose="${v}" onclick="document.querySelectorAll('[data-pose]').forEach(b=>b.classList.add('secondary'));this.classList.remove('secondary');pendingPose='${v}'">${l}</button>
        `).join('')}
      </div>
    </div>
    <div class="form-group">
      <label class="form-label">Suggerimenti</label>
      <div style="background:var(--surface-2);border-radius:12px;padding:12px;font-size:13px;color:var(--text-secondary);line-height:1.5">
        Stessa stanza, stessa luce, stessa posa, stesso orario (al mattino a digiuno è ideale). La costanza fa vedere il progresso.
      </div>
    </div>
    <button class="btn" id="photo-pick">Scatta o seleziona foto</button>
    <div class="spacer-sm"></div>
    <button class="btn ghost" onclick="closeSheet()">Annulla</button>
    <div class="spacer"></div>
  `;
  openSheet('Aggiungi foto progressi', html, () => {
    $('#photo-pick').onclick = () => $('#photo-input').click();
  });
}

$('#photo-input').onchange = async (e) => {
  const file = e.target.files[0];
  if (!file) return;
  // Compress image client-side
  const dataUrl = await compressImage(file, 1200, 0.8);
  const photo = {
    pose: pendingPose,
    date: new Date().toISOString(),
    dataUrl
  };
  await PhotoDB.add(photo);
  $('#photo-input').value = '';
  closeSheet();
  toast('Foto salvata!');
  renderView('progress');
};

function compressImage(file, maxDim, quality) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = (e) => {
      const img = new Image();
      img.onload = () => {
        const ratio = Math.min(1, maxDim / Math.max(img.width, img.height));
        const w = Math.round(img.width * ratio), h = Math.round(img.height * ratio);
        const canvas = document.createElement('canvas');
        canvas.width = w; canvas.height = h;
        const ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0, w, h);
        resolve(canvas.toDataURL('image/jpeg', quality));
      };
      img.onerror = reject;
      img.src = e.target.result;
    };
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
}

// Photo viewer
let viewingPhotoId = null;
function openPhotoViewer(id) {
  const p = allPhotos.find(p => p.id === id);
  if (!p) return;
  viewingPhotoId = id;
  $('#photo-viewer-img').src = p.dataUrl;
  $('#photo-viewer-meta').innerHTML = `<div class="d">${fmtDateLong(p.date)}</div><div class="p">${p.pose}</div>`;
  $('#photo-viewer').classList.add('open');
}
$('#photo-close').onclick = () => $('#photo-viewer').classList.remove('open');
$('#photo-delete').onclick = async () => {
  if (!confirm('Eliminare questa foto?')) return;
  await PhotoDB.delete(viewingPhotoId);
  $('#photo-viewer').classList.remove('open');
  toast('Foto eliminata');
  renderView('progress');
};


// ── Profile view ────────────────────────────────────────────────────
function renderProfile() {
  const p = State.data.profile;
  const totalWorkouts = State.data.workoutHistory.length;
  const totalVolume = State.data.workoutHistory.reduce((s,w) => s + (w.totalVolume||0), 0);
  const startDate = new Date(State.data.startDate);
  const daysSince = Math.floor((Date.now() - startDate.getTime()) / (24*60*60*1000));

  const html = `
    <div class="section" style="padding-top:20px">
      <div style="text-align:center;margin-bottom:24px">
        <div style="width:80px;height:80px;margin:0 auto;border-radius:50%;background:linear-gradient(135deg,var(--accent),var(--accent-dim));display:flex;align-items:center;justify-content:center;font-size:32px;font-weight:700;color:#001a09">
          ${(p.name||'C').charAt(0).toUpperCase()}
        </div>
        <div style="font-size:22px;font-weight:700;margin-top:12px;letter-spacing:-0.4px">${p.name||'Atleta'}</div>
        <div style="font-size:13px;color:var(--text-secondary);margin-top:4px">In allenamento da ${daysSince} giorni</div>
      </div>

      <div class="section-title" style="margin-top:0">Dati personali</div>
      <div class="list">
        <button class="row" onclick="openProfileEditSheet()">
          <div class="row-icon" style="background:rgba(48,209,88,0.15);color:var(--accent)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><circle cx="12" cy="8" r="4"/><path d="M4 21v-1a7 7 0 0 1 14 0v1"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">Nome</div>
            <div class="row-sub">${p.name||'—'}</div>
          </div>
          <span class="chev">›</span>
        </button>
        <button class="row" onclick="openProfileEditSheet()">
          <div class="row-icon" style="background:rgba(10,132,255,0.15);color:var(--info)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><line x1="12" y1="2" x2="12" y2="22"/><line x1="2" y1="12" x2="22" y2="12"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">Altezza</div>
            <div class="row-sub">${p.height ? p.height+' cm' : '—'}</div>
          </div>
          <span class="chev">›</span>
        </button>
        <button class="row" onclick="openProfileEditSheet()">
          <div class="row-icon" style="background:rgba(255,159,10,0.15);color:var(--warning)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><circle cx="12" cy="12" r="9"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">Peso di partenza</div>
            <div class="row-sub">${p.startWeight} kg</div>
          </div>
          <span class="chev">›</span>
        </button>
        <button class="row" onclick="openProfileEditSheet()">
          <div class="row-icon" style="background:rgba(255,69,58,0.15);color:var(--danger)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M12 2L2 7l10 5 10-5-10-5z"/><path d="M2 17l10 5 10-5"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">Obiettivo</div>
            <div class="row-sub" style="white-space:normal;line-height:1.4">${p.goal || '—'}</div>
          </div>
          <span class="chev">›</span>
        </button>
      </div>

      <div class="section-title">Statistiche complessive</div>
      <div class="list">
        <div class="row" style="cursor:default">
          <div class="row-main">
            <div class="row-title">Sessioni completate</div>
          </div>
          <div class="row-trail" style="font-weight:600;color:var(--text)">${totalWorkouts}</div>
        </div>
        <div class="row" style="cursor:default">
          <div class="row-main">
            <div class="row-title">Volume totale sollevato</div>
            <div class="row-sub">Somma di tutti i pesi × ripetizioni</div>
          </div>
          <div class="row-trail" style="font-weight:600;color:var(--text)">${(totalVolume).toFixed(0)} kg</div>
        </div>
        <div class="row" style="cursor:default">
          <div class="row-main">
            <div class="row-title">Pesate registrate</div>
          </div>
          <div class="row-trail" style="font-weight:600;color:var(--text)">${State.data.weightLog.length}</div>
        </div>
        <div class="row" style="cursor:default">
          <div class="row-main">
            <div class="row-title">Foto progressi</div>
          </div>
          <div class="row-trail" style="font-weight:600;color:var(--text)">${allPhotos.length}</div>
        </div>
      </div>

      <div class="section-title">Coach del giorno</div>
      <div class="coach-card">
        <div class="who">
          <div class="avatar">C</div>
          <div>
            <div class="role">Coach Cesare</div>
            <div class="role-sub">50 anni di esperienza</div>
          </div>
        </div>
        <div class="quote">"La costanza batte l'intensità. Vieni qui 4 volte a settimana per 6 mesi e ti vedrai cambiato. Te lo prometto."</div>
      </div>

      <div class="section-title">Dati e privacy</div>
      <div class="list">
        <button class="row" onclick="exportData()">
          <div class="row-icon" style="background:var(--surface-2);color:var(--text-secondary)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M12 3v12"/><polyline points="7,10 12,15 17,10"/><path d="M3 17v3a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-3"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title">Esporta dati (JSON)</div>
            <div class="row-sub">Backup di tutti i tuoi allenamenti</div>
          </div>
          <span class="chev">›</span>
        </button>
        <button class="row" onclick="resetAll()">
          <div class="row-icon" style="background:rgba(255,69,58,0.15);color:var(--danger)">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><polyline points="3,6 5,6 21,6"/><path d="M19 6l-2 14H7L5 6"/></svg>
          </div>
          <div class="row-main">
            <div class="row-title" style="color:var(--danger)">Cancella tutti i dati</div>
            <div class="row-sub">Reset completo dell'app</div>
          </div>
        </button>
      </div>

      <div style="text-align:center;margin-top:30px;padding:20px;color:var(--text-tertiary);font-size:12px">
        GymCoach · v1.0<br>
        Built per Cristian, da Coach Cesare e i suoi soci tecnici 💪
      </div>
    </div>
  `;
  $('#view-profile').innerHTML = html;
}

function openProfileEditSheet() {
  const p = State.data.profile;
  const html = `
    <div class="form-group">
      <label class="form-label">Nome</label>
      <input type="text" id="pf-name" class="form-input" value="${p.name||''}" placeholder="Il tuo nome">
    </div>
    <div class="form-row">
      <div class="form-group">
        <label class="form-label">Altezza (cm)</label>
        <input type="number" inputmode="numeric" id="pf-height" class="form-input" value="${p.height||''}" placeholder="180">
      </div>
      <div class="form-group">
        <label class="form-label">Età</label>
        <input type="number" inputmode="numeric" id="pf-age" class="form-input" value="${p.age||''}" placeholder="30">
      </div>
    </div>
    <div class="form-group">
      <label class="form-label">Peso di partenza (kg)</label>
      <input type="number" inputmode="decimal" id="pf-start" class="form-input" value="${p.startWeight||''}" placeholder="80" step="0.1">
    </div>
    <div class="form-group">
      <label class="form-label">Target proteine giornaliero (g)</label>
      <input type="number" inputmode="numeric" id="pf-protein" class="form-input" value="${p.proteinTarget||''}" placeholder="144">
      <div style="font-size:11px;color:var(--text-tertiary);margin-top:4px;padding:0 4px">Consigliato: 1.6-2.0g per kg di peso (per 80kg → 130-160g)</div>
    </div>
    <div class="form-group">
      <label class="form-label">Obiettivo</label>
      <textarea id="pf-goal" class="form-textarea" placeholder="Cosa vuoi ottenere?">${p.goal||''}</textarea>

```
</div>
<div class="list" style="margin-bottom:14px">
  <div class="row" style="cursor:default">
    <div class="row-main">
      <div class="row-title">Riscaldamento prima dell'allenamento</div>
      <div class="row-sub">Mostra checklist mobilità all'inizio</div>
    </div>
    <label style="position:relative;display:inline-block;width:50px;height:30px">
      <input type="checkbox" id="pf-warmup" ${p.warmupEnabled!==false?'checked':''} style="opacity:0;width:0;height:0">
      <span id="pf-warmup-track" style="position:absolute;cursor:pointer;inset:0;background:${p.warmupEnabled!==false?'var(--accent)':'var(--surface-3)'};border-radius:30px;transition:0.2s">
        <span style="position:absolute;height:24px;width:24px;left:${p.warmupEnabled!==false?'23px':'3px'};top:3px;background:white;border-radius:50%;transition:0.2s"></span>
      </span>
    </label>
  </div>
</div>
<button class="btn" id="pf-save">Salva</button>
<div class="spacer"></div>
```

`;
openSheet(‘Modifica profilo’, html, () => {
// Toggle handler for warmup
const cb = $(’#pf-warmup’);
const track = $(’#pf-warmup-track’);
track.onclick = () => {
cb.checked = !cb.checked;
const knob = track.querySelector(‘span’);
track.style.background = cb.checked ? ‘var(–accent)’ : ‘var(–surface-3)’;
knob.style.left = cb.checked ? ‘23px’ : ‘3px’;
};
$(’#pf-save’).onclick = () => {
State.data.profile.name = $(’#pf-name’).value.trim() || ‘Atleta’;
State.data.profile.height = parseFloat($(’#pf-height’).value) || null;
State.data.profile.age = parseInt($(’#pf-age’).value) || null;
const sw = parseFloat($(’#pf-start’).value);
if (sw > 0) State.data.profile.startWeight = sw;
const pt = parseInt($(’#pf-protein’).value);
if (pt > 0) State.data.profile.proteinTarget = pt;
State.data.profile.goal = $(’#pf-goal’).value.trim();
State.data.profile.warmupEnabled = $(’#pf-warmup’).checked;
State.save();
closeSheet();
toast(‘Profilo aggiornato’);
renderView(‘profile’);
renderView(‘today’);
};
});
}

function exportData() {
const blob = new Blob([JSON.stringify(State.data, null, 2)], { type: ‘application/json’ });
const url = URL.createObjectURL(blob);
const a = document.createElement(‘a’);
a.href = url;
a.download = `gymcoach-backup-${todayKey()}.json`;
document.body.appendChild(a); a.click(); a.remove();
URL.revokeObjectURL(url);
toast(‘Backup scaricato’);
}

function resetAll() {
if (!confirm(‘Eliminare TUTTI i dati (allenamenti, pesate, foto, profilo)? Operazione non reversibile.’)) return;
if (!confirm(‘Sei davvero sicuro? Ultima conferma.’)) return;
localStorage.removeItem(KEY);
// Clear photos
if (PhotoDB.db) {
const tx = PhotoDB.db.transaction(‘photos’, ‘readwrite’);
tx.objectStore(‘photos’).clear();
}
State.load();
toast(‘Tutto cancellato’);
switchTab(‘today’);
}

// ════════════════════════════════════════════════════════════════════
// NEW FEATURES — v2
// ════════════════════════════════════════════════════════════════════

// ── Week strip (Today view) ─────────────────────────────────────────
function renderWeekStrip() {
const today = new Date();
const day = today.getDay();
const offset = day === 0 ? -6 : 1 - day;
const monday = new Date(today); monday.setDate(today.getDate() + offset); monday.setHours(0,0,0,0);
const cells = [];
for (let i = 0; i < 7; i++) {
const d = new Date(monday); d.setDate(monday.getDate() + i);
const isToday = d.toDateString() === today.toDateString();
const isFuture = d > today && !isToday;
const dKey = d.toISOString().slice(0,10);
const hasSession = State.data.workoutHistory.some(w => new Date(w.date).toISOString().slice(0,10) === dKey);
const hasCardio = State.data.cardioLog.some(c => new Date(c.date).toISOString().slice(0,10) === dKey);
const cls = `cal-strip-cell ${isToday?'today':''} ${hasSession?'has-session':''} ${hasCardio?'has-cardio':''} ${isFuture?'future':''}`;
const onclick = (!isFuture && (hasSession || hasCardio)) ? `onclick="openDayDetail('${dKey}')"` : ‘’;
cells.push(`<div class="${cls}" ${onclick}> <span class="dow">${DOW_INITIAL[d.getDay()]}</span> <span class="dn">${d.getDate()}</span> </div>`);
}
return `<div class="cal-strip">${cells.join('')}</div>`;
}

// ── Full month calendar (Progress view) ─────────────────────────────
let calYear = new Date().getFullYear();
let calMonth = new Date().getMonth();

function renderCalendarMonth(year, month) {
const first = new Date(year, month, 1);
const last = new Date(year, month + 1, 0);
const startDay = first.getDay() === 0 ? 6 : first.getDay() - 1;
const daysInMonth = last.getDate();
const today = new Date();

// Index workouts and cardio by day
const sessions = {};
const cardios = {};
State.data.workoutHistory.forEach(w => {
const d = new Date(w.date);
if (d.getFullYear() === year && d.getMonth() === month) {
const day = d.getDate();
if (!sessions[day]) sessions[day] = [];
sessions[day].push(w);
}
});
State.data.cardioLog.forEach(c => {
const d = new Date(c.date);
if (d.getFullYear() === year && d.getMonth() === month) {
const day = d.getDate();
if (!cardios[day]) cardios[day] = [];
cardios[day].push(c);
}
});

const totalCells = Math.ceil((startDay + daysInMonth) / 7) * 7;
let cellsHtml = ‘’;
[‘L’,‘M’,‘M’,‘G’,‘V’,‘S’,‘D’].forEach(d => cellsHtml += `<div class="cal-dow">${d}</div>`);
for (let i = 0; i < totalCells; i++) {
const dayNum = i - startDay + 1;
if (dayNum < 1 || dayNum > daysInMonth) {
cellsHtml += `<div class="cal-cell empty"></div>`;
} else {
const isToday = today.getFullYear() === year && today.getMonth() === month && today.getDate() === dayNum;
const cellDate = new Date(year, month, dayNum);
const isFuture = cellDate > today && !isToday;
const sess = sessions[dayNum];
const card = cardios[dayNum];
const dKey = cellDate.toISOString().slice(0,10);
const onclick = (sess || card) ? `onclick="openDayDetail('${dKey}')"` : ‘’;
cellsHtml += `<div class="cal-cell ${isToday?'today':''} ${sess?'has-session':''} ${card?'has-cardio':''} ${isFuture?'future':''}" ${onclick}> <span class="d">${dayNum}</span> </div>`;
}
}

// Stats for the month
const totalSess = Object.values(sessions).reduce((a,b) => a + b.length, 0);
const totalVol = Object.values(sessions).flat().reduce((s,w) => s + (w.totalVolume||0), 0);
const totalCardio = Object.values(cardios).reduce((a,b) => a + b.length, 0);
const cardioMin = Object.values(cardios).flat().reduce((s,c) => s + (c.duration||0), 0);

return `<div class="cal-month"> <div class="cal-month-header"> <div class="cal-month-title">${MONTHS[month]} ${year}</div> <div class="cal-nav"> <button onclick="changeMonth(-1)" aria-label="Mese precedente"> <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><polyline points="15,18 9,12 15,6"/></svg> </button> <button onclick="changeMonth(1)" aria-label="Mese successivo"> <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><polyline points="9,18 15,12 9,6"/></svg> </button> </div> </div> <div class="cal-grid">${cellsHtml}</div> <div style="margin-top:14px;padding-top:12px;border-top:0.5px solid var(--border);display:flex;gap:14px;font-size:12px;color:var(--text-secondary);flex-wrap:wrap"> <span><span style="display:inline-block;width:6px;height:6px;border-radius:50%;background:var(--accent);margin-right:5px"></span><strong style="color:var(--text)">${totalSess}</strong> palestra</span> <span><span style="display:inline-block;width:6px;height:6px;border-radius:50%;background:var(--warning);margin-right:5px"></span><strong style="color:var(--text)">${totalCardio}</strong> cardio · ${cardioMin}min</span> <span><strong style="color:var(--text)">${(totalVol/1000).toFixed(1)}t</strong> volume</span> </div> </div>`;
}

function changeMonth(delta) {
calMonth += delta;
if (calMonth < 0) { calMonth = 11; calYear–; }
if (calMonth > 11) { calMonth = 0; calYear++; }
$(’#cal-month-container’).innerHTML = renderCalendarMonth(calYear, calMonth);
}

function openDayDetail(dKey) {
const sessions = State.data.workoutHistory.filter(w => new Date(w.date).toISOString().slice(0,10) === dKey);
const cardios = State.data.cardioLog.filter(c => new Date(c.date).toISOString().slice(0,10) === dKey);
if (!sessions.length && !cardios.length) return;
const d = new Date(dKey);

let html = ‘’;
if (sessions.length) {
html += `<div style="font-size:12px;color:var(--accent);font-weight:700;text-transform:uppercase;letter-spacing:0.5px;padding:0 4px 8px">Palestra</div>`;
html += sessions.map(s => `<div class="exercise-item" style="cursor:pointer" onclick="closeSheet();setTimeout(()=>openWorkoutDetailSheet('${s.workoutId}','${s.date}'),250)"> <div class="ex-header"> <div style="flex:1"> <div class="ex-name">${s.workoutName}</div> <div class="ex-target">${fmtTime(s.duration||0)} · ${s.exercisesCompleted}/${s.exercisesTotal} esercizi · ${(s.totalVolume||0).toFixed(0)} kg vol.</div> ${s.note ?`<div style="margin-top:6px;font-size:13px;color:var(--text-secondary);font-style:italic">”${s.note}”</div>`: ''} </div> <span class="chev">›</span> </div> </div>`).join(’’);
}
if (cardios.length) {
html += `<div style="font-size:12px;color:var(--warning);font-weight:700;text-transform:uppercase;letter-spacing:0.5px;padding:14px 4px 8px">Cardio</div>`;
html += cardios.map(c => `<div class="exercise-item"> <div class="ex-header"> <div style="flex:1"> <div class="ex-name">${c.activity}</div> <div class="ex-target">${c.duration} min${c.distance?' · '+c.distance+' km':''}${c.intensity?' · intensità '+c.intensity:''}</div> ${c.notes ?`<div style="margin-top:6px;font-size:13px;color:var(--text-secondary);font-style:italic">”${c.notes}”</div>`: ''} </div> </div> </div>`).join(’’);
}
openSheet(fmtDateLong(dKey), html);
}

// ── Exercise detail with weight history chart ───────────────────────
function openExerciseDetailSheet(exId) {
const def = Object.values(PROGRAM).flatMap(d => d.exercises).find(e => e.id === exId)
|| State.data.customExercises.find(e => e.id === exId);
const log = (State.data.exerciseLog[exId] || []).slice().sort((a,b) => new Date(a.date) - new Date(b.date));
if (!def) return;

const best = getBestWeight(exId);
const last = getLastWeight(exId);
const suggest = suggestNextWeight(exId, def);
const chartId = ‘ex-chart-’ + Date.now();

// Compute average reps and total sets from log
let totalSets = 0, totalReps = 0, totalVol = 0;
log.forEach(e => {
e.sets.forEach(s => { totalSets++; totalReps += (s.reps||0); totalVol += (s.weight||0)*(s.reps||0); });
});

const html = `<div style="margin-bottom:6px"> <div style="font-size:12px;color:var(--accent);font-weight:600;text-transform:uppercase;letter-spacing:0.6px">${def.muscle||''}</div> <div style="font-size:20px;font-weight:700;letter-spacing:-0.3px;margin-top:2px">${def.name}</div> <div style="font-size:13px;color:var(--text-secondary);margin-top:4px">${def.target||''}</div> ${def.tip ?`<div style="font-size:12px;color:var(--accent);margin-top:6px;font-style:italic">💡 ${def.tip}</div>` : ‘’}
</div>

```
<div class="stats-grid" style="margin-top:14px">
  <div class="stat-card">
    <div class="lbl">Ultimo peso</div>
    <div class="val">${last ? last+' kg' : '—'}</div>
  </div>
  <div class="stat-card">
    <div class="lbl">Personal Record</div>
    <div class="val" style="color:var(--warning)">${best ? best+' kg' : '—'}</div>
  </div>
  <div class="stat-card">
    <div class="lbl">Sessioni totali</div>
    <div class="val">${log.length}</div>
  </div>
  <div class="stat-card">
    <div class="lbl">Volume totale</div>
    <div class="val">${(totalVol/1000).toFixed(1)}<span style="font-size:14px;color:var(--text-secondary)"> t</span></div>
  </div>
</div>

${suggest ? `<div class="card" style="padding:14px 16px;margin-top:14px">
  <div style="display:flex;align-items:center;gap:10px">
    <div style="width:34px;height:34px;border-radius:50%;background:rgba(48,209,88,0.18);color:var(--accent);display:flex;align-items:center;justify-content:center;flex-shrink:0">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><path d="M12 2l3 7h7l-5 5 2 8-7-4-7 4 2-8-5-5h7z"/></svg>
    </div>
    <div style="flex:1">
      <div style="font-size:11px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600">Suggerimento Coach</div>
      <div style="font-size:15px;font-weight:600;margin-top:1px">Prossima volta: ${suggest.weight} kg</div>
      <div style="font-size:12px;color:var(--text-secondary);margin-top:1px">${suggest.action==='up'?'Hai chiuso tutte le serie al massimo del range. Sovraccarica.':suggest.action==='down'?'Sei rimasto sotto il range minimo. Scarica e riprova.':'Mantieni il peso, prova ad aggiungere ripetizioni.'}</div>
    </div>
  </div>
</div>` : ''}

<div style="margin-top:14px">
  <button class="btn" onclick="openExerciseLogEditSheet('${exId}')">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
    Aggiungi / modifica sessione
  </button>
</div>

${log.length >= 1 ? `
  <div class="ex-chart-card" style="margin-top:14px">
    <div style="display:flex;justify-content:space-between;align-items:flex-start">
      <div>
        <div style="font-size:11px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600">Progressione peso (top set)</div>
        <div style="font-size:13px;color:var(--text-secondary);margin-top:2px">${log.length} sessioni</div>
      </div>
    </div>
    <canvas class="ex-chart" id="${chartId}"></canvas>
  </div>
` : `<div class="empty" style="padding:20px;margin-top:14px"><div class="t">Nessun dato ancora</div><div class="s">Tocca "Aggiungi sessione" qui sopra per iniziare</div></div>`}

${log.length ? `
  <div class="section-title" style="margin-top:18px">Storico sessioni</div>
  <div style="font-size:12px;color:var(--text-tertiary);padding:0 4px 8px">Tocca una sessione per modificarla o eliminarla</div>
  <div class="list">
    ${log.slice().reverse().slice(0,15).map(e => {
      const sd = new Date(e.date);
      const setsStr = e.sets.map(s => `${s.weight}×${s.reps}`).join(' · ');
      return `<button class="row" onclick="openExerciseLogEditSheet('${exId}', '${e.date}')">
        <div class="day-pill" style="border-radius:10px;width:40px;height:40px;background:var(--surface-2);display:flex;flex-direction:column;align-items:center;justify-content:center;flex-shrink:0">
          <div style="font-size:14px;font-weight:700;line-height:1">${sd.getDate()}</div>
          <div style="font-size:9px;color:var(--text-secondary);margin-top:1px;text-transform:uppercase">${MONTHS_SHORT[sd.getMonth()]}</div>
        </div>
        <div class="row-main">
          <div class="row-title" style="font-size:14px;font-weight:600">${e.topSet} kg top set</div>
          <div class="row-sub">${setsStr}</div>
        </div>
        <span class="chev">›</span>
      </button>`;
    }).join('')}
  </div>
` : ''}
<div class="spacer"></div>
```

`;
openSheet(‘Storico esercizio’, html, () => {
if (log.length >= 1) drawExerciseChart(chartId, log);
});
}

function drawExerciseChart(canvasId, log) {
const canvas = document.getElementById(canvasId);
if (!canvas) return;
const ctx = canvas.getContext(‘2d’);
const dpr = window.devicePixelRatio || 1;
const cssW = canvas.clientWidth;
const cssH = 130;
canvas.width = cssW * dpr; canvas.height = cssH * dpr;
canvas.style.width = cssW + ‘px’; canvas.style.height = cssH + ‘px’;
ctx.scale(dpr, dpr);
ctx.clearRect(0,0,cssW,cssH);

const data = log.map(e => ({ date: new Date(e.date), value: e.topSet || 0 })).filter(d => d.value > 0);
if (data.length < 1) return;
if (data.length === 1) data.push({ date: new Date(), value: data[0].value });

const padX = 10, padY = 14;
const values = data.map(d => d.value);
let min = Math.min(…values), max = Math.max(…values);
if (max === min) { max = min + 5; min = Math.max(0, min - 5); }
const range = max - min;
min = Math.max(0, min - range*0.1); max += range*0.15;

const w = cssW - padX*2, h = cssH - padY*2;
const xStep = data.length > 1 ? w/(data.length-1) : w;

// Grid
ctx.strokeStyle = ‘rgba(255,255,255,0.05)’;
ctx.lineWidth = 1;
for (let i = 0; i <= 2; i++) {
const y = padY + (h*i/2);
ctx.beginPath(); ctx.moveTo(padX, y); ctx.lineTo(cssW-padX, y); ctx.stroke();
}

const points = data.map((d,i) => ({
x: padX + xStep*i,
y: padY + h - ((d.value - min)/(max-min)) * h
}));

// Fill
const grad = ctx.createLinearGradient(0, padY, 0, padY+h);
grad.addColorStop(0, ‘rgba(255,159,10,0.3)’);
grad.addColorStop(1, ‘rgba(255,159,10,0)’);
ctx.fillStyle = grad;
ctx.beginPath();
ctx.moveTo(points[0].x, padY+h);
points.forEach(p => ctx.lineTo(p.x, p.y));
ctx.lineTo(points[points.length-1].x, padY+h);
ctx.closePath(); ctx.fill();

// Line
ctx.strokeStyle = ‘#ff9f0a’;
ctx.lineWidth = 2.5;
ctx.lineJoin = ‘round’; ctx.lineCap = ‘round’;
ctx.beginPath();
points.forEach((p,i) => {
if (i === 0) ctx.moveTo(p.x, p.y);
else {
const prev = points[i-1];
const cpX = (prev.x + p.x) / 2;
ctx.bezierCurveTo(cpX, prev.y, cpX, p.y, p.x, p.y);
}
});
ctx.stroke();

// Points
points.forEach(p => {
ctx.fillStyle = ‘#ff9f0a’;
ctx.beginPath(); ctx.arc(p.x, p.y, 3, 0, Math.PI*2); ctx.fill();
});

// Labels
ctx.font = ‘10px -apple-system, sans-serif’;
ctx.fillStyle = ‘rgba(255,255,255,0.5)’;
ctx.textAlign = ‘right’;
ctx.fillText(max.toFixed(0)+‘kg’, cssW-padX, padY+9);
ctx.fillText(min.toFixed(0)+‘kg’, cssW-padX, padY+h);
}

// ── Exercise swap during workout ───────────────────────────────────
function openSwapSheet(exIdx) {
const ex = activeWorkout.exercises[exIdx];
const category = mapMuscleToCategory(ex.muscle || ‘’);
const alternatives = category ? EXERCISE_LIBRARY[category] : [];
const customs = State.data.customExercises;

const html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> Sostituisci <strong style="color:var(--text)">${ex.name}</strong> con un'alternativa equivalente. Categoria: <strong style="color:var(--text)">${category||'—'}</strong>. </p> <div class="spacer"></div> ${alternatives.length ?`
<div class="muscle-section">
<div class="head">Alternative ${category}</div>
<div class="list">
${alternatives.filter(a => a.name !== ex.name).map((a, i) => `<button class="row" onclick="swapExerciseWith(${exIdx}, ${i}, '${category}')"> <div class="row-main"> <div class="row-title">${a.name}</div> <div class="row-sub">${a.sets} × ${a.reps}</div> </div> <span class="chev">›</span> </button>`).join(’’)}
</div>
</div>
`: ''} ${customs.length ?`
<div class="muscle-section">
<div class="head">I tuoi esercizi</div>
<div class="list">
${customs.map(c => `<button class="row" onclick="swapExerciseWithCustom(${exIdx}, '${c.id}')"> <div class="row-main"> <div class="row-title">${c.name}</div> <div class="row-sub">${c.sets} × ${c.reps} · ${c.muscle}</div> </div> <span class="chev">›</span> </button>`).join(’’)}
</div>
</div>
`: ''} <div class="spacer"></div> <button class="btn ghost" onclick="closeSheet()">Annulla</button>`;
openSheet(‘Sostituisci esercizio’, html);
}

function swapExerciseWith(exIdx, libIdx, category) {
const lib = EXERCISE_LIBRARY[category][libIdx];
if (!lib) return;
const ex = activeWorkout.exercises[exIdx];
// Generate stable id for tracking history
const newId = ‘lib_’ + category + ‘_’ + libIdx;
ex.id = newId;
ex.name = lib.name;
ex.target = `${lib.sets} × ${lib.reps}`;
ex.muscle = category;
ex.custom = true;
// Reset sets to new structure (preserve number of sets to lib spec)
ex.sets = Array.from({length: lib.sets}, () => ({ weight: getLastWeight(newId) || ‘’, reps: ‘’, completed: false }));
closeSheet();
toast(‘Esercizio sostituito’);
renderActiveWorkout();
}

function swapExerciseWithCustom(exIdx, customId) {
const c = State.data.customExercises.find(e => e.id === customId);
if (!c) return;
const ex = activeWorkout.exercises[exIdx];
ex.id = c.id;
ex.name = c.name;
ex.target = c.target;
ex.muscle = c.muscle;
ex.tip = c.tip;
ex.custom = true;
ex.sets = Array.from({length: c.sets}, () => ({ weight: getLastWeight(c.id) || ‘’, reps: ‘’, completed: false }));
closeSheet();
toast(‘Esercizio sostituito’);
renderActiveWorkout();
}

function skipExercise(exIdx) {
if (!confirm(`Saltare "${activeWorkout.exercises[exIdx].name}"? Verrà rimosso da questa sessione.`)) return;
activeWorkout.exercises.splice(exIdx, 1);
toast(‘Esercizio saltato’);
renderActiveWorkout();
}

function openSetsConfigSheet(exIdx) {
const ex = activeWorkout.exercises[exIdx];
const html = `<p class="text-secondary" style="font-size:13px">Modifica numero di serie per questo esercizio in questa sessione.</p> <div class="spacer"></div> <div class="form-group"> <label class="form-label">Numero di serie</label> <input type="number" inputmode="numeric" id="cfg-sets" class="form-input" value="${ex.sets.length}" min="1" max="10"> </div> <button class="btn" onclick="applySetsConfig(${exIdx})">Applica</button> <div class="spacer-sm"></div> <button class="btn ghost" onclick="closeSheet()">Annulla</button>`;
openSheet(‘Configura serie’, html);
}

function applySetsConfig(exIdx) {
const n = parseInt($(’#cfg-sets’).value);
if (isNaN(n) || n < 1 || n > 10) { toast(‘Inserisci un numero valido (1-10)’); return; }
const ex = activeWorkout.exercises[exIdx];
if (n > ex.sets.length) {
const baseWeight = ex.sets.length ? ex.sets[ex.sets.length-1].weight : ‘’;
while (ex.sets.length < n) ex.sets.push({ weight: baseWeight, reps: ‘’, completed: false });
} else if (n < ex.sets.length) {
ex.sets = ex.sets.slice(0, n);
}
closeSheet();
renderActiveWorkout();
}

function openAddExtraSheet() {
const customs = State.data.customExercises;
let html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> Aggiungi un esercizio extra a questa sessione (non modifica il programma). </p> <div class="spacer"></div>`;
if (customs.length) {
html += `<div class="muscle-section"> <div class="head">I tuoi esercizi</div> <div class="list"> ${customs.map(c =>`
<button class="row" onclick="addExtraCustom('${c.id}')">
<div class="row-main">
<div class="row-title">${c.name}</div>
<div class="row-sub">${c.sets} × ${c.reps} · ${c.muscle}</div>
</div>
<span class="chev">›</span>
</button>
`).join('')} </div> </div>`;
}
// All library exercises by category
Object.entries(EXERCISE_LIBRARY).forEach(([cat, list]) => {
html += `<div class="muscle-section"> <div class="head">${cat}</div> <div class="list"> ${list.map((a, i) =>`
<button class="row" onclick="addExtraFromLib('${cat}', ${i})">
<div class="row-main">
<div class="row-title">${a.name}</div>
<div class="row-sub">${a.sets} × ${a.reps}</div>
</div>
<span class="chev">›</span>
</button>
`).join('')} </div> </div>`;
});
html += `<div class="spacer"></div><button class="btn ghost" onclick="closeSheet()">Annulla</button>`;
openSheet(‘Aggiungi esercizio’, html);
}

function addExtraCustom(customId) {
const c = State.data.customExercises.find(e => e.id === customId);
if (!c) return;
activeWorkout.exercises.push({
id: c.id, name: c.name, target: c.target, muscle: c.muscle, tip: c.tip,
custom: true,
sets: Array.from({length: c.sets}, () => ({ weight: getLastWeight(c.id) || ‘’, reps: ‘’, completed: false }))
});
closeSheet();
toast(‘Esercizio aggiunto’);
renderActiveWorkout();
}

function addExtraFromLib(cat, idx) {
const a = EXERCISE_LIBRARY[cat][idx];
if (!a) return;
const newId = ‘lib_’ + cat + ‘_’ + idx;
activeWorkout.exercises.push({
id: newId, name: a.name, target: `${a.sets} × ${a.reps}`, muscle: cat,
custom: true,
sets: Array.from({length: a.sets}, () => ({ weight: getLastWeight(newId) || ‘’, reps: ‘’, completed: false }))
});
closeSheet();
toast(‘Esercizio aggiunto’);
renderActiveWorkout();
}

// ── Workout note ────────────────────────────────────────────────────
function openWorkoutNoteSheet() {
const html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> Annota come ti sei sentito oggi: energia, sonno, dolori, sensazioni. Aiuta a capire dopo perché un giorno è andato meglio o peggio. </p> <div class="form-group" style="margin-top:14px"> <textarea id="note-text" class="form-textarea" placeholder="Es. Ho dormito 5 ore, energia bassa. Spalla destra leggermente fastidiosa.">${draftNote||''}</textarea> </div> <button class="btn" onclick="saveDraftNote()">Salva nota</button> <div class="spacer-sm"></div> <button class="btn ghost" onclick="closeSheet()">Annulla</button>`;
openSheet(‘Nota di sessione’, html);
}

function saveDraftNote() {
draftNote = $(’#note-text’).value.trim();
closeSheet();
toast(‘Nota salvata’);
renderActiveWorkout();
}

// ── Custom exercises CRUD ───────────────────────────────────────────
function openCustomExercisesSheet() {
const list = State.data.customExercises;
const html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> Crea esercizi tuoi (es. una variante che ti piace, un movimento specifico). Li potrai usare durante l'allenamento per sostituire o aggiungere. </p> <div class="spacer"></div> <button class="btn" onclick="openCustomExerciseEditSheet()">+ Nuovo esercizio</button> <div class="spacer"></div> ${list.length ?`
<div class="list">
${list.map(c => `<button class="row" onclick="openCustomExerciseEditSheet('${c.id}')"> <div class="row-icon" style="background:rgba(48,209,88,0.15);color:var(--accent)"> <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><rect x="6" y="10" width="3" height="4" rx="1"/><rect x="15" y="10" width="3" height="4" rx="1"/><line x1="9" y1="12" x2="15" y2="12"/></svg> </div> <div class="row-main"> <div class="row-title">${c.name}</div> <div class="row-sub">${c.sets} × ${c.reps} · ${c.muscle}</div> </div> <span class="chev">›</span> </button>`).join(’’)}
</div>
`: '<div class="empty"><div class="t">Nessun esercizio personalizzato</div><div class="s">Tocca "+ Nuovo esercizio" per crearne uno</div></div>'} <div class="spacer"></div>`;
openSheet(‘I miei esercizi’, html);
}

function openCustomExerciseEditSheet(id) {
const editing = id ? State.data.customExercises.find(c => c.id === id) : null;
const muscles = Object.keys(EXERCISE_LIBRARY);
const html = `<div class="form-group"> <label class="form-label">Nome esercizio</label> <input type="text" id="ce-name" class="form-input" value="${editing?editing.name:''}" placeholder="Es. Pull-over al cavo"> </div> <div class="form-group"> <label class="form-label">Gruppo muscolare</label> <select id="ce-muscle" class="form-select"> ${muscles.map(m =>`<option value=”${m}” ${editing&&editing.muscle===m?‘selected’:’’}>${m}</option>`).join('')} </select> </div> <div class="form-row"> <div class="form-group"> <label class="form-label">Serie</label> <input type="number" inputmode="numeric" id="ce-sets" class="form-input" value="${editing?editing.sets:3}" min="1" max="10"> </div> <div class="form-group"> <label class="form-label">Reps</label> <input type="text" id="ce-reps" class="form-input" value="${editing?editing.reps:'8-10'}" placeholder="8-10"> </div> </div> <div class="form-group"> <label class="form-label">Tip / nota tecnica (opzionale)</label> <textarea id="ce-tip" class="form-textarea" placeholder="Es. Tieni i gomiti vicini al corpo">${editing?editing.tip||'':''}</textarea> </div> <button class="btn" onclick="saveCustomExercise('${id||''}')">Salva</button> <div class="spacer-sm"></div> ${editing ? `<button class="btn danger" onclick="deleteCustomExercise('${id}')">Elimina esercizio</button>`: ''} <div class="spacer-sm"></div> <button class="btn ghost" onclick="openCustomExercisesSheet()">Annulla</button> <div class="spacer"></div>`;
openSheet(editing ? ‘Modifica esercizio’ : ‘Nuovo esercizio’, html, () => {
if (!editing) $(’#ce-name’).focus();
});
}

function saveCustomExercise(id) {
const name = $(’#ce-name’).value.trim();
const muscle = $(’#ce-muscle’).value;
const sets = parseInt($(’#ce-sets’).value) || 3;
const reps = $(’#ce-reps’).value.trim() || ‘8-10’;
const tip = $(’#ce-tip’).value.trim();
if (!name) { toast(‘Nome obbligatorio’); return; }
if (id) {
const ex = State.data.customExercises.find(c => c.id === id);
if (ex) Object.assign(ex, { name, muscle, sets, reps, target: `${sets} × ${reps}`, tip });
} else {
State.data.customExercises.push({
id: ‘custom_’ + Date.now(),
name, muscle, sets, reps, target: `${sets} × ${reps}`, tip
});
}
State.save();
toast(id ? ‘Esercizio aggiornato’ : ‘Esercizio creato’);
openCustomExercisesSheet();
renderView(‘program’);
}

function deleteCustomExercise(id) {
if (!confirm(‘Eliminare questo esercizio personalizzato? Lo storico delle sessioni rimarrà.’)) return;
State.data.customExercises = State.data.customExercises.filter(c => c.id !== id);
State.save();
toast(‘Esercizio eliminato’);
openCustomExercisesSheet();
renderView(‘program’);
}

// ── Body measurements ───────────────────────────────────────────────
const MEASURE_FIELDS = [
{ key: ‘vita’,     label: ‘Vita’,      tip: ‘A livello dell'ombelico, espirando’ },
{ key: ‘fianchi’,  label: ‘Fianchi’,   tip: ‘Punto più largo dei glutei’ },
{ key: ‘petto’,    label: ‘Petto’,     tip: ‘A livello dei capezzoli, braccia rilassate’ },
{ key: ‘braccio’,  label: ‘Braccio’,   tip: ‘Punto più largo, contratto’ },
{ key: ‘coscia’,   label: ‘Coscia’,    tip: ‘Sotto al gluteo, gamba rilassata’ },
{ key: ‘collo’,    label: ‘Collo’,     tip: ‘Sotto il pomo d'Adamo’ }
];

function getLatestMeasurement(key) {
const sorted = State.data.measurements.slice().sort((a,b) => new Date(b.date) - new Date(a.date));
for (const m of sorted) {
if (m[key] != null) return { value: m[key], date: m.date };
}
return null;
}

function getPreviousMeasurement(key, beforeDate) {
const sorted = State.data.measurements
.filter(m => m.date !== beforeDate)
.sort((a,b) => new Date(b.date) - new Date(a.date));
for (const m of sorted) {
if (m[key] != null && new Date(m.date) < new Date(beforeDate)) return { value: m[key], date: m.date };
}
return null;
}

function renderMeasurementsSection() {
const cards = MEASURE_FIELDS.map(f => {
const latest = getLatestMeasurement(f.key);
const prev = latest ? getPreviousMeasurement(f.key, latest.date) : null;
const delta = latest && prev ? (latest.value - prev.value) : null;
return `<div class="measure-card" onclick="openMeasurementsSheet()"> <div class="lbl">${f.label}</div> <div class="val">${latest ? latest.value.toFixed(1) : '—'}<span class="u">cm</span></div> ${delta!=null ?`<div class="delta" style="color:${delta<0?'var(--accent)':delta>0?'var(--warning)':'var(--text-secondary)'}">${delta>0?’+’:’’}${delta.toFixed(1)} cm vs ultima</div>`: '<div class="delta text-secondary">Nessun confronto</div>'} </div>`;
}).join(’’);
return `<div class="measure-grid">${cards}</div> <div style="margin-top:10px"><button class="btn compact" style="width:100%" onclick="openMeasurementsSheet()">+ Aggiungi misure</button></div>`;
}

function openMeasurementsSheet() {
const recent = State.data.measurements.slice().sort((a,b) => new Date(b.date) - new Date(a.date)).slice(0,12);
const todayStr = new Date().toISOString().slice(0,10);
const html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> Misura sempre nello stesso modo: stessa ora (mattino a digiuno è ideale), stesso lato del corpo, metro orizzontale e teso ma non stretto. </p> <div class="form-group" style="margin-top:14px"> <label class="form-label">Data</label> <input type="date" id="mm-date" class="form-input" value="${todayStr}"> </div> <div class="form-row"> ${MEASURE_FIELDS.slice(0,2).map(f =>`
<div class="form-group">
<label class="form-label">${f.label} (cm)</label>
<input type="number" inputmode="decimal" id="mm-${f.key}" class="form-input" placeholder="${f.tip}" step="0.1">
</div>
`).join('')} </div> <div class="form-row"> ${MEASURE_FIELDS.slice(2,4).map(f => `
<div class="form-group">
<label class="form-label">${f.label} (cm)</label>
<input type="number" inputmode="decimal" id="mm-${f.key}" class="form-input" placeholder="${f.tip}" step="0.1">
</div>
`).join('')} </div> <div class="form-row"> ${MEASURE_FIELDS.slice(4,6).map(f => `
<div class="form-group">
<label class="form-label">${f.label} (cm)</label>
<input type="number" inputmode="decimal" id="mm-${f.key}" class="form-input" placeholder="${f.tip}" step="0.1">
</div>
`).join(’’)}
</div>
<p style="font-size:11px;color:var(--text-tertiary);text-align:center;margin-bottom:8px">Compila solo le misure che vuoi. Le altre verranno mantenute dalle precedenti.</p>
<button class="btn" onclick="saveMeasurements()">Salva misure</button>

```
${recent.length ? `
  <div class="section-title" style="margin-top:24px">Storico</div>
  <div class="list">
    ${recent.map(m => {
      const filled = MEASURE_FIELDS.filter(f => m[f.key] != null);
      return `
        <div class="row" style="cursor:default">
          <div class="row-main">
            <div class="row-title">${fmtDateLong(m.date)}</div>
            <div class="row-sub">${filled.map(f => `${f.label.charAt(0)}: ${m[f.key].toFixed(1)}`).join(' · ') || 'Vuoto'}</div>
          </div>
          <button onclick="event.stopPropagation();deleteMeasurement('${m.date}')" style="background:none;border:none;color:var(--danger);cursor:pointer;font-family:inherit;font-size:13px">Elimina</button>
        </div>`;
    }).join('')}
  </div>
` : ''}
<div class="spacer"></div>
```

`;
openSheet(‘Misure circonferenze’, html);
}

function saveMeasurements() {
const date = $(’#mm-date’).value;
if (!date) { toast(‘Inserisci una data’); return; }
const dateIso = new Date(date).toISOString();
const entry = { date: dateIso };
let any = false;
MEASURE_FIELDS.forEach(f => {
const v = parseFloat($(’#mm-’+f.key).value);
if (!isNaN(v) && v > 0 && v < 300) { entry[f.key] = v; any = true; }
});
if (!any) { toast(‘Inserisci almeno una misura’); return; }
// Replace if same date
State.data.measurements = State.data.measurements.filter(m => m.date.slice(0,10) !== dateIso.slice(0,10));
State.data.measurements.push(entry);
State.save();
closeSheet();
toast(‘Misure salvate’);
renderView(‘progress’);
}

function deleteMeasurement(dateIso) {
if (!confirm(‘Eliminare questa rilevazione?’)) return;
State.data.measurements = State.data.measurements.filter(m => m.date !== dateIso);
State.save();
closeSheet();
toast(‘Eliminata’);
renderView(‘progress’);
}

// ════════════════════════════════════════════════════════════════════
// V3 — Cardio · Proteine · Riscaldamento · Confronto foto
// ════════════════════════════════════════════════════════════════════

// ── Daily grid: protein ring + cardio summary ──────────────────────
function renderDailyGrid() {
const todayK = todayKey();
const proteinToday = State.data.proteinLog[todayK] || 0;
const target = State.data.profile.proteinTarget || 144;
const pct = Math.min(100, Math.round((proteinToday / target) * 100));
const r = 25; const C = 2 * Math.PI * r;
const offset = C - (C * pct / 100);
const complete = pct >= 100;

// Today’s cardio
const cardioToday = State.data.cardioLog.filter(c => new Date(c.date).toISOString().slice(0,10) === todayK);
const cardioMin = cardioToday.reduce((s,c) => s + (c.duration||0), 0);
const cardioTitle = cardioToday.length ? cardioToday[0].activity : ‘Nessun cardio’;

return `<div class="daily-grid"> <div class="protein-card" onclick="openProteinSheet()"> <div class="protein-ring ${complete?'complete':''}"> <svg viewBox="0 0 60 60"> <circle class="ring-bg" cx="30" cy="30" r="${r}" stroke-width="5"/> <circle class="ring-fg" cx="30" cy="30" r="${r}" stroke-width="5" stroke-dasharray="${C}" stroke-dashoffset="${offset}"/> </svg> <div class="pct">${pct}%</div> </div> <div class="info"> <div class="lbl">Proteine oggi</div> <div class="val">${proteinToday}<span style="font-size:12px;color:var(--text-secondary);font-weight:500">g</span></div> <div class="target">obiettivo ${target}g</div> </div> </div> <div class="cardio-card" onclick="openCardioSheet()"> <div class="ic"> <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg> </div> <div class="lbl">Cardio oggi</div> <div class="val">${cardioMin > 0 ? cardioMin+' min' : '—'}</div> <div class="sub">${cardioMin > 0 ? cardioTitle : 'Tocca per loggare'}</div> </div> </div>`;
}

// ── Cardio log ──────────────────────────────────────────────────────
const CARDIO_ACTIVITIES = [‘Camminata’,‘Corsa’,‘Cyclette’,‘Bici’,‘Tapis roulant’,‘Ellittica’,‘Vogatore’,‘Nuoto’,‘Stair master’,‘Salto corda’,‘HIIT’,‘Altro’];

function renderCardioSection() {
const recent = State.data.cardioLog.slice().sort((a,b) => new Date(b.date) - new Date(a.date));
const last30 = recent.filter(c => Date.now() - new Date(c.date).getTime() < 30*24*60*60*1000);
const totalMin30 = last30.reduce((s,c) => s + (c.duration||0), 0);
const totalKm30 = last30.reduce((s,c) => s + (c.distance||0), 0);

if (!recent.length) {
return `<div class="card" style="padding:18px"> <div class="empty" style="padding:10px"> <div class="t">Nessuna attività cardio</div> <div class="s" style="margin-bottom:14px">Camminata, corsa, cyclette: tutto conta nei giorni off</div> <button class="btn compact" onclick="openCardioSheet()">+ Logga prima attività</button> </div> </div>`;
}

return `<div class="card" style="padding:14px 16px;margin-bottom:10px"> <div style="display:flex;justify-content:space-between;align-items:center;gap:14px;flex-wrap:wrap"> <div> <div style="font-size:11px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600">Ultimi 30 giorni</div> <div style="font-size:20px;font-weight:700;letter-spacing:-0.4px;margin-top:2px">${totalMin30} min · ${last30.length} sessioni${totalKm30>0?' · '+totalKm30.toFixed(1)+' km':''}</div> </div> <button class="btn compact" onclick="openCardioSheet()">+ Aggiungi</button> </div> </div> <div class="list"> ${recent.slice(0,8).map(c => { const d = new Date(c.date); return`
<div class="row" style="cursor:default">
<div class="day-pill" style="border-radius:10px;width:44px;height:44px;background:var(--surface-2);display:flex;flex-direction:column;align-items:center;justify-content:center;flex-shrink:0">
<div style="font-size:16px;font-weight:700;line-height:1">${d.getDate()}</div>
<div style="font-size:9px;color:var(--text-secondary);margin-top:2px;text-transform:uppercase">${MONTHS_SHORT[d.getMonth()]}</div>
</div>
<div class="row-main">
<div class="row-title">${c.activity}</div>
<div class="row-sub">${c.duration} min${c.distance?’ · ‘+c.distance+’ km’:’’}${c.intensity?’ · ‘+c.intensity:’’}</div>
</div>
<button onclick="deleteCardio('${c.date}')" style="background:none;border:none;color:var(--danger);cursor:pointer;font-family:inherit;font-size:13px">×</button>
</div>
`; }).join('')} </div> `;
}

let selectedCardioActivity = null;

function openCardioSheet() {
selectedCardioActivity = null;
const todayStr = new Date().toISOString().slice(0,10);
const todayCardio = State.data.cardioLog.filter(c => new Date(c.date).toISOString().slice(0,10) === todayKey());
const html = `<div class="form-group"> <label class="form-label">Attività</label> <div class="chips" id="cardio-chips"> ${CARDIO_ACTIVITIES.map(a =>`<button class="chip" data-act="${a}" onclick="selectCardio(this,'${a}')">${a}</button>`).join('')} </div> </div> <div class="form-row"> <div class="form-group"> <label class="form-label">Durata (min)</label> <input type="number" inputmode="numeric" id="cd-duration" class="form-input" placeholder="30"> </div> <div class="form-group"> <label class="form-label">Distanza (km, opz.)</label> <input type="number" inputmode="decimal" id="cd-distance" class="form-input" placeholder="—" step="0.1"> </div> </div> <div class="form-group"> <label class="form-label">Intensità</label> <div class="chips"> <button class="chip" data-int="Bassa" onclick="selectIntensity(this)">Bassa</button> <button class="chip" data-int="Media" onclick="selectIntensity(this)">Media</button> <button class="chip" data-int="Alta" onclick="selectIntensity(this)">Alta</button> </div> </div> <div class="form-group"> <label class="form-label">Data</label> <input type="date" id="cd-date" class="form-input" value="${todayStr}"> </div> <div class="form-group"> <label class="form-label">Note (opzionale)</label> <input type="text" id="cd-notes" class="form-input" placeholder="Es. al parco, in compagnia, ritmo lento"> </div> <button class="btn" onclick="saveCardio()">Salva attività</button> <div class="spacer"></div> ${todayCardio.length ? `
<div style="font-size:11px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600;margin-bottom:8px">Già registrato oggi</div>
<div class="list">
${todayCardio.map(c => `<div class="row" style="cursor:default"> <div class="row-main"> <div class="row-title">${c.activity}</div> <div class="row-sub">${c.duration} min${c.distance?' · '+c.distance+' km':''}</div> </div> <button onclick="deleteCardio('${c.date}')" style="background:none;border:none;color:var(--danger);cursor:pointer;font-family:inherit;font-size:13px">Elimina</button> </div>`).join(’’)}
</div>
`: ''} <div class="spacer"></div>`;
openSheet(‘Logga cardio’, html);
}

function selectCardio(btn, name) {
document.querySelectorAll(’#cardio-chips .chip’).forEach(c => c.classList.remove(‘selected’));
btn.classList.add(‘selected’);
selectedCardioActivity = name;
}

let selectedIntensity = null;
function selectIntensity(btn) {
btn.parentElement.querySelectorAll(’.chip’).forEach(c => c.classList.remove(‘selected’));
btn.classList.add(‘selected’);
selectedIntensity = btn.dataset.int;
}

function saveCardio() {
if (!selectedCardioActivity) { toast(‘Scegli un'attività’); return; }
const duration = parseInt($(’#cd-duration’).value);
if (!duration || duration < 1) { toast(‘Inserisci la durata’); return; }
const distance = parseFloat($(’#cd-distance’).value) || null;
const dateInput = $(’#cd-date’).value;
const date = new Date(dateInput).toISOString();
const notes = $(’#cd-notes’).value.trim();
State.data.cardioLog.push({
date, activity: selectedCardioActivity, duration, distance,
intensity: selectedIntensity, notes
});
State.save();
closeSheet();
toast(‘Cardio registrato’);
renderView(‘today’);
renderView(‘progress’);
}

function deleteCardio(dateIso) {
if (!confirm(‘Eliminare questa attività?’)) return;
State.data.cardioLog = State.data.cardioLog.filter(c => c.date !== dateIso);
State.save();
closeSheet();
toast(‘Eliminata’);
renderView(‘today’);
renderView(‘progress’);
}

// ── Protein log ─────────────────────────────────────────────────────
const PROTEIN_REFERENCES = [
[‘Petto pollo (100g)’, 31],
[‘Tonno in scatola (100g)’, 25],
[‘Uovo intero’, 6],
[‘Albume (1)’, 4],
[‘Fesa tacchino (100g)’, 24],
[‘Bresaola (50g)’, 16],
[‘Manzo magro (100g)’, 26],
[‘Salmone (100g)’, 22],
[‘Whey scoop (30g)’, 24],
[‘Yogurt greco (170g)’, 17],
[‘Skyr (150g)’, 17],
[‘Ricotta (100g)’, 11],
[‘Mozzarella (100g)’, 18],
[‘Parmigiano (30g)’, 11],
[‘Lenticchie cotte (100g)’, 9],
[‘Ceci cotti (100g)’, 9],
[‘Tofu (100g)’, 14]
];

function openProteinSheet() {
const target = State.data.profile.proteinTarget || 144;
const todayK = todayKey();
const todayG = State.data.proteinLog[todayK] || 0;
const remaining = Math.max(0, target - todayG);

// Last 7 days for chart
const days = [];
for (let i = 6; i >= 0; i–) {
const d = new Date(); d.setDate(d.getDate()-i);
const k = d.toISOString().slice(0,10);
days.push({ date: d, value: State.data.proteinLog[k] || 0 });
}
const avg = days.reduce((s,d) => s + d.value, 0) / 7;

const html = `
<div style="text-align:center;margin-bottom:14px">
<div style="font-size:36px;font-weight:700;letter-spacing:-0.8px">${todayG}<span style="font-size:18px;color:var(--text-secondary);font-weight:500">g / ${target}g</span></div>
<div style="font-size:13px;color:${todayG>=target?'var(--accent)':'var(--text-secondary)'};margin-top:2px;font-weight:500">${todayG>=target?‘✓ Obiettivo raggiunto’:’-’+remaining+‘g al target’}</div>
</div>

```
<div class="form-group">
  <label class="form-label">Aggiungi rapidamente</label>
  <div class="chips">
    <button class="chip" onclick="addProtein(15)">+15g</button>
    <button class="chip" onclick="addProtein(20)">+20g</button>
    <button class="chip" onclick="addProtein(25)">+25g</button>
    <button class="chip" onclick="addProtein(30)">+30g</button>
    <button class="chip" onclick="addProtein(40)">+40g</button>
    <button class="chip" onclick="addProtein(50)">+50g</button>
  </div>
</div>

<div class="form-group">
  <label class="form-label">Importo personalizzato (g)</label>
  <div style="display:flex;gap:8px">
    <input type="number" inputmode="numeric" id="pr-custom" class="form-input" placeholder="0" style="flex:1">
    <button class="btn compact" onclick="addProteinCustom()" style="flex:0 0 auto">Aggiungi</button>
  </div>
</div>

${todayG > 0 ? `<button class="btn ghost" onclick="resetTodayProtein()">Azzera oggi</button>` : ''}

<div class="section-title" style="margin-top:24px">Ultimi 7 giorni</div>
<div class="card" style="padding:14px">
  <div style="display:flex;justify-content:space-between;margin-bottom:10px;font-size:12px;color:var(--text-secondary)">
    <span>Media: <strong style="color:var(--text)">${avg.toFixed(0)}g/giorno</strong></span>
    <span>Target: <strong style="color:var(--text)">${target}g</strong></span>
  </div>
  <div style="display:flex;gap:6px;align-items:flex-end;height:70px">
    ${days.map(d => {
      const h = Math.min(100, (d.value / target) * 100);
      const reached = d.value >= target;
      return `<div style="flex:1;display:flex;flex-direction:column;align-items:center;justify-content:flex-end;gap:4px">
        <div style="font-size:9px;color:var(--text-tertiary);font-weight:600">${d.value}</div>
        <div style="width:100%;background:${reached?'var(--accent)':'var(--info)'};opacity:${d.value>0?'1':'0.2'};height:${Math.max(2,h*0.5)}px;border-radius:3px;transition:height 0.3s"></div>
        <div style="font-size:10px;color:var(--text-tertiary);text-transform:uppercase">${DOW_INITIAL[d.date.getDay()]}</div>
      </div>`;
    }).join('')}
  </div>
</div>

<div class="section-title">Riferimenti rapidi</div>
<div class="card">
  <div style="font-size:13px;color:var(--text-secondary);padding:12px 14px 6px">Tap per aggiungere ai grammi di oggi</div>
  <div class="list" style="border:none;background:transparent">
    ${PROTEIN_REFERENCES.map(([name, g]) => `
      <button class="row" onclick="addProtein(${g})">
        <div class="row-main">
          <div class="row-title" style="font-size:14px">${name}</div>
        </div>
        <div style="font-size:14px;font-weight:700;color:var(--info)">+${g}g</div>
      </button>
    `).join('')}
  </div>
</div>
<div class="spacer"></div>
```

`;
openSheet(‘Proteine’, html);
}

function addProtein(grams) {
const k = todayKey();
State.data.proteinLog[k] = (State.data.proteinLog[k] || 0) + grams;
State.save();
toast(`+${grams}g · totale ${State.data.proteinLog[k]}g`);
// Re-render the sheet to update display
openProteinSheet();
renderView(‘today’);
}

function addProteinCustom() {
const v = parseInt($(’#pr-custom’).value);
if (!v || v < 1 || v > 500) { toast(‘Inserisci un valore valido’); return; }
addProtein(v);
}

function resetTodayProtein() {
if (!confirm(‘Azzerare i grammi di proteine di oggi?’)) return;
delete State.data.proteinLog[todayKey()];
State.save();
toast(‘Azzerato’);
openProteinSheet();
renderView(‘today’);
}

// ── Warmup ──────────────────────────────────────────────────────────
let warmupChecked = [];
let warmupTimer = null;
let warmupTimerEnd = null;

function openWarmupSheet(workoutId) {
const routine = getWarmupForWorkout(workoutId);
warmupChecked = new Array(routine.items.length).fill(false);
const html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> ${routine.duration} di mobilità per preparare il corpo. <strong style="color:var(--text)">Riduci il rischio infortunio</strong> e aumenti la performance del primo composto. </p> <div class="spacer"></div> <div id="warmup-list"> ${routine.items.map((item, i) =>`
<div class="warmup-item" onclick="toggleWarmup(${i})" id="warmup-item-${i}">
<div class="warmup-check">
<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round"><polyline points="5,12 10,17 19,7"/></svg>
</div>
<div class="warmup-info">
<div class="warmup-name">${item.name}</div>
<div class="warmup-detail">${item.detail}</div>
</div>
${item.timer ? `<button onclick="event.stopPropagation();startWarmupTimer(${item.timer})" style="background:var(--surface-2);border:1px solid var(--border);color:var(--text);padding:6px 10px;border-radius:8px;font-family:inherit;font-size:12px;font-weight:600;cursor:pointer">⏱ ${Math.round(item.timer/60)}m</button>` : ‘’}
</div>
`).join('')} </div> <div class="spacer"></div> <button class="btn" onclick="finishWarmup('${workoutId}')">Inizia allenamento</button> <div class="spacer-sm"></div> <button class="btn ghost" onclick="finishWarmup('${workoutId}', true)">Salta riscaldamento</button> <div class="spacer-sm"></div> <p style="font-size:11px;color:var(--text-tertiary);text-align:center">Puoi disattivare il riscaldamento in Profilo</p> <div class="spacer"></div> `;
openSheet(routine.name, html);
}

function toggleWarmup(idx) {
warmupChecked[idx] = !warmupChecked[idx];
const el = document.getElementById(‘warmup-item-’+idx);
if (el) el.classList.toggle(‘done’, warmupChecked[idx]);
if (navigator.vibrate) navigator.vibrate(20);
}

function startWarmupTimer(seconds) {
if (warmupTimer) clearInterval(warmupTimer);
warmupTimerEnd = Date.now() + seconds*1000;
// Reuse rest timer UI
showRestTimer();
const el = document.querySelector(’.rest-timer .label’);
if (el) el.textContent = ‘Riscaldamento’;
warmupTimer = setInterval(() => {
const remaining = Math.max(0, Math.ceil((warmupTimerEnd - Date.now())/1000));
const t = document.querySelector(’.rest-timer .time’);
if (t) t.textContent = fmtTime(remaining);
if (remaining <= 0) {
clearInterval(warmupTimer); warmupTimer = null;
stopRestTimer();
if (navigator.vibrate) navigator.vibrate([100,50,100]);
toast(‘Tempo finito’);
}
}, 200);
}

function finishWarmup(workoutId, skipped) {
if (warmupTimer) { clearInterval(warmupTimer); warmupTimer = null; stopRestTimer(); }
closeSheet();
// Now actually start the workout
setTimeout(() => startWorkout(workoutId, true), 250);
if (!skipped && warmupChecked.filter(x => x).length > 0) {
toast(`Riscaldamento ✓ ${warmupChecked.filter(x => x).length}/${warmupChecked.length}`);
}
}

// ── Photo comparison ────────────────────────────────────────────────
let comparePhotoA = null;
let comparePhotoB = null;

async function openCompareOverlay() {
if (!allPhotos.length) {
allPhotos = await PhotoDB.getAll();
}
if (allPhotos.length < 2) {
toast(‘Servono almeno 2 foto’);
return;
}
// Auto-select: oldest as A, newest as B (same pose if possible)
const sorted = allPhotos.slice().sort((a,b) => new Date(a.date) - new Date(b.date));
comparePhotoA = sorted[0];
// Try to find newest with same pose as A
const sameposeNewer = sorted.slice().reverse().find(p => p.pose === comparePhotoA.pose && p.id !== comparePhotoA.id);
comparePhotoB = sameposeNewer || sorted[sorted.length-1];

$(’#compare-overlay’).classList.add(‘open’);
renderCompareBody();
}

function renderCompareBody() {
if (!comparePhotoA || !comparePhotoB) return;
const dA = new Date(comparePhotoA.date);
const dB = new Date(comparePhotoB.date);
const daysDiff = Math.round((dB - dA) / (24*60*60*1000));

// Body weight delta if available
const closestWeight = (date) => {
const log = State.data.weightLog.slice().sort((a,b) => Math.abs(new Date(a.date) - new Date(date)) - Math.abs(new Date(b.date) - new Date(date)));
return log[0] ? log[0].value : null;
};
const wA = closestWeight(comparePhotoA.date);
const wB = closestWeight(comparePhotoB.date);
const weightDelta = (wA != null && wB != null) ? (wB - wA) : null;

const allSorted = allPhotos.slice().sort((a,b) => new Date(a.date) - new Date(b.date));

const html = `
<div class="compare-pair">
<div class="compare-pane">
<img src="${comparePhotoA.dataUrl}" alt="">
<div class="meta">
<div class="d">${fmtDate(comparePhotoA.date)} · ${dA.getFullYear()}</div>
<div class="p">${comparePhotoA.pose}${wA?’ · ‘+wA.toFixed(1)+‘kg’:’’}</div>
</div>
</div>
<div class="compare-pane">
<img src="${comparePhotoB.dataUrl}" alt="">
<div class="meta">
<div class="d">${fmtDate(comparePhotoB.date)} · ${dB.getFullYear()}</div>
<div class="p">${comparePhotoB.pose}${wB?’ · ‘+wB.toFixed(1)+‘kg’:’’}</div>
</div>
</div>
</div>

```
<div class="compare-delta">
  <div class="big">${Math.abs(daysDiff)} giorni</div>
  <div class="sub">tra le due foto</div>
  ${weightDelta != null ? `<div style="margin-top:8px;font-size:14px;font-weight:600;color:${weightDelta<0?'var(--accent)':weightDelta>0?'var(--warning)':'var(--text-secondary)'}">${weightDelta>0?'+':''}${weightDelta.toFixed(1)} kg</div>` : ''}
</div>

<div style="font-size:11px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600;margin:18px 0 6px">Foto A — prima</div>
<div class="compare-thumbs">
  ${allSorted.map(p => `
    <div class="compare-thumb ${p.id===comparePhotoA.id?'selected':''}" onclick="selectComparePhoto('A', ${p.id})">
      <img src="${p.dataUrl}" alt="">
      <div class="lbl">${p.pose.charAt(0).toUpperCase()}</div>
    </div>
  `).join('')}
</div>

<div style="font-size:11px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600;margin:18px 0 6px">Foto B — dopo</div>
<div class="compare-thumbs">
  ${allSorted.map(p => `
    <div class="compare-thumb ${p.id===comparePhotoB.id?'selected':''}" onclick="selectComparePhoto('B', ${p.id})">
      <img src="${p.dataUrl}" alt="">
      <div class="lbl">${p.pose.charAt(0).toUpperCase()}</div>
    </div>
  `).join('')}
</div>

<div style="font-size:12px;color:var(--text-tertiary);text-align:center;margin:18px 0;line-height:1.5">
  Tip: scegli foto della stessa posa per il confronto più accurato
</div>
```

`;
$(’#compare-body’).innerHTML = html;
}

function selectComparePhoto(slot, id) {
const p = allPhotos.find(x => x.id === id);
if (!p) return;
if (slot === ‘A’) comparePhotoA = p;
else comparePhotoB = p;
renderCompareBody();
}

document.getElementById(‘compare-close’).onclick = () => {
document.getElementById(‘compare-overlay’).classList.remove(‘open’);
};

// ════════════════════════════════════════════════════════════════════
// V4 — Editing exercise log entries (per-exercise progress tracking)
// ════════════════════════════════════════════════════════════════════

let editingExerciseId = null;
let editingEntryDate = null;
let editingSetCounter = 0;

function openExerciseLogEditSheet(exId, entryDateIso) {
const def = Object.values(PROGRAM).flatMap(d => d.exercises).find(e => e.id === exId)
|| State.data.customExercises.find(e => e.id === exId);
if (!def) { toast(‘Esercizio non trovato’); return; }

const log = State.data.exerciseLog[exId] || [];
const editing = entryDateIso ? log.find(e => e.date === entryDateIso) : null;

editingExerciseId = exId;
editingEntryDate = entryDateIso || null;

let initSets;
if (editing) {
initSets = editing.sets.map(s => ({ weight: s.weight, reps: s.reps }));
} else {
const sw = suggestNextWeight(exId, def);
const startWeight = sw ? sw.weight : (getLastWeight(exId) || ‘’);
const numSets = def.sets || 3;
initSets = Array.from({length: numSets}, () => ({ weight: startWeight, reps: ‘’ }));
}
editingSetCounter = initSets.length;

const dateStr = editing ? new Date(editing.date).toISOString().slice(0,10) : todayKey();

const html = `
<div style="margin-bottom:6px">
<div style="font-size:12px;color:var(--accent);font-weight:600;text-transform:uppercase;letter-spacing:0.6px">${def.muscle||’’}</div>
<div style="font-size:18px;font-weight:700;letter-spacing:-0.2px;margin-top:2px">${def.name}</div>
<div style="font-size:13px;color:var(--text-secondary);margin-top:2px">Target: ${def.target||’’}</div>
</div>

```
<div class="form-group" style="margin-top:14px">
  <label class="form-label">Data sessione</label>
  <input type="date" id="le-date" class="form-input" value="${dateStr}">
</div>

<div class="form-group">
  <label class="form-label">Serie eseguite</label>
  <div class="sets-header" style="margin-bottom:6px">
    <div>Serie</div>
    <div>Peso (kg)</div>
    <div>Reps</div>
    <div></div>
  </div>
  <div id="le-sets-container">
    ${initSets.map((s, i) => renderLogSetRow(i, s.weight, s.reps)).join('')}
  </div>
  <button onclick="addLogSetRow()" style="margin-top:10px;width:100%;padding:11px;background:transparent;border:1px dashed var(--border-strong);border-radius:10px;color:var(--text-secondary);cursor:pointer;font-family:inherit;font-size:13px;font-weight:600;display:flex;align-items:center;justify-content:center;gap:6px">
    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
    Aggiungi serie
  </button>
</div>

<button class="btn" onclick="saveExerciseLogEntry()">${editing ? 'Salva modifiche' : 'Salva sessione'}</button>
<div class="spacer-sm"></div>
${editing ? `<button class="btn danger" onclick="deleteExerciseLogEntry()">Elimina questa sessione</button><div class="spacer-sm"></div>` : ''}
<button class="btn ghost" onclick="openExerciseDetailSheet('${exId}')">${editing ? 'Annulla' : 'Indietro'}</button>
<div class="spacer"></div>
```

`;
openSheet(editing ? ‘Modifica sessione’ : ‘Aggiungi sessione’, html);
}

function renderLogSetRow(idx, weight, reps) {
return `<div class="set-row" data-set-row="${idx}"> <div class="set-num">${idx+1}</div> <input type="number" inputmode="decimal" placeholder="0" value="${weight!=null && weight!==''?weight:''}" data-le-weight step="0.5"> <input type="number" inputmode="numeric" placeholder="0" value="${reps!=null && reps!==''?reps:''}" data-le-reps> <button onclick="removeLogSetRow(${idx})" style="background:var(--surface-2);border:1px solid var(--border);color:var(--danger);width:40px;height:40px;border-radius:10px;cursor:pointer;font-size:20px;line-height:1;font-family:inherit">×</button> </div>`;
}

function addLogSetRow() {
const container = document.getElementById(‘le-sets-container’);
if (!container) return;
// Inherit weight from last existing row
const rows = container.querySelectorAll(’[data-set-row]’);
let lastWeight = ‘’;
if (rows.length > 0) {
const lastInput = rows[rows.length-1].querySelector(’[data-le-weight]’);
if (lastInput) lastWeight = lastInput.value;
}
const idx = editingSetCounter++;
const div = document.createElement(‘div’);
div.innerHTML = renderLogSetRow(idx, lastWeight, ‘’);
container.appendChild(div.firstElementChild);
renumberLogRows();
}

function removeLogSetRow(idx) {
const row = document.querySelector(`[data-set-row="${idx}"]`);
if (!row) return;
// Don’t allow removing the last row
const container = document.getElementById(‘le-sets-container’);
if (container && container.children.length <= 1) {
toast(‘Almeno una serie è obbligatoria’);
return;
}
row.remove();
renumberLogRows();
}

function renumberLogRows() {
const container = document.getElementById(‘le-sets-container’);
if (!container) return;
container.querySelectorAll(’[data-set-row]’).forEach((r, i) => {
const num = r.querySelector(’.set-num’);
if (num) num.textContent = i+1;
});
}

function saveExerciseLogEntry() {
const exId = editingExerciseId;
if (!exId) return;
const dateStr = $(’#le-date’).value;
if (!dateStr) { toast(‘Data obbligatoria’); return; }
const dateIso = new Date(dateStr + ‘T12:00:00’).toISOString();

const sets = [];
document.querySelectorAll(’#le-sets-container [data-set-row]’).forEach(row => {
const w = parseFloat(row.querySelector(’[data-le-weight]’).value);
const r = parseInt(row.querySelector(’[data-le-reps]’).value);
if (!isNaN(w) && w >= 0 && !isNaN(r) && r > 0) {
sets.push({ weight: w, reps: r });
}
});

if (sets.length === 0) {
toast(‘Inserisci almeno una serie completa (peso + reps)’);
return;
}

const topSet = Math.max(…sets.map(s => s.weight));
const newEntry = { date: dateIso, sets, topSet };

if (!State.data.exerciseLog[exId]) State.data.exerciseLog[exId] = [];

if (editingEntryDate) {
// Update existing entry
const idx = State.data.exerciseLog[exId].findIndex(e => e.date === editingEntryDate);
if (idx >= 0) {
State.data.exerciseLog[exId][idx] = newEntry;
// Try to also update matching entry in workout history if exists
State.data.workoutHistory.forEach(w => {
if (w.date === editingEntryDate && w.exercises) {
const we = w.exercises.find(x => x.id === exId);
if (we) {
we.sets = sets.map(s => ({ …s, completed: true }));
}
}
});
}
} else {
State.data.exerciseLog[exId].push(newEntry);
}

// Sort by date ascending
State.data.exerciseLog[exId].sort((a,b) => new Date(a.date) - new Date(b.date));
State.save();
toast(editingEntryDate ? ‘Sessione aggiornata ✓’ : ‘Sessione registrata ✓’);
// Re-open detail to show updated chart
setTimeout(() => openExerciseDetailSheet(exId), 200);
}

function deleteExerciseLogEntry() {
if (!confirm(‘Eliminare questa sessione di allenamento per questo esercizio?’)) return;
const exId = editingExerciseId;
const date = editingEntryDate;
if (!exId || !date) return;
State.data.exerciseLog[exId] = (State.data.exerciseLog[exId] || []).filter(e => e.date !== date);
State.save();
toast(‘Sessione eliminata’);
setTimeout(() => openExerciseDetailSheet(exId), 200);
}

// ── Quick log: log entire past workout day at once ──────────────────
function openQuickLogSheet(workoutId) {
const w = PROGRAM[workoutId];
if (!w) return;
const todayStr = todayKey();

const html = `
<div style="margin-bottom:6px">
<div style="font-size:12px;color:var(--accent);font-weight:600;text-transform:uppercase;letter-spacing:0.6px">Logga sessione passata</div>
<div style="font-size:20px;font-weight:700;letter-spacing:-0.4px;margin-top:2px">${w.name}</div>
<div style="font-size:13px;color:var(--text-secondary);margin-top:4px">Inserisci solo gli esercizi che hai effettivamente fatto. Lascia vuoto quelli saltati.</div>
</div>

```
<div class="form-group" style="margin-top:14px">
  <label class="form-label">Data sessione</label>
  <input type="date" id="ql-date" class="form-input" value="${todayStr}">
</div>

<div id="ql-exercises">
  ${w.exercises.map((ex, exIdx) => {
    const sw = suggestNextWeight(ex.id, ex);
    const startWeight = sw ? sw.weight : (getLastWeight(ex.id) || '');
    const numSets = ex.sets || 3;
    return `
      <div class="exercise-item" style="margin-bottom:10px" data-ql-ex="${ex.id}">
        <div class="ex-header">
          <div class="ex-num">${exIdx+1}</div>
          <div style="flex:1;min-width:0">
            <div class="ex-name" style="font-size:15px">${ex.name}</div>
            <div class="ex-target">${ex.target} · ${ex.muscle}</div>
          </div>
        </div>
        <div class="sets-grid" style="margin-top:10px">
          <div class="sets-header">
            <div>S.</div>
            <div>Peso</div>
            <div>Reps</div>
            <div></div>
          </div>
          ${Array.from({length: numSets}, (_, i) => `
            <div class="set-row" data-ql-set="${i}">
              <div class="set-num">${i+1}</div>
              <input type="number" inputmode="decimal" placeholder="${startWeight||0}" value="" data-ql-weight step="0.5">
              <input type="number" inputmode="numeric" placeholder="${(ex.reps||'').split('-')[0]||0}" value="" data-ql-reps>
              <div></div>
            </div>
          `).join('')}
        </div>
      </div>`;
  }).join('')}
</div>

<button class="btn" onclick="saveQuickLog('${workoutId}')">Salva sessione</button>
<div class="spacer-sm"></div>
<button class="btn ghost" onclick="closeSheet()">Annulla</button>
<div class="spacer"></div>
```

`;
openSheet(‘Logga sessione’, html);
}

function saveQuickLog(workoutId) {
const w = PROGRAM[workoutId];
if (!w) return;
const dateStr = $(’#ql-date’).value;
if (!dateStr) { toast(‘Data obbligatoria’); return; }
const dateIso = new Date(dateStr + ‘T12:00:00’).toISOString();

let totalVol = 0;
let exDone = 0;
const sessionExercises = [];

document.querySelectorAll(’[data-ql-ex]’).forEach(exEl => {
const exId = exEl.dataset.qlEx;
const def = w.exercises.find(e => e.id === exId);
if (!def) return;
const sets = [];
exEl.querySelectorAll(’[data-ql-set]’).forEach(setEl => {
const wv = parseFloat(setEl.querySelector(’[data-ql-weight]’).value);
const rv = parseInt(setEl.querySelector(’[data-ql-reps]’).value);
if (!isNaN(wv) && wv >= 0 && !isNaN(rv) && rv > 0) {
sets.push({ weight: wv, reps: rv });
}
});
if (sets.length === 0) return;

```
exDone++;
const topSet = Math.max(...sets.map(s => s.weight));
sets.forEach(s => { totalVol += s.weight * s.reps; });

// Save to exerciseLog
if (!State.data.exerciseLog[exId]) State.data.exerciseLog[exId] = [];
// If a same-date entry already exists for this exercise, replace it
const existingIdx = State.data.exerciseLog[exId].findIndex(e => sameDay(e.date, dateIso));
const entry = { date: dateIso, sets, topSet };
if (existingIdx >= 0) {
  State.data.exerciseLog[exId][existingIdx] = entry;
} else {
  State.data.exerciseLog[exId].push(entry);
}
State.data.exerciseLog[exId].sort((a,b) => new Date(a.date) - new Date(b.date));

sessionExercises.push({
  id: exId,
  name: def.name,
  custom: false,
  sets: sets.map(s => ({ ...s, completed: true }))
});
```

});

if (exDone === 0) {
toast(‘Inserisci almeno una serie completa’);
return;
}

// Add to workout history
State.data.workoutHistory.push({
date: dateIso,
workoutId: w.id,
workoutName: w.name,
duration: 0, // unknown for past sessions
exercisesCompleted: exDone,
exercisesTotal: w.exercises.length,
totalVolume: totalVol,
note: ‘Sessione loggata a posteriori’,
exercises: sessionExercises
});
State.data.workoutHistory.sort((a,b) => new Date(a.date) - new Date(b.date));
State.save();
closeSheet();
toast(`Sessione salvata: ${exDone} esercizi · ${totalVol.toFixed(0)} kg vol.`);
renderView(‘today’);
renderView(‘program’);
renderView(‘progress’);
}

// ════════════════════════════════════════════════════════════════════
// V5 — Program editing (full week customization)
// ════════════════════════════════════════════════════════════════════

// ── Overview: pick a day to edit ────────────────────────────────────
function openProgramOverviewSheet() {
const days = DAY_ORDER.map(id => PROGRAM[id]).filter(Boolean);
const html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> Modifica i giorni del tuo programma. Puoi <strong style="color:var(--text)">aggiungere, rimuovere, riordinare e sostituire</strong> esercizi, modificare nome, focus, giorno della settimana, durata, serie e ripetizioni. </p> <div class="spacer"></div> <div class="list"> ${days.map(d =>`
<button class="row" onclick="openProgramDayEditSheet('${d.id}')">
<div class="row-icon" style="background:rgba(48,209,88,0.15);color:var(--accent)">
<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><rect x="6" y="10" width="3" height="4" rx="1"/><rect x="15" y="10" width="3" height="4" rx="1"/><line x1="9" y1="12" x2="15" y2="12"/></svg>
</div>
<div class="row-main">
<div class="row-title">${d.name}</div>
<div class="row-sub">${DOW_NAMES[d.dow]} · ${d.exercises.length} esercizi · ${d.focus}</div>
</div>
<span class="chev">›</span>
</button>
`).join('')} </div> <div class="spacer"></div> <p style="font-size:12px;color:var(--text-tertiary);text-align:center;line-height:1.5"> Suggerimento: lo storico delle tue sessioni resta intatto anche se modifichi il programma. </p> <div class="spacer"></div> `;
openSheet(‘Modifica programma’, html);
}

// ── Day edit sheet ──────────────────────────────────────────────────
function openProgramDayEditSheet(dayId) {
const day = PROGRAM[dayId];
if (!day) return;
const html = `
<div style="margin-bottom:14px">
<div style="font-size:12px;color:var(--accent);font-weight:600;text-transform:uppercase;letter-spacing:0.6px">${DOW_NAMES[day.dow]}</div>
<div style="font-size:20px;font-weight:700;letter-spacing:-0.4px;margin-top:2px">${day.name}</div>
<div style="font-size:13px;color:var(--text-secondary);margin-top:4px">${day.focus}</div>
</div>

```
<button class="btn ghost compact" onclick="openDaySettingsSheet('${dayId}')" style="width:100%;margin-bottom:14px">
  <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
  Impostazioni giorno (nome, dow, focus, durata)
</button>

<div style="font-size:12px;color:var(--text-secondary);text-transform:uppercase;letter-spacing:0.5px;font-weight:600;margin:0 4px 8px">Esercizi (${day.exercises.length})</div>

<div id="day-exercises-list">
  ${day.exercises.map((ex, i) => renderEditorExRow(dayId, i, ex, day.exercises.length)).join('')}
</div>

<button class="add-extra-btn" onclick="openAddExerciseToDaySheet('${dayId}')">
  <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="12" cy="12" r="9"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="16"/></svg>
  Aggiungi esercizio a questo giorno
</button>

<div class="spacer"></div>
<button class="btn ghost" onclick="openProgramOverviewSheet()">← Torna ai giorni</button>
<div class="spacer"></div>
```

`; openSheet(`Modifica ${day.name}`, html);
}

function renderEditorExRow(dayId, idx, ex, total) {
return `<div class="editor-ex-row"> <div class="reorder"> <button onclick="moveExerciseUp('${dayId}', ${idx})" ${idx===0?'disabled':''} aria-label="Su"> <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><polyline points="18,15 12,9 6,15"/></svg> </button> <button onclick="moveExerciseDown('${dayId}', ${idx})" ${idx===total-1?'disabled':''} aria-label="Giù"> <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><polyline points="6,9 12,15 18,9"/></svg> </button> </div> <div class="ex-info"> <div class="nm">${idx+1}. ${ex.name}</div> <div class="det">${ex.target} · ${ex.muscle}</div> </div> <div class="actions"> <button onclick="openEditExerciseInDaySheet('${dayId}', ${idx})" aria-label="Modifica"> <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg> </button> <button class="del" onclick="deleteExerciseFromDay('${dayId}', ${idx})" aria-label="Elimina"> <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><polyline points="3,6 5,6 21,6"/><path d="M19 6l-2 14H7L5 6"/></svg> </button> </div> </div>`;
}

function refreshDayEditor(dayId) {
const day = PROGRAM[dayId];
if (!day) return;
const list = document.getElementById(‘day-exercises-list’);
if (list) {
list.innerHTML = day.exercises.map((ex, i) => renderEditorExRow(dayId, i, ex, day.exercises.length)).join(’’);
}
// Also refresh any header showing exercise count
const hdr = document.querySelector(’.sheet-body div[style*=“Esercizi”]’);
}

function moveExerciseUp(dayId, idx) {
const day = State.data.userProgram[dayId];
if (!day || idx <= 0) return;
[day.exercises[idx-1], day.exercises[idx]] = [day.exercises[idx], day.exercises[idx-1]];
State.save();
// Re-open the whole sheet to refresh order numbers cleanly
openProgramDayEditSheet(dayId);
}

function moveExerciseDown(dayId, idx) {
const day = State.data.userProgram[dayId];
if (!day || idx >= day.exercises.length-1) return;
[day.exercises[idx], day.exercises[idx+1]] = [day.exercises[idx+1], day.exercises[idx]];
State.save();
openProgramDayEditSheet(dayId);
}

function deleteExerciseFromDay(dayId, idx) {
const day = State.data.userProgram[dayId];
if (!day) return;
const ex = day.exercises[idx];
if (!ex) return;
if (!confirm(`Rimuovere "${ex.name}" da ${day.name}? Lo storico delle sessioni passate per questo esercizio resterà.`)) return;
day.exercises.splice(idx, 1);
State.save();
toast(‘Esercizio rimosso’);
openProgramDayEditSheet(dayId);
renderView(‘program’);
}

// ── Day settings (name, dow, focus, duration) ──────────────────────
function openDaySettingsSheet(dayId) {
const day = PROGRAM[dayId];
if (!day) return;
const html = `<div class="form-group"> <label class="form-label">Nome del giorno</label> <input type="text" id="ds-name" class="form-input" value="${day.name}" placeholder="Es. Upper A"> </div> <div class="form-group"> <label class="form-label">Focus muscolare</label> <input type="text" id="ds-focus" class="form-input" value="${day.focus}" placeholder="Es. Petto · Schiena · Spalle"> </div> <div class="form-group"> <label class="form-label">Giorno della settimana</label> <select id="ds-dow" class="form-select"> <option value="1" ${day.dow===1?'selected':''}>Lunedì</option> <option value="2" ${day.dow===2?'selected':''}>Martedì</option> <option value="3" ${day.dow===3?'selected':''}>Mercoledì</option> <option value="4" ${day.dow===4?'selected':''}>Giovedì</option> <option value="5" ${day.dow===5?'selected':''}>Venerdì</option> <option value="6" ${day.dow===6?'selected':''}>Sabato</option> <option value="0" ${day.dow===0?'selected':''}>Domenica</option> </select> <div style="font-size:11px;color:var(--text-tertiary);margin-top:4px;padding:0 4px">Determina quando l'app ti mostra questo allenamento come "oggi"</div> </div> <div class="form-group"> <label class="form-label">Durata stimata (min)</label> <input type="number" inputmode="numeric" id="ds-duration" class="form-input" value="${day.duration}" min="20" max="180"> </div> <button class="btn" onclick="saveDaySettings('${dayId}')">Salva</button> <div class="spacer-sm"></div> <button class="btn ghost" onclick="openProgramDayEditSheet('${dayId}')">Annulla</button> <div class="spacer"></div>`;
openSheet(‘Impostazioni giorno’, html);
}

function saveDaySettings(dayId) {
const day = State.data.userProgram[dayId];
if (!day) return;
const name = $(’#ds-name’).value.trim();
const focus = $(’#ds-focus’).value.trim();
const dow = parseInt($(’#ds-dow’).value);
const duration = parseInt($(’#ds-duration’).value);
if (!name) { toast(‘Nome obbligatorio’); return; }

// Check if another day already uses this dow
const conflict = Object.values(State.data.userProgram).find(d => d.id !== dayId && d.dow === dow);
if (conflict) {
if (!confirm(`"${conflict.name}" è già assegnato a ${DOW_NAMES[dow]}. Continuare comunque? (avrai due allenamenti lo stesso giorno)`)) return;
}

day.name = name;
day.focus = focus || day.focus;
day.dow = dow;
if (!isNaN(duration) && duration > 0) day.duration = duration;
State.save();
toast(‘Impostazioni salvate’);
openProgramDayEditSheet(dayId);
renderView(‘program’);
renderView(‘today’);
}

// ── Add exercise to a day ───────────────────────────────────────────
function openAddExerciseToDaySheet(dayId) {
const customs = State.data.customExercises;
let html = `<p class="text-secondary" style="font-size:13px;line-height:1.5"> Scegli un esercizio dalla libreria, dai tuoi personalizzati, oppure crealo nuovo. </p> <div class="spacer"></div> <button class="btn" onclick="openCreateNewExerciseInDay('${dayId}')"> <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="12" cy="12" r="9"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg> Crea esercizio nuovo </button> <div class="spacer"></div>`;
if (customs.length) {
html += `<div class="muscle-section"> <div class="head">I tuoi esercizi personalizzati</div> <div class="list"> ${customs.map(c =>`
<button class="row" onclick="addCustomExerciseToDay('${dayId}', '${c.id}')">
<div class="row-main">
<div class="row-title">${c.name}</div>
<div class="row-sub">${c.sets} × ${c.reps} · ${c.muscle}</div>
</div>
<span class="chev">›</span>
</button>
`).join('')} </div> </div>`;
}
Object.entries(EXERCISE_LIBRARY).forEach(([cat, list]) => {
html += `<div class="muscle-section"> <div class="head">${cat}</div> <div class="list"> ${list.map((a, i) =>`
<button class="row" onclick="addLibExerciseToDay('${dayId}', '${cat}', ${i})">
<div class="row-main">
<div class="row-title">${a.name}</div>
<div class="row-sub">${a.sets} × ${a.reps}</div>
</div>
<span class="chev">›</span>
</button>
`).join('')} </div> </div>`;
});
html += `<div class="spacer"></div><button class="btn ghost" onclick="openProgramDayEditSheet('${dayId}')">Annulla</button><div class="spacer"></div>`;
openSheet(‘Aggiungi esercizio’, html);
}

function genExerciseId() {
return ‘user_’ + Date.now() + ‘_’ + Math.random().toString(36).slice(2, 7);
}

function addLibExerciseToDay(dayId, cat, idx) {
const a = EXERCISE_LIBRARY[cat][idx];
if (!a) return;
const day = State.data.userProgram[dayId];
if (!day) return;
day.exercises.push({
id: genExerciseId(),
name: a.name,
target: `${a.sets} × ${a.reps}`,
sets: a.sets,
reps: a.reps,
muscle: cat,
tip: ‘’
});
State.save();
toast(‘Esercizio aggiunto’);
openProgramDayEditSheet(dayId);
}

function addCustomExerciseToDay(dayId, customId) {
const c = State.data.customExercises.find(e => e.id === customId);
if (!c) return;
const day = State.data.userProgram[dayId];
if (!day) return;
day.exercises.push({
id: genExerciseId(),
name: c.name,
target: c.target,
sets: c.sets,
reps: c.reps,
muscle: c.muscle,
tip: c.tip || ‘’
});
State.save();
toast(‘Esercizio aggiunto’);
openProgramDayEditSheet(dayId);
}

function openCreateNewExerciseInDay(dayId) {
openEditExerciseInDaySheet(dayId, -1); // -1 = new
}

// ── Edit/create exercise inside a day ──────────────────────────────
function openEditExerciseInDaySheet(dayId, exIdx) {
const day = PROGRAM[dayId];
if (!day) return;
const isNew = exIdx === -1;
const ex = isNew ? { name: ‘’, target: ‘3 × 8-10’, sets: 3, reps: ‘8-10’, muscle: ‘Petto’, tip: ‘’ } : day.exercises[exIdx];
if (!ex) return;
const muscles = Object.keys(EXERCISE_LIBRARY);

const html = `<div class="form-group"> <label class="form-label">Nome esercizio</label> <input type="text" id="ee-name" class="form-input" value="${ex.name}" placeholder="Es. Panca piana con bilanciere"> </div> <div class="form-group"> <label class="form-label">Gruppo muscolare</label> <select id="ee-muscle" class="form-select"> ${muscles.map(m =>`<option value=”${m}” ${ex.muscle===m?‘selected’:’’}>${m}</option>`).join('')} </select> </div> <div class="form-row"> <div class="form-group"> <label class="form-label">Serie</label> <input type="number" inputmode="numeric" id="ee-sets" class="form-input" value="${ex.sets}" min="1" max="10"> </div> <div class="form-group"> <label class="form-label">Reps</label> <input type="text" id="ee-reps" class="form-input" value="${ex.reps}" placeholder="Es. 8-10 o 45s"> </div> </div> <div class="form-group"> <label class="form-label">Tip / nota tecnica (opzionale)</label> <textarea id="ee-tip" class="form-textarea" placeholder="Es. Scapole retratte, gomiti a 45°">${ex.tip||''}</textarea> </div> <button class="btn" onclick="saveExerciseInDay('${dayId}', ${exIdx})">${isNew ? 'Aggiungi al giorno' : 'Salva modifiche'}</button> <div class="spacer-sm"></div> <button class="btn ghost" onclick="openProgramDayEditSheet('${dayId}')">Annulla</button> <div class="spacer"></div> `;
openSheet(isNew ? ‘Nuovo esercizio’ : ‘Modifica esercizio’, html, () => {
if (isNew) $(’#ee-name’).focus();
});
}

function saveExerciseInDay(dayId, exIdx) {
const day = State.data.userProgram[dayId];
if (!day) return;
const name = $(’#ee-name’).value.trim();
const muscle = $(’#ee-muscle’).value;
const sets = parseInt($(’#ee-sets’).value) || 3;
const reps = $(’#ee-reps’).value.trim() || ‘8-10’;
const tip = $(’#ee-tip’).value.trim();
if (!name) { toast(‘Nome obbligatorio’); return; }
const target = `${sets} × ${reps}`;

if (exIdx === -1) {
day.exercises.push({ id: genExerciseId(), name, muscle, sets, reps, target, tip });
toast(‘Esercizio aggiunto’);
} else {
const ex = day.exercises[exIdx];
if (!ex) return;
// Preserve id (so log history stays attached) but update everything else
Object.assign(ex, { name, muscle, sets, reps, target, tip });
toast(‘Esercizio aggiornato’);
}
State.save();
openProgramDayEditSheet(dayId);
}

// ── Reset program to default ───────────────────────────────────────
function resetProgramToDefault() {
if (!confirm(‘Ripristinare il programma predefinito Upper/Lower?\n\nIl tuo storico sessioni e PR rimarranno intatti, ma i giorni e gli esercizi torneranno a quelli originali.’)) return;
if (!confirm(‘Sei sicuro? Le modifiche al programma andranno perse.’)) return;
State.data.userProgram = JSON.parse(JSON.stringify(DEFAULT_PROGRAM));
State.save();
toast(‘Programma ripristinato’);
closeSheet();
renderView(‘program’);
renderView(‘today’);
}

// ── Initialize ──────────────────────────────────────────────────────
async function init() {
State.load();
await PhotoDB.init();
allPhotos = await PhotoDB.getAll();
switchTab(‘today’);
}

// PWA install hint (one-shot)
if (!localStorage.getItem(‘gymcoach.installHintShown’)) {
setTimeout(() => {
if (!window.matchMedia(’(display-mode: standalone)’).matches && /iPhone|iPad/i.test(navigator.userAgent)) {
toast(‘Tocca Condividi → “Aggiungi a Home” per installare’);
localStorage.setItem(‘gymcoach.installHintShown’, ‘1’);
}
}, 2500);
}

document.addEventListener(‘DOMContentLoaded’, init);
if (document.readyState !== ‘loading’) init();

// Prevent accidental reload on pull-to-refresh
let touchStartY = 0;
document.addEventListener(‘touchstart’, (e) => {
touchStartY = e.touches[0].clientY;
}, { passive: true });
document.addEventListener(‘touchmove’, (e) => {
const view = document.querySelector(’.view.active’) || document.querySelector(’.workout-body’) || document.querySelector(’.sheet-body’);
if (view && view.scrollTop === 0 && e.touches[0].clientY > touchStartY) {
if (e.cancelable) e.preventDefault();
}
}, { passive: false });

</script>
</body>
</html>

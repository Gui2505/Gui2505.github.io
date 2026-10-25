<!doctype html>
<html lang="pt-BR" data-theme="dark">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Umax AI · Análise facial</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,600..800&family=Figtree:wght@400;500;600&display=swap" rel="stylesheet">
<script>try{document.documentElement.dataset.theme=localStorage.getItem('umax-theme')||'dark'}catch(e){}</script>
<style>
:root{--display:"Bricolage Grotesque",system-ui,sans-serif;--body:"Figtree",system-ui,-apple-system,sans-serif}
:root,[data-theme=dark]{color-scheme:dark;--paper:#0E0F1C;--card:#171829;--raise:#202139;--ink:#ECEDF7;--muted:#9A9DB8;--line:#2B2D47;--track:#2A2C47;--accent:#8F7BFF;--on-accent:#0E0F1C;--warn:#FF9A5C;--warnbg:#3A2417;--ok:#4ADE9A;--stage:#080913}
[data-theme=light]{color-scheme:light;--paper:#F2F1F8;--card:#fff;--raise:#F2F1F8;--ink:#15162B;--muted:#62657E;--line:#DEDFEA;--track:#E6E5F1;--accent:#5B3DF5;--on-accent:#fff;--warn:#C2410C;--warnbg:#FFF1E8;--ok:#0F7B4F;--stage:#14152A}
*{box-sizing:border-box}
body{margin:0;background:var(--paper);color:var(--ink);font:16px/1.55 var(--body);min-height:100vh}
button{font:inherit;color:inherit}
:focus-visible{outline:3px solid var(--accent);outline-offset:2px}
.hidden,[hidden]{display:none!important}
.top{width:min(100% - 32px,1120px);margin:0 auto;padding:22px 0 8px;display:flex;justify-content:space-between;align-items:center;gap:12px 20px;flex-wrap:wrap}
.brand{font:800 1.5rem/1 var(--display);letter-spacing:-.03em}
.brand i{font-style:normal;color:var(--accent)}
.privacy{color:var(--muted);font-size:.86rem;margin:0;flex:1 1 280px}
.app{width:min(100% - 32px,1120px);margin:14px auto 56px;display:grid;grid-template-columns:minmax(0,5fr) minmax(0,6fr);gap:24px;align-items:start}
.stage-col{position:sticky;top:16px}
.chip{border:1px solid var(--line);background:var(--card);border-radius:999px;padding:7px 14px;font-size:.88rem;font-weight:500;cursor:pointer;transition:background .15s}
.chip[aria-pressed=true]{background:var(--ink);border-color:var(--ink);color:var(--paper)}
.chip:disabled{opacity:.45;cursor:not-allowed}
.chip.done::after{content:" ✓";color:var(--ok);font-weight:700}
.chip.done[aria-pressed=true]::after{color:inherit}
.modes,.tabs,.tools{display:flex;gap:8px;flex-wrap:wrap}
.tabs{margin:14px 0 12px}.tools{margin-top:14px}
.callout{margin:10px 0 0;padding:10px 14px;border-radius:14px;font-size:.88rem;background:var(--raise);color:var(--muted)}
.callout.warn{background:var(--warnbg);color:var(--warn)}

.stage{position:relative;background:var(--stage);border-radius:28px;overflow:hidden;aspect-ratio:4/5;max-height:72vh;width:100%;display:grid;place-items:center}
.stage canvas{display:none;position:absolute;inset:0;width:100%;height:100%;object-fit:contain}
.stage.has-image canvas{display:block}
.stage input{position:absolute;opacity:0;width:1px;height:1px}
.drop{position:absolute;inset:14px;border:2px dashed rgba(255,255,255,.28);border-radius:20px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:6px;text-align:center;color:#fff;cursor:pointer;padding:20px;transition:background .2s,border-color .2s}
.drop:hover,.stage.dragging .drop,.stage:has(input:focus-visible) .drop{background:rgba(255,255,255,.07);border-color:#fff}
.drop b{font:700 1.3rem/1.2 var(--display);letter-spacing:-.02em}
.drop span{color:#B7B9D3;font-size:.9rem;max-width:34ch}
.stage.has-image .drop{display:none}
.sweep{position:absolute;left:0;right:0;top:0;height:30%;display:none;pointer-events:none;background:linear-gradient(180deg,transparent,rgba(140,120,255,.55));border-bottom:2px solid #CFC6FF}
.stage.scanning .sweep{display:block;animation:sweep 1.6s ease-in-out infinite}
@keyframes sweep{0%{transform:translateY(-100%)}100%{transform:translateY(340%)}}
.cta{width:100%;margin-top:14px;border:0;border-radius:16px;background:var(--accent);color:var(--on-accent);font:700 1.05rem var(--display);padding:15px;cursor:pointer;transition:transform .15s,filter .15s}
.cta:hover:not(:disabled){filter:brightness(1.1)}.cta:active:not(:disabled){transform:scale(.99)}
.cta:disabled{background:var(--track);color:var(--muted);cursor:not-allowed}
.status{margin:10px 2px 0;color:var(--muted);font-size:.9rem;min-height:1.4em}
.status.err{color:var(--warn)}

.report{background:var(--card);border:1px solid var(--line);border-radius:28px;padding:28px}
.report h2{font:700 1.35rem/1.2 var(--display);letter-spacing:-.02em;margin:0 0 4px}
.sub{color:var(--muted);margin:0 0 18px;font-size:.95rem;max-width:60ch}
.empty-note{background:var(--raise);border-radius:18px;padding:18px;color:var(--muted);max-width:60ch}
.refs{display:flex;align-items:center;gap:8px;flex-wrap:wrap;margin-bottom:22px;font-size:.9rem;color:var(--muted)}
.hero{display:flex;gap:22px;align-items:center;padding:20px;border-radius:22px;background:var(--raise);margin-bottom:12px}
.ring{position:relative;width:116px;height:116px;flex:none}
.ring svg{width:100%;height:100%;transform:rotate(-90deg)}
.ring circle{fill:none;stroke-width:9;stroke-linecap:round}
.ring .bg{stroke:var(--track)}
.ring .fg{stroke:var(--accent);stroke-dasharray:314;stroke-dashoffset:314;transition:stroke-dashoffset 1.2s cubic-bezier(.2,.8,.2,1)}
.ring b{position:absolute;inset:0;display:grid;place-items:center;font:800 2.5rem/1 var(--display);letter-spacing:-.04em}
.hero h3{font:700 1.15rem/1.25 var(--display);margin:0 0 8px}
.quality{display:flex;flex-wrap:wrap;gap:6px;margin:0;padding:0;list-style:none}
.quality li{font-size:.8rem;padding:3px 10px;border-radius:999px;background:var(--card);border:1px solid var(--line)}
.quality li::before{content:"";display:inline-block;width:7px;height:7px;border-radius:50%;margin-right:7px;background:var(--ok)}
.quality li.warn::before{background:var(--warn)}
.sec{display:flex;justify-content:space-between;align-items:baseline;font:700 1.15rem var(--display);margin:24px 0 0}
.sec span{color:var(--accent);font-size:1.3rem}
.metrics{list-style:none;margin:8px 0 0;padding:0}
.m{padding:16px 0;border-top:1px solid var(--line)}
.m-head{display:flex;justify-content:space-between;align-items:baseline;gap:12px}
.m-name{font-weight:600}
.m-score{font:800 1.3rem/1 var(--display);letter-spacing:-.02em}
.track{position:relative;height:10px;border-radius:999px;background:var(--track);margin:11px 0 7px}
.zone{position:absolute;top:0;bottom:0;border-radius:999px;background:color-mix(in srgb,var(--accent) 25%,transparent);box-shadow:inset 0 0 0 1px color-mix(in srgb,var(--accent) 55%,transparent)}
.mark{position:absolute;top:50%;width:18px;height:18px;margin:-9px 0 0 -9px;border-radius:50%;background:var(--accent);border:3px solid var(--card);box-shadow:0 1px 4px rgba(0,0,0,.45);left:0;transition:left 1s cubic-bezier(.2,.8,.2,1)}
.m-foot{display:flex;justify-content:space-between;gap:10px;font-size:.82rem;color:var(--muted);font-variant-numeric:tabular-nums}
.m-desc{margin:6px 0 0;font-size:.88rem;color:var(--muted);max-width:62ch}
.notes{margin-top:8px;display:grid;gap:10px}
.note{padding:14px 16px;border-radius:16px;background:var(--raise);font-size:.92rem;max-width:66ch}
.note.warn{background:var(--warnbg)}
.fine{margin:22px 0 0;font-size:.8rem;color:var(--muted);max-width:66ch}
@media (max-width:860px){.app{grid-template-columns:1fr}.stage-col{position:static}.hero{flex-direction:column;align-items:flex-start}.report{padding:20px}}
@media (prefers-reduced-motion:reduce){.stage.scanning .sweep{animation:none;top:35%}.ring .fg,.mark{transition:none}}
</style>
</head>
<body>
<header class="top">
  <div class="brand">AureaScore</div>
  <p class="privacy">As fotos são processadas no seu navegador e não são enviadas a nenhum servidor.</p>
  <div class="modes" role="group" aria-label="Tema">
    <button class="chip" data-theme-btn="dark">Escuro</button>
    <button class="chip" data-theme-btn="light">Claro</button>
  </div>
</header>

<main class="app">
  <section class="stage-col" aria-label="Fotos">
    <div class="modes" role="group" aria-label="O que analisar">
      <button class="chip" data-mode="both" aria-pressed="true">Frente + perfil</button>
      <button class="chip" data-mode="front" aria-pressed="false">Só frente</button>
      <button class="chip" data-mode="side" aria-pressed="false">Só perfil</button>
    </div>
    <p class="callout" id="hint"></p>
    <div class="tabs" role="group" aria-label="Foto">
      <button class="chip" data-tab="front">Frente</button>
      <button class="chip" data-tab="side">Perfil</button>
    </div>
    <div id="stages"></div>
    <div class="tools">
      <button class="chip" id="t-mesh" aria-pressed="true" disabled>Malha</button>
      <button class="chip" id="t-meas" aria-pressed="true" disabled>Medidas</button>
      <button class="chip" id="swap" disabled>Trocar foto</button>
      <button class="chip" id="flip" hidden>Inverter lado do perfil</button>
    </div>
    <button class="cta" id="analyze" disabled>Analisar rosto</button>
    <p class="status" id="status" role="status" aria-live="polite">Escolha as fotos para começar.</p>
  </section>

  <section class="report" aria-label="Relatório">
    <h2>Relatório de proporções</h2>
    <p class="sub">Medidas a partir de 478 pontos do rosto, da linha do cabelo e do contorno do perfil.</p>
    <div class="empty-note" id="empty">Envie as fotos e toque em “Analisar rosto”. Cada medida aparece em uma escala, com a faixa de referência destacada.</div>
    <div id="results" class="hidden">
      <div class="refs" role="group" aria-label="Referência">Referência:
        <button class="chip" data-ref="neutra" aria-pressed="true">Neutra</button>
        <button class="chip" data-ref="masc" aria-pressed="false">Masculina</button>
        <button class="chip" data-ref="fem" aria-pressed="false">Feminina</button>
      </div>
      <div class="hero">
        <div class="ring"><svg viewBox="0 0 116 116" aria-hidden="true"><circle class="bg" cx="58" cy="58" r="50"/><circle class="fg" id="ring" cx="58" cy="58" r="50"/></svg><b id="overall">–</b></div>
        <div><h3 id="verdict"></h3><ul class="quality" id="quality"></ul></div>
      </div>
      <div id="groups"></div>
      <h2 style="margin-top:26px">O que observar</h2>
      <div class="notes" id="notes"></div>
      <p class="fine">Estas medidas descrevem a geometria das fotos enviadas. Ângulo da câmera, lente, expressão e luz mudam os números, e as faixas de referência são heurísticas. Proporções não definem beleza nem valor pessoal.</p>
    </div>
  </section>
</main>

<script type="module">
const $ = s => document.querySelector(s), $$ = s => [...document.querySelectorAll(s)];
const clamp = (v, a, b) => Math.min(b, Math.max(a, v));
const statusEl = $('#status'), analyzeBtn = $('#analyze');
const setStatus = (m, err = false) => { statusEl.textContent = m; statusEl.classList.toggle('err', err); };
const CDN = 'https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.14', MODELS = 'https://storage.googleapis.com/mediapipe-models/', MAX = 1600;
let mode = 'both', active = 'front', ref = 'neutra', flipSide = false, busy = false, vision, fileset, lmP, segP;
const show = { mesh: true, meas: true };
const COPY = { front: ['Foto de frente', 'Olhe para a câmera, mostre o rosto inteiro com testa e cabelo'], side: ['Foto de perfil', 'Olhe para o lado (90°), orelha visível, cabelo fora do rosto, fundo liso'] };

/* ---------- Tema ---------- */
const root = document.documentElement;
$$('[data-theme-btn]').forEach(b => {
  b.setAttribute('aria-pressed', b.dataset.themeBtn === root.dataset.theme);
  b.onclick = () => {
    root.dataset.theme = b.dataset.themeBtn;
    $$('[data-theme-btn]').forEach(x => x.setAttribute('aria-pressed', x === b));
    try { localStorage.setItem('umax-theme', b.dataset.themeBtn); } catch {}
  };
});

/* ---------- Slots de foto ---------- */
const slots = {};
for (const k of ['front', 'side']) {
  const el = document.createElement('div'); el.className = 'stage';
  el.innerHTML = `<input id="f-${k}" type="file" accept="image/*"><label class="drop" for="f-${k}"><b>${COPY[k][0]}</b><span>Solte aqui ou toque para escolher</span><span>${COPY[k][1]}</span></label><canvas></canvas><div class="sweep"></div>`;
  $('#stages').append(el);
  const view = el.querySelector('canvas'), base = document.createElement('canvas');
  const s = slots[k] = { k, el, view, ctx: view.getContext('2d'), base, bctx: base.getContext('2d', { willReadFrequently: true }), has: false, res: null, mask: null };
  el.querySelector('input').onchange = e => loadFile(s, e.target.files?.[0]);
  el.addEventListener('dragover', e => { e.preventDefault(); el.classList.add('dragging'); });
  el.addEventListener('dragleave', () => el.classList.remove('dragging'));
  el.addEventListener('drop', e => { e.preventDefault(); el.classList.remove('dragging'); loadFile(s, e.dataTransfer.files?.[0]); });
}
const need = () => mode === 'both' ? ['front', 'side'] : [mode];

async function loadFile(s, file) {
  if (!file || !file.type.startsWith('image/')) return setStatus('Envie um arquivo de imagem (JPG, PNG ou WebP).', true);
  const bmp = await createImageBitmap(file, { imageOrientation: 'from-image' });
  const k = Math.min(1, MAX / Math.max(bmp.width, bmp.height));
  s.base.width = s.view.width = Math.round(bmp.width * k); s.base.height = s.view.height = Math.round(bmp.height * k);
  s.bctx.drawImage(bmp, 0, 0, s.base.width, s.base.height); bmp.close?.();
  s.el.style.aspectRatio = `${s.base.width}/${s.base.height}`; s.el.classList.add('has-image');
  s.has = true; s.res = null; s.mask = null;
  resetResults(); paint(s); refreshUI();
  setStatus(need().every(n => slots[n].has) ? 'Tudo pronto. Toque em “Analisar rosto”.' : 'Foto carregada. Envie a outra vista para uma análise mais precisa.');
}

function resetResults() { $('#results').classList.add('hidden'); $('#empty').classList.remove('hidden'); }

function refreshUI() {
  const nd = need(); if (!nd.includes(active)) active = nd[0];
  $$('[data-tab]').forEach(b => { const k = b.dataset.tab; b.hidden = nd.length < 2; b.setAttribute('aria-pressed', k === active); b.classList.toggle('done', slots[k].has); });
  for (const k in slots) slots[k].el.hidden = k !== active;
  const s = slots[active];
  ['#t-mesh', '#t-meas', '#swap'].forEach(i => $(i).disabled = !s.has);
  $('#flip').hidden = active !== 'side' || !s.has;
  analyzeBtn.disabled = busy || !nd.every(k => slots[k].has);
  const h = $('#hint'); h.classList.toggle('warn', mode !== 'both');
  h.textContent = mode === 'both' ? 'Com frente e perfil juntos, as medidas ficam mais precisas.' : 'Com só uma vista a análise é menos precisa. Use as duas fotos para o melhor resultado.';
}
$$('[data-mode]').forEach(b => b.onclick = () => { mode = b.dataset.mode; $$('[data-mode]').forEach(x => x.setAttribute('aria-pressed', x === b)); resetResults(); refreshUI(); });
$$('[data-tab]').forEach(b => b.onclick = () => { active = b.dataset.tab; refreshUI(); });
$('#swap').onclick = () => { const i = slots[active].el.querySelector('input'); i.value = ''; i.click(); };
$('#flip').onclick = () => { flipSide = !flipSide; if (!analyzeBtn.disabled) analyzeBtn.click(); };
for (const [id, key] of [['#t-mesh', 'mesh'], ['#t-meas', 'meas']]) $(id).onclick = e => { show[key] = !show[key]; e.currentTarget.setAttribute('aria-pressed', show[key]); for (const k in slots) paint(slots[k]); };

/* ---------- Modelos ---------- */
async function create(Cls, o) {
  for (const delegate of ['GPU', 'CPU']) {
    try { return await Cls.createFromOptions(fileset, { ...o, baseOptions: { ...o.baseOptions, delegate } }); }
    catch (e) { if (delegate === 'CPU') throw e; }
  }
}
async function getVision() {
  if (vision) return;
  setStatus('Carregando modelos (só na primeira vez)…');
  vision = await import(`${CDN}/vision_bundle.mjs`);
  fileset = await vision.FilesetResolver.forVisionTasks(`${CDN}/wasm`);
}
const landmarker = () => lmP ??= (async () => { await getVision(); return create(vision.FaceLandmarker, { baseOptions: { modelAssetPath: MODELS + 'face_landmarker/face_landmarker/float16/1/face_landmarker.task' }, runningMode: 'IMAGE', numFaces: 1, minFaceDetectionConfidence: .3, minFacePresenceConfidence: .3 }); })().catch(e => { lmP = null; throw e; });
const segmenter = () => segP ??= (async () => { await getVision(); return create(vision.ImageSegmenter, { baseOptions: { modelAssetPath: MODELS + 'image_segmenter/selfie_multiclass_256x256/float32/latest/selfie_multiclass_256x256.tflite' }, runningMode: 'IMAGE', outputCategoryMask: true, outputConfidenceMasks: false }); })().catch(e => { segP = null; throw e; });

// Máscara com classes: 0 fundo, 1 cabelo, 2 pele do corpo, 3 pele do rosto, 4 roupa, 5 acessórios
async function segment(s) {
  if (s.mask) return s.mask;
  try {
    const r = (await segmenter()).segment(s.base), m = r.categoryMask;
    s.mask = { m: Uint8Array.from(m.getAsUint8Array()), mw: m.width, mh: m.height };
    r.close?.(); return s.mask;
  } catch (e) { console.warn('Segmentação indisponível', e); return null; }
}

// Detecta o rosto inteiro. Se a foto estiver muito justa, repete com margem extra ao redor.
function detectFace(lm, s) {
  const W = s.base.width, H = s.base.height;
  const run = pad => {
    const pw = Math.round(W * pad), ph = Math.round(H * pad), c = document.createElement('canvas');
    c.width = W + 2 * pw; c.height = H + 2 * ph;
    const x = c.getContext('2d'); x.fillStyle = '#808080'; x.fillRect(0, 0, c.width, c.height); x.drawImage(s.base, pw, ph);
    const f = lm.detect(c).faceLandmarks?.[0];
    return f && f.map(p => ({ x: (p.x * c.width - pw) / W, y: (p.y * c.height - ph) / H, z: p.z }));
  };
  let f = run(0);
  if (!f || f.some(p => p.x < .005 || p.x > .995 || p.y < .005 || p.y > .995)) f = run(.3) || f;
  return f;
}

/* ---------- Frente ---------- */
const PAIRS = [[33,263],[133,362],[70,300],[105,334],[234,454],[172,397],[61,291],[129,358],[93,323],[58,288],[46,276]];

// Sobe a partir do ponto 10 (meio da testa) enquanto ainda for pele do rosto, para achar a linha do cabelo real
function hairline(raw, a, Wf, Hf, mk, W, H) {
  const fallback = { t: .12 * Hf, est: true };
  if (!mk) return fallback;
  const up = { x: Math.sin(a), y: -Math.cos(a) }, ax = { x: Math.cos(a), y: Math.sin(a) }, p = raw[10], max = .5 * Hf;
  const cls = (x, y) => { const i = Math.round(y * mk.mh / H), j = Math.round(x * mk.mw / W); return i < 0 || j < 0 || i >= mk.mh || j >= mk.mw ? -1 : mk.m[i * mk.mw + j]; };
  let last = 0, miss = 0;
  for (let t = 0; t <= max; t += Hf * .01) {
    let hit = 0;
    for (let s = -3; s <= 3; s++) if (cls(p.x + up.x * t + ax.x * s * Wf * .04, p.y + up.y * t + ax.y * s * Wf * .04) === 3 || cls(p.x + up.x * t + ax.x * s * Wf * .04, p.y + up.y * t + ax.y * s * Wf * .04) === 2) hit++;
    if (hit >= 4) { last = t; miss = 0; } else if (++miss >= 4) break;
  }
  return last < Hf * .02 ? fallback : { t: last, est: false, capped: last >= max * .97 };
}

function measureFront(face, s, mk) {
  const W = s.base.width, H = s.base.height, raw = face.map(p => ({ x: p.x * W, y: p.y * H }));
  const a = Math.atan2(raw[263].y - raw[33].y, raw[263].x - raw[33].x), cos = Math.cos(a), sin = Math.sin(a);
  const c = { x: (raw[33].x + raw[263].x) / 2, y: (raw[33].y + raw[263].y) / 2 };
  const q = raw.map(p => { const dx = p.x - c.x, dy = p.y - c.y; return { x: dx * cos + dy * sin, y: -dx * sin + dy * cos }; });
  const d = (i, j) => Math.hypot(q[i].x - q[j].x, q[i].y - q[j].y);
  const Wf = d(234, 454), hl = hairline(raw, a, Wf, d(10, 152), mk, W, H);
  // Sem cabelo não existe linha do cabelo: assume terço superior = terço médio
  if (hl.capped) { hl.t = Math.max(.05 * d(10, 152), q[10].y - 2 * q[9].y + q[2].y); hl.est = true; hl.bald = true; }

  // Altura total do rosto: da linha do cabelo ao queixo. Terços reais: cabelo-glabela, glabela-nariz, nariz-queixo
  const yT = q[10].y - hl.t, tot = q[152].y - yT;
  const th = [q[9].y - yT, q[2].y - q[9].y, q[152].y - q[2].y].map(v => v / tot);

  const midX = PAIRS.reduce((t, [l, r]) => t + (q[l].x + q[r].x) / 2, 0) / PAIRS.length;
  const errs = PAIRS.map(([l, r]) => (Math.abs((q[l].x + q[r].x) / 2 - midX) + Math.abs(q[l].y - q[r].y)) / 2);
  [1, 0, 152].forEach(i => errs.push(Math.abs(q[i].x - midX)));
  const dl = Math.abs(q[1].x - q[234].x), dr = Math.abs(q[454].x - q[1].x), ew = (d(33, 133) + d(263, 362)) / 2;
  const tl = Math.atan2(q[133].y - q[33].y, Math.abs(q[33].x - q[133].x)), tr = Math.atan2(q[362].y - q[263].y, Math.abs(q[263].x - q[362].x));
  return {
    raw, a, hl, rollDeg: a * 180 / Math.PI, faceW: Wf, yaw: Math.abs(dl - dr) / (dl + dr),
    jaw: d(172, 397) / Wf, prop: Wf / tot, fwhr: Wf / d(9, 0),
    thirds: th.reduce((t, v) => t + Math.abs(v - 1 / 3), 0) / 3, eyes: d(133, 362) / ew,
    tilt: (tl + tr) / 2 * 180 / Math.PI, sym: errs.reduce((t, v) => t + v, 0) / errs.length / Wf,
    skin: skinIndex(s.base, raw, Wf)
  };
}

// Pele: 3 amostras (bochechas e testa), só pixels de tom de pele; textura fina + uniformidade de cor, mediana das amostras
function skinIndex(base, pts, Wf) {
  const n = 40, p = Math.round(Wf * .085), cv = document.createElement('canvas'); cv.width = cv.height = n;
  const x = cv.getContext('2d', { willReadFrequently: true }), out = [];
  for (const i of [50, 280, 151]) {
    x.clearRect(0, 0, n, n); x.drawImage(base, pts[i].x - p / 2, pts[i].y - p / 2, p, p, 0, 0, n, n);
    const px = x.getImageData(0, 0, n, n).data, L = new Float32Array(n * n), ok = new Uint8Array(n * n), crs = [];
    for (let k = 0; k < n * n; k++) {
      const r = px[k*4], g = px[k*4+1], b = px[k*4+2]; L[k] = .299*r + .587*g + .114*b;
      const cb = 128 - .168736*r - .331264*g + .5*b, cr = 128 + .5*r - .418688*g - .081312*b;
      if (cb >= 77 && cb <= 127 && cr >= 133 && cr <= 173) { ok[k] = 1; crs.push(cr); }
    }
    if (crs.length < n * n * .6) continue;
    let tex = 0, lum = 0, t = 0;
    for (let yy = 1; yy < n - 1; yy++) for (let xx = 1; xx < n - 1; xx++) {
      const k = yy * n + xx; if (!ok[k]) continue;
      let m = 0; for (let dy = -1; dy <= 1; dy++) for (let dx = -1; dx <= 1; dx++) m += L[k + dy * n + dx];
      tex += Math.abs(L[k] - m / 9); lum += L[k]; t++;
    }
    if (!t) continue;
    const mc = crs.reduce((u, v) => u + v, 0) / crs.length;
    out.push({ tex: tex / t / (lum / t), sd: Math.sqrt(crs.reduce((u, v) => u + (v - mc) ** 2, 0) / crs.length) });
  }
  if (!out.length) return null;
  const med = k => out.map(o => o[k]).sort((u, v) => u - v)[out.length >> 1];
  return Math.round(40 + 58 * (.6 * clamp(1 - (med('tex') - .01) / .06, 0, 1) + .4 * clamp(1 - (med('sd') - 2) / 10, 0, 1)));
}

/* ---------- Perfil (contorno da pele do rosto) ---------- */
function profile(mk, flip) {
  const { m, mw, mh } = mk, L = [], R = [];
  let top = -1, bot = -1;
  for (let y = 0; y < mh; y++) {
    let lo = -1, hi = -1, n = 0;
    for (let x = 0; x < mw; x++) if (m[y * mw + x] === 3) { if (lo < 0) lo = x; hi = x; n++; }
    if (n >= 2) { L[y] = lo; R[y] = hi; if (top < 0) top = y; bot = y; }
  }
  if (top < 0 || bot - top < 20) return null;
  const H = bot - top;
  // Contorno x(y) sempre no referencial "olhando para a direita"; suaviza com média móvel
  const build = dir => {
    const cx = []; let prev = null;
    for (let y = top; y <= bot; y++) { let v = dir === 1 ? R[y] : (L[y] == null ? null : mw - 1 - L[y]); if (v == null) v = prev; prev = v; cx[y] = v; }
    return cx.map((v, y) => { let s = 0, n = 0; for (let k = -2; k <= 2; k++) { const u = cx[y + k]; if (u != null) { s += u; n++; } } return s / n; });
  };
  const arg = (cx, a, b, mx = true) => {
    let bi = -1, bv = mx ? -1e9 : 1e9;
    for (let y = Math.max(top, Math.round(a)); y <= Math.min(bot, Math.round(b)); y++) if (mx ? cx[y] > bv : cx[y] < bv) { bv = cx[y]; bi = y; }
    return bi;
  };
  const b0 = top + .2 * H, b1 = top + .72 * H;
  const prot = cx => { const t = arg(cx, b0, b1), v = []; for (let y = Math.round(b0); y <= Math.round(b1); y++) v.push(cx[y]); v.sort((u, w) => u - w); return cx[t] - v[v.length >> 1]; };
  const c1 = build(1), c2 = build(-1);
  let dir = prot(c1) >= prot(c2) ? 1 : -1; if (flip) dir = -dir;
  const cx = dir === 1 ? c1 : c2;

  const tip = arg(cx, b0, b1), N = arg(cx, tip - .30 * H, tip - .04 * H, false), G = arg(cx, top, N),
    Sn = arg(cx, tip + .03 * H, tip + .16 * H, false), Ls = arg(cx, Sn, Sn + .10 * H),
    St = arg(cx, Ls + .01 * H, Ls + .07 * H, false), Li = arg(cx, St, St + .07 * H),
    Sul = arg(cx, Li, Li + .10 * H, false), Pg = arg(cx, Sul, bot);
  const ys = [G, N, tip, Sn, Ls, Li, Pg], ok = ys.every((v, i) => v >= 0 && (!i || v > ys[i - 1]));
  const out = { ok, dir, cx, top, bot, mw, mh };
  if (!ok) return out;
  const P = y => ({ x: cx[y], y }), pt = { G: P(G), N: P(N), Pn: P(tip), Sn: P(Sn), Ls: P(Ls), Li: P(Li), Pg: P(Pg) };
  const cmY = Math.round(Sn - .5 * (Sn - tip)); pt.Cm = P(cmY);
  const ang = (a, v, b) => { const x1 = a.x - v.x, y1 = a.y - v.y, x2 = b.x - v.x, y2 = b.y - v.y; return Math.acos(clamp((x1*x2 + y1*y2) / (Math.hypot(x1, y1) * Math.hypot(x2, y2)), -1, 1)) * 180 / Math.PI; };
  // Linha E: nariz → queixo. Distância dos lábios à linha, normalizada pelo comprimento dela (negativo = atrás da linha)
  const dv = { x: pt.Pg.x - pt.Pn.x, y: pt.Pg.y - pt.Pn.y }, len = Math.hypot(dv.x, dv.y);
  const off = p => ((p.x - pt.Pn.x) * dv.y - (p.y - pt.Pn.y) * dv.x) / (len * len);
  out.pt = pt;
  out.m = { nf: ang(pt.G, pt.N, pt.Pn), nl: ang(pt.Cm, pt.Sn, pt.Ls), cv: ang(pt.G, pt.Sn, pt.Pg), el: (off(pt.Ls) + off(pt.Li)) / 2 };
  return out;
}

/* ---------- Desenho ---------- */
function paint(s) {
  if (!s.has) return;
  const c = s.ctx, lw = Math.max(.7, s.view.width / 1400);
  c.clearRect(0, 0, s.view.width, s.view.height); c.drawImage(s.base, 0, 0);
  if (!s.res || !vision) return;
  const line = (a, b, col, dash = []) => { c.setLineDash(dash.map(v => v * lw)); c.strokeStyle = col; c.lineWidth = lw * 2; c.beginPath(); c.moveTo(a.x, a.y); c.lineTo(b.x, b.y); c.stroke(); c.setLineDash([]); };
  const dots = ps => { c.fillStyle = '#fff'; for (const p of ps) { c.beginPath(); c.arc(p.x, p.y, lw * 3, 0, 7); c.fill(); } };
  if (s.k === 'front') {
    const { face, M } = s.res;
    if (show.mesh) {
      const du = new vision.DrawingUtils(c), FL = vision.FaceLandmarker;
      du.drawConnectors(face, FL.FACE_LANDMARKS_TESSELATION, { color: 'rgba(255,255,255,.22)', lineWidth: lw * .8 });
      for (const set of [FL.FACE_LANDMARKS_FACE_OVAL, FL.FACE_LANDMARKS_LEFT_EYE, FL.FACE_LANDMARKS_RIGHT_EYE, FL.FACE_LANDMARKS_LEFT_EYEBROW, FL.FACE_LANDMARKS_RIGHT_EYEBROW, FL.FACE_LANDMARKS_LIPS])
        du.drawConnectors(face, set, { color: 'rgba(196,184,255,.9)', lineWidth: lw * 1.3 });
    }
    if (show.meas) {
      const P = i => M.raw[i], ux = Math.cos(M.a), uy = Math.sin(M.a), half = M.faceW * .6;
      const T = { x: P(10).x + Math.sin(M.a) * M.hl.t, y: P(10).y - Math.cos(M.a) * M.hl.t };
      line(P(234), P(454), '#FFD166'); line(P(172), P(397), '#7CF0C2'); line(T, P(152), '#fff', [6, 5]);
      for (const p of [T, P(9), P(2), P(152)]) line({ x: p.x - ux * half, y: p.y - uy * half }, { x: p.x + ux * half, y: p.y + uy * half }, 'rgba(255,255,255,.75)', [3, 5]);
      dots([P(234), P(454), P(172), P(397), T, P(9), P(2), P(152)]);
    }
  } else {
    const r = s.res, sx = s.view.width / r.mw, sy = s.view.height / r.mh, X = x => (r.dir === 1 ? x : r.mw - 1 - x) * sx;
    if (show.mesh) {
      c.strokeStyle = 'rgba(196,184,255,.95)'; c.lineWidth = lw * 2; c.beginPath();
      for (let y = r.top; y <= r.bot; y++) y === r.top ? c.moveTo(X(r.cx[y]), y * sy) : c.lineTo(X(r.cx[y]), y * sy);
      c.stroke();
    }
    if (show.meas && r.ok) {
      const T = p => ({ x: X(p.x), y: p.y * sy }), { G, N, Pn, Sn, Ls, Pg, Cm } = Object.fromEntries(Object.entries(r.pt).map(([k, p]) => [k, T(p)]));
      line(G, N, '#FFD166'); line(N, Pn, '#FFD166'); line(Pn, Pg, '#fff', [6, 5]); line(Cm, Sn, '#7CF0C2'); line(Sn, Ls, '#7CF0C2');
      dots(Object.values(r.pt).map(T));
    }
  }
}

/* ---------- Pontuação ---------- */
const band = (v, ideal, tol, sig) => Math.round(clamp(98 * Math.exp(-.5 * (Math.max(0, Math.abs(v - ideal) - tol) / sig) ** 2), 30, 98));
const REF = { neutra: { jaw: .76, fwhr: 1.90, nl: 100 }, masc: { jaw: .80, fwhr: 1.98, nl: 95 }, fem: { jaw: .73, fwhr: 1.84, nl: 105 } };
const f2 = v => v.toFixed(2), deg = v => v.toFixed(0) + '°', pct = v => (v * 100).toFixed(1) + '%';

function frontDefs(M) {
  const R = REF[ref];
  return [
    { name: 'Mandíbula', w: .20, v: M.jaw, ideal: R.jaw, tol: .035, sig: .06, dom: [.6, 1], f: f2, desc: 'Largura da mandíbula dividida pela largura das maçãs do rosto.', tip: 'Refaça a foto com o queixo neutro e o celular na altura dos olhos.' },
    { name: 'Simetria', w: .20, v: M.sym, ideal: 0, tol: .012, sig: .03, dom: [0, .07], f: v => pct(v) + ' de desvio', desc: 'Quanto os pares de pontos (olhos, sobrancelhas, boca, maçãs) fogem da linha média.', tip: 'Pequenas assimetrias são normais. Sorriso de um lado e cabeça girada aumentam o desvio.' },
    { name: 'Proporção do rosto', w: .12, v: M.prop, ideal: .70, tol: .045, sig: .06, dom: [.5, .9], f: f2, desc: 'Largura das maçãs dividida pela altura total, da linha do cabelo ao queixo.', tip: 'Fotos muito de perto esticam o rosto. Afaste-se e use zoom 2x.' },
    { name: 'Terços faciais', w: .10, v: M.thirds, ideal: 0, tol: .02, sig: .04, dom: [0, .12], f: v => pct(v) + ' de desvio', desc: 'Quanto cabelo–sobrancelhas, sobrancelhas–nariz e nariz–queixo se afastam de três partes iguais.', tip: 'Boca entreaberta aumenta o terço inferior. Feche os lábios sem apertar.' },
    { name: 'Robustez (fWHR)', w: .08, v: M.fwhr, ideal: R.fwhr, tol: .10, sig: .18, dom: [1.4, 2.5], f: f2, desc: 'Largura das maçãs dividida pela altura entre as sobrancelhas e o lábio superior.', tip: 'Sobrancelhas levantadas ou boca aberta alteram essa altura.' },
    { name: 'Distância entre os olhos', w: .10, v: M.eyes, ideal: 1, tol: .08, sig: .15, dom: [.7, 1.4], f: f2, desc: 'Espaço entre os cantos internos dos olhos comparado com a largura de um olho.', tip: 'Olhe direto para a lente.' },
    { name: 'Inclinação dos olhos', w: .05, v: M.tilt, ideal: 5, tol: 3, sig: 5, dom: [-8, 14], f: deg, desc: 'Ângulo entre os cantos interno e externo dos olhos. Positivo: canto externo mais alto.', tip: 'Foto de cima ou de baixo inclina os olhos artificialmente.' },
    M.skin == null ? null : { name: 'Pele (textura e cor)', w: .15, v: M.skin, ideal: 90, tol: 8, sig: 22, dom: [40, 100], f: v => v + ' de 100', direct: true, desc: 'Textura fina e uniformidade de cor em bochechas e testa, só em pixels de tom de pele.', tip: 'Prefira luz de janela, sem flash. Nitidez e compressão pesam muito aqui.' }
  ].filter(Boolean);
}
function sideDefs(m) {
  return [
    { name: 'Ângulo nasofrontal', w: .25, v: m.nf, ideal: 128, tol: 8, sig: 10, dom: [100, 160], f: deg, desc: 'Ângulo entre a testa e o dorso do nariz, na altura da raiz do nariz.', tip: 'Mantenha o olhar na linha do horizonte, sem erguer ou baixar o queixo.' },
    { name: 'Ângulo nasolabial', w: .25, v: m.nl, ideal: REF[ref].nl, tol: 7, sig: 12, dom: [70, 130], f: deg, desc: 'Ângulo entre a base do nariz e o lábio superior.', tip: 'Cabeça levemente para cima ou para baixo muda muito esse ângulo.' },
    { name: 'Convexidade facial', w: .30, v: m.cv, ideal: 168, tol: 4, sig: 5, dom: [150, 185], f: deg, desc: 'Quanto o perfil é reto ou curvo, medido da testa ao nariz e ao queixo.', tip: 'Queixo recuado ou projetado na foto altera a leitura. Mantenha a mandíbula relaxada.' },
    { name: 'Lábios e linha E', w: .20, v: m.el, ideal: -.04, tol: .03, sig: .05, dom: [-.2, .12], f: pct, desc: 'Posição dos lábios em relação à linha entre a ponta do nariz e o queixo. Negativo: atrás da linha.', tip: 'Lábios relaxados e fechados, sem projetar nem apertar.' }
  ];
}

/* ---------- Relatório ---------- */
function render(animate) {
  const ks = need().filter(k => slots[k].res), both = ks.length === 2, gw = both ? { front: .6, side: .4 } : { front: 1, side: 1 };
  const groups = ks.map(k => {
    const defs = k === 'front' ? frontDefs(slots.front.res.M) : sideDefs(slots.side.res.m);
    for (const d of defs) d.score = d.direct ? d.v : band(d.v, d.ideal, d.tol, d.sig);
    const ws = defs.reduce((t, d) => t + d.w, 0);
    return { k, name: k === 'front' ? 'Frente' : 'Perfil', defs, score: Math.round(defs.reduce((t, d) => t + d.score * d.w, 0) / ws), gw: gw[k] };
  });
  const overall = Math.round(groups.reduce((t, g) => t + g.score * g.gw, 0) / groups.reduce((t, g) => t + g.gw, 0));
  $('#overall').textContent = overall;
  $('#verdict').textContent = overall >= 85 ? 'Proporções muito próximas da referência' : overall >= 70 ? 'Proporções equilibradas, com pontos a observar' : overall >= 55 ? 'Alguns pontos fogem da referência' : 'Vários pontos fogem da referência. Vale refazer as fotos antes de concluir algo.';

  const q = [], F = slots.front.res?.M;
  if (F) q.push(F.yaw < .10 ? ['Frente: pose frontal', 0] : ['Frente: rosto girado', 1], F.faceW >= 220 ? ['Frente: resolução boa', 0] : ['Frente: rosto pequeno', 1],
    F.hl.bald ? ['Sem cabelo: testa estimada', 1] : F.hl.est ? ['Testa estimada (cabelo cobre a linha)', 1] : ['Linha do cabelo medida', 0]);
  if (slots.side.res) q.push(['Perfil identificado', 0]);
  $('#quality').innerHTML = q.map(([t, w]) => `<li class="${w ? 'warn' : ''}">${t}</li>`).join('');

  $('#groups').innerHTML = groups.map(g => `<h3 class="sec">${g.name}<span>${g.score}</span></h3><ol class="metrics">${g.defs.map(m => {
    const [d0, d1] = m.dom, pos = v => clamp((v - d0) / (d1 - d0), 0, 1) * 100;
    const z0 = Math.max(d0, m.ideal - m.tol), z1 = Math.min(d1, m.ideal + m.tol);
    return `<li class="m"><div class="m-head"><span class="m-name">${m.name}</span><span class="m-score">${m.score}</span></div>
      <div class="track" role="img" aria-label="${m.name}: ${m.f(m.v)}, referência de ${m.f(z0)} a ${m.f(z1)}"><i class="zone" style="left:${pos(z0)}%;width:${pos(z1) - pos(z0)}%"></i><i class="mark" data-pos="${pos(m.v)}"></i></div>
      <div class="m-foot"><span>Medido: ${m.f(m.v)}</span><span>Referência: ${m.f(z0)} a ${m.f(z1)}</span></div><p class="m-desc">${m.desc}</p></li>`;
  }).join('')}</ol>`).join('');

  const notes = [];
  if (!both) notes.push(['warn', `Análise parcial: sem a foto de ${ks[0] === 'front' ? 'perfil' : 'frente'}, faltam medidas importantes. Envie as duas para um resultado mais preciso.`]);
  if (F?.hl.bald) notes.push(['warn', 'Sem cabelo não há linha do cabelo para medir. A testa foi estimada igualando o terço superior ao médio, então a proporção do rosto e os terços são aproximados.']);
  if (F?.yaw >= .10) notes.push(['warn', 'Na foto de frente a cabeça está girada, o que distorce simetria e proporções. Olhe direto para a câmera.']);
  if (F?.faceW < 220) notes.push(['warn', 'O rosto ocupa poucos pixels na foto de frente. Aproxime o enquadramento ou use uma foto de maior resolução.']);
  groups.flatMap(g => g.defs).filter(d => d.score < 80).sort((a, b) => a.score - b.score).slice(0, 2).forEach(d => notes.push(['', `<b>${d.name} (${d.score}):</b> ${d.tip}`]));
  if (!notes.length) notes.push(['', 'Nenhuma medida ficou muito fora da faixa. Para confirmar, repita com outras fotos e compare os resultados.']);
  $('#notes').innerHTML = notes.map(([c, t]) => `<div class="note ${c}">${t}</div>`).join('');

  const marks = $$('.mark'), ring = $('#ring');
  const go = () => { marks.forEach(e => e.style.left = e.dataset.pos + '%'); ring.style.strokeDashoffset = 314 * (1 - overall / 100); };
  if (animate) { ring.style.strokeDashoffset = 314; requestAnimationFrame(() => requestAnimationFrame(go)); } else go();
}
$$('[data-ref]').forEach(b => b.onclick = () => { ref = b.dataset.ref; $$('[data-ref]').forEach(x => x.setAttribute('aria-pressed', x === b)); if (!$('#results').classList.contains('hidden')) render(false); });

/* ---------- Análise ---------- */
analyzeBtn.onclick = async () => {
  busy = true; analyzeBtn.disabled = true; need().forEach(k => slots[k].el.classList.add('scanning'));
  try {
    if (need().includes('front')) {
      const s = slots.front; await getVision(); setStatus('Detectando o rosto inteiro…');
      const face = detectFace(await landmarker(), s);
      if (!face) return setStatus('Nenhum rosto encontrado na foto de frente. Use uma foto clara, de frente e sem nada cobrindo o rosto.', true);
      setStatus('Medindo a linha do cabelo…');
      s.res = { face, M: measureFront(face, s, await segment(s)) }; paint(s);
    }
    if (need().includes('side')) {
      const s = slots.side; setStatus('Lendo o contorno do perfil…');
      const mk = await segment(s), p = mk && profile(mk, flipSide);
      if (!p) return setStatus('Não foi possível ler o perfil. Use fundo liso, boa luz e cabelo longe do rosto.', true);
      s.res = p; paint(s);
      if (!p.ok) { active = 'side'; return setStatus('Perfil não identificado com segurança. Tente “Inverter lado do perfil” ou use uma foto de 90° com fundo liso.', true); }
    }
    $('#empty').classList.add('hidden'); $('#results').classList.remove('hidden'); render(true);
    setStatus('Análise concluída. Use os botões acima para ver a malha e as medidas em cada foto.');
  } catch (e) {
    console.error(e); setStatus('Não foi possível carregar os modelos. Verifique a conexão com a internet e tente de novo.', true);
  } finally {
    busy = false; need().forEach(k => slots[k].el.classList.remove('scanning')); refreshUI();
  }
};
refreshUI();
</script>
</body>
</html>

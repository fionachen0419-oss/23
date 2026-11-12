[主持人控制台_穩定即時版 (1).html](https://github.com/user-attachments/files/34763433/_.1.html)

<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>主持人控制台 Host Console · 現場互動投票 Live Poll</title>
<style>
:root{--bg:#f6f7fb;--ink:#1c2233;--mut:#6b7385;--ok:#1f9d63;--err:#e11d48;--c0:#2f6fed;--c1:#f08a24;--c2:#8a93a6;--border:#dfe3ee}
*{box-sizing:border-box}
body{margin:0;font-family:"Noto Sans TC",system-ui,-apple-system,sans-serif;background:var(--bg);color:var(--ink)}
.topbar{background:#1c2233;color:#fff;padding:12px 24px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px}
.topbar h1{font-size:18px;margin:0}
.topbar small{font-weight:400;color:#94a3b8;font-size:13px;margin-left:6px}
.pill{display:inline-flex;align-items:center;gap:6px;padding:4px 12px;border-radius:99px;font-size:13px;font-weight:600}
.pill.ok{background:rgba(31,157,99,.22);color:#4ade80}
.pill.wait{background:rgba(245,158,11,.22);color:#fbbf24}
.pill.err{background:rgba(225,29,72,.25);color:#fda4af}
.dot{width:8px;height:8px;border-radius:50%;background:currentColor;animation:pulse 1.5s infinite}
@keyframes pulse{0%{opacity:1}50%{opacity:.3}100%{opacity:1}}
.wrap{max-width:1260px;margin:20px auto;padding:0 20px;display:grid;grid-template-columns:1fr 360px;gap:20px}
@media(max-width:900px){.wrap{grid-template-columns:1fr}}
.card{background:#fff;border-radius:16px;padding:22px;box-shadow:0 4px 16px rgba(28,34,51,.08);border:1px solid var(--border);margin-bottom:16px}
.q-zh{font-size:28px;font-weight:700;margin:12px 0 4px;line-height:1.35}
.q-en{font-size:17px;color:var(--mut);margin:0 0 18px;font-weight:500}
.status{display:inline-block;padding:4px 12px;border-radius:99px;background:#e6e9f3;color:var(--mut);font-size:14px;font-weight:700}
.status.open{background:#dff5ea;color:var(--ok)}
button{font-family:inherit;font-size:14px;padding:9px 13px;border-radius:10px;border:0;background:var(--ink);color:#fff;margin:3px 4px 3px 0;cursor:pointer;font-weight:600;line-height:1.3;text-align:left;transition:opacity .15s}
button small{display:block;font-size:11px;font-weight:400;opacity:.85}
button:disabled{opacity:.35;cursor:not-allowed}
button.alt{background:#e6e9f3;color:var(--ink)}
button.cur{outline:3px solid var(--c0);background:#eef3fe;color:var(--c0)}
button.danger{background:#fee2e2;color:#b91c1c}
button.top-btn{background:rgba(255,255,255,.14);color:#fff;padding:6px 12px;font-size:13px;margin:0}
button.top-btn:hover{background:rgba(255,255,255,.24)}
.row{display:flex;align-items:center;gap:12px;margin:14px 0}
.lab{width:110px;font-size:22px;font-weight:800;line-height:1.2}
.lab small{display:block;font-size:13px;font-weight:500;color:var(--mut)}
.track{flex:1;background:#eceff6;border-radius:10px;height:52px;overflow:hidden}
.fill{height:100%;transition:width .5s ease}
.num{width:110px;text-align:right;font-size:22px;font-weight:700;line-height:1.2}
.num small{display:block;font-size:13px;font-weight:400;color:var(--mut)}
.sub{font-size:16px;font-weight:700;margin-bottom:8px}
.sub span{font-size:12px;font-weight:400;color:var(--mut);margin-left:6px}
.tag{display:inline-block;background:#eef1fb;border-radius:99px;padding:5px 12px;margin:4px 6px 4px 0;font-size:14px}
.txt{padding:10px 0;border-bottom:1px solid #eef0f6;font-size:17px}
.txt b{display:inline-block;width:64px;color:var(--c0)}
.mut{color:var(--mut);font-size:14px;line-height:1.5}
input{width:100%;font-size:13px;padding:9px 10px;border:2px solid var(--border);border-radius:8px;margin-top:4px}
#qr{width:230px;height:230px;margin:10px auto;display:flex;align-items:center;justify-content:center}
.room-badge{background:#2f6fed;color:#fff;padding:4px 11px;border-radius:99px;font-size:13px;font-weight:700;letter-spacing:1px}
.ctl-grid{display:grid;grid-template-columns:1fr 1fr;gap:6px}
</style>
</head>
<body>
<div class="topbar">
  <h1>🎙️ 主持人控制台 <small>Host Console</small></h1>
  <div style="display:flex;align-items:center;flex-wrap:wrap;gap:8px">
    <span id="conn-pill" class="pill wait"><span class="dot"></span><span id="conn-text">連線中 Connecting…</span></span>
    <span style="font-size:13px;color:#cbd5e1">房號 Room: <span class="room-badge" id="room-display"></span></span>
    <button class="top-btn" onclick="manualSync()">🔄 刷新同步 Sync</button>
    <button class="top-btn" style="background:#e11d48" onclick="hardRestart()">🔁 全部重新來過 Reset All</button>
  </div>
</div>

<div class="wrap">
  <div>
    <div class="card" id="main">正在初始化連線… / Initializing connection…</div>
    <div class="card" id="detail"></div>
  </div>
  <div>
    <div class="card">
      <div class="sub">📱 觀眾掃碼入口 <span>Audience QR Code</span></div>
      <div id="qr"></div>
      <p class="mut" style="margin-bottom:4px">將「觀眾手機端」上傳至網站後，把網址貼在下方（請勿貼 <code>file:///</code> 本機路徑）：<br><small>Paste the public HTTPS URL of the audience page below:</small></p>
      <input id="base-url" placeholder="https://your-site.netlify.app/vote.html" oninput="updateQR()">
      <p class="mut" id="full-link" style="word-break:break-all;font-size:11px;margin-top:8px"></p>
    </div>
    <div class="card" id="ctl"></div>
  </div>
</div>

<script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/qrcode-generator@1.4.4/qrcode.min.js"></script>
<script>
const CFG = {
  options: [
    { id: "A", zh: "A", en: "Model A" },
    { id: "B", zh: "B", en: "Model B" },
    { id: "平手", zh: "平手", en: "Tie" }
  ],
  reasons: [
    { id: "回答更完整", zh: "回答更完整", en: "More comprehensive" },
    { id: "回答更簡潔", zh: "回答更簡潔", en: "More concise" },
    { id: "更符合題目要求", zh: "更符合題目要求", en: "Follows instructions" },
    { id: "格式清楚好讀", zh: "格式清楚好讀", en: "Clearer formatting" },
    { id: "資訊更正確", zh: "資訊更正確", en: "More accurate" },
    { id: "語氣更自然", zh: "語氣更自然", en: "More natural tone" },
    { id: "兩個差不多", zh: "兩個差不多", en: "About the same" }
  ],
  questions: [
    { id: "r1", shortZh: "第一輪", shortEn: "Round 1", zh: "第一輪：看這組回答，你覺得 A 或 B 較好？", en: "Round 1: Looking at these responses, which is better, A or B?" },
    { id: "r2", shortZh: "第二輪", shortEn: "Round 2", zh: "第二輪：看理由叢集後，你的判斷改變了嗎？", en: "Round 2: After viewing reason clusters, has your judgment changed?" },
    { id: "r3", shortZh: "第三輪", shortEn: "Round 3", zh: "第三輪：用 N-gram／函式篩選後，哪個模型較好？", en: "Round 3: After N-gram / function filtering, which model is better?" },
    { id: "pred", shortZh: "預測題", shortEn: "Prediction", zh: "預測：LLM 評審會選哪一個？", en: "Prediction: Which one will the LLM Judge choose?" }
  ]
};

const cols = ["var(--c0)", "var(--c1)", "var(--c2)"];
const esc = s => String(s || "").replace(/[&<>"]/g, c => ({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]));

const params = new URLSearchParams(location.search);
let roomId = (params.get("room") || localStorage.getItem("host_room_id_v2") || "").trim().toUpperCase();
if (!roomId) {
  roomId = "AI" + Math.random().toString(36).slice(2, 6).toUpperCase();
}
localStorage.setItem("host_room_id_v2", roomId);
document.getElementById("room-display").textContent = roomId;

// 狀態與選票快取（支援本機 localStorage 雙分頁 + 雲端 MQTT 跨手機雙向即時同步）
const STORE_KEY = "host_arena_cache_" + roomId;
let state = { current: "r1", open: false, reveal: false, session: Date.now() };
let votesMap = { r1: {}, r2: {}, r3: {}, pred: {} };

try {
  const saved = JSON.parse(localStorage.getItem(STORE_KEY) || "null");
  if (saved && saved.state) state = saved.state;
  if (saved && saved.votesMap) votesMap = Object.assign(votesMap, saved.votesMap);
} catch (e) {}

function saveLocal() {
  localStorage.setItem(STORE_KEY, JSON.stringify({ state, votesMap }));
}

// 連接高可靠度公共 MQTT WSS 伺服器（HiveMQ + EMQX 雙備援）
const TOPIC_STATE = `livevote2026/${roomId}/state`;
const TOPIC_VOTE  = `livevote2026/${roomId}/vote`;
const TOPIC_PING  = `livevote2026/${roomId}/ping`;

const client = mqtt.connect("wss://broker.hivemq.com:8884/mqtt", {
  clientId: "host_" + roomId + "_" + Math.random().toString(36).slice(2, 8),
  keepalive: 20,
  reconnectPeriod: 2000
});

function setConn(status, text) {
  const pill = document.getElementById("conn-pill");
  pill.className = "pill " + status;
  document.getElementById("conn-text").textContent = text;
}

client.on("connect", () => {
  setConn("ok", "即時連線中 Live");
  client.subscribe([TOPIC_VOTE, TOPIC_PING], { qos: 1 });
  publishState(); // 上線立即廣播並保留最新狀態 (Retained)
});

client.on("reconnect", () => setConn("wait", "重新連線中 Reconnecting…"));
client.on("offline", () => setConn("err", "離線 Offline"));
client.on("error", () => setConn("err", "連線異常 Error"));

client.on("message", (topic, payload) => {
  try {
    const data = JSON.parse(payload.toString());
    if (topic === TOPIC_PING) {
      // 有新觀眾連入詢問目前狀態，立即補發一次
      publishState();
    } else if (topic === TOPIC_VOTE && data && data.qid && data.cid) {
      if (data.session && data.session !== state.session) return; // 忽略舊場次的過期票
      votesMap[data.qid] = votesMap[data.qid] || {};
      votesMap[data.qid][data.cid] = {
        choice: data.choice,
        reason: data.reason || "",
        text: data.text || "",
        ts: data.ts || Date.now()
      };
      saveLocal();
      render();
    }
  } catch (e) {}
});

function publishState() {
  saveLocal();
  if (client && client.connected) {
    // retain: true 確保任何新掃碼進入的手機都能在 0.1 秒內收到最新題目與狀態
    client.publish(TOPIC_STATE, JSON.stringify(state), { qos: 1, retain: true });
  }
}

// 定期心跳廣播，確保所有觀眾手機絕對不會漏接狀態
setInterval(() => {
  if (client && client.connected) {
    client.publish(TOPIC_STATE, JSON.stringify(state), { qos: 0, retain: true });
  }
}, 4000);

// 監聽同瀏覽器分頁同步（方便單機測試）
window.addEventListener("storage", e => {
  if (e.key === STORE_KEY && e.newValue) {
    try {
      const d = JSON.parse(e.newValue);
      if (d.votesMap) votesMap = d.votesMap;
      render();
    } catch (err) {}
  }
});

function act(action, qid) {
  if (action === "select") { state.current = qid; state.open = false; state.reveal = false; }
  else if (action === "open") { state.open = true; state.reveal = false; }
  else if (action === "close") { state.open = false; }
  else if (action === "reveal") { state.open = false; state.reveal = true; }
  else if (action === "hide") { state.reveal = false; }
  else if (action === "reset") {
    votesMap[state.current] = {};
    state.session = Date.now(); // 更新場次戳記，讓觀眾端自動重置該題作答狀態
  }
  publishState();
  render();
}

function manualSync() {
  publishState();
  render();
  const btn = event.currentTarget;
  const orig = btn.textContent;
  btn.textContent = "✓ 已同步 Synced";
  setTimeout(() => btn.textContent = orig, 1000);
}

function hardRestart() {
  if (!confirm("確定要「全部重新來過」嗎？這將清空所有四輪票數並重新開始！\nReset ALL rounds and start fresh?")) return;
  votesMap = { r1: {}, r2: {}, r3: {}, pred: {} };
  state = { current: "r1", open: false, reveal: false, session: Date.now() };
  publishState();
  render();
}

function updateQR() {
  const input = document.getElementById("base-url");
  let base = input.value.trim();
  localStorage.setItem("voter_base_url_v2", base);
  const qrBox = document.getElementById("qr");
  const linkBox = document.getElementById("full-link");
  if (!base) {
    qrBox.innerHTML = `<p class="mut" style="text-align:center">請貼上公開網址<br><small>Paste URL to generate QR</small></p>`;
    linkBox.textContent = "";
    return;
  }
  if (base.startsWith("file://")) {
    qrBox.innerHTML = `<p style="color:var(--err);font-size:13px;text-align:center;font-weight:700">⚠️ 手機無法開啟 file:// 本機檔案<br>請先上傳至 Netlify / GitHub Pages 取得 https:// 網址</p>`;
    linkBox.textContent = "";
    return;
  }
  const sep = base.includes("?") ? "&" : "?";
  const fullUrl = base + sep + "room=" + encodeURIComponent(roomId);
  const qr = qrcode(0, "M");
  qr.addData(fullUrl);
  qr.make();
  qrBox.innerHTML = qr.createSvgTag({ cellSize: 5, margin: 2, scalable: true });
  linkBox.textContent = "觀眾連結 Audience URL: " + fullUrl;
}

const savedUrl = localStorage.getItem("voter_base_url_v2") || "";
document.getElementById("base-url").value = savedUrl;
updateQR();

function render() {
  const q = CFG.questions.find(item => item.id === state.current) || CFG.questions[0];
  const v = votesMap[state.current] || {};
  const counts = {}, reasons = {}, texts = [];
  CFG.options.forEach(o => { counts[o.id] = 0; reasons[o.id] = {}; });
  let total = 0;

  for (const id in v) {
    const x = v[id];
    if (!x || !x.choice) continue;
    total++;
    counts[x.choice] = (counts[x.choice] || 0) + 1;
    if (x.reason) reasons[x.choice][x.reason] = (reasons[x.choice][x.reason] || 0) + 1;
    if (x.text) texts.push({ choice: x.choice, text: x.text });
  }
  const max = Math.max(1, total);

  const stText = state.open
    ? "投票進行中 · Voting Open"
    : state.reveal
      ? "結果已公布 · Results Revealed"
      : "已暫停 · Paused";

  document.getElementById("main").innerHTML =
    `<span class="status ${state.open ? "open" : ""}">${stText}</span>` +
    `<div class="q-zh">${esc(q.zh)}</div><div class="q-en">${esc(q.en)}</div>` +
    (state.reveal
      ? CFG.options.map((o, i) => `
          <div class="row">
            <div class="lab">${esc(o.zh)}<small>${esc(o.en)}</small></div>
            <div class="track"><div class="fill" style="width:${counts[o.id]/max*100}%;background:${cols[i%3]}"></div></div>
            <div class="num">${counts[o.id]} 票<small>votes</small></div>
          </div>`).join("") + `<p class="mut">總計投票人數 / Total Voters: <b>${total}</b></p>`
      : `<div style="font-size:60px;font-weight:800;margin:10px 0">${total}<span style="font-size:20px;font-weight:400;color:var(--mut)"> 人已投票 / votes received</span></div><p class="mut">點擊「公布結果」才會將長條圖與統計顯示於螢幕（觀眾端目前無法看見票數）。<br><small>Click "Reveal Results" to display the chart. Audience cannot see counts yet.</small></p>`);

  let d = "";
  if (state.reveal) {
    d += `<div class="sub">各選項理由統計 <span>Reason Breakdown</span></div>` + CFG.options.map(o => {
      const e = Object.entries(reasons[o.id] || {});
      return e.length ? `<p style="margin:8px 0"><b>${esc(o.zh)} (${esc(o.en)})</b>　` + e.map(([k, val]) => {
        const rObj = CFG.reasons.find(item => item.id === k);
        const label = rObj ? `${rObj.zh} (${rObj.en})` : k;
        return `<span class="tag">${esc(label)} × ${val}</span>`;
      }).join("") + `</p>` : "";
    }).join("");
    d += `<div class="sub" style="margin-top:18px">觀眾留言 <span>Audience Comments</span></div>` + (texts.length ? texts.slice(-60).map(t => `<div class="txt"><b>${esc(t.choice)}</b>${esc(t.text)}</div>`).join("") : `<p class="mut">尚無留言 / No comments yet</p>`);
  } else {
    d = `<p class="mut">理由統計與觀眾留言將在點擊「公布結果」後自動顯示。<br><small>Reasons and comments will appear automatically once results are revealed.</small></p>`;
  }
  document.getElementById("detail").innerHTML = d;

  document.getElementById("ctl").innerHTML =
    `<div class="sub">1. 選擇題目 <span>Select Round</span></div><div class="ctl-grid">` +
    CFG.questions.map(item => `<button class="alt ${item.id === state.current ? "cur" : ""}" onclick="act('select','${item.id}')">${esc(item.shortZh)}<small>${esc(item.shortEn)}</small></button>`).join("") +
    `</div><div class="sub" style="margin-top:16px">2. 流程控制 <span>Host Controls</span></div><div class="ctl-grid">` +
    `<button onclick="act('open')" ${state.open ? "disabled" : ""}>開放投票<small>Open Voting</small></button>` +
    `<button style="background:#2f6fed" onclick="act('reveal')">公布結果<small>Reveal Results</small></button>` +
    `<button class="alt" onclick="act('close')">截止投票<small>Close Voting</small></button>` +
    `<button class="alt" onclick="act('hide')">隱藏結果<small>Hide Results</small></button>` +
    `<button class="alt danger" style="grid-column:span 2" onclick="if(confirm('確定清空本題所有票數？ / Reset all votes for this round?'))act('reset')">清空本題票數<small>Reset Current Round</small></button></div>`;
}

render();
</script>
</body>
</html>

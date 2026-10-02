# 前端项目------完整代码

## GitHub 信息

-   GitHub 用户名：`cwj268450-art`
-   GitHub 仓库：`Front-end`
-   GitHub 地址：`https://github.com/cwj268450-art/Front-end`

## 完整代码

### 1. `calculator-frontend/src/index.html`

``` html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>前后端分离计算器</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
<main class="page">
<section class="card">
<header><div><small>FRONTEND / BACKEND</small><h1>智能计算器</h1>
<p>表达式由 FastAPI 后端安全解析与计算</p></div><span id="status">检查服务中</span></header>
<div class="calc">
<input id="expression" placeholder="例如：(1+2)*3" autocomplete="off">
<div id="result">0</div>
<div class="keys">
<button data-action="clear">AC</button><button data-value="(">(</button>
<button data-value=")">)</button><button class="op" data-value="/">÷</button>
<button data-value="7">7</button><button data-value="8">8</button>
<button data-value="9">9</button><button class="op" data-value="*">×</button>
<button data-value="4">4</button><button data-value="5">5</button>
<button data-value="6">6</button><button class="op" data-value="-">−</button>
<button data-value="1">1</button><button data-value="2">2</button>
<button data-value="3">3</button><button class="op" data-value="+">+</button>
<button data-value="0">0</button><button data-value=".">.</button>
<button data-action="backspace">⌫</button><button class="equal" data-action="calculate">=</button>
</div>
<div class="quick">
<button data-expression="1+2*3">优先级</button>
<button data-expression="(1+2)*3">括号</button>
<button data-expression="-5+8">负数</button>
<button data-expression="1.5*2">小数</button>
</div>
</div>
</section>
<section class="card history"><header><div><small>DATABASE</small><h2>计算历史</h2></div>
<button id="clear-history">清空全部</button></header>
<div id="history">正在加载...</div></section>
</main>
<script src="app.js"></script>
</body>
</html>
```

### 2. `calculator-frontend/src/style.css`

``` css
*{box-sizing:border-box}body{margin:0;min-height:100vh;font-family:"Segoe UI","Microsoft YaHei",sans-serif;background:#f1f5f9;color:#1e293b}.page{width:min(1050px,92vw);margin:auto;padding:40px 0;display:grid;grid-template-columns:1fr 1fr;gap:22px}.card{background:white;border:1px solid #e2e8f0;border-radius:22px;padding:26px;box-shadow:0 16px 40px #0f172a14}header{display:flex;justify-content:space-between;gap:15px;align-items:flex-start}small{color:#4f46e5;font-weight:700;letter-spacing:.12em}h1,h2{margin:6px 0}p{color:#64748b;margin-top:5px}#status{background:#dcfce7;color:#15803d;padding:7px 10px;border-radius:999px;font-size:12px}.calc{margin-top:20px;background:#111827;padding:18px;border-radius:18px}#expression{width:100%;background:none;border:0;outline:0;color:white;text-align:right;font-size:26px;padding:10px}#expression::placeholder{color:#64748b}#result{min-height:62px;text-align:right;color:#a5b4fc;font-size:36px;padding:8px 4px 16px;overflow-wrap:anywhere}.keys{display:grid;grid-template-columns:repeat(4,1fr);gap:9px}.keys button,.quick button,#clear-history{border:0;border-radius:12px;padding:15px;cursor:pointer}.keys button{background:#1f2937;color:white;font-size:20px}.keys .op{background:#3730a3}.keys .equal{background:#4f46e5}.quick{display:grid;grid-template-columns:repeat(4,1fr);gap:7px;margin-top:12px}.quick button{padding:8px;color:#4338ca;background:#eef2ff;font-size:12px}.history{min-height:500px}.history-list,.item{display:grid;gap:10px}.item{grid-template-columns:1fr auto;align-items:center;padding:14px;border:1px solid #e2e8f0;border-radius:12px;margin-top:10px}.expr{font-weight:700}.res{color:#4f46e5;font-weight:700}.time{font-size:12px;color:#94a3b8}.del{border:0;background:#fee2e2;color:#b91c1c;border-radius:8px;padding:7px;cursor:pointer}.empty{text-align:center;color:#94a3b8;padding:40px}#clear-history{background:#fff1f2;color:#be123c}@media(max-width:800px){.page{grid-template-columns:1fr;padding:20px 0}}
```

### 3. `calculator-frontend/src/app.js`

``` javascript
const API_BASE = "http://127.0.0.1:8000/api";
const input = document.getElementById("expression");
const result = document.getElementById("result");
const history = document.getElementById("history");
const status = document.getElementById("status");

function safe(v) {
    return String(v).replaceAll("&","&amp;").replaceAll("<","&lt;")
        .replaceAll(">","&gt;").replaceAll('"',"&quot;").replaceAll("'","&#039;");
}

async function calculate() {
    const expression = input.value.trim();
    if (!expression) { result.textContent = "请输入表达式"; return; }
    result.textContent = "计算中...";
    try {
        const response = await fetch(`${API_BASE}/calculate`, {
            method:"POST",
            headers:{"Content-Type":"application/json"},
            body:JSON.stringify({expression})
        });
        const data = await response.json();
        if (!response.ok) {
            result.textContent = data.detail?.message || "表达式无效";
            return;
        }
        result.textContent = data.result;
        loadHistory();
    } catch (e) {
        result.textContent = "无法连接后端";
    }
}

async function loadHistory() {
    try {
        const response = await fetch(`${API_BASE}/history`);
        const items = await response.json();
        if (!items.length) {
            history.innerHTML = '<div class="empty">暂无计算历史</div>';
            return;
        }
        history.innerHTML = items.map(item => `
            <div class="item">
                <div><div class="expr">${safe(item.expression)}</div>
                <div class="res">= ${safe(item.result)}</div>
                <div class="time">${safe(item.created_at)}</div></div>
                <button class="del" data-id="${item.id}">删除</button>
            </div>`).join("");
    } catch (e) {
        history.innerHTML = '<div class="empty">无法读取历史，请检查后端</div>';
    }
}

async function deleteItem(id) {
    await fetch(`${API_BASE}/history/${id}`, {method:"DELETE"});
    loadHistory();
}

document.querySelectorAll(".keys button").forEach(button => {
    button.onclick = () => {
        const value = button.dataset.value;
        const action = button.dataset.action;
        if (value !== undefined) input.value += value;
        if (action === "clear") { input.value = ""; result.textContent = "0"; }
        if (action === "backspace") input.value = input.value.slice(0,-1);
        if (action === "calculate") calculate();
        input.focus();
    };
});

document.querySelectorAll(".quick button").forEach(button => {
    button.onclick = () => { input.value = button.dataset.expression; calculate(); };
});
history.onclick = e => { if (e.target.classList.contains("del")) deleteItem(e.target.dataset.id); };
document.getElementById("clear-history").onclick = async () => {
    if (confirm("确定清空全部历史吗？")) {
        await fetch(`${API_BASE}/history`, {method:"DELETE"});
        loadHistory();
    }
};
input.onkeydown = e => { if (e.key === "Enter") calculate(); };

async function health() {
    try {
        const response = await fetch(`${API_BASE}/health`);
        status.textContent = response.ok ? "后端在线" : "后端离线";
    } catch (e) { status.textContent = "后端离线"; }
}
health();
loadHistory();
setInterval(health,10000);
```

### 4. `calculator-frontend/.gitignore`

``` text
.DS_Store
Thumbs.db
```

## GitHub 上传命令

``` powershell
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/cwj268450-art/Front-end.git
git push -u origin main
```

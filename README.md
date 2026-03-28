<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Finance Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet" />
<style>
  :root {
    --bg: #0e0e10;
    --surface: #18181c;
    --surface2: #222228;
    --border: #2e2e38;
    --text: #f0eff4;
    --muted: #7c7c90;
    --income: #3dffa0;
    --expense: #ff5e7d;
    --accent: #c8ff57;
    --radius: 14px;
    --mono: 'DM Mono', monospace;
    --sans: 'Syne', sans-serif;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    min-height: 100vh;
    padding: 2rem 1rem 4rem;
  }

  /* HEADER */
  header {
    max-width: 860px;
    margin: 0 auto 2.5rem;
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 1rem;
    border-bottom: 1px solid var(--border);
    padding-bottom: 1.5rem;
  }

  .logo {
    font-size: 1.6rem;
    font-weight: 800;
    letter-spacing: -0.03em;
  }
  .logo span { color: var(--accent); }

  .theme-toggle {
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--text);
    font-family: var(--mono);
    font-size: 0.75rem;
    padding: 0.45rem 1rem;
    border-radius: 999px;
    cursor: pointer;
    transition: background 0.2s;
  }
  .theme-toggle:hover { background: var(--border); }

  /* BALANCE CARDS */
  .stats {
    max-width: 860px;
    margin: 0 auto 2rem;
    display: grid;
    grid-template-columns: 2fr 1fr 1fr;
    gap: 1rem;
  }

  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.4rem 1.6rem;
  }

  .card-label {
    font-family: var(--mono);
    font-size: 0.68rem;
    color: var(--muted);
    letter-spacing: 0.12em;
    text-transform: uppercase;
    margin-bottom: 0.5rem;
  }

  .card-value {
    font-family: var(--mono);
    font-size: 1.9rem;
    font-weight: 500;
    letter-spacing: -0.02em;
    transition: color 0.3s;
  }

  .card.balance .card-value { color: var(--accent); }
  .card.income-card .card-value { color: var(--income); }
  .card.expense-card .card-value { color: var(--expense); }

  /* MAIN GRID */
  .main {
    max-width: 860px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.2rem;
  }

  /* FORM */
  .panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.6rem;
  }

  .panel-title {
    font-size: 0.9rem;
    font-weight: 700;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 1.2rem;
  }

  .type-toggle {
    display: grid;
    grid-template-columns: 1fr 1fr;
    background: var(--surface2);
    border-radius: 8px;
    padding: 3px;
    margin-bottom: 1rem;
    gap: 3px;
  }

  .type-btn {
    border: none;
    border-radius: 6px;
    padding: 0.55rem;
    font-family: var(--sans);
    font-size: 0.82rem;
    font-weight: 600;
    cursor: pointer;
    background: transparent;
    color: var(--muted);
    transition: all 0.2s;
  }

  .type-btn.active-income {
    background: var(--income);
    color: #000;
  }
  .type-btn.active-expense {
    background: var(--expense);
    color: #fff;
  }

  input, select {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    color: var(--text);
    font-family: var(--mono);
    font-size: 0.85rem;
    padding: 0.65rem 0.9rem;
    margin-bottom: 0.75rem;
    outline: none;
    transition: border-color 0.2s;
  }
  input:focus, select:focus { border-color: var(--accent); }
  input::placeholder { color: var(--muted); }
  select option { background: var(--surface2); }

  .add-btn {
    width: 100%;
    background: var(--accent);
    color: #000;
    border: none;
    border-radius: 8px;
    padding: 0.7rem;
    font-family: var(--sans);
    font-size: 0.9rem;
    font-weight: 700;
    cursor: pointer;
    letter-spacing: 0.04em;
    transition: opacity 0.2s, transform 0.1s;
    margin-top: 0.3rem;
  }
  .add-btn:hover { opacity: 0.85; }
  .add-btn:active { transform: scale(0.98); }

  /* CHART */
  .chart-panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.6rem;
  }

  .bar-list { display: flex; flex-direction: column; gap: 0.65rem; margin-top: 0.5rem; }

  .bar-item { }
  .bar-meta {
    display: flex;
    justify-content: space-between;
    font-family: var(--mono);
    font-size: 0.72rem;
    margin-bottom: 0.3rem;
  }
  .bar-cat { color: var(--text); }
  .bar-amt { color: var(--muted); }

  .bar-track {
    height: 6px;
    background: var(--surface2);
    border-radius: 99px;
    overflow: hidden;
  }

  .bar-fill {
    height: 100%;
    border-radius: 99px;
    background: var(--expense);
    transition: width 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  }
  .bar-fill.income { background: var(--income); }

  .empty-chart {
    font-family: var(--mono);
    font-size: 0.78rem;
    color: var(--muted);
    text-align: center;
    padding: 2rem 0;
  }

  /* TRANSACTIONS */
  .txn-panel {
    grid-column: 1 / -1;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.6rem;
  }

  .txn-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.2rem;
    flex-wrap: wrap;
    gap: 0.6rem;
  }

  .filter-row { display: flex; gap: 0.5rem; }

  .filter-btn {
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--muted);
    font-family: var(--mono);
    font-size: 0.7rem;
    padding: 0.35rem 0.75rem;
    border-radius: 999px;
    cursor: pointer;
    transition: all 0.15s;
  }
  .filter-btn.active {
    background: var(--text);
    color: var(--bg);
    border-color: var(--text);
  }

  .txn-list { display: flex; flex-direction: column; gap: 0.5rem; }

  .txn-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.85rem 1rem;
    background: var(--surface2);
    border-radius: 8px;
    border: 1px solid transparent;
    animation: slideIn 0.25s ease;
    transition: border-color 0.2s;
  }
  .txn-item:hover { border-color: var(--border); }

  @keyframes slideIn {
    from { opacity: 0; transform: translateY(-6px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .txn-left { display: flex; align-items: center; gap: 0.8rem; }

  .txn-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    flex-shrink: 0;
  }
  .txn-dot.income { background: var(--income); }
  .txn-dot.expense { background: var(--expense); }

  .txn-name {
    font-size: 0.88rem;
    font-weight: 600;
  }
  .txn-cat {
    font-family: var(--mono);
    font-size: 0.68rem;
    color: var(--muted);
    margin-top: 2px;
  }

  .txn-right { display: flex; align-items: center; gap: 1rem; }

  .txn-amount {
    font-family: var(--mono);
    font-size: 0.92rem;
    font-weight: 500;
  }
  .txn-amount.income { color: var(--income); }
  .txn-amount.expense { color: var(--expense); }

  .txn-date {
    font-family: var(--mono);
    font-size: 0.68rem;
    color: var(--muted);
    min-width: 60px;
    text-align: right;
  }

  .del-btn {
    background: none;
    border: none;
    color: var(--muted);
    font-size: 1rem;
    cursor: pointer;
    line-height: 1;
    padding: 2px 4px;
    border-radius: 4px;
    transition: color 0.15s, background 0.15s;
  }
  .del-btn:hover { color: var(--expense); background: rgba(255,94,125,0.1); }

  .empty-state {
    font-family: var(--mono);
    font-size: 0.8rem;
    color: var(--muted);
    text-align: center;
    padding: 2.5rem 0;
  }

  /* LIGHT THEME */
  body.light {
    --bg: #f5f4f0;
    --surface: #ffffff;
    --surface2: #eeede8;
    --border: #d8d7d0;
    --text: #111110;
    --muted: #888880;
  }

  @media (max-width: 600px) {
    .stats { grid-template-columns: 1fr 1fr; }
    .stats .balance { grid-column: 1 / -1; }
    .main { grid-template-columns: 1fr; }
    .txn-panel { grid-column: 1; }
    .txn-date { display: none; }
  }
</style>
</head>
<body>

<header>
  <div class="logo">cash<span>.</span>flow</div>
  <button class="theme-toggle" onclick="toggleTheme()">◐ Toggle Theme</button>
</header>

<div class="stats">
  <div class="card balance">
    <div class="card-label">Net Balance</div>
    <div class="card-value" id="balance">$0.00</div>
  </div>
  <div class="card income-card">
    <div class="card-label">Total Income</div>
    <div class="card-value" id="total-income">$0.00</div>
  </div>
  <div class="card expense-card">
    <div class="card-label">Total Expenses</div>
    <div class="card-value" id="total-expenses">$0.00</div>
  </div>
</div>

<div class="main">
  <!-- FORM -->
  <div class="panel">
    <div class="panel-title">Add Transaction</div>
    <div class="type-toggle">
      <button class="type-btn active-income" id="btn-income" onclick="setType('income')">＋ Income</button>
      <button class="type-btn" id="btn-expense" onclick="setType('expense')">− Expense</button>
    </div>
    <input type="text" id="desc" placeholder="Description" />
    <input type="number" id="amount" placeholder="Amount (e.g. 120)" min="0" step="0.01" />
    <select id="category">
      <option value="">Category</option>
      <option>Food</option>
      <option>Transport</option>
      <option>Housing</option>
      <option>Entertainment</option>
      <option>Health</option>
      <option>Shopping</option>
      <option>Salary</option>
      <option>Freelance</option>
      <option>Investment</option>
      <option>Other</option>
    </select>
    <button class="add-btn" onclick="addTransaction()">Add Transaction</button>
  </div>

  <!-- CHART -->
  <div class="chart-panel">
    <div class="panel-title">Spending by Category</div>
    <div class="bar-list" id="bar-list">
      <div class="empty-chart">No data yet</div>
    </div>
  </div>

  <!-- TRANSACTIONS -->
  <div class="txn-panel">
    <div class="txn-header">
      <div class="panel-title" style="margin:0">Transactions</div>
      <div class="filter-row">
        <button class="filter-btn active" onclick="setFilter('all', this)">All</button>
        <button class="filter-btn" onclick="setFilter('income', this)">Income</button>
        <button class="filter-btn" onclick="setFilter('expense', this)">Expense</button>
      </div>
    </div>
    <div class="txn-list" id="txn-list">
      <div class="empty-state">No transactions yet. Add one above!</div>
    </div>
  </div>
</div>

<script>
  let transactions = JSON.parse(localStorage.getItem('cf_txns') || '[]');
  let currentType = 'income';
  let currentFilter = 'all';

  function setType(t) {
    currentType = t;
    document.getElementById('btn-income').className = 'type-btn' + (t === 'income' ? ' active-income' : '');
    document.getElementById('btn-expense').className = 'type-btn' + (t === 'expense' ? ' active-expense' : '');
  }

  function setFilter(f, btn) {
    currentFilter = f;
    document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    renderTransactions();
  }

  function addTransaction() {
    const desc = document.getElementById('desc').value.trim();
    const amt = parseFloat(document.getElementById('amount').value);
    const cat = document.getElementById('category').value || 'Other';
    if (!desc || isNaN(amt) || amt <= 0) return;

    transactions.unshift({
      id: Date.now(),
      type: currentType,
      desc,
      amount: amt,
      category: cat,
      date: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric' })
    });

    save();
    document.getElementById('desc').value = '';
    document.getElementById('amount').value = '';
    document.getElementById('category').value = '';
  }

  function deleteTransaction(id) {
    transactions = transactions.filter(t => t.id !== id);
    save();
  }

  function save() {
    localStorage.setItem('cf_txns', JSON.stringify(transactions));
    render();
  }

  function fmt(n) {
    return '$' + n.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',');
  }

  function render() {
    const income = transactions.filter(t => t.type === 'income').reduce((s, t) => s + t.amount, 0);
    const expenses = transactions.filter(t => t.type === 'expense').reduce((s, t) => s + t.amount, 0);
    const balance = income - expenses;

    document.getElementById('total-income').textContent = fmt(income);
    document.getElementById('total-expenses').textContent = fmt(expenses);
    const balEl = document.getElementById('balance');
    balEl.textContent = fmt(balance);
    balEl.style.color = balance < 0 ? 'var(--expense)' : balance === 0 ? 'var(--muted)' : 'var(--accent)';

    renderChart(expenses);
    renderTransactions();
  }

  function renderChart(totalExpenses) {
    const expTxns = transactions.filter(t => t.type === 'expense');
    const catMap = {};
    expTxns.forEach(t => { catMap[t.category] = (catMap[t.category] || 0) + t.amount; });
    const cats = Object.entries(catMap).sort((a, b) => b[1] - a[1]);

    const el = document.getElementById('bar-list');
    if (!cats.length) { el.innerHTML = '<div class="empty-chart">No expense data yet</div>'; return; }

    const max = cats[0][1];
    el.innerHTML = cats.map(([cat, amt]) => `
      <div class="bar-item">
        <div class="bar-meta">
          <span class="bar-cat">${cat}</span>
          <span class="bar-amt">${fmt(amt)}</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" style="width: ${(amt / max * 100).toFixed(1)}%"></div>
        </div>
      </div>
    `).join('');
  }

  function renderTransactions() {
    const filtered = currentFilter === 'all' ? transactions : transactions.filter(t => t.type === currentFilter);
    const el = document.getElementById('txn-list');
    if (!filtered.length) {
      el.innerHTML = '<div class="empty-state">No transactions here yet.</div>';
      return;
    }
    el.innerHTML = filtered.map(t => `
      <div class="txn-item">
        <div class="txn-left">
          <div class="txn-dot ${t.type}"></div>
          <div>
            <div class="txn-name">${t.desc}</div>
            <div class="txn-cat">${t.category}</div>
          </div>
        </div>
        <div class="txn-right">
          <span class="txn-amount ${t.type}">${t.type === 'income' ? '+' : '−'}${fmt(t.amount)}</span>
          <span class="txn-date">${t.date}</span>
          <button class="del-btn" onclick="deleteTransaction(${t.id})" title="Delete">×</button>
        </div>
      </div>
    `).join('');
  }

  function toggleTheme() {
    document.body.classList.toggle('light');
  }

  render();
</script>
</body>
</html>

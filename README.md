# agent-passport-registry
<!doctype html>
<html lang="en-GB">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Agent Passport Registry</title>
<meta name="description" content="Every AI agent in use: who owns it, what it can reach, and when its authority ends.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Public+Sans:wght@400;500;700;800&family=IBM+Plex+Mono:wght@500&display=swap" rel="stylesheet">
<style>
:root{
  --paper:#E9EEEA; --paper-2:#F4F7F5; --ink:#18233A; --ink-2:#4A5568; --rule:#BCC7C1;
  --cover:#5E1A26; --cover-ink:#F1E6DC; --gilt:#C9A86A;
  --expired:#A3312B; --overdue:#B0521A; --soon:#7E6210; --ok:#2E6A4A; --inactive:#6B7280;
  --guilloche:rgba(46,106,74,.07);
  --sans:"Public Sans",system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;
  --mono:"IBM Plex Mono",ui-monospace,"SFMono-Regular",Menlo,Consolas,monospace;
  box-sizing:border-box;
  padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px);
}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){
  --paper:#111824; --paper-2:#182131; --ink:#DCE4DF; --ink-2:#9AA6B2; --rule:#2C3748;
  --cover:#3E1119; --cover-ink:#EBDDD0; --gilt:#C9A86A;
  --expired:#E07A72; --overdue:#E39A5E; --soon:#D9BB5A; --ok:#7CC4A0; --inactive:#8B95A3;
  --guilloche:rgba(124,196,160,.06);
}}
:root[data-theme="dark"]{
  --paper:#111824; --paper-2:#182131; --ink:#DCE4DF; --ink-2:#9AA6B2; --rule:#2C3748;
  --cover:#3E1119; --cover-ink:#EBDDD0; --gilt:#C9A86A;
  --expired:#E07A72; --overdue:#E39A5E; --soon:#D9BB5A; --ok:#7CC4A0; --inactive:#8B95A3;
  --guilloche:rgba(124,196,160,.06);
}
*,*::before,*::after{box-sizing:inherit}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--paper);color:var(--ink);font:400 1rem/1.55 var(--sans);-webkit-font-smoothing:antialiased}
a{color:inherit}
:focus-visible{outline:3px solid var(--gilt);outline-offset:2px}

.cover{background:var(--cover);color:var(--cover-ink);padding:3.5rem 1.25rem 3rem;position:relative;overflow:hidden}
.cover::after{content:"";position:absolute;inset:0;pointer-events:none;opacity:.22;
  background:repeating-radial-gradient(circle at 110% -10%,transparent 0 14px,var(--gilt) 14px 15px)}
.cover-inner{max-width:68rem;margin:0 auto;position:relative;z-index:1}
.cover h1{font-weight:800;font-size:clamp(2.2rem,6vw,4rem);line-height:1.02;letter-spacing:-.02em;margin:0 0 1rem;max-width:14ch}
.cover p{margin:0;max-width:40rem;font-size:1.125rem;opacity:.92}
.cover .seal{display:inline-block;width:2.75rem;height:2.75rem;border:1.5px solid var(--gilt);border-radius:50%;margin-bottom:1.5rem;
  background:radial-gradient(circle,transparent 55%,var(--gilt) 56% 59%,transparent 60%)}

main{max-width:68rem;margin:0 auto;padding:2.5rem 1.25rem 4rem}
.summary{font-size:clamp(1.25rem,2.6vw,1.6rem);font-weight:500;line-height:1.35;max-width:46rem;margin:0 0 2.5rem}
.summary b{font-weight:800}
.summary .bad{color:var(--expired)}

.controls{display:flex;flex-wrap:wrap;gap:.75rem;align-items:end;margin-bottom:1rem}
.controls label{display:flex;flex-direction:column;gap:.3rem;font-size:.875rem;color:var(--ink-2);font-weight:500}
.controls input,.controls select{font:inherit;color:var(--ink);background:var(--paper-2);border:1px solid var(--rule);border-radius:6px;padding:.5rem .65rem;min-width:11rem}
.controls input{min-width:15rem}

.table-wrap{overflow-x:auto;border-top:2px solid var(--ink)}
table{width:100%;border-collapse:collapse;min-width:44rem}
th{text-align:left;font-size:.875rem;font-weight:700;color:var(--ink-2);padding:.75rem .75rem;border-bottom:1px solid var(--rule)}
th button{all:unset;cursor:pointer}
th button:focus-visible{outline:3px solid var(--gilt)}
th[aria-sort="ascending"] button::after{content:" ▲";font-size:.7em}
th[aria-sort="descending"] button::after{content:" ▼";font-size:.7em}
td{padding:.9rem .75rem;border-bottom:1px solid var(--rule);vertical-align:top}
tbody tr{cursor:pointer}
td.date{white-space:nowrap}
tbody tr:hover{background:var(--paper-2)}
.agent-name{font-weight:700}
.agent-name button{all:unset;cursor:pointer;text-decoration:underline;text-decoration-color:var(--rule);text-underline-offset:3px}
.agent-name button:focus-visible{outline:3px solid var(--gilt)}
.sub{display:block;font-size:.875rem;font-weight:400;color:var(--ink-2)}
.state{display:inline-flex;align-items:center;gap:.45rem;font-weight:700;font-size:.9rem;white-space:nowrap}
.state::before{content:"";width:.65rem;height:.65rem;border-radius:50%;background:currentColor}
.state.expired{color:var(--expired)} .state.overdue{color:var(--overdue)} .state.due-soon{color:var(--soon)}
.state.ok{color:var(--ok)} .state.inactive{color:var(--inactive)}
.state.expired::before{border-radius:1px;transform:rotate(45deg)}
.empty{padding:2rem .75rem;color:var(--ink-2)}

footer{max-width:68rem;margin:0 auto;padding:0 1.25rem 3rem;color:var(--ink-2);font-size:.875rem}

/* Passport data page */
dialog{border:none;padding:0;width:min(46rem,calc(100% - 1.5rem));max-height:calc(100% - 2rem);border-radius:10px;
  background:var(--paper-2);color:var(--ink);box-shadow:0 30px 80px rgba(10,15,25,.45)}
dialog::backdrop{background:rgba(15,20,30,.6)}
.page{position:relative;padding:1.75rem 1.75rem 0;
  background-image:repeating-radial-gradient(ellipse at 0 100%,transparent 0 9px,var(--guilloche) 9px 10px),
                   repeating-radial-gradient(ellipse at 100% 0,transparent 0 11px,var(--guilloche) 11px 12px)}
.page-head{display:flex;justify-content:space-between;gap:1rem;align-items:start;border-bottom:1px solid var(--rule);padding-bottom:1rem;margin-bottom:1.25rem}
.page-head h2{margin:0;font-size:1.6rem;line-height:1.15;font-weight:800}
.page-head .sub{margin-top:.25rem}
.close{font:inherit;font-weight:700;background:none;border:1px solid var(--rule);color:var(--ink);border-radius:6px;padding:.35rem .7rem;cursor:pointer}
.purpose{font-size:1.05rem;margin:0 0 1.5rem;max-width:60ch}
.fields{display:grid;grid-template-columns:repeat(auto-fit,minmax(13rem,1fr));gap:1rem 1.5rem;margin:0 0 1.5rem}
.fields div{min-width:0}
.fields dt{font-size:.8rem;color:var(--ink-2);font-weight:500}
.fields dd{margin:.1rem 0 0;font-weight:700;overflow-wrap:anywhere}
.section{margin:0 0 1.4rem}
.section h3{font-size:1rem;margin:0 0 .4rem;font-weight:800}
.section ul{margin:0;padding-left:1.1rem}
.section li{margin:.15rem 0}
.systems{width:100%;min-width:0;border-collapse:collapse;font-size:.95rem}
.systems td{padding:.4rem .5rem .4rem 0;border-bottom:1px dashed var(--rule);cursor:default}
.access{font-weight:700}
.access.write{color:var(--overdue)} .access.admin{color:var(--expired)}
.limits{display:grid;grid-template-columns:repeat(auto-fit,minmax(12rem,1fr));gap:1rem 1.5rem}
.note{border-left:3px solid var(--overdue);padding:.4rem .8rem;background:var(--paper);margin:0 0 1.4rem}
.source{display:inline-block;margin:0 0 1.25rem;font-weight:500}
.mrz{margin:0 -1.75rem;padding:1rem 1.75rem 1.2rem;background:var(--paper);border-top:1px solid var(--rule);
  font:500 clamp(.62rem,1.9vw,.9rem)/1.6 var(--mono);letter-spacing:.12em;white-space:pre;overflow-x:auto;color:var(--ink);border-radius:0 0 10px 10px}

@media (prefers-reduced-motion:no-preference){
  dialog[open]{animation:open .22s ease-out}
  @keyframes open{from{opacity:0;transform:translateY(12px) scale(.98)}to{opacity:1;transform:none}}
}
@media (max-width:40rem){
  .cover{padding:2.5rem 1.25rem 2.25rem}
  .controls input,.controls select{min-width:0;width:100%}
  .controls label{flex:1 1 100%}
  .page{padding:1.25rem 1.1rem 0}
  .mrz{margin:0 -1.1rem;padding:1rem 1.1rem}
}
</style>
</head>
<body>
<header class="cover">
  <div class="cover-inner">
    <span class="seal" aria-hidden="true"></span>
    <h1>Agent Passport Registry</h1>
    <p>Every AI agent in use: who owns it, what it can reach, and when its authority ends.</p>
  </div>
</header>

<main>
  <p class="summary" id="summary" aria-live="polite"></p>

  <div class="controls" role="search">
    <label>Search
      <input id="q" type="search" placeholder="Agent, owner or system" autocomplete="off">
    </label>
    <label>Review status
      <select id="f-state">
        <option value="">All</option>
        <option value="expired">Access expired</option>
        <option value="overdue">Review overdue</option>
        <option value="due-soon">Due in 14 days</option>
        <option value="ok">Up to date</option>
        <option value="inactive">Suspended, revoked or retired</option>
      </select>
    </label>
    <label>Risk
      <select id="f-risk"><option value="">All</option><option>high</option><option>medium</option><option>low</option></select>
    </label>
    <label>Owner
      <select id="f-owner"><option value="">All</option></select>
    </label>
  </div>

  <div class="table-wrap">
    <table>
      <thead><tr>
        <th scope="col" data-key="name"><button type="button">Agent</button></th>
        <th scope="col" data-key="owner"><button type="button">Owner</button></th>
        <th scope="col" data-key="risk"><button type="button">Risk</button></th>
        <th scope="col" data-key="state" aria-sort="ascending"><button type="button">Review status</button></th>
        <th scope="col" data-key="last"><button type="button">Last review</button></th>
        <th scope="col" data-key="expires"><button type="button">Access expires</button></th>
      </tr></thead>
      <tbody id="rows"></tbody>
    </table>
  </div>
</main>

<footer>
  <p>Built from the passports in <a href="#">the registry repository</a> on 05 October 2026, 10:29 UTC. Review status is recalculated each time you open this page.</p>
</footer>

<dialog id="passport" aria-labelledby="p-title"></dialog>

<script>
const AGENTS = [{"id": "finance-reconciliation", "name": "Finance Reconciliation Agent", "status": "active", "purpose": "Matches bank statement lines against open invoices each night and produces an exceptions report for the accounts team. It proposes matches; a person confirms them.\n", "owner": {"name": "Helen Achterberg", "role": "Financial Controller", "contact": "helen.achterberg@example.com"}, "backup_owner": {"name": "Rohit Menon", "role": "Accounts Payable Manager", "contact": "rohit.menon@example.com"}, "authorised_by": {"name": "Daniel Okafor", "role": "AI Governance Lead", "contact": "daniel.okafor@example.com", "date": "2026-07-01"}, "technical_identity": {"type": "service_account", "identifier": "svc-fin-recon", "shared_with_human": false}, "risk_level": "high", "data_sensitivity": "restricted", "connected_systems": [{"system": "ERP (accounts receivable)", "access": "read"}, {"system": "Bank statement feed", "access": "read"}, {"system": "Finance shared drive", "access": "write", "scope": "/Reconciliation/Exceptions only"}], "allowed_actions": ["Read open invoices and bank statement lines", "Propose matches between payments and invoices", "Write the nightly exceptions report"], "prohibited_actions": ["Posting journal entries", "Editing vendor bank details", "Approving or releasing payments"], "approval_triggers": ["Any proposed match where the amounts differ", "Any exception above the materiality threshold"], "policy": "policies/finance-ops.yaml", "assessment": "assessments/finance-reconciliation.md", "dates": {"approved": "2026-07-01", "last_review": "2026-08-28", "next_review": "2026-09-27", "access_expires": "2026-09-29"}, "revocation": {"method": "Disable svc-fin-recon; rotate the bank feed read key; pause the nightly scheduler job.", "contact": "finance-systems@example.com", "target_minutes": 30}, "audit_log": {"location": "ERP audit log plus \"agents-prod\" log name finance-reconciliation", "retention_days": 2555}, "disclosure": {"people_affected": "Accounts team (8 people)", "notified": true, "notice": "templates/employee-disclosure.md"}, "notes": "Review overdue. Owner on leave; backup owner asked to complete the September review.", "_source": ""}, {"id": "research-assistant", "name": "Market Research Assistant", "status": "active", "purpose": "Summarises published industry reports and competitor announcements into a weekly briefing for the strategy team. Reads public sources and the shared research folder only.\n", "owner": {"name": "Priya Raman", "role": "Head of Strategy", "contact": "priya.raman@example.com"}, "authorised_by": {"name": "Daniel Okafor", "role": "AI Governance Lead", "contact": "daniel.okafor@example.com", "date": "2026-08-12"}, "technical_identity": {"type": "service_account", "identifier": "svc-research-assistant", "shared_with_human": false}, "risk_level": "low", "data_sensitivity": "internal", "connected_systems": [{"system": "Web search", "access": "read"}, {"system": "Google Drive", "access": "read", "scope": "/Strategy/Research only"}, {"system": "Slack", "access": "write", "scope": "#strategy-briefings channel only"}], "allowed_actions": ["Search and read public web pages", "Read files in the research folder", "Post the weekly briefing to the strategy channel"], "prohibited_actions": ["Reading files outside the research folder", "Messaging individuals directly", "Contacting anyone outside the organisation"], "approval_triggers": ["Including any figure in the briefing that it could not source to a link"], "policy": "policies/org-baseline.yaml", "assessment": "assessments/research-assistant.md", "dates": {"approved": "2026-08-12", "last_review": "2026-08-12", "next_review": "2027-02-08", "access_expires": "2027-08-11"}, "revocation": {"method": "Disable svc-research-assistant in the identity console; remove the Slack app from the workspace.", "contact": "it-helpdesk@example.com", "target_minutes": 240}, "audit_log": {"location": "Cloud logging project \"agents-prod\", log name research-assistant", "retention_days": 365}, "disclosure": {"people_affected": "Strategy team (12 people)", "notified": true, "notice": "templates/employee-disclosure.md"}, "_source": ""}, {"id": "support-triage", "name": "Customer Support Triage Agent", "status": "active", "purpose": "Reads incoming support tickets, tags them by product and urgency, and drafts a suggested first reply for a human agent to edit and send. It never sends replies to customers itself.\n", "owner": {"name": "Marcus Lindqvist", "role": "Support Operations Manager", "contact": "marcus.lindqvist@example.com"}, "backup_owner": {"name": "Aisha Bello", "role": "Senior Support Lead", "contact": "aisha.bello@example.com"}, "authorised_by": {"name": "Daniel Okafor", "role": "AI Governance Lead", "contact": "daniel.okafor@example.com", "date": "2026-04-14"}, "technical_identity": {"type": "oauth_app", "identifier": "helpdesk-app-triage-01", "shared_with_human": false}, "risk_level": "medium", "data_sensitivity": "confidential", "connected_systems": [{"system": "Helpdesk", "access": "write", "scope": "Tags and internal draft notes only"}, {"system": "Product knowledge base", "access": "read"}], "allowed_actions": ["Read new tickets and customer history on the ticket", "Apply product and urgency tags", "Write an internal draft reply visible only to staff"], "prohibited_actions": ["Sending any message to a customer", "Issuing refunds or credits", "Closing or merging tickets", "Editing customer account details"], "approval_triggers": ["Tagging a ticket as a legal, safety or data-protection issue", "Escalating a ticket to an executive queue"], "policy": "policies/org-baseline.yaml", "assessment": "assessments/support-triage.md", "dates": {"approved": "2026-04-14", "last_review": "2026-07-16", "next_review": "2026-10-14", "access_expires": "2026-10-16"}, "revocation": {"method": "Revoke the helpdesk OAuth token from the admin panel; disable the triage automation rule.", "contact": "support-ops-oncall@example.com", "target_minutes": 60}, "audit_log": {"location": "Helpdesk audit trail plus \"agents-prod\" log name support-triage", "retention_days": 730}, "disclosure": {"people_affected": "Support team (40 people); customers are told replies may be AI-assisted", "notified": true, "notice": "templates/employee-disclosure.md"}, "_source": ""}];
const DUE_SOON = 14;
const STATE_ORDER = {expired:0, overdue:1, "due-soon":2, ok:3, inactive:4};
const STATE_LABEL = {expired:"Access expired", overdue:"Review overdue", "due-soon":"Due soon", ok:"Up to date", inactive:"Inactive"};
const RISK_ORDER = {high:0, medium:1, low:2};
const today = new Date(); today.setHours(0,0,0,0);
const d = s => { const [y,m,dd] = s.split("-").map(Number); return new Date(y, m-1, dd); };
const days = (a,b) => Math.round((a-b)/86400000);
const fmt = s => d(s).toLocaleDateString("en-GB",{day:"numeric",month:"short",year:"numeric"});
const esc = s => String(s ?? "").replace(/[&<>"']/g, c => ({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));

function stateOf(a){
  if (["revoked","retired","suspended"].includes(a.status)) return "inactive";
  const exp = d(a.dates.access_expires), rev = d(a.dates.next_review);
  if (exp < today) return "expired";
  if (rev < today) return "overdue";
  if (days(exp < rev ? exp : rev, today) <= DUE_SOON) return "due-soon";
  return "ok";
}
function stateDetail(a, s){
  const exp = d(a.dates.access_expires), rev = d(a.dates.next_review);
  if (s === "expired") return `${days(today,exp)} days ago`;
  if (s === "overdue") return `${days(today,rev)} days late`;
  if (s === "due-soon") { const n = days(exp < rev ? exp : rev, today); return n === 0 ? "today" : `in ${n} day${n===1?"":"s"}`; }
  if (s === "inactive") return a.status;
  return `next ${fmt(a.dates.next_review)}`;
}

AGENTS.forEach(a => a._state = stateOf(a));

// Summary sentence
(function(){
  const live = AGENTS.filter(a => a._state !== "inactive");
  const n = k => AGENTS.filter(a => a._state === k).length;
  const plural = (x,w) => `${x} ${w}${x===1?"":"s"}`;
  const bad = n("expired") + n("overdue"), soon = n("due-soon");
  const parts = [];
  if (bad) parts.push(`<b class="bad">${bad} ${bad===1?"is":"are"} past ${bad===1?"its":"their"} review or expiry date</b>`);
  if (soon) parts.push(`<b>${soon}</b> ${soon===1?"needs":"need"} review in the next two weeks`);
  let s = `<b>${plural(live.length,"agent")}</b> currently hold access. `;
  s += parts.length ? parts.join(", and ") + "." : "All reviews are up to date.";
  document.getElementById("summary").innerHTML = s;
})();

// Owner filter
const owners = [...new Set(AGENTS.map(a => a.owner.name))].sort();
const fOwner = document.getElementById("f-owner");
owners.forEach(o => fOwner.add(new Option(o, o)));

let sortKey = "state", sortDir = 1;
const keyFn = {
  name: a => a.name.toLowerCase(), owner: a => a.owner.name.toLowerCase(),
  risk: a => RISK_ORDER[a.risk_level], state: a => STATE_ORDER[a._state],
  last: a => a.dates.last_review, expires: a => a.dates.access_expires
};

function render(){
  const q = document.getElementById("q").value.trim().toLowerCase();
  const fs = document.getElementById("f-state").value, fr = document.getElementById("f-risk").value, fo = fOwner.value;
  const list = AGENTS.filter(a => {
    if (fs && a._state !== fs) return false;
    if (fr && a.risk_level !== fr) return false;
    if (fo && a.owner.name !== fo) return false;
    if (!q) return true;
    const hay = [a.name, a.id, a.owner.name, a.owner.role, a.purpose, ...a.connected_systems.map(s=>s.system)].join(" ").toLowerCase();
    return hay.includes(q);
  }).sort((x,y) => {
    const a = keyFn[sortKey](x), b = keyFn[sortKey](y);
    return (a < b ? -1 : a > b ? 1 : 0) * sortDir || x.dates.access_expires.localeCompare(y.dates.access_expires);
  });
  const tb = document.getElementById("rows");
  if (!list.length){ tb.innerHTML = `<tr><td colspan="6" class="empty">No agents match these filters. Clear the search or set a filter back to All.</td></tr>`; return; }
  tb.innerHTML = list.map(a => `
    <tr data-id="${esc(a.id)}">
      <td class="agent-name"><button type="button" data-open="${esc(a.id)}">${esc(a.name)}</button><span class="sub">${esc(a.id)}</span></td>
      <td>${esc(a.owner.name)}<span class="sub">${esc(a.owner.role)}</span></td>
      <td>${esc(a.risk_level)}<span class="sub">${esc(a.data_sensitivity)} data</span></td>
      <td><span class="state ${a._state}">${STATE_LABEL[a._state]}</span><span class="sub">${esc(stateDetail(a,a._state))}</span></td>
      <td class="date">${fmt(a.dates.last_review)}</td>
      <td class="date">${fmt(a.dates.access_expires)}</td>
    </tr>`).join("");
}

document.querySelectorAll("th[data-key] button").forEach(btn => btn.addEventListener("click", () => {
  const th = btn.parentElement, k = th.dataset.key;
  sortDir = (sortKey === k) ? -sortDir : 1; sortKey = k;
  document.querySelectorAll("th[data-key]").forEach(t => t.removeAttribute("aria-sort"));
  th.setAttribute("aria-sort", sortDir === 1 ? "ascending" : "descending");
  render();
}));
["q","f-state","f-risk","f-owner"].forEach(id => document.getElementById(id).addEventListener("input", render));
document.getElementById("rows").addEventListener("click", e => {
  const tr = e.target.closest("tr[data-id]"); if (tr) openPassport(tr.dataset.id);
});

// Machine-readable zone, 44 characters per line like a real passport
function mrz(a){
  const clean = s => String(s).toUpperCase().replace(/[^A-Z0-9]+/g,"<").replace(/^<|<$/g,"");
  const pad = s => (s + "<".repeat(44)).slice(0,44);
  const ymd = s => s.slice(2).replace(/-/g,"");
  const surname = clean(a.owner.name.split(" ").slice(-1)[0]);
  const l1 = pad(`AP<${clean(a.id)}<<${clean(a.status)}`);
  const l2 = pad(`${clean(a.risk_level)}<${clean(a.data_sensitivity)}<EXP${ymd(a.dates.access_expires)}<OWN<${surname}`);
  return l1 + "\n" + l2;
}

const dlg = document.getElementById("passport");
let lastFocus = null;
function openPassport(id){
  const a = AGENTS.find(x => x.id === id); if (!a) return;
  lastFocus = document.activeElement;
  const li = arr => arr.map(x => `<li>${esc(x)}</li>`).join("");
  const person = p => p ? `${esc(p.name)}<span class="sub">${esc(p.role)}</span>` : "None named";
  dlg.innerHTML = `
  <article class="page">
    <div class="page-head">
      <div><h2 id="p-title">${esc(a.name)}</h2>
        <span class="state ${a._state}">${STATE_LABEL[a._state]}</span> <span class="sub" style="display:inline">${esc(stateDetail(a,a._state))}</span></div>
      <button class="close" type="button" data-close>Close</button>
    </div>
    <p class="purpose">${esc(a.purpose)}</p>
    <dl class="fields">
      <div><dt>Owner</dt><dd>${person(a.owner)}</dd></div>
      <div><dt>Backup owner</dt><dd>${person(a.backup_owner)}</dd></div>
      <div><dt>Authorised by</dt><dd>${person(a.authorised_by)}<span class="sub">on ${fmt(a.authorised_by.date)}</span></dd></div>
      <div><dt>Runs as</dt><dd>${esc(a.technical_identity.identifier)}<span class="sub">${esc(a.technical_identity.type.replace("_"," "))}</span></dd></div>
      <div><dt>Risk and data</dt><dd>${esc(a.risk_level)} risk<span class="sub">${esc(a.data_sensitivity)} data</span></dd></div>
      <div><dt>Status</dt><dd>${esc(a.status)}</dd></div>
      <div><dt>Last review</dt><dd>${fmt(a.dates.last_review)}</dd></div>
      <div><dt>Next review</dt><dd>${fmt(a.dates.next_review)}</dd></div>
      <div><dt>Access expires</dt><dd>${fmt(a.dates.access_expires)}</dd></div>
    </dl>
    ${a.notes ? `<p class="note">${esc(a.notes)}</p>` : ""}
    <section class="section"><h3>What it can reach</h3>
      <table class="systems"><tbody>${a.connected_systems.map(s => `<tr><td>${esc(s.system)}${s.scope?`<span class="sub">${esc(s.scope)}</span>`:""}</td><td class="access ${esc(s.access)}">${esc(s.access)}</td></tr>`).join("")}</tbody></table>
    </section>
    <div class="limits">
      <section class="section"><h3>Allowed to</h3><ul>${li(a.allowed_actions)}</ul></section>
      <section class="section"><h3>Never allowed to</h3><ul>${li(a.prohibited_actions)}</ul></section>
      <section class="section"><h3>Needs a person to approve</h3><ul>${li(a.approval_triggers)}</ul></section>
    </div>
    <section class="section"><h3>How to switch it off</h3>
      <p style="margin:0">${esc(a.revocation.method)} Contact ${esc(a.revocation.contact)}; target ${a.revocation.target_minutes} minutes.</p></section>
    <section class="section"><h3>Where its actions are logged</h3>
      <p style="margin:0">${esc(a.audit_log.location)}, kept ${a.audit_log.retention_days} days.</p></section>
    ${a.disclosure ? `<section class="section"><h3>People affected</h3><p style="margin:0">${esc(a.disclosure.people_affected || "")}${a.disclosure.notified === false ? " Not yet notified." : ""}</p></section>` : ""}
    ${a._source ? `<a class="source" href="${esc(a._source)}">Open the passport file on GitHub</a>` : ""}
    <div class="mrz" aria-label="Machine-readable summary">${esc(mrz(a))}</div>
  </article>`;
  dlg.showModal();
  dlg.querySelector("[data-close]").focus();
  history.replaceState(null, "", "#" + a.id);
}
dlg.addEventListener("click", e => { if (e.target === dlg || e.target.closest("[data-close]")) dlg.close(); });
dlg.addEventListener("close", () => { history.replaceState(null, "", location.pathname); lastFocus && lastFocus.focus(); });

render();
if (location.hash) openPassport(decodeURIComponent(location.hash.slice(1)));
</script>
</body>
</html>

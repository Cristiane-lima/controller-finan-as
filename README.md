# controller-finan-as
Pequeno aplicativo que tem como finalidade auxiliar no processo de controle financeiro diãrio


<!doctype html><html><head><meta charset=utf8><meta name=viewport content="width=device-width,initial-scale=1,viewport-fit=cover"><style>:root{color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}html{scroll-padding-top:env(safe-area-inset-top,0px)}body{margin:0;padding:0;font:14px -apple-system,BlinkMacSystemFont,sans-serif;background:#fff;color:#000}img{max-width:100%}[hidden]:not([hidden=until-found i]){display:none!important}</style></head><body>
<title>Controller Finanças</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Space+Grotesk:wght@500;600;700&display=swap">
<style>
/* Layout: tela de acesso centralizada; painel em uma coluna com resumo no topo, formulário, gráfico diário e extrato. */
:root {
  --bg: #eef2f0;
  --surface: #ffffff;
  --surface-2: #e5ebe8;
  --ink: #14211e;
  --muted: #5a6b66;
  --line: #d3dcd8;
  --accent: #0e6b55;
  --accent-ink: #ffffff;
  --fixed: #3b5b8c;
  --daily: #0e6b55;
  --bad: #b3412f;
  --good: #1d7a4f;
  --font-display: "Space Grotesk", "Segoe UI", system-ui, sans-serif;
  --font-body: "Inter", "Segoe UI", system-ui, sans-serif;
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #0d1412; --surface: #152220; --surface-2: #1d2f2b; --ink: #e5eeeb; --muted: #93a8a1;
    --line: #28403a; --accent: #4fc3a1; --accent-ink: #08201a; --fixed: #86a9de; --daily: #4fc3a1;
    --bad: #f08a76; --good: #62d19a; color-scheme: dark;
  }
}
:root[data-theme="dark"] {
  --bg: #0d1412; --surface: #152220; --surface-2: #1d2f2b; --ink: #e5eeeb; --muted: #93a8a1;
  --line: #28403a; --accent: #4fc3a1; --accent-ink: #08201a; --fixed: #86a9de; --daily: #4fc3a1;
  --bad: #f08a76; --good: #62d19a; color-scheme: dark;
}
* { box-sizing: border-box; }
body {
  background: var(--bg); color: var(--ink); font-family: var(--font-body); font-size: 15px; line-height: 1.5;
  padding-block: 20px 48px;
  padding-left: max(16px, env(safe-area-inset-left, 0px));
  padding-right: max(16px, env(safe-area-inset-right, 0px));
}
h1, h2, h3 { font-family: var(--font-display); margin: 0; text-wrap: balance; }
button, input, select { font: inherit; color: inherit; }
.num { font-variant-numeric: tabular-nums; }
.wrap { max-width: 880px; margin-inline: auto; display: flex; flex-direction: column; gap: 16px; }

/* acesso */
.auth { max-width: 400px; margin: 6vh auto 0; display: flex; flex-direction: column; gap: 20px; }
.brand { display: flex; align-items: center; gap: 10px; font-family: var(--font-display); font-weight: 700; font-size: 20px; }
.brand-mark { width: 30px; height: 30px; border-radius: 8px; background: var(--accent); color: var(--accent-ink); display: grid; place-items: center; font-size: 17px; }
.panel { background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 20px; }
.tabs { display: grid; grid-template-columns: 1fr 1fr; background: var(--surface-2); border-radius: 9px; padding: 3px; margin-bottom: 18px; }
.tabs button { border: 0; background: transparent; padding: 8px; border-radius: 7px; cursor: pointer; font-weight: 500; color: var(--muted); }
.tabs button[aria-selected="true"] { background: var(--surface); color: var(--ink); box-shadow: 0 1px 2px rgba(0,0,0,.12); }
.auth h1 { font-size: 24px; margin-bottom: 4px; }
.lead { color: var(--muted); margin: 0 0 16px; }
.field { display: flex; flex-direction: column; gap: 5px; min-width: 0; }
.field label { font-size: 12px; font-weight: 600; letter-spacing: .04em; text-transform: uppercase; color: var(--muted); }
input[type=text], input[type=email], input[type=password], input[type=date], select {
  width: 100%; background: var(--bg); border: 1px solid var(--line); border-radius: 8px; padding: 10px 12px; min-height: 42px;
}
input:focus-visible, select:focus-visible, button:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
.stack { display: flex; flex-direction: column; gap: 14px; }
.btn { background: var(--accent); color: var(--accent-ink); border: 0; border-radius: 8px; padding: 10px 16px; font-weight: 600; cursor: pointer; min-height: 42px; }
.btn:hover { filter: brightness(1.08); }
.btn.ghost { background: transparent; color: var(--ink); border: 1px solid var(--line); font-weight: 500; }
.msg { min-height: 20px; font-size: 13px; color: var(--bad); margin: 0; }
.msg.ok { color: var(--good); }
.note { font-size: 12px; color: var(--muted); margin: 0; }

/* cabeçalho do painel */
.top { display: flex; align-items: center; justify-content: space-between; gap: 12px; flex-wrap: wrap; }
.who { display: flex; align-items: center; gap: 10px; color: var(--muted); font-size: 14px; }
.who strong { color: var(--ink); font-weight: 600; }
.monthbar { display: flex; align-items: center; justify-content: space-between; gap: 12px; flex-wrap: wrap; }
.monthnav { display: flex; align-items: center; gap: 8px; }
.monthnav h2 { font-size: 26px; min-width: 11ch; text-align: center; text-transform: capitalize; }
.iconbtn { width: 40px; height: 40px; border-radius: 8px; border: 1px solid var(--line); background: var(--surface); cursor: pointer; font-size: 18px; }
.iconbtn:hover { background: var(--surface-2); }

/* resumo */
.summary { display: grid; grid-template-columns: 1.3fr 1fr 1fr; gap: 12px; }
.tile { background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 14px 16px; display: flex; flex-direction: column; gap: 4px; min-width: 0; }
.tile .k { font-size: 12px; font-weight: 600; letter-spacing: .04em; text-transform: uppercase; color: var(--muted); }
.tile .v { font-family: var(--font-display); font-size: 24px; font-weight: 600; overflow-wrap: anywhere; }
.tile.main .v { font-size: 34px; }
.tile.main.neg .v { color: var(--bad); }
.tile .s { font-size: 13px; color: var(--muted); }
.bar { height: 8px; background: var(--surface-2); border-radius: 99px; overflow: hidden; display: flex; margin-top: 6px; }
.bar span { display: block; height: 100%; }
.bar .f { background: var(--fixed); }
.bar .d { background: var(--daily); }
.legend { display: flex; gap: 14px; flex-wrap: wrap; font-size: 12px; color: var(--muted); }
.dot { display: inline-block; width: 9px; height: 9px; border-radius: 3px; margin-right: 5px; }

/* formulários */
.cols { display: grid; grid-template-columns: 1fr 1.6fr; gap: 16px; align-items: start; }
.panel h3 { font-size: 17px; margin-bottom: 12px; }
.inline { display: flex; gap: 8px; align-items: flex-end; }
.inline .field { flex: 1; }
.seg { display: grid; grid-template-columns: 1fr 1fr; background: var(--surface-2); border-radius: 9px; padding: 3px; }
.seg label { text-transform: none; letter-spacing: 0; font-size: 14px; font-weight: 500; color: var(--muted); text-align: center; padding: 8px; border-radius: 7px; cursor: pointer; }
.seg input { position: absolute; opacity: 0; pointer-events: none; }
.seg input:checked + label { background: var(--surface); color: var(--ink); box-shadow: 0 1px 2px rgba(0,0,0,.12); }
.seg input:focus-visible + label { outline: 2px solid var(--accent); }
.grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.check { display: flex; gap: 8px; align-items: center; font-size: 14px; }
.check input { width: 18px; height: 18px; accent-color: var(--accent); }

/* gráfico */
.chart { display: flex; align-items: flex-end; gap: 2px; height: 120px; border-bottom: 1px solid var(--line); padding-top: 6px; }
.chart .col { flex: 1; min-width: 0; height: 100%; display: flex; align-items: flex-end; }
.chart .col i { display: block; width: 100%; min-height: 0; background: var(--daily); border-radius: 3px 3px 0 0; }
.chart .col.zero i { background: var(--line); height: 2px !important; }
.axis { display: flex; gap: 2px; font-size: 11px; color: var(--muted); margin-top: 4px; }
.axis span { flex: 1; min-width: 0; text-align: center; overflow: visible; white-space: nowrap; }
.chartmax { font-size: 12px; color: var(--muted); display: flex; justify-content: space-between; margin-bottom: 4px; }

/* extrato */
.listhead { display: flex; justify-content: space-between; align-items: center; gap: 10px; flex-wrap: wrap; margin-bottom: 8px; }
.filters { display: flex; gap: 6px; }
.chip { border: 1px solid var(--line); background: transparent; border-radius: 99px; padding: 5px 12px; font-size: 13px; cursor: pointer; color: var(--muted); }
.chip[aria-pressed="true"] { background: var(--accent); border-color: var(--accent); color: var(--accent-ink); }
.rows { list-style: none; margin: 0; padding: 0; }
.row { display: grid; grid-template-columns: 52px 1fr auto auto; gap: 12px; align-items: center; padding: 11px 0; border-top: 1px solid var(--line); }
.row:first-child { border-top: 0; }
.date { text-align: center; line-height: 1.1; }
.date b { display: block; font-family: var(--font-display); font-size: 20px; }
.date small { font-size: 11px; color: var(--muted); text-transform: uppercase; }
.desc { min-width: 0; }
.desc div { overflow-wrap: anywhere; font-weight: 500; }
.desc small { color: var(--muted); font-size: 12px; display: flex; gap: 8px; flex-wrap: wrap; align-items: center; }
.tag { border-radius: 99px; padding: 1px 8px; font-size: 11px; font-weight: 600; border: 1px solid currentColor; }
.tag.fixo { color: var(--fixed); }
.tag.diario { color: var(--daily); }
.amount { font-family: var(--font-display); font-weight: 600; text-align: right; }
.del { border: 0; background: transparent; color: var(--muted); cursor: pointer; padding: 6px 8px; border-radius: 6px; font-size: 13px; }
.del:hover { background: var(--surface-2); color: var(--bad); }
.del.armed { color: var(--bad); font-weight: 600; }
.empty { color: var(--muted); padding: 18px 0 6px; text-align: center; }

@media (max-width: 720px) {
  .summary { grid-template-columns: 1fr 1fr; }
  .tile.main { grid-column: 1 / -1; }
  .cols { grid-template-columns: 1fr; }
  .row { grid-template-columns: 44px 1fr auto; }
  .row .del { grid-column: 3; grid-row: 2; justify-self: end; padding: 2px 4px; }
  .monthnav h2 { font-size: 22px; min-width: 9ch; }
}
@media (prefers-reduced-motion: reduce) { * { transition: none !important; animation: none !important; } }
</style>

<div id="app"></div>

<script>
(function () {
  "use strict";
  var MESES = ["janeiro","fevereiro","março","abril","maio","junho","julho","agosto","setembro","outubro","novembro","dezembro"];
  var MESES_CURTOS = ["jan","fev","mar","abr","mai","jun","jul","ago","set","out","nov","dez"];
  var CATEGORIAS = ["Moradia","Alimentação","Transporte","Saúde","Educação","Lazer","Contas","Outros"];
  var brl = new Intl.NumberFormat("pt-BR", { style: "currency", currency: "BRL" });

  /* ---------- armazenamento (localStorage com reserva em memória) ---------- */
  var mem = {};
  var store = {
    get: function (k) {
      try { var v = localStorage.getItem(k); if (v !== null) return JSON.parse(v); } catch (e) {}
      return Object.prototype.hasOwnProperty.call(mem, k) ? mem[k] : null;
    },
    set: function (k, v) {
      mem[k] = v;
      try { localStorage.setItem(k, JSON.stringify(v)); } catch (e) {}
    },
    del: function (k) { delete mem[k]; try { localStorage.removeItem(k); } catch (e) {} }
  };

  /* ---------- utilidades ---------- */
  function $(s, r) { return (r || document).querySelector(s); }
  function esc(s) { return String(s).replace(/[&<>"']/g, function (c) { return ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" })[c]; }); }
  function pad(n) { return n < 10 ? "0" + n : "" + n; }
  function uid() { return Date.now().toString(36) + Math.random().toString(36).slice(2, 8); }
  function diasNoMes(ano, mes) { return new Date(ano, mes, 0).getDate(); }
  function chaveMes(ano, mes) { return ano + "-" + pad(mes); }
  function parseValor(txt) {
    var t = String(txt).replace(/[R$\s]/g, "");
    if (!t) return NaN;
    if (t.indexOf(",") >= 0) t = t.replace(/\./g, "").replace(",", ".");
    var n = Number(t);
    return isFinite(n) ? n : NaN;
  }
  function hoje() { var d = new Date(); return { ano: d.getFullYear(), mes: d.getMonth() + 1, dia: d.getDate() }; }
  function titleMes(ano, mes) { return MESES[mes - 1] + " de " + ano; }

  function sha(text) {
    try {
      if (window.crypto && crypto.subtle) {
        return crypto.subtle.digest("SHA-256", new TextEncoder().encode(text)).then(function (buf) {
          return Array.prototype.map.call(new Uint8Array(buf), function (b) { return ("0" + b.toString(16)).slice(-2); }).join("");
        });
      }
    } catch (e) {}
    var h = 5381;
    for (var i = 0; i < text.length; i++) h = ((h << 5) + h + text.charCodeAt(i)) | 0;
    return Promise.resolve("x" + h);
  }

  /* ---------- estado ---------- */
  var state = { user: null, data: null, ano: 0, mes: 0, filtro: "todos", aba: "entrar", armed: null };

  function usuarios() { return store.get("cf_users") || []; }
  function dadosKey(id) { return "cf_data_" + id; }
  function salvarDados() { store.set(dadosKey(state.user.id), state.data); }

  /* ---------- cálculos ---------- */
  function lancamentosDoMes(ano, mes) {
    var chave = chaveMes(ano, mes), ndias = diasNoMes(ano, mes), lista = [];
    state.data.lancamentos.forEach(function (l) {
      var ano0 = +l.data.slice(0, 4), mes0 = +l.data.slice(5, 7), dia0 = +l.data.slice(8, 10);
      if (l.recorrente) {
        if (ano * 12 + mes >= ano0 * 12 + mes0) {
          lista.push({ l: l, data: chave + "-" + pad(Math.min(dia0, ndias)), repetido: !(ano === ano0 && mes === mes0) });
        }
      } else if (l.data.slice(0, 7) === chave) {
        lista.push({ l: l, data: l.data, repetido: false });
      }
    });
    lista.sort(function (a, b) { return a.data < b.data ? 1 : a.data > b.data ? -1 : 0; });
    return lista;
  }

  /* ---------- telas ---------- */
  function renderAuth(msg, ok) {
    var entrar = state.aba === "entrar";
    $("#app").innerHTML =
      '<div class="auth">' +
        '<div class="brand"><span class="brand-mark" aria-hidden="true">$</span>Controller Finanças</div>' +
        '<div class="panel">' +
          '<div class="tabs" role="tablist">' +
            '<button role="tab" id="tab-entrar" aria-selected="' + entrar + '">Entrar</button>' +
            '<button role="tab" id="tab-cadastro" aria-selected="' + !entrar + '">Criar conta</button>' +
          '</div>' +
          '<h1>' + (entrar ? "Bem-vindo de volta" : "Crie sua conta") + '</h1>' +
          '<p class="lead">' + (entrar ? "Entre para ver seu salário e seus gastos do mês." : "Leva menos de um minuto. Depois é só lançar seu salário e seus custos.") + '</p>' +
          '<form id="form-auth" class="stack" novalidate>' +
            (entrar ? "" : '<div class="field"><label for="a-nome">Nome</label><input type="text" id="a-nome" autocomplete="name"></div>') +
            '<div class="field"><label for="a-email">E-mail</label><input type="email" id="a-email" autocomplete="email" inputmode="email"></div>' +
            '<div class="field"><label for="a-senha">Senha</label><input type="password" id="a-senha" autocomplete="' + (entrar ? "current-password" : "new-password") + '"></div>' +
            (entrar ? "" : '<div class="field"><label for="a-senha2">Repita a senha</label><input type="password" id="a-senha2" autocomplete="new-password"></div>') +
            '<p class="msg' + (ok ? " ok" : "") + '" id="a-msg" role="alert">' + esc(msg || "") + '</p>' +
            '<button class="btn" type="submit">' + (entrar ? "Entrar" : "Criar conta") + '</button>' +
          '</form>' +
        '</div>' +
        '<p class="note">Seus dados ficam salvos somente neste navegador, neste aparelho.</p>' +
      '</div>';

    $("#tab-entrar").onclick = function () { state.aba = "entrar"; renderAuth(); };
    $("#tab-cadastro").onclick = function () { state.aba = "cadastro"; renderAuth(); };
    $("#form-auth").onsubmit = function (e) { e.preventDefault(); entrar ? fazerLogin() : fazerCadastro(); };
  }

  function fazerCadastro() {
    var nome = $("#a-nome").value.trim(), email = $("#a-email").value.trim().toLowerCase();
    var s1 = $("#a-senha").value, s2 = $("#a-senha2").value;
    if (nome.length < 2) return renderAuthKeep("Informe seu nome.");
    if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(email)) return renderAuthKeep("Informe um e-mail válido.");
    if (s1.length < 6) return renderAuthKeep("A senha precisa ter pelo menos 6 caracteres.");
    if (s1 !== s2) return renderAuthKeep("As senhas não são iguais.");
    var us = usuarios();
    if (us.some(function (u) { return u.email === email; })) return renderAuthKeep("Já existe uma conta com esse e-mail. Use a aba Entrar.");
    var salt = uid();
    sha(salt + s1).then(function (hash) {
      var u = { id: uid(), nome: nome, email: email, salt: salt, hash: hash };
      us.push(u);
      store.set("cf_users", us);
      store.set("cf_session", u.id);
      abrirPainel(u);
    });
  }

  function fazerLogin() {
    var email = $("#a-email").value.trim().toLowerCase(), senha = $("#a-senha").value;
    if (!email || !senha) return renderAuthKeep("Preencha e-mail e senha.");
    var u = usuarios().filter(function (x) { return x.email === email; })[0];
    if (!u) return renderAuthKeep("Não encontramos uma conta com esse e-mail. Crie uma na aba Criar conta.");
    sha(u.salt + senha).then(function (hash) {
      if (hash !== u.hash) return renderAuthKeep("Senha incorreta. Confira e tente de novo.");
      store.set("cf_session", u.id);
      abrirPainel(u);
    });
  }

  function renderAuthKeep(msg) {
    var campos = {};
    ["a-nome", "a-email"].forEach(function (id) { var el = document.getElementById(id); if (el) campos[id] = el.value; });
    renderAuth(msg);
    Object.keys(campos).forEach(function (id) { var el = document.getElementById(id); if (el) el.value = campos[id]; });
    var alvo = document.getElementById("a-senha"); if (alvo) alvo.focus();
  }

  function abrirPainel(u) {
    state.user = u;
    state.data = store.get(dadosKey(u.id)) || { salarios: {}, lancamentos: [] };
    var h = hoje();
    state.ano = h.ano; state.mes = h.mes; state.filtro = "todos"; state.armed = null;
    renderPainel();
  }

  function sair() {
    store.del("cf_session");
    state.user = null; state.data = null; state.aba = "entrar";
    renderAuth();
  }

  function renderPainel() {
    var ano = state.ano, mes = state.mes, chave = chaveMes(ano, mes);
    var itens = lancamentosDoMes(ano, mes);
    var salario = state.data.salarios[chave];
    var temSalario = typeof salario === "number";
    var totFixo = 0, totDiario = 0;
    itens.forEach(function (i) { if (i.l.tipo === "fixo") totFixo += i.l.valor; else totDiario += i.l.valor; });
    var total = totFixo + totDiario;
    var saldo = (temSalario ? salario : 0) - total;
    var pctF = temSalario && salario > 0 ? Math.min(100, totFixo / salario * 100) : 0;
    var pctD = temSalario && salario > 0 ? Math.min(100 - pctF, totDiario / salario * 100) : 0;
    var usoTxt = temSalario && salario > 0 ? Math.round(total / salario * 100) + "% do salário usado" : "Defina o salário para ver o saldo";

    var h = hoje();
    var dataPadrao = (ano === h.ano && mes === h.mes) ? chave + "-" + pad(h.dia) : chave + "-01";
    var filtrados = itens.filter(function (i) { return state.filtro === "todos" || i.l.tipo === state.filtro; });

    // gráfico diário (só custos que caem em cada dia)
    var nd = diasNoMes(ano, mes), porDia = [], maxDia = 0, d;
    for (d = 1; d <= nd; d++) porDia.push(0);
    itens.forEach(function (i) { var dia = +i.data.slice(8, 10); porDia[dia - 1] += i.l.valor; });
    porDia.forEach(function (v) { if (v > maxDia) maxDia = v; });
    var barras = "", eixo = "";
    porDia.forEach(function (v, idx) {
      var dia = idx + 1, alt = maxDia > 0 ? (v / maxDia * 100) : 0;
      barras += '<div class="col' + (v === 0 ? " zero" : "") + '" title="Dia ' + dia + ": " + esc(brl.format(v)) + '"><i style="height:' + alt.toFixed(1) + '%"></i></div>';
      eixo += "<span>" + ((dia === 1 || dia % 5 === 0) ? dia : "") + "</span>";
    });

    var linhas = filtrados.map(function (i) {
      var l = i.l, dia = +i.data.slice(8, 10), mm = +i.data.slice(5, 7);
      var armado = state.armed === l.id;
      return '<li class="row">' +
        '<div class="date num"><b>' + pad(dia) + '</b><small>' + MESES_CURTOS[mm - 1] + '</small></div>' +
        '<div class="desc"><div>' + esc(l.desc) + '</div><small><span class="tag ' + l.tipo + '">' + (l.tipo === "fixo" ? "Fixo" : "Diário") + '</span>' +
          esc(l.categoria) + (l.recorrente ? " · todo mês" : "") + (i.repetido ? " · repetido" : "") + '</small></div>' +
        '<div class="amount num">' + esc(brl.format(l.valor)) + '</div>' +
        '<button class="del" data-id="' + l.id + '" aria-label="Excluir ' + esc(l.desc) + '">' + (armado ? "Confirmar?" : "Excluir") + '</button>' +
      '</li>';
    }).join("");

    var vazio = state.data.lancamentos.length === 0
      ? "Nenhum custo lançado ainda. Use o formulário ao lado para adicionar o primeiro."
      : "Nenhum custo para este filtro em " + titleMes(ano, mes) + ".";

    var opcCat = CATEGORIAS.map(function (c) { return "<option>" + c + "</option>"; }).join("");

    $("#app").innerHTML =
      '<div class="wrap">' +
        '<div class="top">' +
          '<div class="brand"><span class="brand-mark" aria-hidden="true">$</span>Controller Finanças</div>' +
          '<div class="who"><span>Olá, <strong>' + esc(state.user.nome.split(" ")[0]) + '</strong></span><button class="btn ghost" id="btn-sair" type="button">Sair</button></div>' +
        '</div>' +

        '<div class="monthbar"><div class="monthnav">' +
          '<button class="iconbtn" id="mes-ant" type="button" aria-label="Mês anterior">‹</button>' +
          '<h2>' + esc(titleMes(ano, mes)) + '</h2>' +
          '<button class="iconbtn" id="mes-prox" type="button" aria-label="Próximo mês">›</button>' +
        '</div><button class="btn ghost" id="mes-hoje" type="button">Ir para hoje</button></div>' +

        '<div class="summary">' +
          '<div class="tile main' + (saldo < 0 ? " neg" : "") + '"><span class="k">Saldo do mês</span><span class="v num">' + esc(brl.format(saldo)) + '</span>' +
            '<div class="bar" aria-hidden="true"><span class="f" style="width:' + pctF.toFixed(1) + '%"></span><span class="d" style="width:' + pctD.toFixed(1) + '%"></span></div>' +
            '<span class="s">' + esc(usoTxt) + '</span></div>' +
          '<div class="tile"><span class="k">Salário</span><span class="v num">' + (temSalario ? esc(brl.format(salario)) : "—") + '</span><span class="s">Entrada de ' + esc(MESES[mes - 1]) + '</span></div>' +
          '<div class="tile"><span class="k">Total gasto</span><span class="v num">' + esc(brl.format(total)) + '</span>' +
            '<span class="legend"><span><i class="dot" style="background:var(--fixed)"></i>Fixos ' + esc(brl.format(totFixo)) + '</span><span><i class="dot" style="background:var(--daily)"></i>Diários ' + esc(brl.format(totDiario)) + '</span></span></div>' +
        '</div>' +

        '<div class="cols">' +
          '<div class="stack">' +
            '<div class="panel"><h3>Salário do mês</h3>' +
              '<form id="form-salario" class="inline" novalidate>' +
                '<div class="field"><label for="s-valor">Valor em R$</label><input type="text" id="s-valor" inputmode="decimal" placeholder="0,00" value="' + (temSalario ? esc(String(salario).replace(".", ",")) : "") + '"></div>' +
                '<button class="btn" type="submit">Salvar</button>' +
              '</form><p class="msg" id="s-msg" role="alert"></p></div>' +

            '<div class="panel"><h3>Novo custo</h3>' +
              '<form id="form-custo" class="stack" novalidate>' +
                '<div class="seg" role="radiogroup" aria-label="Tipo de custo">' +
                  '<input type="radio" name="tipo" id="t-diario" value="diario" checked><label for="t-diario">Diário</label>' +
                  '<input type="radio" name="tipo" id="t-fixo" value="fixo"><label for="t-fixo">Fixo</label>' +
                '</div>' +
                '<div class="field"><label for="c-desc">Descrição</label><input type="text" id="c-desc" maxlength="80" placeholder="Ex.: Mercado, Aluguel"></div>' +
                '<div class="grid2">' +
                  '<div class="field"><label for="c-valor">Valor em R$</label><input type="text" id="c-valor" inputmode="decimal" placeholder="0,00"></div>' +
                  '<div class="field"><label for="c-data">Data (dia/mês/ano)</label><input type="date" id="c-data" value="' + dataPadrao + '"></div>' +
                '</div>' +
                '<div class="field"><label for="c-cat">Categoria</label><select id="c-cat">' + opcCat + '</select></div>' +
                '<label class="check" id="rec-wrap" hidden><input type="checkbox" id="c-rec"> Repetir todo mês a partir desta data</label>' +
                '<p class="msg" id="c-msg" role="alert"></p>' +
                '<button class="btn" type="submit">Adicionar custo</button>' +
              '</form></div>' +
          '</div>' +

          '<div class="stack">' +
            '<div class="panel"><h3>Gastos por dia</h3>' +
              '<div class="chartmax num"><span>Maior dia</span><span>' + esc(brl.format(maxDia)) + '</span></div>' +
              '<div class="chart" role="img" aria-label="Gastos por dia em ' + esc(titleMes(ano, mes)) + '">' + barras + '</div>' +
              '<div class="axis num" aria-hidden="true">' + eixo + '</div></div>' +

            '<div class="panel">' +
              '<div class="listhead"><h3 style="margin:0">Extrato de ' + esc(MESES[mes - 1]) + '</h3>' +
                '<div class="filters">' +
                  ['todos', 'fixo', 'diario'].map(function (f) {
                    var nome = f === "todos" ? "Todos" : f === "fixo" ? "Fixos" : "Diários";
                    return '<button class="chip" type="button" data-f="' + f + '" aria-pressed="' + (state.filtro === f) + '">' + nome + '</button>';
                  }).join("") +
                '</div></div>' +
              (linhas ? '<ul class="rows">' + linhas + '</ul>' : '<p class="empty">' + esc(vazio) + '</p>') +
            '</div>' +
          '</div>' +
        '</div>' +
      '</div>';

    ligarEventos();
  }

  function ligarEventos() {
    $("#btn-sair").onclick = sair;
    $("#mes-ant").onclick = function () { mudarMes(-1); };
    $("#mes-prox").onclick = function () { mudarMes(1); };
    $("#mes-hoje").onclick = function () { var h = hoje(); state.ano = h.ano; state.mes = h.mes; state.armed = null; renderPainel(); };

    $("#form-salario").onsubmit = function (e) {
      e.preventDefault();
      var v = parseValor($("#s-valor").value), msg = $("#s-msg");
      if (isNaN(v) || v < 0) { msg.className = "msg"; msg.textContent = "Digite um valor válido, por exemplo 4500,00."; return; }
      state.data.salarios[chaveMes(state.ano, state.mes)] = Math.round(v * 100) / 100;
      salvarDados();
      renderPainel();
      var m = $("#s-msg"); m.className = "msg ok"; m.textContent = "Salário de " + MESES[state.mes - 1] + " salvo.";
    };

    var radios = document.querySelectorAll('input[name="tipo"]');
    function atualizaRec() {
      var fixo = $("#t-fixo").checked;
      $("#rec-wrap").hidden = !fixo;
      if (!fixo) $("#c-rec").checked = false;
    }
    Array.prototype.forEach.call(radios, function (r) { r.onchange = atualizaRec; });

    $("#form-custo").onsubmit = function (e) {
      e.preventDefault();
      var msg = $("#c-msg"); msg.className = "msg";
      var desc = $("#c-desc").value.trim(), valor = parseValor($("#c-valor").value), data = $("#c-data").value;
      var fixo = $("#t-fixo").checked;
      if (!desc) { msg.textContent = "Escreva uma descrição para o custo."; return; }
      if (isNaN(valor) || valor <= 0) { msg.textContent = "Digite um valor maior que zero, por exemplo 89,90."; return; }
      if (!/^\d{4}-\d{2}-\d{2}$/.test(data)) { msg.textContent = "Escolha a data completa: dia, mês e ano."; return; }
      state.data.lancamentos.push({
        id: uid(), desc: desc, valor: Math.round(valor * 100) / 100, data: data,
        tipo: fixo ? "fixo" : "diario", categoria: $("#c-cat").value, recorrente: fixo && $("#c-rec").checked
      });
      salvarDados();
      state.ano = +data.slice(0, 4); state.mes = +data.slice(5, 7);
      renderPainel();
      var m = $("#c-msg"); m.className = "msg ok"; m.textContent = "Custo adicionado.";
    };

    Array.prototype.forEach.call(document.querySelectorAll(".chip"), function (b) {
      b.onclick = function () { state.filtro = b.getAttribute("data-f"); state.armed = null; renderPainel(); };
    });
    Array.prototype.forEach.call(document.querySelectorAll(".del"), function (b) {
      b.onclick = function () {
        var id = b.getAttribute("data-id");
        if (state.armed === id) {
          state.data.lancamentos = state.data.lancamentos.filter(function (l) { return l.id !== id; });
          state.armed = null; salvarDados(); renderPainel();
        } else {
          state.armed = id; renderPainel();
          setTimeout(function () { if (state.armed === id) { state.armed = null; if (state.user) renderPainel(); } }, 4000);
        }
      };
    });
  }

  function mudarMes(delta) {
    var m = state.mes + delta, a = state.ano;
    if (m < 1) { m = 12; a--; } else if (m > 12) { m = 1; a++; }
    state.mes = m; state.ano = a; state.armed = null;
    renderPainel();
  }

  /* ---------- início ---------- */
  var sessao = store.get("cf_session");
  var u = sessao ? usuarios().filter(function (x) { return x.id === sessao; })[0] : null;
  if (u) abrirPainel(u); else renderAuth();
})();
</script>

</body></html>

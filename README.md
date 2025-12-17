<!DOCTYPE html><html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Daysha Kércia | Camarão Premium</title>
  <style>
    body {font-family: Arial, sans-serif; margin:0; background:#0f172a; color:#fff}
    header {background:linear-gradient(90deg,#020617,#1e293b); padding:24px; text-align:center}
    header h1 {font-size:1.6em; margin:0}
    header p {font-size:0.95em; opacity:0.9}
    section {padding:24px 16px; max-width:1100px; margin:auto}
    h2 {color:#facc15; font-size:1.3em}
    .cards {display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:16px}
    .card {background:#020617; border-radius:18px; padding:18px; box-shadow:0 10px 25px rgba(0,0,0,.4)}
    .price {font-size:1.4em; color:#22c55e}
    .old {text-decoration:line-through; color:#94a3b8; font-size:0.85em}
    .btn {display:inline-block; margin-top:12px; background:#22c55e; color:#020617; padding:12px 18px; border-radius:999px; text-decoration:none; font-weight:bold; width:100%; text-align:center}
    .locations {display:grid; grid-template-columns:1fr 1fr; gap:12px}
    footer {background:#020617; text-align:center; padding:18px; font-size:0.85em; opacity:0.8}
    table {width:100%; border-collapse:collapse; margin-top:16px; font-size:0.9em}
    th,td {border:1px solid #334155; padding:8px; text-align:center}
    th {background:#1e293b}
    input,button,select {padding:10px; border-radius:10px; border:none; width:100%; margin-top:6px}
    button {background:#facc15; font-weight:bold; cursor:pointer}
    .hidden {display:none}
    .total {margin-top:16px; font-weight:bold}
  </style>
</head>
<body><header>
  <h1>🦐 Daysha Kércia Camarão Premium</h1>
  <p>Promoção de inauguração – Estoque limitado</p>
</header><section>
  <h2>🔥 Promoção Principal</h2>
  <div class="cards">
    <div class="card">
      <h3>1kg Camarão Premium</h3>
      <p class="old">De R$ 70,00</p>
      <p class="price">R$ 60,00</p>
      <a class="btn" href="https://wa.me/5585999566794" target="_blank">Comprar no WhatsApp</a>
    </div>
  </div>
</section><section>
  <h2>🎁 Combos</h2>
  <div class="cards">
    <div class="card"><h3>Combo Família</h3><p>2kg</p><p class="price">R$ 115,00</p></div>
    <div class="card"><h3>Combo Churrasco</h3><p>3kg</p><p class="price">R$ 165,00</p></div>
    <div class="card"><h3>Combo Experimente</h3><p>1kg</p><p class="price">R$ 60,00</p></div>
  </div>
</section><section>
  <h2>📍 Unidades</h2>
  <div class="locations">
    <div class="card">Bom Jardim</div>
    <div class="card">Barra do Ceará</div>
  </div>
</section><section>
  <h2>🔐 Acesso Gestão</h2>
  <input id="senha" type="password" placeholder="Digite a senha" />
  <button onclick="login()">Entrar</button>
</section><section id="gestao" class="hidden">
  <h2>📊 Gestão Empresarial</h2>  <h3>Registrar</h3>
  <select id="tipo">
    <option value="Venda">Venda</option>
    <option value="Despesa">Despesa</option>
  </select>
  <input id="desc" placeholder="Descrição" />
  <input id="valor" type="number" placeholder="Valor R$" />
  <button onclick="addRegistro()">Salvar</button>  <table>
    <thead><tr><th>Tipo</th><th>Descrição</th><th>Valor</th></tr></thead>
    <tbody id="registros"></tbody>
  </table>  <div class="total" id="totais"></div>
</section><footer>
  <p>📲 WhatsApp: (85) 99956-6794</p>
  <p>© 2025 Daysha Kércia</p>
</footer><script>
  const PASSWORD = '15day88';
  const dados = JSON.parse(localStorage.getItem('dados')) || [];

  function login(){
    if(document.getElementById('senha').value === PASSWORD){
      document.getElementById('gestao').classList.remove('hidden');
      render();
    } else { alert('Senha incorreta'); }
  }

  function render(){
    const tbody = document.getElementById('registros');
    tbody.innerHTML='';
    let vendas=0, despesas=0;
    dados.forEach(d=>{
      const tr=document.createElement('tr');
      tr.innerHTML=`<td>${d.tipo}</td><td>${d.desc}</td><td>R$ ${d.valor.toFixed(2)}</td>`;
      tbody.appendChild(tr);
      d.tipo==='Venda'?vendas+=d.valor:despesas+=d.valor;
    });
    document.getElementById('totais').innerHTML=`Total Vendas: R$ ${vendas.toFixed(2)} | Total Despesas: R$ ${despesas.toFixed(2)} | Lucro: R$ ${(vendas-despesas).toFixed(2)}`;
    localStorage.setItem('dados',JSON.stringify(dados));
  }

  function addRegistro(){
    dados.push({tipo:tipo.value, desc:desc.value, valor:Number(valor.value)});
    desc.value=''; valor.value=''; render();
  }
</script></body>
</html>

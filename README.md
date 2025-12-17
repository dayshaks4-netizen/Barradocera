<!DOCTYPE html><html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Daysha Kércia | Camarão Premium</title>
<style>
body{margin:0;font-family:Arial;background:#0f172a;color:#fff}
header{padding:20px;text-align:center;background:#020617}
section{padding:20px}
h2{color:#facc15}
.card{background:#020617;border-radius:18px;padding:18px;margin-bottom:15px}
.btn{width:100%;padding:14px;border-radius:999px;border:none;background:#22c55e;font-weight:bold;margin-top:10px}
input,select{width:100%;padding:10px;border-radius:10px;border:none;margin-top:6px}
.hidden{display:none}
table{width:100%;border-collapse:collapse;font-size:0.9em}
th,td{border:1px solid #334155;padding:8px;text-align:center}
th{background:#1e293b}
.total{margin-top:10px;font-weight:bold}
</style>
</head>
<body>
<header>
<h1>🦐 Daysha Kércia Camarão Premium</h1>
<p>Pedidos automáticos • Gestão com senha</p>
</header><section>
<h2>🛒 Fazer Pedido</h2>
<div class="card"><h3>Atacado</h3><p>R$ 50 / kg</p><button class="btn" onclick="abrirPedido('Atacado',50)">Comprar Atacado</button></div>
<div class="card"><h3>Varejo</h3><p>R$ 60 / kg</p><button class="btn" onclick="abrirPedido('Varejo',60)">Comprar Varejo</button></div>
<div class="card"><h3>Combo</h3><p>Preço especial</p><button class="btn" onclick="abrirPedido('Combo',55)">Comprar Combo</button></div>
</section><section id="pedido" class="hidden">
<h2>📝 Dados do Pedido</h2>
<form action="https://formsubmit.co/Dayshaks3@gmail.com" method="POST" onsubmit="registrarPedido()">
<input type="hidden" name="_subject" value="🦐 Novo Pedido - Daysha Kércia" />
<input type="hidden" name="_template" value="table" />
<input type="hidden" name="_captcha" value="false" />
<p id="tipo"></p>
<select id="unidade" name="Unidade"><option>Bom Jardim</option><option>Barra do Ceará</option></select>
<input id="nome" name="Nome" placeholder="Nome" required />
<input id="whats" name="WhatsApp" placeholder="WhatsApp" required />
<input id="kg" name="Quantidade (kg)" type="number" placeholder="Quantidade (kg)" oninput="calc()" required />
<input id="km" name="Distância (km)" type="number" placeholder="Distância (km)" oninput="calc()" required />
<input id="end" name="Endereço" placeholder="Endereço completo" required />
<input type="hidden" id="totalEmail" name="Valor Total" />
<p id="resumo"></p>
<button class="btn" type="submit">Enviar Pedido</button>
</form>
</section><section id="pix" class="hidden">
<h2>💳 Pagamento PIX</h2>
<p id="pixInfo"></p>
<button class="btn" onclick="confirmarPagamento()">Já paguei no PIX</button>
</section><section>
<h2>🔐 Gestão</h2>
<input id="senha" type="password" placeholder="Senha" />
<button class="btn" onclick="login()">Entrar</button>
</section><section id="gestao" class="hidden">
<h2>📊 Histórico de Pedidos</h2>
<select id="filtroMes" onchange="render()">
  <option value="todos">Todos os meses</option>
</select>
<table>
<thead><tr><th>Data</th><th>Unidade</th><th>Tipo</th><th>Cliente</th><th>Total</th></tr></thead>
<tbody id="lista"></tbody>
</table>
<div class="total" id="totais"></div>
<button class="btn" onclick="gerarPDF()">Gerar Relatório em PDF</button>
</section><script>
const PASS='15day88';
let tipoAtual='',preco=0,total=0;
const pedidos=JSON.parse(localStorage.getItem('pedidos')||'[]'); pedidos.forEach(p=>{ if(!p.data){ p.data=new Date().toISOString().slice(0,7);} });(localStorage.getItem('pedidos')||'[]');
function abrirPedido(t,p){tipoAtual=t;preco=p;pedido.classList.remove('hidden');tipo.innerText='Tipo: '+t}
function calc(){let q=Number(kg.value||0),d=Number(km.value||0);let frete=d<=10?7:7+(d-10)*2;total=q*preco+frete;resumo.innerText=`Produto: R$ ${(q*preco).toFixed(2)} | Frete: R$ ${frete.toFixed(2)} | Total: R$ ${total.toFixed(2)}`;pix.classList.remove('hidden');pixInfo.innerText=`PIX: 85 99956-6794 | Valor: R$ ${total.toFixed(2)}`;document.getElementById('totalEmail').value='R$ '+total.toFixed(2);}(){let q=Number(kg.value||0),d=Number(km.value||0);let frete=d<=10?7:7+(d-10)*2;total=q*preco+frete;resumo.innerText=`Produto: R$ ${(q*preco).toFixed(2)} | Frete: R$ ${frete.toFixed(2)} | Total: R$ ${total.toFixed(2)}`;pix.classList.remove('hidden');pixInfo.innerText=`PIX: 85 99956-6794 | Valor: R$ ${total.toFixed(2)}`}
function registrarPedido(){pedidos.push({data:new Date().toISOString().slice(0,7),unidade:unidade.value,tipo:tipoAtual,cliente:nome.value,total});localStorage.setItem('pedidos',JSON.stringify(pedidos));const msg=`Pedido ${tipoAtual}
Unidade:${unidade.value}
Nome:${nome.value}
Whats:${whats.value}
Total:R$ ${total.toFixed(2)}`;window.open(`https://wa.me/5585999566794?text=${encodeURIComponent(msg)}`,'_blank');}(){const msg=`Pedido ${tipoAtual}\nUnidade:${unidade.value}\nNome:${nome.value}\nWhats:${whats.value}\nTotal:R$ ${total.toFixed(2)}`;window.open(`https://wa.me/5585999566794?text=${encodeURIComponent(msg)}`,'_blank');pedidos.push({unidade:unidade.value,tipo:tipoAtual,cliente:nome.value,total});localStorage.setItem('pedidos',JSON.stringify(pedidos))}
function confirmarPagamento(){alert('Pagamento confirmado! Envie o comprovante no WhatsApp.')}(){alert('Pagamento confirmado! Pedido registrado.')}
function login(){if(senha.value===PASS){gestao.classList.remove('hidden');render()}else alert('Senha incorreta')}
function render(){let v=0;lista.innerHTML='';const mes=filtroMes.value;let meses=new Set();pedidos.forEach(p=>{meses.add(p.data)});filtroMes.innerHTML='<option value="todos">Todos os meses</option>'+[...meses].map(m=>`<option value="${m}">${m}</option>`).join('');pedidos.filter(p=>mes==='todos'||p.data===mes).forEach(p=>{v+=p.total;lista.innerHTML+=`<tr><td>${p.data}</td><td>${p.unidade}</td><td>${p.tipo}</td><td>${p.cliente}</td><td>R$ ${p.total.toFixed(2)}</td></tr>`});totais.innerText='Total faturado: R$ '+v.toFixed(2)}</td><td>${p.tipo}</td><td>${p.cliente}</td><td>R$ ${p.total.toFixed(2)}</td></tr>`});totais.innerText='Total faturado: R$ '+v.toFixed(2)}
function gerarPDF(){const w=window.open('');w.document.write('<h2>Relatório de Pedidos</h2>'+document.querySelector('#gestao').innerHTML);w.print()}
</script></body>
</html>

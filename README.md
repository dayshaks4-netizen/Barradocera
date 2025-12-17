<!DOCTYPE html><html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Daysha Kércia | Sistema Profissional</title>
<style>
body{margin:0;font-family:Arial;background:#0f172a;color:#fff}
header{padding:18px;text-align:center;background:#020617}
section{padding:18px}
h2{color:#facc15}
.card{background:#020617;border-radius:18px;padding:16px;margin-bottom:14px}
.btn{width:100%;padding:18px;border-radius:999px;border:none;background:#22c55e;font-weight:bold;margin-top:12px;font-size:1.1em}
input,select{width:100%;padding:14px;border-radius:10px;border:none;margin-top:8px;font-size:1em}
.hidden{display:none}
table{width:100%;border-collapse:collapse;font-size:0.95em}
th,td{border:1px solid #334155;padding:8px;text-align:center}
th{background:#1e293b}
.status{font-weight:bold}
#resumo{font-size:1.2em;margin-top:12px;color:#facc15;font-weight:bold}
#relatorio{font-size:1.2em;margin-top:12px;color:#22c55e;font-weight:bold}
</style>
</head>
<body>
<header>
<h1>🦐 Daysha Kércia Camarão Premium</h1>
<p>Pedidos salvos na nuvem • Gestão profissional</p>
</header><section>
<h2>🛒 Fazer Pedido</h2>
<div class="card"><button class="btn" onclick="abrirPedido('Atacado',50)">Atacado • R$50/kg</button></div>
<div class="card"><button class="btn" onclick="abrirPedido('Varejo',60)">Varejo • R$60/kg</button></div>
</section><section id="pedido" class="hidden">
<h2>📝 Dados do Pedido</h2>
<select id="unidade"><option>Bom Jardim</option><option>Barra do Ceará</option></select>
<input id="nome" placeholder="Nome" />
<input id="whats" placeholder="WhatsApp" />
<input id="kg" type="number" placeholder="Quantidade (kg)" oninput="calcular()" />
<input id="end" placeholder="Endereço completo" />
<p id="resumo"></p>
<button class="btn" onclick="enviarPedido()">Enviar Pedido</button>
</section><section id="admin" class="hidden">
<h2>📊 Painel Administrativo</h2>
<table>
<thead><tr><th>Data</th><th>Unidade</th><th>Cliente</th><th>Total</th><th>Status</th></tr></thead>
<tbody id="lista"></tbody>
</table>
<p id="relatorio"></p>
</section><script type="module">
// Firebase
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
import { getFirestore, collection, addDoc, onSnapshot, updateDoc, doc } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyAMf6UgrkaNjpaP2ra1r793efZho7cxjQc",
  authDomain: "daysha-camarao.firebaseapp.com",
  projectId: "daysha-camarao",
  storageBucket: "daysha-camarao.firebasestorage.app",
  messagingSenderId: "26292720146",
  appId: "1:26292720146:web:97a2be238db0182d6ffecb"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

let tipo='',preco=0,total=0;

if(location.hash==="#admin"){
  const s = prompt('Senha admin');
  if(s==='15day88'){
    admin.classList.remove('hidden');
    carregarPedidos();
  }
}

window.abrirPedido = (t,p)=>{ tipo=t; preco=p; pedido.classList.remove('hidden'); calcular(); }

window.calcular = ()=>{
  let q=Number(kg.value||0);
  total = q*preco;
  resumo.innerText = `Produto: R$ ${(total).toFixed(2)} | Total: R$ ${(total).toFixed(2)}`;
}

window.enviarPedido = async ()=>{
  await addDoc(collection(db,'pedidos'),{
    data: new Date().toLocaleString(),
    tipo, unidade:unidade.value,
    cliente:nome.value,
    whats:whats.value,
    endereco:end.value,
    total, status:'Recebido'
  });
  window.open(`https://wa.me/5585999566794?text=Novo pedido ${nome.value} • R$ ${total.toFixed(2)}`,'_blank');
  alert('Pedido enviado com sucesso!');
}

function carregarPedidos(){
  let totalDia = 0;
  onSnapshot(collection(db,'pedidos'),snap=>{
    lista.innerHTML='';
    totalDia = 0;
    snap.forEach(docSnap=>{
      const p=docSnap.data();
      totalDia += p.total;
      lista.innerHTML+=`<tr><td>${p.data}</td><td>${p.unidade}</td><td>${p.cliente}</td><td>R$ ${p.total.toFixed(2)}</td><td class='status'><select onchange="mudarStatus('${docSnap.id}',this.value)"><option ${p.status==='Recebido'?'selected':''}>Recebido</option><option ${p.status==='Pago'?'selected':''}>Pago</option><option ${p.status==='Saiu para entrega'?'selected':''}>Saiu para entrega</option></select></td></tr>`;
    });
    relatorio.innerText = `Faturamento total registrado: R$ ${totalDia.toFixed(2)}`;
    new Audio('https://actions.google.com/sounds/v1/alarms/beep_short.ogg').play();
  })
}

window.mudarStatus = async (id,novo)=>{
  await updateDoc(doc(db,'pedidos',id),{status:novo});
}
</script></body>
</html>

<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daysha Kércia | Camarão Premium</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
<script>
// ==========================
// CONFIGURAÇÃO FUNCIONAL
// ==========================
const EMAILJS_USER_ID = 'WCSxdZiWjexDbb90h';       // Seu User ID
const EMAILJS_SERVICE_ID = 'WJk7dL8RH5x1xcHTxp5ny'; // Seu Service ID
const EMAILJS_TEMPLATE_ID = 'template_w2uclpq';     // Template já criado

(function(){emailjs.init(EMAILJS_USER_ID);})();
</script>
<style>
body{margin:0;font-family:'Montserrat',sans-serif;background:linear-gradient(180deg,#0f172a,#1e293b);color:#fff}
header{padding:24px;text-align:center;background:linear-gradient(90deg,#facc15,#22c55e);color:#0f172a}
header h1{margin:0;font-size:2em;}
header p{margin-top:4px;font-size:1.1em}
section{padding:24px}
h2{color:#facc15;margin-bottom:16px;text-align:center}
.card{background:rgba(255,255,255,0.05);border-radius:18px;padding:24px;margin-bottom:16px;transition:all 0.3s ease;cursor:pointer}
.card:hover{background:rgba(255,255,255,0.15);transform:scale(1.03)}
.btn{width:100%;padding:18px;border-radius:999px;border:none;background:#22c55e;color:#fff;font-weight:bold;margin-top:12px;font-size:1.2em;transition:all 0.3s ease;cursor:pointer}
.btn:hover{background:#16a34a;transform:scale(1.05)}
input,select{width:100%;padding:16px;border-radius:12px;border:none;margin-top:8px;font-size:1em}
.hidden{display:none}
table{width:100%;border-collapse:collapse;font-size:1em;margin-top:16px}
th,td{border:1px solid #334155;padding:12px;text-align:center}
th{background:#1e293b}
.status{font-weight:bold}
#resumo{font-size:1.3em;margin-top:12px;color:#facc15;font-weight:bold;text-align:center}
#relatorio{font-size:1.3em;margin-top:12px;color:#22c55e;font-weight:bold;text-align:center}
footer{padding:16px;text-align:center;color:#facc15;font-size:0.9em;margin-top:24px}
</style>
</head>
<body>
<header>
<h1>🦐 Daysha Kércia Camarão Premium</h1>
<p>Pedidos salvos na nuvem • Gestão profissional</p>
</header>

<section>
<h2>🛒 Fazer Pedido</h2>
<div class="card"><button class="btn" onclick="abrirPedido('Atacado',50,true)">Atacado • R$50/kg (mínimo 30kg)</button></div>
<div class="card"><button class="btn" onclick="abrirPedido('Varejo',60,false)">Varejo • R$60/kg</button></div>
</section>

<section id="pedido" class="hidden">
<h2>📝 Dados do Pedido</h2>
<div class="card">
<select id="unidade"><option>Bom Jardim</option><option>Barra do Ceará</option></select>
<input id="nome" placeholder="Nome" />
<input id="whats" placeholder="WhatsApp" />
<input id="kg" type="number" placeholder="Quantidade (kg)" oninput="calcular()" />
<input id="end" placeholder="Endereço completo" />
<p id="resumo"></p>
<button class="btn" onclick="enviarPedido()">Enviar Pedido</button>
</div>
</section>

<section id="admin" class="hidden">
<h2>📊 Painel Administrativo</h2>
<table>
<thead><tr><th>Data</th><th>Unidade</th><th>Cliente</th><th>WhatsApp</th><th>Endereço</th><th>Kg</th><th>Total</th><th>Status</th></tr></thead>
<tbody id="lista"></tbody>
</table>
<p id="relatorio"></p>
</section>

<footer>
&copy; 2025 Daysha Kércia • Todos os direitos reservados
</footer>

<script type="module">
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

let tipo='', preco=0, total=0, minimo=false;

if(location.hash==="#admin"){
  const s = prompt('Senha admin');
  if(s==='15day88'){
    admin.classList.remove('hidden');
    carregarPedidos();
  }
}

window.abrirPedido = (t,p,m)=>{ tipo=t; preco=p; minimo=m; pedido.classList.remove('hidden'); calcular(); }

window.calcular = ()=>{
  let q = Number(kg.value || 0);
  if(minimo && q<30){ resumo.innerText='Quantidade mínima para atacado é 30kg'; total=0; return; }
  total = q*preco;
  resumo.innerText = `Produto: ${tipo} | Quantidade: ${q}kg | Total: R$ ${total.toFixed(2)}`;
}

window.enviarPedido = async ()=>{
  if(total===0 || !nome.value || !whats.value || !end.value){ alert('Preencha todos os campos corretamente'); return; }
  await addDoc(collection(db,'pedidos'),{
    data: new Date().toLocaleString(), tipo, unidade:unidade.value,
    cliente:nome.value, whats:whats.value, endereco:end.value, kg:Number(kg.value), total, status:'Recebido'
  });

  // Envio de Email automático via EmailJS
  emailjs.send(EMAILJS_SERVICE_ID, EMAILJS_TEMPLATE_ID,{
    cliente:nome.value,
    whatsapp:whats.value,
    unidade:unidade.value,
    kg:kg.value,
    total:total.toFixed(2),
    endereco:end.value
  }).then(()=>console.log('Email enviado com sucesso')).catch(err=>console.error('Erro ao enviar email', err));

  // WhatsApp link com todos os dados
  const mensagem = `Novo pedido:\nCliente: ${nome.value}\nWhatsApp: ${whats.value}\nUnidade: ${unidade.value}\nQuantidade: ${kg.value}kg\nTotal: R$ ${total.toFixed(2)}\nEndereço: ${end.value}`;
  window.open(`https://wa.me/5585999566794?text=${encodeURIComponent(mensagem)}`, '_blank');

  alert('Pedido enviado com sucesso!');
  nome.value=''; whats.value=''; kg.value=''; end.value=''; resumo.innerText='';
}

function carregarPedidos(){
  let totalDia=0;
  onSnapshot(collection(db,'pedidos'), snap=>{
    lista.innerHTML=''; totalDia=0;
    snap.forEach(docSnap=>{
      const p=docSnap.data();
      totalDia+=p.total;
      lista.innerHTML+=`<tr>
        <td>${p.data}</td>
        <td>${p.unidade}</td>
        <td>${p.cliente}</td>
        <td>${p.whats}</td>
        <td>${p.endereco}</td>
        <td>${p.kg}</td>
        <td>R$ ${p.total.toFixed(2)}</td>
        <td class='status'>
          <select onchange="mudarStatus('${docSnap.id}',this.value)">
            <option ${p.status==='Recebido'?'selected':''}>Recebido</option>
            <option ${p.status==='Pago'?'selected':''}>Pago</option>
            <option ${p.status==='Saiu para entrega'?'selected':''}>Saiu para entrega</option>
          </select>
        </td>
      </tr>`;
    });
    relatorio.innerText = `Faturamento total registrado: R$ ${totalDia.toFixed(2)}`;
    new Audio('https://actions.google.com/sounds/v1/alarms/beep_short.ogg').play();
  });
}

window.mudarStatus = async (id, novo)=>{
  await updateDoc(doc(db,'pedidos',id), { status:novo });
}
</script>
</body>

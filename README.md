<!DOCTYPE html><html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cassino Pro Max Demo</title>
<style>
body{margin:0;font-family:Arial;background:#0b1020;color:white}
nav{display:flex;justify-content:space-between;align-items:center;background:#0f172a;padding:15px 25px}
nav h1{margin:0;font-size:22px}
nav .saldo{font-weight:bold}
nav button{background:#22c55e;border:none;color:white;padding:8px 16px;border-radius:6px;cursor:pointer}
.container{padding:25px}
.games{display:flex;flex-wrap:wrap;gap:20px;justify-content:center}
.card{background:#1f2937;border-radius:16px;padding:20px;width:260px;text-align:center;box-shadow:0 15px 35px rgba(0,0,0,0.4)}
button{background:#22c55e;border:none;color:white;padding:10px 16px;border-radius:8px;margin-top:10px;cursor:pointer}
input{padding:10px;border-radius:6px;border:none;margin-top:10px;width:80%}
.hidden{display:none}
.center{text-align:center}/* roleta */ .wheel{ width:120px; height:120px; border-radius:50%; border:8px solid #111; margin:10px auto; background:conic-gradient(red 0deg 90deg, green 90deg 180deg, red 180deg 270deg, black 270deg 360deg); transition:transform 2s ease-out; }

/* slot */ .slot-number{ font-size:32px; letter-spacing:10px; margin:10px 0; }

.modal{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.7);display:flex;align-items:center;justify-content:center} .modal-box{background:#1f2937;padding:30px;border-radius:12px;text-align:center}

</style>
<style>
/* ranking */
#ranking{max-width:500px;margin:30px auto;background:#111827;padding:15px;border-radius:12px}
#ranking h3{margin-top:0}
#ranking ul{list-style:none;padding:0;margin:0}
#ranking li{padding:6px 0;border-bottom:1px solid #1f2937}
</style>
</head>
<body><nav>
<h1>🎰 Cassino Pro Max</h1>
<div>
<span class="saldo">Saldo: R$ <span id="saldo">100</span></span>
<button onclick="abrirPix()">Depositar</button>
</div>
</nav><div class="container"><div id="login" class="card center" style="margin:auto">
<h2>Entrar no Cassino</h2>
<input id="usuario" placeholder="Digite seu nome">
<br>
<button onclick="entrar()">Entrar</button>
</div><div id="cassino" class="hidden">
<h2 class="center">Jogos do Cassino</h2><div class="games"><div class="card">
<h3>🎰 Slot</h3>
<div id="slotNums" class="slot-number">0 0 0</div>
<p id="slotResultado">Aposte R$10</p>
<button onclick="slot()">Girar</button>
</div><div class="card">
<h3>🎡 Roleta</h3>
<div id="wheel" class="wheel"></div>
<p id="roletaResultado">Aposte R$10</p>
<button onclick="roleta()">Rodar</button>
</div><div class="card">
<h3>🎲 Dado</h3>
<p id="dadoResultado">Clique para jogar</p>
<button onclick="dado()">Jogar</button>
</div><div class="card">
<h3>🃏 Carta</h3>
<p id="cartaResultado">Clique para puxar</p>
<button onclick="carta()">Puxar</button>
</div></div><div id="ranking" class="hidden">
<h3>🏆 Ranking de Jogadores</h3>
<ul id="rankingLista"></ul>
</div></div>
</div><div id="pixModal" class="modal hidden">
<div class="modal-box">
<h2>Depósito via PIX</h2>
<p>Simulação</p>
<button onclick="depositar(50)">Adicionar R$50</button><br><br>
<button onclick="depositar(100)">Adicionar R$100</button><br><br>
<button onclick="fecharPix()">Fechar</button>
</div>
</div><script>
let saldo = 100
let ranking = JSON.parse(localStorage.getItem('rankingCassino')||'[]')

function atualizarSaldo(); atualizarRanking(){
document.getElementById('saldo').innerText = saldo
}

function entrar(){
let nome=document.getElementById('usuario').value
if(nome===''){alert('Digite seu nome');return}
localStorage.setItem('jogador',nome)
mostrarCassino()
}

function mostrarCassino(){
document.getElementById('ranking').classList.remove('hidden')
document.getElementById('login').classList.add('hidden')
document.getElementById('cassino').classList.remove('hidden')
}

function abrirPix(){document.getElementById('pixModal').classList.remove('hidden')}
function fecharPix(){document.getElementById('pixModal').classList.add('hidden')}

function depositar(v){saldo+=v;atualizarSaldo(); atualizarRanking();fecharPix()}

function apostar(v){
if(saldo<v){alert('Saldo insuficiente');return false}
saldo-=v
atualizarSaldo(); atualizarRanking()
return true
}

function slot(){
if(!apostar(10)) return

let a=Math.floor(Math.random()*5)
let b=Math.floor(Math.random()*5)
let c=Math.floor(Math.random()*5)

let texto=a+" "+b+" "+c

document.getElementById('slotNums').innerText=texto

if(a===b && b===c){
saldo+=100
atualizarSaldo(); atualizarRanking()
document.getElementById('slotResultado').innerText='🎉 JACKPOT R$100'
}else if(a===b || b===c){
saldo+=20
atualizarSaldo(); atualizarRanking()
document.getElementById('slotResultado').innerText='✨ Ganhou R$20'
}else{
document.getElementById('slotResultado').innerText='😢 Não ganhou'
}
}

function roleta(){
if(!apostar(10)) return

let deg = Math.floor(Math.random()*720)+360
let wheel=document.getElementById('wheel')
wheel.style.transform="rotate("+deg+"deg)"

setTimeout(()=>{
let win=Math.random()<0.45

if(win){
saldo+=25
atualizarSaldo(); atualizarRanking()
document.getElementById('roletaResultado').innerText='🟢 Ganhou R$25'
}else{
document.getElementById('roletaResultado').innerText='🔴 Perdeu'
}
},2000)
}

function dado(){
if(!apostar(10)) return

let n=Math.floor(Math.random()*6)+1

if(n>=4){
saldo+=20
atualizarSaldo(); atualizarRanking()
document.getElementById('dadoResultado').innerText='🎲 '+n+' ganhou R$20'
}else{
document.getElementById('dadoResultado').innerText='🎲 '+n+' perdeu'
}
}

function carta(){
if(!apostar(10)) return

let cartas=['A','K','Q','J','10','9']
let c=cartas[Math.floor(Math.random()*cartas.length)]

if(c==='A'){
saldo+=50
atualizarSaldo(); atualizarRanking()
document.getElementById('cartaResultado').innerText='🃏 '+c+' JACKPOT R$50'
}else{
document.getElementById('cartaResultado').innerText='🃏 '+c+' sem prêmio'
}
}

atualizarSaldo(); atualizarRanking()

if(localStorage.getItem('jogador')) mostrarCassino()

function atualizarRanking(){
let nome = localStorage.getItem('jogador') || 'Jogador'
let existente = ranking.find(r=>r.nome===nome)

if(existente){
existente.saldo = saldo
}else{
ranking.push({nome:nome,saldo:saldo})
}

ranking.sort((a,b)=>b.saldo-a.saldo)

localStorage.setItem('rankingCassino',JSON.stringify(ranking))

let lista=document.getElementById('rankingLista')
if(!lista) return

lista.innerHTML=''
ranking.slice(0,5).forEach(r=>{
let li=document.createElement('li')
li.innerText=r.nome+' - R$'+r.saldo
lista.appendChild(li)
})
}

atualizarRanking()

</script></body>
</html>

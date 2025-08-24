<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Takajito Casino 🎰</title>
<style>
body{font-family:Arial,sans-serif;text-align:center;background:#222;color:#fff}
button,input,select{padding:8px;margin:5px;font-size:14px}
.slot,#card{font-size:50px;margin:20px}
.wallet{font-size:18px;margin-top:20px}
#loginForm,#casino,#adminPanel{display:none}
.tab{cursor:pointer;padding:10px 20px;background:#444;margin:5px;display:inline-block;border-radius:5px}
.tab.active{background:#888}
#leaderboard,#history,#deposit,#withdraw{margin-top:20px}
header{font-size:32px;font-weight:bold;margin-bottom:20px}
</style>
</head>
<body>
<header>Takajito Casino 🎰</header>
<div id="loginForm">
<input id="username" placeholder="Username"><br>
<input id="referral" placeholder="Referral code (optional)"><br>
<button onclick="login()">Enter Casino</button>
</div>
<div id="casino">
<div class="wallet">Coins: <span id="coins">1000</span></div>
<div>
<span class="tab active" onclick="tab('slotsTab', this)">Slots 🎰</span>
<span class="tab" onclick="tab('cardsTab', this)">Card 🃏</span>
<span class="tab" onclick="tab('adminTab', this)">Admin 🔧</span>
<span class="tab" onclick="tab('leaderboard', this)">Leaderboard 🏆</span>
<span class="tab" onclick="tab('history', this)">History 📜</span>
<span class="tab" onclick="tab('deposit', this)">Deposit 💰</span>
<span class="tab" onclick="tab('withdraw', this)">Withdraw 💸</span>
</div>
<div id="slotsTab"><span class="slot" id="slot1">🍒</span><span class="slot" id="slot2">🍋</span><span class="slot" id="slot3">🍊</span><br><button onclick="spin()">Spin 🎰</button></div>
<div id="cardsTab" style="display:none"><div id="card">🂠</div><br><button onclick="flip()">Flip 🃏</button></div>
<div id="adminTab" style="display:none">User: <span id="adminUser"></span><br>Coins: <span id="adminCoins"></span><br><button onclick="resetCoins()">Reset Coins</button></div>
<div id="leaderboard" style="display:none"><ul id="lb"></ul></div>
<div id="history" style="display:none"><ul id="hist"></ul></div>
<div id="deposit" style="display:none">
<h3>Deposit Coins</h3>
<input id="depAmount" type="number" placeholder="Amount"><br>
<select id="depMethod">
<option value="bKash">bKash</option>
<option value="Nogot">Nogot</option>
</select><br>
<button onclick="deposit()">Deposit</button>
</div>
<div id="withdraw" style="display:none">
<h3>Withdraw Coins</h3>
<input id="withAmount" type="number" placeholder="Amount"><br>
<select id="withMethod">
<option value="bKash">bKash</option>
<option value="Nogot">Nogot</option>
</select><br>
<button onclick="withdraw()">Withdraw</button>
</div>
<br><button onclick="logout()">Logout</button>
<div id="refInfo" style="margin-top:20px;font-size:16px"></div>
</div>
<script>
let coins=1000,currentUser='',referral='',lbData=[],hist=[];
const emojis=['🍒','🍋','🍊','🍉','🍇','⭐'],cards=['🂡','🂢','🂣','🂤','🂥','🂦','🂧','🂨','🂩','🂪','🂫','🂭','🂮'];
function login(){
currentUser=document.getElementById('username').value.trim();
referral=document.getElementById('referral').value.trim();
if(!currentUser){alert('Enter username!');return;}
if(referral){coins+=50;document.getElementById('refInfo').innerText='50 bonus coins for referral!';}
document.getElementById('loginForm').style.display='none';
document.getElementById('casino').style.display='block';
document.getElementById('coins').innerText=coins;
document.getElementById('adminUser').innerText=currentUser;
document.getElementById('adminCoins').innerText=coins;
updateLB();
}
function logout(){coins=1000;currentUser='';referral='';document.getElementById('loginForm').style.display='block';document.getElementById('casino').style.display='none';document.getElementById('username').value='';document.getElementById('referral').value='';document.getElementById('refInfo').innerText='';}
function spin(){if(coins<=0){alert('No coins!');return;}coins-=10;let s=[emojis[Math.floor(Math.random()*emojis.length)],emojis[Math.floor(Math.random()*emojis.length)],emojis[Math.floor(Math.random()*emojis.length)]];['slot1','slot2','slot3'].forEach((id,i)=>document.getElementById(id).innerText=s[i]);let res='Slots: '+s.join(' ');if(s[0]===s[1]&&s[1]===s[2]){coins+=100;res+=' -> Jackpot! +100';}hist.push(res);updateUI();}
function flip(){if(coins<=0){alert('No coins!');return;}coins-=5;let c=cards[Math.floor(Math.random()*cards.length)];document.getElementById('card').innerText=c;let res='Card: '+c;if(c==='🂡'){coins+=50;res+=' -> Ace! +50';}hist.push(res);updateUI();}
function tab(tabName,el){['slotsTab','cardsTab','adminTab','leaderboard','history','deposit','withdraw'].forEach(id=>document.getElementById(id).style.display='none');document.getElementById(tabName).style.display='block';document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));el.classList.add('active');}
function resetCoins(){coins=1000;updateUI();alert('Coins reset');}
function updateUI(){document.getElementById('coins').innerText=coins;document.getElementById('adminCoins').innerText=coins;updateLB();updateHist();}
function updateLB(){lbData.push({user:currentUser,coins});let sorted=[...lbData].sort((a,b)=>b.coins-a.coins).slice(0,5);let lb=document.getElementById('lb');lb.innerHTML='';sorted.forEach(e=>{let li=document.createElement('li');li.innerText=e.user+' - '+e.coins;lb.appendChild(li);});}
function updateHist(){let h=document.getElementById('hist');h.innerHTML='';hist.forEach(e=>{let li=document.createElement('li');li.innerText=e;h.appendChild(li);});}
function deposit(){let amt=parseInt(document.getElementById('depAmount').value);let method=document.getElementById('depMethod').value;if(!amt || amt<=0){alert('Enter valid amount');return;}coins+=amt;alert('Deposited '+amt+' coins via '+method);updateUI();document.getElementById('depAmount').value='';}
function withdraw(){let amt=parseInt(document.getElementById('withAmount').value);let method=document.getElementById('withMethod').value;if(!amt || amt<=0 || amt>coins){alert('Invalid amount');return;}coins-=amt;alert('Withdrew '+amt+' coins via '+method);updateUI();document.getElementById('withAmount').value='';}
</script>
</body>
</html>

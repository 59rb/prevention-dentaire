<div id="dentix-app" style="max-width:420px;margin:auto;padding:20px;border-radius:20px;background:#f0f9ff;border:2px solid #2563eb;font-family:system-ui;text-align:center">

<h2 style="margin-top:0;color:#1e40af">🦷 Chrono-Brossage</h2>

<!-- PARAMÈTRES -->
<div style="display:flex;gap:10px;justify-content:center;margin-bottom:10px">
<select id="mode" style="padding:8px;border-radius:10px">
  <option value="child">👶 Enfant</option>
  <option value="adult" selected>🧑 Adulte</option>
</select>

<select id="duration" style="padding:8px;border-radius:10px">
  <option value="60">1 min</option>
  <option value="120" selected>2 min</option>
  <option value="180">3 min</option>
</select>
</div>

<!-- TIMER -->
<div id="time" style="font-size:3rem;font-weight:bold;color:#2563eb">02:00</div>

<!-- DENTS -->
<div id="teeth" style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin:15px 0">
  <div class="zone">⬆️ Gauche</div>
  <div class="zone">⬆️ Droite</div>
  <div class="zone">⬇️ Gauche</div>
  <div class="zone">⬇️ Droite</div>
</div>

<!-- PROGRESS -->
<div style="height:12px;background:#dbeafe;border-radius:10px;overflow:hidden">
  <div id="bar" style="height:100%;width:0%;background:#2563eb"></div>
</div>

<p id="instruction" style="font-weight:600;margin:15px 0">
Prêt ? On commence 💪
</p>

<!-- BOUTONS -->
<div style="display:flex;gap:10px;justify-content:center">
<button id="start" onclick="toggle()" style="background:#22c55e;color:white;padding:12px 25px;border:none;border-radius:50px;font-weight:bold">Démarrer</button>
<button onclick="resetAll()" style="background:#6b7280;color:white;padding:12px 25px;border:none;border-radius:50px">Reset</button>
</div>

</div>

<script>
let timer=null;
let total=120;
let left=120;
let stepIndex=0;

const timeEl=time;
const bar=document.getElementById("bar");
const instruction=document.getElementById("instruction");
const startBtn=document.getElementById("start");
const zones=document.querySelectorAll(".zone");

const voices={
 child:[
  "Bas gauche !",
  "Bas droite !",
  "Haut gauche !",
  "Haut droite !"
 ],
 adult:[
  "Quadrant inférieur gauche",
  "Quadrant inférieur droit",
  "Quadrant supérieur gauche",
  "Quadrant supérieur droit"
 ]
};

function speak(txt){
  const u=new SpeechSynthesisUtterance(txt);
  u.lang="fr-FR";
  speechSynthesis.speak(u);
}

function update(){
  const m=Math.floor(left/60);
  const s=left%60;
  timeEl.textContent=`${String(m).padStart(2,"0")}:${String(s).padStart(2,"0")}`;
  bar.style.width=((total-left)/total*100)+"%";
}

function highlight(){
  zones.forEach(z=>z.style.background="#e0f2fe");
  zones[stepIndex].style.background="#93c5fd";
}

function toggle(){
  if(timer){
    clearInterval(timer);
    timer=null;
    startBtn.textContent="Reprendre";
    return;
  }

  startBtn.textContent="Pause";
  const mode=document.getElementById("mode").value;

  timer=setInterval(()=>{
    left--;
    update();

    if(left%30===0){
      stepIndex=(stepIndex+1)%4;
      highlight();
      speak(voices[mode][stepIndex]);
      if(navigator.vibrate) navigator.vibrate(200);
    }

    if(left<=0){
      clearInterval(timer);
      instruction.textContent="🎉 Bravo ! Dents propres !";
      speak("Bravo ! Brossage terminé");
      startBtn.style.display="none";
    }
  },1000);
}

function resetAll(){
  clearInterval(timer);
  timer=null;
  total=parseInt(duration.value);
  left=total;
  stepIndex=0;
  update();
  highlight();
  instruction.textContent="Prêt ? On commence 💪";
  startBtn.style.display="inline-block";
  startBtn.textContent="Démarrer";
}

duration.onchange=resetAll;
update();
highlight();
</script>

<style>
.zone{
 padding:10px;
 border-radius:12px;
 background:#e0f2fe;
 font-weight:600;
}
</style>

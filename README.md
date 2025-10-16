[admin.html](https://github.com/user-attachments/files/22957327/admin.html)[data.js](https://github.com/user-attachments/files/22957321/data.js)<img width="496" height="493" alt="icon-512" src="https://github.com/user-attachments/assets/e64bcba0-c373-4c76-81d0-432c9ffc7d54" /># PRONOTE-RP
PRONOTE[index.html](https://github.com/user-attachments/files/22957319/index.html)<!DOCTYPE html>
<html lang="fr">![Uploading icon-512.png…]<img width="192"// [Uplo<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>PRONOTE RP — Panneau Administrateur (Collège La Rondelle)</title>
<style>
  :root{
    --green:#17623b;
    --accent:#18a090;
    --danger:#d32f2f;
    --card:#ffffff;
    --bg:#f4f6f5;
    --text:#0e2b26;
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,Arial,sans-serif;background:var(--bg);color:var(--text)}
  header{background:linear-gradient(90deg,var(--green),#2e7d60);color:#fff;padding:14px 18px;display:flex;justify-content:space-between;align-items:center;box-shadow:0 4px 18px rgba(0,0,0,.08)}
  header h1{margin:0;font-size:18px}
  header .sub{font-size:13px;opacity:.95}
  nav.admin-nav{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;padding:10px;background:#eaf7f0;border-bottom:3px solid var(--green)}
  nav.admin-nav button{background:#fff;color:var(--green);border:2px solid var(--green);padding:9px 14px;border-radius:8px;cursor:pointer;font-weight:700;transition:transform .06s, background .2s, color .2s}
  nav.admin-nav button:hover{background:var(--green);color:#fff;transform:translateY(-2px)}
  nav.admin-nav button.inactive{background:var(--green);color:#fff}
  main{max-width:1150px;margin:18px auto;padding:12px}
  .grid{display:grid;grid-template-columns:1fr 360px;gap:14px}
  .full{grid-column:1/-1}
  .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 6px 18px rgba(0,0,0,.04)}
  h2{margin:0 0 10px;color:var(--green)}
  p.muted{color:#63706a;margin:6px 0}
  .controls{display:flex;gap:8px;flex-wrap:wrap}
  .btn{background:var(--green);color:#fff;border:0;padding:8px 12px;border-radius:8px;cursor:pointer}
  .btn.ghost{background:#fff;color:var(--green);border:1px solid #e6eee8}
  .btn.warn{background:var(--danger)}
  input[type=text],input[type=password],select,textarea{padding:8px;border-radius:8px;border:1px solid #e6eee8;width:100%;font-size:14px}
  table{width:100%;border-collapse:collapse;margin-top:10px}
  th,td{border:1px solid #e9f3ee;padding:8px;text-align:left;font-size:14px}
  th{background:#f0faf6;color:var(--green);font-weight:700}
  select{padding:6px;border-radius:6px}
  .smalltxt{font-size:13px;color:#6b756f}
  .perm-btn{background:#eee;border:0;border-radius:8px;padding:6px 10px;cursor:pointer;min-width:56px}
  .perm-btn.active{background:var(--accent);color:#fff}
  .blocked{opacity:.45}
  @media(max-width:980px){.grid{grid-template-columns:1fr}.admin-nav{overflow:auto;padding:8px}}
  .hidden{display:none}
</style>
</head>
<body>

<header>
  <div>
    <h1>PRONOTE RP — Collège La Rondelle</h1>
    <div class="sub">Panneau administrateur</div>
  </div>
  <div style="display:flex;gap:10px;align-items:center">
    <div class="smalltxt">Connecté : <strong>Directeur</strong></div>
    <div id="notifBell" style="position:relative;cursor:pointer;display:none;font-size:18px">🔔
      <span id="notifCount" style="position:absolute;top:-8px;right:-12px;background:var(--danger);color:#fff;border-radius:50%;padding:3px 7px;font-weight:700;font-size:12px">0</span>
    </div>
    <button class="btn ghost" id="exportData">Exporter JSON</button>
    <input type="file" id="importFile" style="display:none" accept="application/json"/>
    <button class="btn ghost" id="importData">Importer JSON</button>
  </div>
</header>

<nav class="admin-nav" aria-label="menu admin">
  <button data-section="dashboard" class="small active">🏠 Tableau de bord</button>
  <button data-section="users" class="small inactive">👥 Utilisateurs</button>
  <button data-section="permissions" class="small inactive">🔐 Permissions</button>
  <button data-section="timetables" class="small inactive">🗓️ Emplois du temps</button>
  <button data-section="absences" class="small inactive">📕 Absences</button>
  <button data-section="observations" class="small inactive">📝 Observations</button>
  <button data-section="settings" class="small inactive">⚙️ Paramètres</button>
</nav>

<main>
  <div class="grid">

    <div class="card section active full" id="dashboard">
      <h2>Tableau de bord</h2>
      <div style="display:flex;justify-content:space-between;gap:12px;align-items:center;flex-wrap:wrap">
        <div>
          <p class="smalltxt">Date / heure :</p>
          <div id="now" class="pill" style="display:inline-block;padding:6px 10px;background:#eef8f4;border-radius:8px;color:var(--green)"></div>
          <p class="smalltxt" style="margin-top:6px">Total comptes : <strong id="countUsers">0</strong></p>
        </div>
        <div style="min-width:320px">
          <div style="display:flex;gap:8px;flex-wrap:wrap">
            <button class="btn warn" id="blockAllBtn">🔒 Bloquer tout le monde</button>
            <button class="btn" id="unblockAllBtn">🔓 Débloquer tout le monde</button>
            <button class="btn ghost" id="clearData">Réinitialiser données</button>
          </div>
          <p class="smalltxt" style="margin-top:8px">Recherche rapide :</p>
          <input id="searchTop" placeholder="Rechercher (nom / identifiant)" />
        </div>
      </div>
    </div>

    <!-- Users -->
    <div class="card section" id="users" style="display:none">
      <h2>Gestion des utilisateurs</h2>

      <div style="display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap;margin-bottom:10px">
        <div style="display:flex;gap:8px;align-items:center;">
          <button class="btn" id="toggleAdd">+ Ajouter un utilisateur</button>
          <input id="searchUser" placeholder="Filtrer utilisateurs..." />
        </div>
        <div style="display:flex;gap:8px;align-items:center">
          <button class="btn ghost" id="exportUsers">Exporter utilisateurs</button>
          <button class="btn ghost" id="importUsers">Importer utilisateurs</button>
        </div>
      </div>

      <!-- add form -->
      <div id="addForm" style="display:none;margin-bottom:10px" class="card">
        <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:10px">
          <input id="in_prenom" placeholder="Prénom">
          <input id="in_nom" placeholder="Nom">
          <select id="in_role">
            <option value="Professeur">Professeur</option>
            <option value="Élève">Élève</option>
            <option value="Parent">Parent</option>
            <option value="Vie scolaire">Vie scolaire</option>
            <option value="Administrateur">Administrateur</option>
          </select>
          <input id="in_classe" placeholder="Classe (optionnel)">
          <input id="in_identifiant" placeholder="Nom d'utilisateur (laisser vide => auto)">
          <input id="in_mdp" type="password" placeholder="Mot de passe (sera haché)">
          <input id="in_pin" placeholder="PIN (4 chiffres)" maxlength="4">
          <select id="in_matiere1">
            <option value="">Matière 1 (si prof)</option>
          </select>
          <select id="in_matiere2">
            <option value="">Matière 2 (si prof)</option>
          </select>
          <div style="display:flex;gap:8px">
            <button class="btn" id="createUserBtn">Créer</button>
            <button class="btn ghost" id="cancelCreate">Annuler</button>
          </div>
        </div>
      </div>

      <div style="overflow:auto;max-height:520px">
        <table id="usersTable">
          <thead>
            <tr>
              <th>Nom</th><th>Prénom</th><th>Rôle</th><th>Classe</th>
              <th>Matière 1</th><th>Matière 2</th><th>Identifiant</th><th>Mot de passe</th><th>PIN</th><th>QR</th><th>Actions</th>
            </tr>
          </thead>
          <tbody id="usersBody"></tbody>
        </table>
      </div>
    </div>

    <!-- Permissions -->
    <div class="card section" id="permissions" style="display:none">
      <h2>Permissions globales</h2>
      <p class="smalltxt">Gère précisément qui peut faire quoi.</p>

      <div style="display:flex;gap:8px;align-items:center;margin-bottom:8px">
        <button class="btn" id="perm_check_all">✅ Tout activer</button>
        <button class="btn ghost" id="perm_uncheck_all">✖️ Tout désactiver</button>
        <button class="btn warn" id="perm_block_all">🔒 Bloquer tout</button>
        <button class="btn green" id="perm_unblock_all">🔓 Débloquer tout</button>
        <input id="permSearch" placeholder="Filtrer utilisateur..." style="margin-left:8px">
      </div>

      <div style="overflow:auto;max-height:560px">
        <table id="permTable">
          <thead id="permHead"></thead>
          <tbody id="permBody"></tbody>
        </table>
      </div>
    </div>

    <!-- Timetables -->
    <div class="card section" id="timetables" style="display:none">
      <h2>Emplois du temps</h2>
      <p class="smalltxt">(Module de démonstration)</p>
      <div style="display:flex;gap:8px;margin-bottom:8px">
        <input id="edt_classe" placeholder="Classe (ex : 5A)">
        <input id="edt_prof" placeholder="Prof (ex : Mme Dupont)">
        <input id="edt_matiere" placeholder="Matière">
        <input id="edt_horaire" placeholder="Horaire (ex: 9h00-10h00)">
        <button class="btn" id="edt_add">Ajouter</button>
      </div>
      <table id="edtTable"><thead><tr><th>Classe</th><th>Prof</th><th>Matière</th><th>Horaire</th><th>Actions</th></tr></thead><tbody></tbody></table>
    </div>

    <!-- Absences -->
    <div class="card section" id="absences" style="display:none">
      <h2>Absences & Remplacements</h2>
      <p class="smalltxt">Doro Party visible par tous si coché.</p>

      <div style="display:flex;gap:8px;margin-bottom:8px">
        <input id="abs_nom" placeholder="Nom du prof/élève">
        <input id="abs_type" placeholder="Type (absent / remplacé / doro / salle)">
        <input id="abs_date" type="date">
        <button class="btn" id="abs_add">Signaler</button>
      </div>
      <table id="absTable"><thead><tr><th>Nom</th><th>Type</th><th>Date</th><th>Actions</th></tr></thead><tbody></tbody></table>
    </div>

    <!-- Observations -->
    <div class="card section" id="observations" style="display:none">
      <h2>Observations élèves</h2>
      <div style="display:flex;gap:8px;margin-bottom:8px;flex-wrap:wrap">
        <input id="obs_eleve" placeholder="Nom élève">
        <input id="obs_date" type="date">
        <input id="obs_text" placeholder="Observation (texte)">
        <button class="btn" id="obs_add">Ajouter</button>
      </div>
      <table id="obsTable"><thead><tr><th>Élève</th><th>Date</th><th>Observation</th><th>Actions</th></tr></thead><tbody></tbody></table>
    </div>

    <!-- Settings -->
    <div class="card section" id="settings" style="display:none">
      <h2>Paramètres</h2>
      <div class="card">
        <label>Nom de l'établissement <input id="setting_name" type="text" value="Collège La Rondelle" /></label>
        <label>Email contact <input id="setting_email" type="email" value="contact@larondelle.edu" /></label>
        <div style="margin-top:10px">
          <label><input type="checkbox" id="setting_allow_notes"> Autoriser la saisie des notes par les profs</label><br>
          <label><input type="checkbox" id="setting_allow_doro"> Autoriser Doro Party (si coché)</label>
        </div>
        <div style="margin-top:10px">
          <button class="btn" id="saveSettings">Enregistrer paramètres</button>
        </div>
      </div>
    </div>

  </div>
</main>

<footer style="text-align:center;color:#6b756f;padding:10px;">
  © 2025 – PRONOTE RP · Collège La Rondelle
</footer>

<!-- ====================== SCRIPT PRINCIPAL ====================== -->
<script>
/* -----------------------------
   Utilitaires & état initial
   ----------------------------- */
const DATA_KEY = 'PRONOTE_RP_DATA_v1'; // stocke tout
// fonction SHA-256 (utilisée pour hacher mot de passe)
async function sha256(str){
  const buf = await crypto.subtle.digest("SHA-256", new TextEncoder().encode(String(str)));
  return Array.from(new Uint8Array(buf)).map(b=>b.toString(16).padStart(2,'0')).join('');
}

let state = {
  users: [],
  timetable: [],
  absences: [],
  observations: [],
  settings: { name: "Collège La Rondelle", email:"contact@larondelle.edu", allow_notes:false, allow_doro:false },
  history: []
};

function saveState(){ localStorage.setItem(DATA_KEY, JSON.stringify(state)); updateCounters(); }
function loadState(){
  const raw = localStorage.getItem(DATA_KEY);
  if(raw){ try{ state = JSON.parse(raw); }catch(e){ console.error(e); } }
  // si vide, initialiser exemples
  if(!state.users || state.users.length===0){
    (async ()=>{
      // create seeded users with hashed passwords
      const a = await sha256('adminpass');
      const b = await sha256('profpass');
      state.users = [
        { id:'u1', nom:'Dupont', prenom:'Jean', role:'Professeur', classe:'5A', matiere1:'Mathématiques', matiere2:'Physique-Chimie', identifiant:'jdupont', password:a, pin:'0000', actif:true, perms:{} },
        { id:'u2', nom:'Martin', prenom:'Alice', role:'Élève', classe:'4B', matiere1:'', matiere2:'', identifiant:'amartin', password:b, pin:'1111', actif:true, perms:{} },
        { id:'u3', nom:'Arthur', prenom:'L', role:'Administrateur', classe:'', matiere1:'', matiere2:'', identifiant:'arthur', password:a, pin:'2222', actif:true, perms:{} }
      ];
      state.history.push(`[${new Date().toLocaleString()}] Données initialisées`);
      saveState();
      renderAll();
    })();
  }
}
loadState();

/* ---------- affichage date & notifications ---------- */
function renderNow(){ document.getElementById('now').textContent = new Date().toLocaleString('fr-FR'); }
setInterval(renderNow, 60*1000); renderNow();

function updateNotifications(){
  const count = state.absences.length + state.observations.length;
  const bell = document.getElementById('notifBell');
  const countEl = document.getElementById('notifCount');
  if(count>0){ bell.style.display='inline-block'; countEl.textContent = count; }
  else bell.style.display='none';
}
updateNotifications();

/* ---------- NAV ---------- */
document.querySelectorAll('nav.admin-nav button').forEach(btn=>{
  btn.addEventListener('click', ()=> {
    document.querySelectorAll('nav.admin-nav button').forEach(b=>b.classList.remove('inactive'));
    btn.classList.add('inactive');
    const sec = btn.dataset.section;
    document.querySelectorAll('.section').forEach(s=>s.style.display='none');
    const el = document.getElementById(sec);
    if(el) el.style.display='block';
    renderAll();
  });
});

/* ---------- Matières list (finale) ---------- */
const MATIERES = [
  "Français","Mathématiques","Histoire-Géographie","Anglais","Espagnol",
  "SVT","Physique-Chimie","Technologie","EPS","Arts plastiques",
  "Éducation musicale","EMC","Roblox","Doro"
];
// populate selects for matieres
function populateMatieres(){
  const s1 = document.getElementById('in_matiere1');
  const s2 = document.getElementById('in_matiere2');
  s1.innerHTML = '<option value="">Matière 1 (si prof)</option>';
  s2.innerHTML = '<option value="">Matière 2 (si prof)</option>';
  MATIERES.forEach(m=>{ s1.innerHTML += `<option>${m}</option>`; s2.innerHTML += `<option>${m}</option>`; });
}
populateMatieres();

/* ---------- Render / Users ---------- */
const usersBody = document.getElementById('usersBody');
function renderUsers(filter=''){
  usersBody.innerHTML = '';
  const q = (filter||'').toLowerCase();
  state.users.forEach(u=>{
    const fullname = (u.prenom + ' ' + u.nom).toLowerCase();
    if(q && !fullname.includes(q) && !(u.identifiant||'').toLowerCase().includes(q)) return;
    const tr = document.createElement('tr');
    tr.className = u.actif? '' : 'blocked';
    tr.innerHTML = `
      <td>${u.nom}</td>
      <td>${u.prenom}</td>
      <td>${u.role}</td>
      <td>${u.classe||''}</td>
      <td>${u.matiere1||''}</td>
      <td>${u.matiere2||''}</td>
      <td>${u.identifiant||''}</td>
      <td><span class="smalltxt">••••••</span></td>
      <td>${u.pin||''}</td>
      <td><button class="btn ghost small" data-id="${u.id}" data-action="qr">🔳</button></td>
      <td style="white-space:nowrap">
        <button class="btn ghost" data-id="${u.id}" data-action="show">👁️</button>
        <button class="btn" data-id="${u.id}" data-action="toggle">${u.actif? 'Désactiver' : 'Activer'}</button>
        <button class="btn warn" data-id="${u.id}" data-action="del">Suppr</button>
      </td>
    `;
    usersBody.appendChild(tr);
  });
  attachUserActions();
  updateCounters();
}

function attachUserActions(){
  document.querySelectorAll('#usersBody button').forEach(btn=>{
    btn.addEventListener('click', async (e)=>{
      const id = btn.dataset.id;
      const action = btn.dataset.action;
      const userIndex = state.users.findIndex(x=>x.id===id);
      if(userIndex === -1) return;
      const user = state.users[userIndex];
      if(action==='toggle'){
        if(user.role==='Administrateur'){ alert('Impossible de désactiver un administrateur'); return; }
        user.actif = !user.actif; saveState(); renderAll();
      } else if(action==='del'){
        if(!confirm(`Supprimer ${user.prenom} ${user.nom} ?`)) return;
        state.users.splice(userIndex,1); saveState(); renderAll();
      } else if(action==='show'){
        alert(`Utilisateur: ${user.prenom} ${user.nom}\nIdentifiant: ${user.identifiant}\nPIN: ${user.pin||'(aucun)'}\nRôle: ${user.role}`);
      } else if(action==='qr'){
        // payload simple pour démonstration (ne pas utiliser en prod)
        const payload = `user:${user.identifiant};pin:${user.pin||''};role:${user.role}`;
        const url = `https://chart.googleapis.com/chart?cht=qr&chs=300x300&chl=${encodeURIComponent(payload)}`;
        window.open(url,'_blank');
      }
    });
  });
}

/* add user form handling (uses hashing) */
document.getElementById('toggleAdd').addEventListener('click', ()=> {
  document.getElementById('addForm').style.display = document.getElementById('addForm').style.display === 'none' ? 'block' : 'none';
});
document.getElementById('cancelCreate').addEventListener('click', ()=> { document.getElementById('addForm').style.display='none'; });

document.getElementById('createUserBtn').addEventListener('click', async ()=>{
  const prenom = document.getElementById('in_prenom').value.trim();
  const nom = document.getElementById('in_nom').value.trim();
  const role = document.getElementById('in_role').value;
  const classe = document.getElementById('in_classe').value.trim();
  let ident = document.getElementById('in_identifiant').value.trim();
  const rawPass = document.getElementById('in_mdp').value || Math.random().toString(36).slice(2,9);
  const pin = document.getElementById('in_pin').value.trim();
  const mat1 = document.getElementById('in_matiere1').value;
  const mat2 = document.getElementById('in_matiere2').value;

  if(!prenom || !nom){ alert('Nom et prénom requis'); return; }
  if(pin && !/^\d{4}$/.test(pin)){ alert('PIN = 4 chiffres'); return; }
  if(!ident) ident = (prenom[0]||'') + '.' + nom.toLowerCase();
  // uniqueness
  if(state.users.some(u=>u.identifiant && u.identifiant.toLowerCase()===ident.toLowerCase())){ alert('Identifiant déjà pris'); return; }

  const passHash = await sha256(rawPass);
  const newUser = {
    id: 'u_'+Date.now(),
    nom, prenom, role, classe,
    matiere1: role==='Professeur'? mat1 : '',
    matiere2: role==='Professeur'? mat2 : '',
    identifiant: ident,
    password: passHash,
    pin: pin||null,
    actif: true,
    perms: {}
  };
  state.users.push(newUser);
  state.history.unshift(`[${new Date().toLocaleString()}] Création compte ${ident} (${role})`);
  saveState();
  alert(`Compte créé : ${ident}\nMot de passe (en clair) : ${rawPass}\n(Conserver ce mot de passe pour le test)`);
  // reset form
  document.getElementById('addForm').querySelectorAll('input').forEach(i=>i.value='');
  document.getElementById('in_matiere1').value = '';
  document.getElementById('in_matiere2').value = '';
  renderAll();
});

/* ---------- Permissions table ---------- */
const permsList = [
  "Modifier les notes",
  "Mettre des commentaires",
  "Accéder aux emplois du temps",
  "Modifier les absences",
  "Gérer les utilisateurs",
  "Voir les incidents",
  "Créer des sondages",
  "Envoyer des messages",
  "Exporter les données",
  "Activer le mode Doro Party"
];

function renderPermTable(filter=''){
  const head = document.getElementById('permHead'); const body = document.getElementById('permBody');
  head.innerHTML=''; body.innerHTML='';
  const hr = document.createElement('tr');
  hr.innerHTML = `<th>Utilisateur</th><th>Rôle</th><th>Matières</th>` + permsList.map(p=>`<th style="white-space:nowrap">${p}</th>`).join('') + `<th>Statut</th>`;
  head.appendChild(hr);
  const q = (filter||'').toLowerCase();
  state.users.forEach(u=>{
    const fullname = `${u.prenom} ${u.nom}`.toLowerCase();
    if(q && !fullname.includes(q) && !(u.identifiant||'').toLowerCase().includes(q)) return;
    const tr = document.createElement('tr');
    const matDisplay = (u.role==='Professeur')? `<div><strong>${u.matiere1||'—'}</strong></div><div><strong>${u.matiere2||'—'}</strong></div>` : '<span style="color:#aaa">—</span>';
    tr.innerHTML = `<td>${u.prenom} ${u.nom}</td><td>${u.role}</td><td>${matDisplay}</td>` +
      permsList.map((p,idx)=>{
        const key = 'p'+idx;
        const on = !!(u.perms && u.perms[key]);
        return `<td><button class="perm-btn ${on?'active':''}" data-uid="${u.id}" data-key="${key}">${on?'ON':'OFF'}</button></td>`;
      }).join('') + `<td>${u.actif? '✅ Actif' : '🔒 Bloqué'}</td>`;
    body.appendChild(tr);
  });
  // listeners
  document.querySelectorAll('.perm-btn').forEach(b=>{
    b.addEventListener('click', ()=>{
      const uid = b.dataset.uid; const key = b.dataset.key;
      const user = state.users.find(x=>x.id===uid); if(!user) return;
      user.perms = user.perms || {}; user.perms[key] = !user.perms[key];
      saveState(); renderPermTable();
    });
  });
}

/* perm controls */
document.getElementById('perm_check_all').addEventListener('click', ()=>{
  state.users.forEach(u=>{ u.perms = u.perms || {}; permsList.forEach((p,idx)=> u.perms['p'+idx]=true); }); saveState(); renderPermTable();
});
document.getElementById('perm_uncheck_all').addEventListener('click', ()=>{
  state.users.forEach(u=>{ u.perms = u.perms || {}; permsList.forEach((p,idx)=> u.perms['p'+idx]=false); }); saveState(); renderPermTable();
});
document.getElementById('perm_block_all').addEventListener('click', ()=>{
  if(!confirm('Bloquer tous les utilisateurs (sauf admin) ?')) return;
  state.users.forEach(u=>{ if(u.role!=='Administrateur') u.actif=false; }); saveState(); renderAll();
});
document.getElementById('perm_unblock_all').addEventListener('click', ()=>{
  state.users.forEach(u=> u.actif=true); saveState(); renderAll();
});
document.getElementById('permSearch').addEventListener('input', (e)=> renderPermTable(e.target.value));

/* ---------- Timetable ---------- */
document.getElementById('edt_add').addEventListener('click', ()=>{
  const cl = document.getElementById('edt_classe').value.trim(); const prof = document.getElementById('edt_prof').value.trim();
  const mat = document.getElementById('edt_matiere').value.trim(); const horaire = document.getElementById('edt_horaire').value.trim();
  if(!cl||!prof||!mat||!horaire) return alert('Remplis tous les champs');
  state.timetable.push({id:Date.now(),classe:cl,prof,matiere:mat,horaire}); saveState(); renderAll();
});
function renderTimetable(){
  const tbody = document.querySelector('#edtTable tbody'); tbody.innerHTML='';
  state.timetable.forEach(r=>{ const tr=document.createElement('tr'); tr.innerHTML=`<td>${r.classe}</td><td>${r.prof}</td><td>${r.matiere}</td><td>${r.horaire}</td><td><button class="btn" onclick="deleteTT(${r.id})">Suppr</button></td>`; tbody.appendChild(tr); });
}
window.deleteTT = function(id){ state.timetable = state.timetable.filter(x=>x.id!==id); saveState(); renderAll(); };

/* ---------- Absences ---------- */
document.getElementById('abs_add').addEventListener('click', ()=>{
  const nom = document.getElementById('abs_nom').value.trim(); const type = document.getElementById('abs_type').value.trim(); const date = document.getElementById('abs_date').value;
  if(!nom||!type||!date) return alert('Remplis tous les champs');
  state.absences.push({id:Date.now(),nom,type,date,public: type==='doro'});
  saveState(); renderAll();
});
function renderAbsences(){
  const tbody = document.querySelector('#absTable tbody'); tbody.innerHTML='';
  state.absences.forEach((a,i)=>{ const tr=document.createElement('tr'); tr.innerHTML=`<td>${a.nom}</td><td>${a.type}</td><td>${a.date}</td><td><button class="btn" onclick="deleteAbs(${i})">Suppr</button></td>`; tbody.appendChild(tr); });
}
window.deleteAbs = function(i){ state.absences.splice(i,1); saveState(); renderAll(); };

/* ---------- Observations ---------- */
document.getElementById('obs_add').addEventListener('click', ()=>{
  const nom = document.getElementById('obs_eleve').value.trim(); const date = document.getElementById('obs_date').value; const txt = document.getElementById('obs_text').value.trim();
  if(!nom||!date||!txt) return alert('Remplis tous les champs');
  state.observations.push({id:Date.now(),eleve:nom,date,txt}); saveState(); renderAll();
});
function renderObservations(){ const tbody=document.querySelector('#obsTable tbody'); tbody.innerHTML=''; state.observations.forEach(o=>{ const tr=document.createElement('tr'); tr.innerHTML=`<td>${o.eleve}</td><td>${o.date}</td><td>${o.txt}</td><td><button class="btn" onclick="deleteObs(${o.id})">Suppr</button></td>`; tbody.appendChild(tr); }); }
window.deleteObs = function(id){ state.observations = state.observations.filter(x=>x.id!==id); saveState(); renderAll(); };

/* ---------- Settings ---------- */
document.getElementById('saveSettings').addEventListener('click', ()=>{
  state.settings.name = document.getElementById('setting_name').value;
  state.settings.email = document.getElementById('setting_email').value;
  state.settings.allow_notes = document.getElementById('setting_allow_notes').checked;
  state.settings.allow_doro = document.getElementById('setting_allow_doro').checked;
  saveState(); alert('Paramètres enregistrés');
});

/* ---------- Export / Import ---------- */
document.getElementById('exportData').addEventListener('click', ()=>{
  const blob = new Blob([JSON.stringify(state,null,2)],{type:'application/json'});
  const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = 'pronote_data.json'; a.click();
});
document.getElementById('importData').addEventListener('click', ()=> document.getElementById('importFile').click());
document.getElementById('importFile').addEventListener('change', (e)=>{
  const f = e.target.files[0]; if(!f) return;
  const r = new FileReader(); r.onload = ()=> {
    try{ state = JSON.parse(r.result); saveState(); renderAll(); alert('Import OK'); } catch(err){ alert('Erreur import'); }
  }; r.readAsText(f);
});

/* ---------- Clear / block controls ---------- */
document.getElementById('blockAllBtn').addEventListener('click', ()=> {
  if(!confirm('Bloquer tous les utilisateurs (sauf admin) ?')) return;
  state.users.forEach(u=>{ if(u.role!=='Administrateur') u.actif=false; }); saveState(); renderAll();
});
document.getElementById('unblockAllBtn').addEventListener('click', ()=> {
  state.users.forEach(u=> u.actif=true); saveState(); renderAll();
});
document.getElementById('clearData').addEventListener('click', ()=> {
  if(!confirm('Réinitialiser les données locales ?')) return;
  localStorage.removeItem(DATA_KEY); state = {users:[],timetable:[],absences:[],observations:[],settings:{name:"Collège La Rondelle",email:"contact@larondelle.edu",allow_notes:false,allow_doro:false},history:[]}; saveState(); renderAll();
});

/* ---------- UI helpers ---------- */
function updateCounters(){ document.getElementById('countUsers').textContent = state.users.length; updateNotifications(); }
function renderAll(){
  renderUsers(document.getElementById('searchUser')?.value || '');
  renderPermTable(document.getElementById('permSearch')?.value || '');
  renderTimetable();
  renderAbsences();
  renderObservations();
  updateCounters();
}
document.getElementById('searchUser').addEventListener('input',(e)=> renderUsers(e.target.value));
document.getElementById('searchTop').addEventListener('input',(e)=> renderUsers(e.target.value));

/* initial render */
renderAll();

/* expose for console debugging */
window._PRONOTE = { state, saveState, loadState, renderAll };

</script>

</body>
</html>ading admin.html…]()
==========================
//  FICHIER : admin.js
// ==========================

// --- Sélection de tous les boutons du menu
const navButtons = document.querySelectorAll(".nav-btn");
const sections = document.querySelectorAll(".section");

// --- Fonction pour changer de section
function showSection(id) {
  sections.forEach(section => {
    section.classList.remove("active");
    if (section.id === id) {
      section.classList.add("active");
    }
  });
}

// --- Événements au clic sur les boutons
navButtons.forEach(btn => {
  btn.addEventListener("click", () => {
    const target = btn.getAttribute("data-target");
    showSection(target);

    // Visuellement, on montre quel bouton est sélectionné
    navButtons.forEach(b => b.classList.remove("active"));
    btn.classList.add("active");
  });
});

// --- Style pour la section active
document.addEventListener("DOMContentLoaded", () => {
  sections.forEach(section => {
    if (!section.classList.contains("active")) {
      section.style.display = "none";
    }
  });

  // Montrer la première section
  document.querySelector(".section.active").style.display = "block";
});

// --- Mutation automatique pour cacher / afficher les sections
const observer = new MutationObserver(() => {
  sections.forEach(section => {
    section.style.display = section.classList.contains("active") ? "block" : "none";
  });
});
observer.observe(document.body, { subtree: true, attributes: true, attributeFilter: ["class"] });[admin.js](https://github.com/user-attachments/files/22957324/admin.js)
 height="191" alt="icon-192" src="https://github.com/user-attachment<script>
// ...
(function renderGrades(){
  const me=PRN.getSession(); const db=PRN.loadDB();
  const grades = PRN.listGradesForStudent(me.id);
  const box = document.getElementById('grades');
  const empty = document.getElementById('gradesEmpty');
  if(!grades.length){ box.innerHTML=""; empty.style.display='block'; return; }
  empty.style.display='none';
  box.innerHTML = grades.slice(-10).reverse().map(g=>{
    const c = g.comment ? `<div class="muted" style="margin-top:2px">💬 ${g.comment}</div>` : '';
    return `
      <div class="item">
        <div class="left">
          <span class="subj">${g.subject}</span> — ${g.title||'Évaluation'} (${g.date})
          ${c}
        </div>
        <div class="right">${g.value} / ${g.max}</div>
      </div>`;
  }).join('');
})();
</script>
<!-- MENU LATERAL UNIVERSEL -->
<div id="menuOverlay" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,0.3);z-index:90;"></div>

<nav id="sideMenu" style="
  position:fixed;top:0;left:-270px;width:270px;height:100%;
  background:#333;color:white;z-index:100;overflow-y:auto;
  transition:0.3s ease all;padding:16px;font-family:'Inter';
">
  <div style="margin-bottom:12px;border-bottom:1px solid rgba(255,255,255,0.1);padding-bottom:10px;">
    <div style="display:flex;align-items:center;gap:10px;">
      <div style="font-size:24px;">🏫</div>
      <div>
        <strong id="menuName" style="font-size:14px;">Utilisateur</strong><br>
        <span id="menuRole" style="font-size:12px;opacity:0.8;">Rôle</span><br>
        <span style="font-size:11px;opacity:0.7;">Dernière connexion : ven. 11 juil. à 13h30</span>
      </div>
    </div>
  </div>

  <div id="menuLinks">
    <p><a href="#" class="menuLink">Emploi du temps</a></p>
    <p><a href="#" class="menuLink">Incidents</a></p>
    <p><a href="#" class="menuLink">Mes données</a></p>
    <p><a href="#" class="menuLink">Appel et suivi</a></p>
    <p><a href="#" class="menuLink">Forums pédagogiques</a></p>
    <p><a href="#" class="menuLink">Stage</a></p>
    <p><a href="#" class="menuLink">Communication</a></p>
    <p><a href="#" class="menuLink">Missions</a></p>
    <hr style="border:none;border-top:1px solid rgba(255,255,255,0.1);margin:12px 0">
    <p><a href="#" id="logoutBtn" class="menuLink">🚪 Déconnexion</a></p>
  </div>
</nav>

<!-- BOUTON MENU ET CLOCHE -->
<div id="menuBtn" style="position:fixed;top:16px;right:16px;z-index:99;background:#fff;padding:8px;border-radius:10px;box-shadow:0 3px 10px rgba(0,0,0,0.2);cursor:pointer;">☰</div>

<div id="notifBtn" style="position:fixed;top:70px;right:16px;z-index:99;background:#fff;padding:8px 10px;border-radius:50%;box-shadow:0 3px 10px rgba(0,0,0,0.2);cursor:pointer;">
  🔔<span style="background:red;color:white;border-radius:50%;font-size:11px;padding:2px 5px;position:relative;top:-8px;left:-5px;">16</span>
</div>

<script>
(function(){
  const menu = document.getElementById('sideMenu');
  const overlay = document.getElementById('menuOverlay');
  const btn = document.getElementById('menuBtn');
  const notifBtn = document.getElementById('notifBtn');
  const logout = document.getElementById('logoutBtn');

  // afficher nom + rôle
  const s = APP.getSession();
  document.getElementById('menuName').textContent = s ? s.name : 'Utilisateur';
  document.getElementById('menuRole').textContent = s ? s.role.toUpperCase() : 'Visiteur';

  // clic menu
  function openMenu(){
    menu.style.left = "0";
    overlay.style.display = "block";
  }
  function closeMenu(){
    menu.style.left = "-270px";
    overlay.style.display = "none";
  }
  btn.onclick = openMenu;
  overlay.onclick = closeMenu;

  // clic cloche
  notifBtn.onclick = ()=>{ window.location.href = "notifications.html"; };

  // logout
  logout.onclick = ()=>{ APP.clearSession(); alert('Déconnecté.'); location.href='index.html'; };

  // adaptation par rôle
  if(s){
    const all = document.querySelectorAll('.menuLink');
    all.forEach(l=>{
      if(s.role==='eleve' || s.role==='parent'){
        // cacher appel, forums, missions
        if(l.textContent.includes('Appel') || l.textContent.includes('Forums') || l.textContent.includes('Missions')){
          l.style.display='none';
        }
      }
      if(s.role==='prof'){
        if(l.textContent.includes('Stage') || l.textContent.includes('Communication')){
          l.style.display='none';
        }
      }
    });
  }
})();
</script>[app-eleve.html](https://github.com/user-attachments/files/22957323/app-eleve.html)
s/assets/60768165-6dc9-40a9-8804-eb5f8d4b30dc" />[Uploading data.js…]// PRONOTE RP - data.js (v5)
const DB_KEY='PRONOTE_RP_DB_V5'; const SESSION_KEY='PRP_SESSION';

const PERMS={
  ADMIN:['manage.users','see.all','write.all','settings.edit','backup','impersonate'],
  PROF:['hw.add','hw.edit','att.view','grades.add','grades.view','grades.comment'],
  VIESCO:['att.add','att.edit','att.view','timetable.edit'],
  ELEVE:[], PARENT:[]
};

// ---------- Utils ----------
const todayISO=(d=new Date())=>d.toISOString().slice(0,10);
const plusDays=n=>todayISO(new Date(Date.now()+n*86400000));
const uid=()=>Math.random().toString(36).slice(2,10);

// ---------- DB ----------
function loadDB(){
  let db; try{db=JSON.parse(localStorage.getItem(DB_KEY))||{}}catch{db={}}
  if(!db.version){
    db={
      version:5,
      etab:{name:'Collège La Rondelle', uai:'0000000X'},
      settings:{
        theme:'violet', maintenance:false,
        banner:{enabled:false, text:''},
        features:{messages:true, canteen:true, documents:true, observations:true}
      },
      classes:[{id:1,name:'6C',level:'6e'}],
      subjects:[{id:1,name:'FRANÇAIS',short:'FRA'},{id:2,name:'MATHÉMATIQUES',short:'MATH'}],
      users:[
        {id:1,role:'admin',username:'directeur',pass:'admin',name:'Direction',active:true,perms:[...PERMS.ADMIN]},
        {id:2,role:'prof',username:'prof1',pass:'prof',name:'Mme Dupont',active:true,perms:[...PERMS.PROF]},
        {id:3,role:'viesco',username:'vie1',pass:'viesco',name:'Vie Scolaire',active:true,perms:[...PERMS.VIESCO]},
        {id:4,role:'eleve',username:'eleve1',pass:'1234',name:'Élève Démo',classId:1,active:true,perms:[...PERMS.ELEVE]},
        {id:5,role:'parent',username:'parent1',pass:'parent',name:'Parent Démo',children:[4],active:true,perms:[...PERMS.PARENT]}
      ],
      timetable:[{id:1,date:todayISO(),start:'08:30',end:'09:30',subjectId:1,classId:1,room:'F2',teacherUserId:2}],
      homeworks:[{id:1,classId:1,subjectId:2,title:'Exercices 12 à 16',details:'Cahier p.42',given:todayISO(),due:plusDays(1),byUserId:2}],
      grades:[], // {id, studentUserId, subjectId, period, value, max, coeff, title, date, byUserId, comment}
      attendance:{absences:[],tardies:[],detentions:[]},
      observations:[], // {id, studentUserId, date, severity, reason, byUserId}
      announcements:[], documents:[], canteen:[],
      audit:[], // {id, dateISO, whoUserId, action, details}
      lastIds:{user:5,hw:1,tt:1,abs:0,tard:0,det:0,grade:0,obs:0,ann:0,doc:0},
      qrTokens:{}
    };
    saveDB(db);
  }
  return db;
}
function saveDB(db){ localStorage.setItem(DB_KEY, JSON.stringify(db)); }
function logAudit(whoUserId, action, details){ const db=loadDB(); db.audit.unshift({id:uid(), dateISO:new Date().toISOString(), whoUserId, action, details}); db.audit=db.audit.slice(0,500); saveDB(db); }

// ---------- Session ----------
function setSession(user){ sessionStorage.setItem(SESSION_KEY, JSON.stringify(user)); }
function getSession(){ try{return JSON.parse(sessionStorage.getItem(SESSION_KEY))}catch{return null} }
function clearSession(){ sessionStorage.removeItem(SESSION_KEY); }

// Login / Impersonate
function authLogin(username,pass){
  const db=loadDB();
  const u=db.users.find(x=>x.username===username && x.pass===pass && x.active!==false);
  if(!u) return null;
  const s={id:u.id,role:u.role,username:u.username,name:u.name,classId:u.classId,children:u.children||[],perms:u.perms||[]};
  setSession(s); logAudit(u.id,'login','');
  return u;
}
function impersonate(userId){
  const db=loadDB(); const me=getSession();
  if(!hasPerm(me,'impersonate')) throw new Error('Droit impersonate requis');
  const u=db.users.find(x=>x.id===userId && x.active!==false); if(!u) throw new Error('Utilisateur introuvable');
  const s={id:u.id,role:u.role,username:u.username,name:u.name,classId:u.classId,children:u.children||[],perms:u.perms||[]};
  setSession(s); logAudit(me.id,'impersonate',`-> ${u.username}`); return s;
}

// ---------- QR login ----------
function createQrToken(userId, minutes=30){
  const db=loadDB(); const expires=new Date(Date.now()+minutes*60000).toISOString();
  const token=btoa(unescape(encodeURIComponent(`${userId}|${expires}`)));
  db.qrTokens[token]={userId,expires}; saveDB(db); return token;
}
function useQrToken(token){
  const db=loadDB(); const rec=db.qrTokens[token]; if(!rec) return null;
  if(new Date(rec.expires)<new Date()){ delete db.qrTokens[token]; saveDB(db); return null; }
  const u=db.users.find(x=>x.id===rec.userId && x.active!==false); if(!u) return null;
  delete db.qrTokens[token]; saveDB(db);
  const s={id:u.id,role:u.role,username:u.username,name:u.name,classId:u.classId,children:u.children||[],perms:u.perms||[]};
  setSession(s); logAudit(u.id,'qr-login',''); return u;
}

// ---------- Droits ----------
function hasPerm(uOrS,perm){ const s=uOrS?.perms?uOrS:getSession(); return Array.isArray(s?.perms)&&s.perms.includes(perm); }
function grantPerm(userId,perm){ const db=loadDB(); const u=db.users.find(x=>x.id===userId); if(!u)return; u.perms=u.perms||[]; if(!u.perms.includes(perm)) u.perms.push(perm); saveDB(db); logAudit(getSession()?.id||0,'grant',`${userId}:${perm}`); }
function revokePerm(userId,perm){ const db=loadDB(); const u=db.users.find(x=>x.id===userId); if(!u||!u.perms)return; u.perms=u.perms.filter(p=>p!==perm); saveDB(db); logAudit(getSession()?.id||0,'revoke',`${userId}:${perm}`); }

// ---------- Admin / données ----------
function addUser(d){ const db=loadDB(); db.lastIds.user++; d.id=db.lastIds.user; if(!d.perms){ d.perms=[...(PERMS[(d.role||'').toUpperCase()]||[])] } db.users.push(d); saveDB(db); logAudit(getSession()?.id||0,'user.add',`${d.username}`); return d; }
function listUsersByRole(role){ return loadDB().users.filter(u=>u.role===role); }

function addHomework(hw){ if(!hasPerm(null,'hw.add')) throw new Error('Pas le droit'); const db=loadDB(); db.lastIds.hw++; hw.id=db.lastIds.hw; db.homeworks.push(hw); saveDB(db); logAudit(getSession()?.id||0,'hw.add',`class:${hw.classId}`); return hw; }
function listHomeworksForClass(classId,{fromDate=null}={}){ const db=loadDB(); return db.homeworks.filter(h=>h.classId==classId && (!fromDate||h.due>=fromDate)).map(h=>({...h,subject:db.subjects.find(s=>s.id===h.subjectId)?.name||'Matière'})); }

function listDayTimetable(classId,dateISO){ const db=loadDB(); return db.timetable.filter(t=>t.classId==classId&&t.date===dateISO).map(t=>({...t,subject:db.subjects.find(s=>s.id===t.subjectId)?.name||'Cours'})); }

// ✨ Notes + commentaire (contrôlé par permission)
function addGrade(g){
  if(!hasPerm(null,'grades.add') && !hasPerm(null,'write.all')) throw new Error('Pas le droit');
  const db=loadDB(); db.lastIds.grade++; g.id=db.lastIds.grade;
  if(typeof g.max==='undefined') g.max=20;
  if(typeof g.coeff==='undefined') g.coeff=1;
  const canComment = hasPerm(null,'grades.comment') || hasPerm(null,'write.all');
  g.comment = canComment ? (g.comment || '') : '';
  db.grades.push(g); saveDB(db); logAudit(getSession()?.id||0,'grade.add',`stu:${g.studentUserId} subj:${g.subjectId}`);
  return g;
}
function listGradesForStudent(studentUserId){
  const db=loadDB();
  return db.grades.filter(x=>x.studentUserId===studentUserId).map(x=>({...x,subject:db.subjects.find(s=>s.id===x.subjectId)?.name||'Matière'}));
}

// Annonces / Sauvegarde / Réglages
function addAnnouncement(a){ const db=loadDB(); db.lastIds.ann++; a.id=db.lastIds.ann; a.dateISO=new Date().toISOString(); db.announcements.unshift(a); db.announcements=db.announcements.slice(0,200); saveDB(db); logAudit(getSession()?.id||0,'ann.add',a.title); return a; }
function exportJSON(){ return JSON.stringify(loadDB(),null,2); }
function importJSON(text){ const obj=JSON.parse(text); if(!obj||!obj.version) throw new Error('Fichier invalide'); localStorage.setItem(DB_KEY, JSON.stringify(obj)); logAudit(getSession()?.id||0,'backup.restore',''); }
function setSetting(path,value){ const db=loadDB(); const parts=path.split('.'); let cur=db.settings; while(parts.length>1){ const k=parts.shift(); cur=cur[k]; } cur[parts[0]]=value; saveDB(db); logAudit(getSession()?.id||0,'settings.set',`${path}=${value}`); }

// --- Codes personnels (PIN) ---
function setUserPin(userId, pin){
  const db = loadDB(); const u = db.users.find(x=>x.id===userId);
  if(!u) throw new Error('Utilisateur introuvable');
  if(!/^\d{4,8}$/.test(pin)) throw new Error('Code invalide (4 à 8 chiffres).');
  u.pin = pin; saveDB(db); logAudit(getSession()?.id||0,'pin.set',`${userId}`);
}
function clearUserPin(userId){ const db=loadDB(); const u=db.users.find(x=>x.id===userId); if(!u) return; delete u.pin; saveDB(db); logAudit(getSession()?.id||0,'pin.clear',`${userId}`); }
function hasUserPin(userId){ const u=loadDB().users.find(x=>x.id===userId); return !!(u && u.pin); }

window.PRN={
  loadDB, saveDB, getSession, clearSession, authLogin, impersonate,
  createQrToken, useQrToken,
  hasPerm, grantPerm, revokePerm, PERMS,
  addUser, listUsersByRole,
  addHomework, listHomeworksForClass, listDayTimetable,
  addGrade, listGradesForStudent,
  addAnnouncement, exportJSON, importJSON, setSetting,
  setUserPin, clearUserPin, hasUserPin
};
// -------------------------------
// 📘 Données des absences profs
// -------------------------------

const profsAbsents = [
  {
    nom: "Mme Dupont",
    date: "2025-10-17",
    heure: "08h00 - 09h00",
    classe: "5B",
    type: "absent"
  },
  {
    nom: "M. Laurent",
    date: "2025-10-17",
    heure: "10h00 - 11h00",
    classe: "4A",
    type: "doro" // 🔴 Doro Party
  },
  {
    nom: "Mme Petit",
    date: "2025-10-17",
    heure: "11h00 - 12h00",
    classe: "6C",
    type: "remplace"
  },
  {
    nom: "M. Bernard",
    date: "2025-10-17",
    heure: "14h00 - 15h00",
    classe: "3D",
    type: "changement"
  }
];
// data.js
// Simple "backend" localStorage pour PRONOTE RP (données locales, pas de serveur)
// Ne modifie rien d'existant — on ajoute des fonctions utilisables par les pages.

// --- Key ---
const DB_KEY = 'PRONOTE_RP_DB_v1';
const SESSION_KEY = 'PRONOTE_RP_SESSION';

// --- Initialisation DB ---
function defaultDB(){
  return {
    meta: { name: 'Collège La Rondelle', created: new Date().toISOString() },
    users: [
      // id, role: admin | viesco | prof | eleve | parent
      { id: 1, role: 'admin', username: 'admin', pass: 'admin', firstname: 'Jeanne', lastname: 'JUILLIARD', name: 'JUILLIARD Jeanne' },
      { id: 2, role: 'viesco', username: 'viesco', pass: 'viesco', firstname: 'Sophie', lastname: 'HURAULT', name: 'HURAULT Sophie' },
      { id: 3, role: 'prof', username: 'prof1', pass: 'prof', firstname: 'Marc', lastname: 'DURAND', name: 'DURAND Marc' },
      { id: 4, role: 'eleve', username: 'eleve1', pass: '1234', firstname: 'Luc', lastname: 'MARTIN', name: 'MARTIN Luc' }
    ],
    profsAbsents: [
      { id: 1, nom: "Mme Dupont", date: "2025-10-17", heure: "08h00 - 09h00", classe: "5B", type: "absent", createdBy:1, createdAt: new Date().toISOString() },
      { id: 2, nom: "M. Laurent", date: "2025-10-17", heure: "10h00 - 11h00", classe: "4A", type: "doro", createdBy:1, createdAt: new Date().toISOString() },
      { id: 3, nom: "Mme Petit", date: "2025-10-17", heure: "11h00 - 12h00", classe: "6C", type: "remplace", createdBy:2, createdAt: new Date().toISOString() }
    ],
    lastIds: { user: 4, profAbs: 3 }
  };
}

function loadDB(){
  try{
    const raw = localStorage.getItem(DB_KEY);
    if(!raw) { const db = defaultDB(); saveDB(db); return db; }
    return JSON.parse(raw);
  }catch(e){
    const db = defaultDB(); saveDB(db); return db;
  }
}
function saveDB(db){
  localStorage.setItem(DB_KEY, JSON.stringify(db));
}

// --- Session (simple) ---
function setSession(user){
  sessionStorage.setItem(SESSION_KEY, JSON.stringify(user));
}
function getSession(){
  try{ return JSON.parse(sessionStorage.getItem(SESSION_KEY)); }catch(e){ return null; }
}
function clearSession(){ sessionStorage.removeItem(SESSION_KEY); }

// --- Auth ---
function authLogin(username, pass){
  const db = loadDB();
  const u = db.users.find(x=>x.username===username && x.pass===pass);
  if(!u) return null;
  const s = { id: u.id, role: u.role, username: u.username, name: u.name, firstname: u.firstname, lastname: u.lastname };
  setSession(s);
  return s;
}

// --- Users helper ---
function findUserById(id){ return loadDB().users.find(u=>u.id===id); }

// --- Absences / événements profs ---
// Allowed types: absent, doro, remplace, changement
function listProfsAbsents(){
  return loadDB().profsAbsents.slice().sort((a,b)=> (b.createdAt||'') - (a.createdAt||''));
}
function addProfAbsence({nom,date,heure,classe,type,byUserId}){
  const db = loadDB();
  // rule: only admin can add type 'doro'
  const user = findUserById(byUserId);
  if(!user) throw new Error('Utilisateur introuvable');
  if(type === 'doro' && user.role !== 'admin') throw new Error('Seul l’administrateur peut ajouter Doro Party.');
  db.lastIds.profAbs = (db.lastIds.profAbs || 0) + 1;
  const rec = { id: db.lastIds.profAbs, nom, date, heure, classe, type, createdBy: byUserId, createdAt: new Date().toISOString() };
  db.profsAbsents.push(rec);
  saveDB(db);
  return rec;
}
function removeProfAbsence(id, byUserId){
  const db = loadDB();
  const i = db.profsAbsents.findIndex(x=>x.id===id);
  if(i===-1) throw new Error('Absence introuvable');
  // allow admin or creator
  const s = findUserById(byUserId);
  if(!s) throw new Error('Utilisateur introuvable');
  if(s.role!=='admin' && db.profsAbsents[i].createdBy !== byUserId) throw new Error('Droits insuffisants');
  db.profsAbsents.splice(i,1);
  saveDB(db);
  return true;
}

// --- Expose API globally ---
window.APP = {
  loadDB, saveDB, getSession, setSession, clearSession, authLogin, findUserById,
  listProfsAbsents, addProfAbsence, removeProfAbsence
};()

()

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Choix des espaces — Collège La Rondelle</title>
  <style>
    body {
      margin: 0;
      font-family: "Segoe UI", Arial, sans-serif;
      background-color: #ffffff;
    }

    header {
      background-color: #17623b;
      color: white;
      text-align: center;
      padding: 20px 0;
      font-size: 18px;
      font-weight: bold;
      border-bottom-left-radius: 35px;
      border-bottom-right-radius: 35px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    }

    main {
      width: 90%;
      max-width: 500px;
      margin: 25px auto;
      background: #fff;
      border-radius: 20px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
      overflow: hidden;
    }

    ul { list-style: none; margin: 0; padding: 0; }

    li {
      display: flex;
      align-items: center;
      padding: 15px 20px;
      border-bottom: 1px solid #e5e5e5;
      cursor: pointer;
      transition: background 0.2s ease;
    }
    li:hover { background-color: #f6f6f6; }

    .icon {
      width: 40px;
      height: 40px;
      margin-right: 15px;
      border-radius: 50%;
      flex-shrink: 0;
    }

    li span {
      font-size: 16px;
      color: #2c2c2c;
      font-weight: 500;
    }

    .footer {
      text-align: center;
      padding: 15px;
      color: #777;
      font-size: 13px;
    }

    .footer button {
      margin-top: 10px;
      background-color: transparent;
      border: 1px solid #17623b;
      color: #17623b;
      padding: 8px 18px;
      border-radius: 25px;
      font-weight: bold;
      cursor: pointer;
      transition: 0.3s;
    }

    .footer button:hover {
      background-color: #17623b;
      color: white;
    }

    .classic {
      color: #333;
      font-weight: bold;
      margin-top: 12px;
      cursor: pointer;
      text-decoration: underline;
    }

    .copy {
      font-size: 12px;
      color: #999;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <header>COLLÈGE LA RONDELLE</header>

  <main>
    <ul>
      <li onclick="go('admin.html')">
        <svg class="icon" viewBox="0 0 100 100"><circle cx="50" cy="50" r="48" fill="#e0c04a"/><rect x="25" y="35" width="50" height="30" rx="3" ry="3" fill="white"/></svg>
        <span>Direction</span>
      </li>

      <li onclick="go('prof-notes.html')">
        <svg class="icon" viewBox="0 0 100 100"><circle cx="50" cy="50" r="48" fill="#8a56ac"/><path d="M25 65 L75 65 L75 35 L25 35 Z M35 45 L45 45 M55 45 L65 45 M35 55 L65 55" stroke="white" stroke-width="5" stroke-linecap="round"/></svg>
        <span>Professeurs</span>
      </li>

      <li onclick="go('viesco-absences.html')">
        <svg class="icon" viewBox="0 0 100 100"><circle cx="50" cy="50" r="48" fill="#4a90e2"/><rect x="30" y="25" width="40" height="50" rx="3" fill="white"/><line x1="35" y1="40" x2="65" y2="40" stroke="#4a90e2" stroke-width="4"/><line x1="35" y1="50" x2="65" y2="50" stroke="#4a90e2" stroke-width="4"/></svg>
        <span>Vie scolaire</span>
      </li>

      <li onclick="go('parents.html')">
        <svg class="icon" viewBox="0 0 100 100"><circle cx="50" cy="50" r="48" fill="#f278b2"/><circle cx="35" cy="45" r="10" fill="white"/><circle cx="65" cy="45" r="10" fill="white"/><path d="M25 65 Q50 85 75 65" stroke="white" stroke-width="6" fill="none" stroke-linecap="round"/></svg>
        <span>Parents</span>
      </li>

      <li onclick="go('accompagnants.html')">
        <svg class="icon" viewBox="0 0 100 100"><circle cx="50" cy="50" r="48" fill="#50c0b3"/><path d="M50 25 Q55 45 45 75" stroke="white" stroke-width="8" stroke-linecap="round"/><circle cx="50" cy="25" r="8" fill="white"/></svg>
        <span>Accompagnants</span>
      </li>

      <li onclick="go('app-eleve.html')">
        <svg class="icon" viewBox="0 0 100 100"><circle cx="50" cy="50" r="48" fill="#6fcf97"/><circle cx="50" cy="40" r="12" fill="white"/><path d="M30 70 Q50 85 70 70" stroke="white" stroke-width="6" stroke-linecap="round"/></svg>
        <span>Élèves</span>
      </li>
    </ul>

    <div class="footer">
      <p>Vous avez été redirigé vers la version mobile</p>
      <button onclick="alert('Demande d’accès envoyée !')">Faire une demande d’accès</button>
      <p class="classic">Accéder à la version classique</p>
      <p class="copy">PRONOTÉE © 2025</p>
    </div>
  </main>

  <script>
    function go(page) {
      window.location.href = page;
    }
  </script>
</body>
</html>

[viesco-absences.html](https://github.com/user-attachments/files/22957311/viesco-absences.html)
<!doctype html>[login.html](https://github.com/user-attachments/files/22957318/login.html)<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Connexion - PRONOTE RP</title>
<style>
  body{font-family:Inter,Arial;margin:0;background:#f3f7f6;height:100vh;display:flex;align-items:center;justify-content:center;}
  .login-box{background:white;padding:30px;border-radius:16px;box-shadow:0 4px 20px rgba(0,0,0,0.08);width:320px;text-align:center;}
  h1{color:#0f6b5a;font-size:20px;margin-bottom:16px;}
  input{width:100%;margin-bottom:10px;padding:10px;border-radius:8px;border:1px solid #ccc;font-size:14px;}
  button{width:100%;padding:10px;background:#18a090;border:none;border-radius:8px;color:white;font-size:15px;cursor:pointer;}
  button:hover{background:#0f6b5a;}
  .error{color:#b71c1c;margin-top:8px;font-size:13px;display:none;}
</style>
</head>
<body>
  <div class="login-box">
    <h1>Connexion PRONOTE RP</h1>
    <input id="username" placeholder="Nom d’utilisateur" autocomplete="off">
    <input id="password" type="password" placeholder="Mot de passe">
    <button id="loginBtn">Se connecter</button>
    <div id="error" class="error">Identifiants incorrects</div>
  </div>

<script>
async function sha256(str){
  const buf = await crypto.subtle.digest("SHA-256", new TextEncoder().encode(str));
  return Array.from(new Uint8Array(buf)).map(x=>x.toString(16).padStart(2,"0")).join("");
}

document.getElementById("loginBtn").onclick = async ()=>{
  const u = document.getElementById("username").value.trim().toLowerCase();
  const p = document.getElementById("password").value.trim();
  if(!u || !p) return alert("Veuillez remplir tous les champs.");

  // Charger les comptes depuis le localStorage
  const data = JSON.parse(localStorage.getItem("USERS")||"[]");

  // On compare la version hachée du mot de passe
  const passHash = await sha256(p);
  const user = data.find(x=>x.username.toLowerCase()===u && x.password===passHash);

  if(user){
    // sauvegarde de la session
    localStorage.setItem("currentUser", JSON.stringify(user));
    if(user.role==="admin") location.href="admin.html";
    else if(user.role==="prof") location.href="app-prof.html";
    else if(user.role==="eleve") location.href="app-eleve.html";
    else if(user.role==="viesco") location.href="vie_scolaire.html";
    else location.href="index.html";
  } else {
    document.getElementById("error").style.display="block";
  }
};
</script>
</body>
</html>

<html lang="fr">
<head>[notifications.html](https://github.com/user-attachments/files/22957316/notifications.html)<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Notifications — PRONOTE RP</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header class="header">
    <div class="left">
      <div class="avatar" style="background-image:url('icon-192.png')"></div>
      <div>
        <h1>Notifications / Informations</h1>
        <p>Dernières annonces du collège</p>
      </div>
    </div>
  </header>

  <div class="container">
    <section class="section">
      <h2>📢 Informations générales</h2>
      <div class="card">
        <ul style="margin:0;padding-left:18px">
          <li>Conseil de classe prévu le 24 octobre</li>
          <li>Sortie scolaire des 6A : autorisations à rendre avant vendredi</li>
          <li>Le self sera exceptionnellement fermé le lundi 21</li>
        </ul>
      </div>
    </section>

    <section class="section">
      <h2>📝 Sondages</h2>
      <div class="card">
        <ul style="margin:0;padding-left:18px">
          <li>Souhaitez-vous participer à la journée “Non au harcèlement” ?</li>
          <li>Choix du menu pour la journée des talents</li>
          <li>Proposition d’ateliers pour la semaine culturelle</li>
        </ul>
      </div>
    </section>
  </div>

  <script src="data.js"></script>
</body>
</html>

  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Vie scolaire — Collège La Rondelle</title>
[prof-notes.html](https://github.com/user-attachments/files/22957315/prof-notes.html)<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>Professeurs — Notes</title>
<style>
  :root{--violet:#6d28d9;--line:#e7e7e7;--text:#111}
  *{box-sizing:border-box;font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial}
  body{margin:0;background:#f6f5fb;color:var(--text)}
  .header{background:linear-gradient(135deg,var(--violet),#9d6cfb);color:#fff;text-align:center;padding:22px 10px;border-bottom-left-radius:22px;border-bottom-right-radius:22px}
  main{max-width:900px;margin:-14px auto 30px;padding:0 14px}
  .card{background:#fff;border:1px solid #ece9f7;border-radius:14px;box-shadow:0 8px 22px rgba(109,40,217,.18);padding:14px;margin:10px 0}
  .row{display:flex;gap:8px;flex-wrap:wrap}
  .input,.select,textarea{border:1px solid var(--line);border-radius:10px;padding:8px 10px}
  table{width:100%;border-collapse:collapse} th,td{padding:8px;border-bottom:1px solid var(--line);text-align:left}
  .btn{background:var(--violet);border:0;color:#fff;border-radius:10px;padding:8px 12px;cursor:pointer}
</style>
</head>
<body>
<div class="header"><h2>PROFESSEURS — Saisie de notes</h2></div>
<main>
  <div class="card">
    <div class="row">
      <select id="classe" class="select"></select>
      <select id="eleve" class="select"></select>
      <select id="subject" class="select"></select>
      <input id="period" class="input" placeholder="Période (ex: T1)">
      <input id="title" class="input" placeholder="Titre (Contrôle n°1)">
      <input id="date" class="input" type="date">
      <input id="value" class="input" type="number" step="0.5" placeholder="Note">
      <input id="max" class="input" type="number" value="20">
      <input id="coeff" class="input" type="number" value="1" step="0.5">
      <textarea id="comment" class="input" rows="2" style="flex:1;min-width:260px" placeholder="Commentaire (si autorisé)"></textarea>
      <button class="btn" onclick="add()">Ajouter la note</button>
    </div>
  </div>

  <div class="card">
    <h3>Dernières notes de l’élève</h3>
    <table id="tbl"><thead><tr>
      <th>Date</th><th>Matière</th><th>Titre</th><th>Note</th><th>Coeff</th><th>Commentaire</th>
    </tr></thead><tbody></tbody></table>
  </div>
</main>

<script src="data.js"></script>
<script>
const db=PRN.loadDB(); const me=PRN.getSession()||{id:2,role:'prof',perms:['grades.add','grades.comment']};
date.value = new Date().toISOString().slice(0,10);

// Remplir classes / élèves / matières
db.classes.forEach(c=>{ const o=document.createElement('option'); o.value=c.id; o.textContent=c.name; classe.appendChild(o); });
function fillEleves(){ eleve.innerHTML=''; PRN.loadDB().users.filter(u=>u.role==='eleve'&&u.classId==classe.value).forEach(e=>{ const o=document.createElement('option'); o.value=e.id; o.textContent=e.name||e.username; eleve.appendChild(o); }); render(); }
db.subjects.forEach(s=>{ const o=document.createElement('option'); o.value=s.id; o.textContent=s.name; subject.appendChild(o); });
classe.onchange=fillEleves; fillEleves();

function add(){
  try{
    PRN.addGrade({
      studentUserId:parseInt(eleve.value,10),
      subjectId:parseInt(subject.value,10),
      period:period.value||'T1',
      value:parseFloat(value.value), max:parseFloat(max.value||'20'),
      coeff:parseFloat(coeff.value||'1'),
      title:title.value.trim()||'Évaluation',
      date:date.value, byUserId:me.id,
      comment:comment.value.trim()
    });
    value.value=''; title.value=''; comment.value='';
    render();
  }catch(e){ alert(e.message||e); }
}

function render(){
  const list = PRN.listGradesForStudent(parseInt(eleve.value,10)).slice(-20).reverse();
  const tbody=document.querySelector('#tbl tbody'); tbody.innerHTML='';
  list.forEach(g=>{
    const tr=document.createElement('tr');
    tr.innerHTML = `<td>${g.date}</td><td>${g.subject}</td><td>${g.title||''}</td>
                    <td>${g.value} / ${g.max}</td><td>${g.coeff||1}</td><td>${g.comment||''}</td>`;
    tbody.appendChild(tr);
  });
}
eleve.onchange=render; render();
</script>
</body>
</html>

  <style>[prof.html](https://github.com/user-attachments/files/22957314/prof.html)<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Professeurs — Collège La Rondelle</title>
  <style>
    :root{
      --vert:#17623b;
      --fond:#f4f7f5;
      --card:#fff;
      --muted:#5c6b64;
      --bleu:#0078d7;
      --rouge:#d40000;
      --shadow:0 6px 18px rgba(0,0,0,0.08);
      --radius:14px;
    }
    *{box-sizing:border-box;}
    body{
      margin:0;
      font-family:"Inter","Segoe UI",Roboto,system-ui;
      background:var(--fond);
      color:#163a33;
    }

    header{
      background:linear-gradient(180deg,var(--vert),#218967);
      color:white;
      padding:14px 18px;
      display:flex;
      justify-content:space-between;
      align-items:center;
      border-bottom-left-radius:18px;
      border-bottom-right-radius:18px;
      box-shadow:var(--shadow);
      position:sticky;top:0;z-index:40;
    }
    header h1{margin:0;font-size:16px}
    header p{margin:0;font-size:12px;opacity:.9}
    .left{display:flex;align-items:center;gap:10px}
    .avatar{width:44px;height:44px;border-radius:50%;background:#fff;background-size:cover;background-position:center;}

    main{max-width:980px;margin:14px auto;padding:12px;}

    section{
      background:var(--card);
      border-radius:var(--radius);
      padding:14px;
      margin-bottom:14px;
      box-shadow:0 4px 12px rgba(0,0,0,0.04);
    }
    section h2{margin:0 0 10px;font-size:15px;color:var(--vert)}
    .muted{color:var(--muted);font-size:13px}

    .form-row{display:flex;flex-wrap:wrap;gap:8px;margin-top:6px}
    input,select,button,textarea{padding:9px;border-radius:8px;border:1px solid #e6eee8;font-size:14px}
    button.primary{background:var(--vert);color:white;border:0;cursor:pointer}
    button.ghost{background:#fff;border:1px solid #ccc;cursor:pointer}

    /* Emploi du temps amélioré */
    .edt-wrapper{overflow-x:auto;}
    table.edt{
      width:100%;
      border-collapse:collapse;
      background:white;
      border-radius:12px;
      overflow:hidden;
      box-shadow:0 4px 10px rgba(0,0,0,0.05);
    }
    table.edt th, table.edt td{
      padding:10px;
      text-align:center;
      border:1px solid #e5eee8;
      font-size:14px;
    }
    table.edt th{
      background:var(--vert);
      color:white;
      font-weight:600;
    }
    .cours{
      background:#e8f6ef;
      color:#17623b;
      border-radius:6px;
      padding:4px 6px;
      font-weight:600;
    }
    .vide{color:#bbb;font-size:13px}
  </style>
</head>
<body>

<header>
  <div class="left">
    <div class="avatar" style="background-image:url('icon-192.png')"></div>
    <div>
      <p>PROFESSEUR —</p>
      <h1 id="profName">Espace professeur</h1>
    </div>
  </div>
  <button id="notifBtn" style="background:white;border:none;border-radius:50%;width:36px;height:36px;cursor:pointer;">🔔</button>
</header>

<main>
  <!-- Section déclaration maladie -->
  <section>
    <h2>🩺 Déclarer une absence (maladie)</h2>
    <p class="muted">Préviens la vie scolaire si tu es malade.</p>
    <div class="form-row">
      <input id="f_date" type="date">
      <input id="f_duree" placeholder="Durée (ex: 08h00 - 17h00)">
      <input id="f_classe" placeholder="Classe concernée (ex: 3B)">
      <button class="primary" id="btnDeclare">Déclarer</button>
    </div>
  </section>

  <!-- Emploi du temps -->
  <section>
    <h2>📅 Mon emploi du temps</h2>
    <div class="edt-wrapper" id="emploiContainer">
      <table class="edt">
        <thead>
          <tr>
            <th>Heure</th>
            <th>Lundi</th>
            <th>Mardi</th>
            <th>Mercredi</th>
            <th>Jeudi</th>
            <th>Vendredi</th>
          </tr>
        </thead>
        <tbody id="edtBody"></tbody>
      </table>
    </div>
  </section>

  <!-- Bloc notes et devoirs -->
  <section>
    <h2>📝 Saisie des notes et devoirs</h2>
    <div style="background:#f8f9f8;padding:12px;border-radius:10px">
      <p class="muted">Entre une note ou un devoir pour ta classe.</p>
      <div class="form-row">
        <input id="n_classe" placeholder="Classe (ex: 5A)">
        <input id="n_matiere" placeholder="Matière (ex: Français)">
        <textarea id="n_desc" placeholder="Description ou devoir..." rows="2" style="flex:1"></textarea>
        <input id="n_note" type="number" placeholder="Note /20" style="width:100px">
      </div>
      <div style="margin-top:8px">
        <button class="primary" id="btnNote">Enregistrer</button>
      </div>
    </div>
    <div id="listeNotes" style="margin-top:12px">
      <p class="muted">Aucun devoir ou note enregistrée.</p>
    </div>
  </section>
</main>

<script>
  // ----- Session simulée -----
  const hasAPP = (typeof APP !== 'undefined' && APP);
  const session = hasAPP ? APP.getSession() : {id:10,role:'prof',name:'Mme Dupont'};

  document.getElementById('profName').textContent = session.name;
  document.getElementById('notifBtn').onclick = ()=>location.href='notifications.html';

  // ----- Déclaration de maladie -----
  document.getElementById('btnDeclare').onclick = ()=>{
    const date=document.getElementById('f_date').value;
    const duree=document.getElementById('f_duree').value.trim();
    const classe=document.getElementById('f_classe').value.trim();
    if(!date||!duree){alert("Remplis tous les champs.");return;}
    if(hasAPP){
      APP.addProfAbsence({nom:session.name,date,heure:duree,classe,type:'absent',byUserId:session.id});
      const db=APP.loadDB();APP.saveDB(db);
    } else {
      let abs=JSON.parse(localStorage.getItem("profAbsences")||"[]");
      abs.push({nom:session.name,date,heure:duree,classe});
      localStorage.setItem("profAbsences",JSON.stringify(abs));
    }
    alert("Absence envoyée !");
    document.getElementById('f_date').value='';
    document.getElementById('f_duree').value='';
    document.getElementById('f_classe').value='';
  };

  // ----- Emploi du temps complet -----
  const edtData = [
    {heure:"8h00 - 9h00", lundi:"6B - Mathématiques", mardi:"6A - Mathématiques", mercredi:"Libre", jeudi:"Réunion", vendredi:"6A - Mathématiques"},
    {heure:"9h00 - 10h06", lundi:"6C - Français", mardi:"5A - Mathématiques", mercredi:"Réunion", jeudi:"4B - Physique", vendredi:"5B - Mathématiques"},
    {heure:"10h06 - 11h06", lundi:"Libre", mardi:"6A - Mathématiques", mercredi:"Libre", jeudi:"4B - Physique", vendredi:"3C - Physique"},
    {heure:"11h06 - 12h00", lundi:"5B - Physique", mardi:"Libre", mercredi:"Libre", jeudi:"3A - Mathématiques", vendredi:"Libre"},
    {heure:"15h00 - 16h05", lundi:"3A - Mathématiques", mardi:"5C - Physique", mercredi:"Libre", jeudi:"3C - Mathématiques", vendredi:"Libre"},
  ];

  const edtBody = document.getElementById("edtBody");
  edtData.forEach(ligne=>{
    const tr=document.createElement("tr");
    const tdHeure=document.createElement("td");
    tdHeure.textContent=ligne.heure;
    tr.appendChild(tdHeure);

    ["lundi","mardi","mercredi","jeudi","vendredi"].forEach(j=>{
      const td=document.createElement("td");
      const val=ligne[j];
      if(val && val!=="Libre"){
        td.innerHTML=`<div class='cours'>${val}</div>`;
      } else {
        td.innerHTML="<span class='vide'>—</span>";
      }
      tr.appendChild(td);
    });
    edtBody.appendChild(tr);
  });

  // ----- Saisie de notes -----
  document.getElementById("btnNote").onclick=()=>{
    const c=document.getElementById("n_classe").value.trim();
    const m=document.getElementById("n_matiere").value.trim();
    const d=document.getElementById("n_desc").value.trim();
    const n=document.getElementById("n_note").value.trim();
    if(!c||!m||!d){alert("Remplis tous les champs");return;}
    let notes=JSON.parse(localStorage.getItem("notesProfs")||"[]");
    notes.push({prof:session.name,classe:c,matiere:m,desc:d,note:n});
    localStorage.setItem("notesProfs",JSON.stringify(notes));
    renderNotes();
    document.getElementById("n_classe").value="";
    document.getElementById("n_matiere").value="";
    document.getElementById("n_desc").value="";
    document.getElementById("n_note").value="";
  };

  function renderNotes(){
    const cont=document.getElementById("listeNotes");
    let notes=JSON.parse(localStorage.getItem("notesProfs")||"[]").filter(x=>x.prof===session.name);
    cont.innerHTML="";
    if(notes.length===0){cont.innerHTML='<p class="muted">Aucun devoir enregistré.</p>';return;}
    notes.forEach(n=>{
      const p=document.createElement("p");
      p.innerHTML=`<strong>${n.matiere}</strong> — ${n.classe} : ${n.desc} ${n.note?`<em>(${n.note}/20)</em>`:''}`;
      cont.appendChild(p);
    });
  }
  renderNotes();
</script>
</body>
</html>

    :root{
      --vert-pronote: #17623b;
      --vert-fonce:#0f4d3b;
      --fond:#f4f7f5;
      --card:#ffffff;
      --muted:#5c6b64;
      --bleu:#0078d7;
      --rouge:#d40000;
      --jaune:#ffe17b;
      --radius:12px;
      --shadow: 0 6px 18px rgba(0,0,0,0.08);
      --text:#163a33;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: "Inter","Segoe UI",Roboto,system-ui,-apple-system,Arial;
      background:var(--fond);
      color:var(--text);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      line-height:1.45;
    }

    /* header fixed top */
    .header {
      background: linear-gradient(180deg,var(--vert-pronote),#218967);
      color:white;
      padding:14px 18px;
      display:flex;
      align-items:center;
      justify-content:space-between;
      position:sticky;
      top:0;
      z-index:40;
      border-bottom-left-radius:18px;
      border-bottom-right-radius:18px;
      box-shadow:var(--shadow);
    }
    .header .left{display:flex;align-items:center;gap:12px}
    .avatar{
      width:44px;height:44px;border-radius:50%;background:#fff;background-size:cover;background-position:center;border:2px solid rgba(255,255,255,0.12)
    }
    .header h1{margin:0;font-size:16px}
    .header p{margin:0;font-size:12px;opacity:.95}

    /* page container */
    .container{max-width:980px;margin:14px auto;padding:10px;}

    .section{
      background:var(--card);
      border-radius:var(--radius);
      padding:12px 14px;
      margin-bottom:12px;
      box-shadow:0 4px 12px rgba(0,0,0,0.04);
    }
    .section h2{
      display:flex;justify-content:space-between;align-items:center;margin:0;font-size:15px;color:var(--vert-fonce);
    }
    .section h2 small{font-size:12px;color:var(--muted);font-weight:500}
    .card{background:#f8f9f8;padding:10px;border-radius:10px;margin-top:10px;font-size:14px;color:var(--muted)}

    .list p{
      margin:8px 0;padding:8px;border-radius:10px;display:flex;justify-content:space-between;align-items:center;
    }
    .left{display:flex;gap:12px;align-items:center}
    .icon{
      width:40px;height:40px;border-radius:9px;display:flex;align-items:center;justify-content:center;font-size:20px;color:white;flex-shrink:0;
    }
    .chev{opacity:.6;margin-left:8px}

    /* colors for types */
    .absent .item-text, .doro .item-text { color:var(--rouge); font-weight:600; }
    .remplace .item-text, .changement .item-text { color:var(--bleu); font-weight:600; }

    /* form */
    .form-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
    input, select, button {padding:9px;border-radius:8px;border:1px solid #e6efe8;font-size:14px}
    button.primary {background:var(--vert-pronote);color:#fff;border:0;cursor:pointer}
    button.ghost{background:transparent;border:1px solid #ccc}

    .pensebete{background:var(--jaune);color:#3b2f00}

    /* small screens */
    @media (max-width:640px){
      .header h1{font-size:15px}
      .container{padding:8px}
      .icon{width:36px;height:36px;font-size:18px;border-radius:8px}
    }

    /* dark mode */
    @media (prefers-color-scheme:dark){
      :root{--fond:#071211;--card:#071816;--text:#d4efe8;--muted:#9fb3aa;--shadow:0 6px 18px rgba(0,0,0,0.6)}
      .header{box-shadow:none}
    }

    /* small helpers */
    .muted{color:var(--muted);font-size:13px}
    .right-actions{display:flex;gap:8px;align-items:center}
    .small-btn{padding:6px 8px;border-radius:8px;background:#fff;border:1px solid rgba(0,0,0,0.06);cursor:pointer}
  </style>
</head>
<body>

  <header class="header">
    <div class="left">
      <div class="avatar" id="headerAvatar" style="background-image:url('icon-192.png')"></div>
      <div>
        <p id="headerRole">CPE —</p>
        <h1 id="headerName">Page d'accueil</h1>
      </div>
    </div>
    <div class="right-actions">
      <button class="small-btn" id="menuToggle">☰</button>
      <button class="small-btn" id="notifBtn">🔔 <span id="notifCount" style="background:gold;color:#111;padding:2px 6px;border-radius:999px;margin-left:6px;font-size:12px">16</span></button>
    </div>
  </header>

  <main class="container">

    <!-- Cours non assurés -->
    <section class="section">
      <h2>📚 Cours non assurés des professeurs <a class="muted" href="#" id="voirCours">Tout voir →</a></h2>
      <div class="card">
        <div class="list">
          <p><span class="left"><span class="icon" style="background:#f7c46b">📌</span><span>21 juil. — 08h00 — 5D - 205</span></span><span class="chev">›</span></p>
          <p><span class="left"><span class="icon" style="background:#f7c46b">📌</span><span>25 juil. — 10h00 — 6A - 205</span></span><span class="chev">›</span></p>
        </div>
      </div>
    </section>

    <!-- Absences du personnel -->
    <section class="section">
      <h2>🧑‍💼 Absences du personnel</h2>
      <div class="card">
        <div class="list">
          <p><span class="left"><span class="icon" style="background:#c86b8a">⏱️</span><span>21 juil. — 08h00 à 18h00 — HURAULT S.</span></span><span class="chev">›</span></p>
        </div>
      </div>
    </section>

    <!-- Appels non faits -->
    <section class="section">
      <h2>📋 Appels non faits</h2>
      <div class="card">
        <div class="list">
          <p><span class="left"><span class="icon" style="background:#8aa1ff">📞</span><span>Lun. 21 — 11h00 — ULIS</span></span><span class="chev">›</span></p>
          <p><span class="left"><span class="icon" style="background:#8aa1ff">📞</span><span>Lun. 14 — 11h00 — ULIS</span></span><span class="chev">›</span></p>
          <p><span class="left"><span class="icon" style="background:#8aa1ff">📞</span><span>Lun. 07 — 11h00 — ULIS</span></span><span class="chev">›</span></p>
        </div>
      </div>
    </section>

    <!-- Professeurs absents -->
    <section class="section" id="sectionProfs">
      <h2>🧑‍🏫 Professeurs absents
        <small id="countAbs" class="muted"></small>
      </h2>

      <!-- ajout (visible selon rôle) -->
      <div id="formWrap" style="margin-top:10px;"></div>

      <div class="card" id="listeProfsAbsents">
        <!-- Rempli par JS -->
        <p class="muted">Chargement…</p>
      </div>
    </section>

    <!-- Pense-bête -->
    <section class="section pensebete">
      <h2>💡 Pense-bête</h2>
      <div class="card">
        <ul style="margin:0;padding-left:18px">
          <li>Protocole sanitaire</li>
          <li>Les éco-délégués, c’est quoi ?</li>
        </ul>
      </div>
    </section>

    <!-- Incidents -->
    <section class="section">
      <h2>🚨 Incidents <a class="muted" href="#">Tout voir →</a></h2>
      <div class="card">
        <div class="list">
          <p><span class="left"><span class="icon" style="background:#ff9a7a">⚠️</span><span>25/07/25 à 15h04 — signalé par PROFESSEUR Maxime</span></span><span class="chev">›</span></p>
        </div>
      </div>
    </section>

  </main>

  <!-- Optional side menu (small) -->
  <div id="sideMenu" style="display:none;position:fixed;left:0;top:0;height:100%;width:280px;background:white;box-shadow:2px 0 18px rgba(0,0,0,0.2);z-index:60;padding:16px;">
    <button id="closeMenu" style="float:right">✕</button>
    <h3 style="margin-top:10px">Menu</h3>
    <p id="menuUser" class="muted"></p>
    <hr/>
    <p><a href="viesco-absences.html">Vie scolaire</a></p>
    <p><a href="admin.html">Administrateur</a></p>
    <p><a href="prof-notes.html">Professeurs</a></p>
    <p><a href="app-eleve.html">Élèves</a></p>
    <p><a href="notifications.html">Notifications</a></p>
    <p><a href="#" id="logoutLink">Déconnexion</a></p>
  </div>

  <script>
    /******************************************************
     * Vie scolaire page script
     * - Works with APP (data.js) if present, else uses local demo data
     * - Doro Party: visible by all, can only be created by admin (enforced client-side)
     ******************************************************/

    // safe helpers
    const hasAPP = (typeof APP !== 'undefined' && APP && typeof APP.loadDB === 'function');

    // demo fallback DB (kept in-memory)
    const demoDB = {
      profsAbsents: [
        { id: 1, nom: "Mme Dupont", date: "2025-10-17", heure: "08h00 - 09h00", classe: "5B", type: "absent", createdBy: 2, createdAt: new Date().toISOString() },
        { id: 2, nom: "M. Laurent", date: "2025-10-17", heure: "10h00 - 11h00", classe: "4A", type: "doro", createdBy: 1, createdAt: new Date().toISOString() },
        { id: 3, nom: "Mme Petit", date: "2025-10-17", heure: "11h00 - 12h00", classe: "6C", type: "remplace", createdBy: 2, createdAt: new Date().toISOString() }
      ]
    };

    // session detection
    function getSession(){
      if(hasAPP) return APP.getSession();
      // fallback demo session (viesco)
      return { id: 2, role: 'viesco', name: 'HURAULT Sophie', firstname:'Sophie', lastname:'HURAULT' };
    }

    const session = getSession();

    // render header name and role
    (function initHeader(){
      const nameEl = document.getElementById('headerName');
      const roleEl = document.getElementById('headerRole');
      const avatar = document.getElementById('headerAvatar');
      if(session){
        nameEl.textContent = session.name || (session.firstname && session.lastname ? `${session.lastname} ${session.firstname}` : 'Utilisateur');
        roleEl.textContent = (session.role || 'viesco').toUpperCase() + ' —';
      } else {
        nameEl.textContent = 'JUILLIARD Jeanne';
        roleEl.textContent = 'CPE —';
      }
      // avatar fallback if file exists, otherwise leave blank
      if(avatar && typeof avatar.style !== 'undefined') {
        // keep as-is (uses icon-192.png if present)
      }
    })();

    // get list of absent profs (from APP or demo)
    function fetchAbsences(){
      if(hasAPP){
        try{ return APP.listProfsAbsents(); }catch(e){ console.warn(e); return demoDB.profsAbsents; }
      } else {
        return demoDB.profsAbsents.slice().sort((a,b)=> (b.createdAt||'') - (a.createdAt||''));
      }
    }

    // add absence (calls APP.addProfAbsence if available)
    async function addAbsence({nom,date,heure,classe,type}){
      if(!session){ alert('Tu dois être connecté pour ajouter.'); return; }
      // client-side rule: only admin can add doro
      if(type === 'doro' && session.role !== 'admin'){
        alert('Seul l’administrateur peut ajouter "Doro Party".');
        return;
      }
      if(hasAPP){
        try{
          APP.addProfAbsence({nom,date,heure,classe,type,byUserId: session.id});
          // ensure DB saved for other tabs
          const db = APP.loadDB(); APP.saveDB(db);
          renderList();
          return;
        }catch(e){ alert(e.message || e); return; }
      } else {
        // demo push
        const id = (demoDB.profsAbsents.reduce((s,x)=>Math.max(s,x.id),0) || 0) + 1;
        demoDB.profsAbsents.push({id,nom,date,heure,classe,type,createdBy:session.id,createdAt:new Date().toISOString()});
        renderList();
      }
    }

    function removeAbsence(id){
      if(!session){ alert('Tu dois être connecté.'); return; }
      if(hasAPP){
        try{
          APP.removeProfAbsence(id, session.id);
          const db = APP.loadDB(); APP.saveDB(db);
          renderList();
          return;
        }catch(e){ alert(e.message||e); return; }
      } else {
        const i = demoDB.profsAbsents.findIndex(x=>x.id===id);
        if(i===-1) return alert('Introuvable');
        // allow admin or creator
        if(session.role !== 'admin' && demoDB.profsAbsents[i].createdBy !== session.id){
          return alert('Droits insuffisants pour supprimer cette absence.');
        }
        demoDB.profsAbsents.splice(i,1);
        renderList();
      }
    }

    // render add form (visible to admin and vie scolaire)
    function renderForm(){
      const wrap = document.getElementById('formWrap');
      wrap.innerHTML = '';
      // show form only to admin or viesco or prof if you want (here admin+viesco)
      if(!session) return;
      if(['admin','viesco','prof'].indexOf(session.role) === -1) return;
      const form = document.createElement('div');
      form.innerHTML = `
        <div style="display:flex;gap:8px;flex-wrap:wrap">
          <input id="f_nom" placeholder="Nom du professeur" style="flex:1">
          <input id="f_date" type="date" style="width:150px">
          <input id="f_heure" placeholder="Heure (ex:08h00-09h00)" style="width:170px">
          <input id="f_classe" placeholder="Classe (ex: 5A)" style="width:120px">
          <select id="f_type" style="width:170px">
            <option value="absent">Absent</option>
            <option value="remplace">Remplacé</option>
            <option value="changement">Changement de salle</option>
            ${session.role === 'admin' ? '<option value="doro">Doro Party 🎉</option>' : ''}
          </select>
        </div>
        <div style="margin-top:8px">
          <button id="btnAdd" class="primary">Ajouter</button>
          <button id="btnClear" class="ghost">Effacer</button>
        </div>
      `;
      wrap.appendChild(form);

      document.getElementById('btnAdd').onclick = ()=>{
        const nom = document.getElementById('f_nom').value.trim();
        const date = document.getElementById('f_date').value;
        const heure = document.getElementById('f_heure').value.trim();
        const classe = document.getElementById('f_classe').value.trim();
        const type = document.getElementById('f_type').value;
        if(!nom || !date || !heure || !classe){ alert('Remplis tous les champs'); return; }
        addAbsence({nom,date,heure,classe,type});
        // reset
        document.getElementById('f_nom').value=''; document.getElementById('f_date').value=''; document.getElementById('f_heure').value=''; document.getElementById('f_classe').value='';
      };
      document.getElementById('btnClear').onclick = ()=>{ document.getElementById('f_nom').value=''; document.getElementById('f_date').value=''; document.getElementById('f_heure').value=''; document.getElementById('f_classe').value=''; };
    }

    // render list of absences
    function renderList(){
      const list = fetchAbsences();
      const container = document.getElementById('listeProfsAbsents');
      container.innerHTML = '';
      if(!list || !list.length){
        container.innerHTML = '<p class="muted">Aucune absence enregistrée.</p>';
        document.getElementById('countAbs').textContent = '';
        return;
      }
      document.getElementById('countAbs').textContent = `(${list.length})`;

      list.forEach(item=>{
        const p = document.createElement('p');
        // set class for color styling
        p.className = item.type;
        // left area
        const left = document.createElement('span'); left.className = 'left';
        const icon = document.createElement('span'); icon.className = 'icon';
        // choose icon color & emoji
        if(item.type === 'doro'){ icon.style.background = '#f7c46b'; icon.textContent = '🎉'; }
        else if(item.type === 'absent'){ icon.style.background = '#d35454'; icon.textContent = '🚫'; }
        else if(item.type === 'remplace'){ icon.style.background = '#4a90e2'; icon.textContent = '🔁'; }
        else if(item.type === 'changement'){ icon.style.background = '#4a90e2'; icon.textContent = '📍'; }
        else { icon.style.background = '#888'; icon.textContent = 'ℹ️'; }

        const text = document.createElement('span'); text.className = 'item-text'; text.textContent = `${item.date} — ${item.heure} — ${item.nom} (${item.classe})`;

        left.appendChild(icon); left.appendChild(text);

        // right area: optional label + actions
        const right = document.createElement('span');
        right.style.display = 'flex'; right.style.alignItems = 'center'; right.style.gap = '8px';

        // label for special types
        if(item.type === 'doro'){
          const lab = document.createElement('span'); lab.textContent = 'Doro Party'; lab.style.marginLeft='6px'; lab.style.fontWeight='600'; lab.style.color='var(--muted)'; right.appendChild(lab);
        } else if(item.type === 'remplace'){
          const lab = document.createElement('span'); lab.textContent = 'Remplacé'; lab.style.color='var(--muted)'; right.appendChild(lab);
        } else if(item.type === 'changement'){
          const lab = document.createElement('span'); lab.textContent = 'Changement'; lab.style.color='var(--muted)'; right.appendChild(lab);
        }

        // delete button for admin or creator
        const canDelete = (session && (session.role === 'admin' || item.createdBy === session.id));
        if(canDelete){
          const del = document.createElement('button'); del.textContent='Supprimer'; del.style.padding='6px 8px'; del.style.borderRadius='8px'; del.style.border='1px solid #e6e6e6'; del.style.cursor='pointer';
          del.onclick = ()=>{ if(confirm('Supprimer cette absence ?')) removeAbsence(item.id); };
          right.appendChild(del);
        }

        const chev = document.createElement('span'); chev.className='chev'; chev.textContent='›';
        right.appendChild(chev);

        p.appendChild(left); p.appendChild(right);
        container.appendChild(p);
      });
    }

    // initial render
    (function(){
      renderForm();
      renderList();

      // if APP present, listen to storage updates to refresh list when admin adds in another tab
      window.addEventListener('storage', function(e){
        if(e.key && e.key.indexOf && e.key.indexOf('PRONOTE_RP_DB')===0){
          renderList();
        }
      });
    })();

    // small UI: menu & notifications
    document.getElementById('menuToggle').onclick = ()=>{
      document.getElementById('sideMenu').style.display = 'block';
      document.getElementById('menuUser').textContent = session ? `${session.name} — ${session.role}` : 'Visiteur';
    };
    document.getElementById('closeMenu').onclick = ()=>{ document.getElementById('sideMenu').style.display = 'none'; };
    document.getElementById('notifBtn').onclick = ()=>{ location.href='notifications.html'; };
    document.getElementById('logoutLink').onclick = (e)=>{ e.preventDefault(); if(hasAPP){ APP.clearSession(); } alert('Déconnecté'); location.href='index.html'; };

  </script>
</body>
</html>

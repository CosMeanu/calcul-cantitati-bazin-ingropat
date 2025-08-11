<!DOCTYPE html>
<html lang="ro">
<head>
  <meta charset="UTF-8">
  <title>Workbook Bazin ingropat</title>
  <style>
    body          { font-family: Arial; background:#f4f4f4; padding:20px; }
    .tabs         { display:flex; gap:5px; margin-bottom:10px; }
    .tab-btn      { padding:10px 20px; border:1px solid #888; background:#ddd; cursor:pointer; }
    .tab-btn.active { background:#fff; font-weight:bold; }
    .sheet        { display:none; background:#fff; padding:15px; border:1px solid #ccc; }
    .sheet.active { display:block; }
    .form-row     { display:flex; gap:10px; margin-bottom:10px; }
    .form-row label { width:120px; }
    .form-row input { width:60px; }
    .output       { margin-top:10px; background:#e1f5fe; padding:10px; white-space:pre-line; }
  </style>
</head>
<body>

  <h1>Workbook Cantități bazin ingropat</h1>

  <div class="tabs" id="tabs"></div>
  <button onclick="adaugaBazin()">➕ Adaugă Bazin</button>

  <div id="sheets_container"></div>

  <!-- === ȘABLON FIȘĂ BAZIN === -->
<template id="template_bazin">
  <div class="sheet sheet-bazin">
    <h2 class="titlu">Bazin</h2>

    <div style="display: flex; gap: 5px; align-items: flex-start; flex-wrap: wrap;">
      
      <!-- FORMULAR INPUTURI -->
      <div style="flex: 1; min-width: 100px; max-width: 650px;">
        <div class="form-row">
          <label>n<sub>b</sub> [–]</label>
          <input type="number" class="numar_bazine" value="1" min="1" />
          <span>n<sub>b</sub> – număr bazine asemenea</span>
        </div>

        <div class="form-row">
          <label>L<sub>ib</sub> [m]</label><input type="number" id="Lib" step="0.1" class="lungime_int_bazin" />
          <span>L<sub>ib</sub> – Lungime (interioara) bazin</span>
        </div>
        <div class="form-row">
          <label>B<sub>ib</sub> [m]</label><input type="number" id="Bib" step="0.1" class="latime_int_bazin" />
          <span>B<sub>ib</sub> – Lățime (interioara) bazin</span>
        </div>
        <div class="form-row">
          <label>+/-0.00 [-m]</label><input type="number" id="Cota0" step="0.1" class="cota_0" />
          <span>+/-0.00 – Distanta fata de cota superioara bazin</span>
        </div>
        <div class="form-row">
          <label>H<sub>pb</sub> [m]</label><input type="number" id="Hpb" step="0.1" class="inaltime_perete_bazin" />
          <span>H<sub>pb</sub> – Inaltime (libera) perete bazin</span>
        </div>
        <div class="form-row">
          <label>t<sub>pb</sub> [m]</label><input type="number" id="tpb" step="0.1" class="grosime_perete_bazin" />
          <span>t<sub>pb</sub> – Grosime perete bazin </span>
        </div>
        <div class="form-row">
          <label>t<sub>rb</sub> [m]</label><input type="number" id="trb" step="0.1" class="grosime_radier_bazin" />
          <span>t<sub>rb</sub> – Grosime radier bazin </span>
        </div>
        <div class="form-row">
          <label>ev<sub>r</sub> [m]</label><input type="number" id="evr" step="0.1" class="evazare_radier" />
          <span>ev<sub>r</sub> – Evazare radier </span>
        </div>
        <div class="form-row">
          <label>m/V [kg/m<sup>3</sup>]</label><input type="number" id="m/V" step="0.1" class="consum_specific_armatura" />
          <span>m/V – Consum specific armatura (orientativ, 80-110 kg/mc) </span>
        </div>
        <div class="form-row">
          <label>h<sub>bs</sub> [m]</label><input type="number" id="hbs" step="0.1" class="grosime_bs" />
          <span>h<sub>bs</sub> – Grosime beton simplu (egalizare)</span>
        </div>
        <div class="form-row">
          <label>ev<sub>exc</sub> [m]</label><input type="number" id="evexc" step="0.1" class="evazare_excavatie" />
          <span>ev<sub>exc</sub> – Evazare săpătură</span>
        </div>
        <div class="form-row">
          <label>p [1/x]</label><input type="number" id="panta" step="0.1"  class="panta_excavatie" value="1" />
          <span>p – Panta săpătură</span>
        </div>

<div class="form-row">
  <label>Ø<sub>Lr</sub> [mm]</label>
  <input type="number" class="diam_Lr" value="12" />   
  <span>Diametru bară perpendicular pe L<sub>r</sub> inferior </span>
</div>

<div class="form-row">
<label>Pas Ø<sub>Lr</sub> [cm]</label>
  <input type="number" class="pas_diam_Lr" value="20" />
  <span>Distanța între bare perpendicular pe L<sub>r</sub> inferior </span>
</div>

<div class="form-row">
  <label>Ø<sub>Br</sub> [mm]</label>
  <input type="number" class="diam_Br" value="12" />
  <span>Diametru bară perpendicular pe B<sub>r</sub> inferior </span>
</div>

<div class="form-row">
  <label>Pas Ø<sub>Br</sub> [cm]</label>
  <input type="number" class="pas_diam_Br" value="20" />
  <span>Distanța între bare perpendicular pe B<sub>r</sub> inferior </span>
</div>

<div class="form-row">
  <label>Ø<sub>Lrsup</sub> [mm]</label>
  <input type="number" class="diam_Lr_sup" value="12" />   
  <span>Diametru bară perpendicular pe L<sub>r</sub> superior </span>
</div>

<div class="form-row">
<label>Pas Ø<sub>Lrsup</sub> [cm]</label>
  <input type="number" class="pas_diam_Lr_sup" value="20" />
  <span>Distanța între bare perpendicular pe L<sub>r</sub> superior </span>
</div>

<div class="form-row">
  <label>Ø<sub>Brsup</sub> [mm]</label>
  <input type="number" class="diam_Br_sup" value="12" />
  <span>Diametru bară perpendicular pe B<sub>r</sub> superior </span>
</div>

<div class="form-row">
  <label>Pas Ø<sub>Brsup</sub> [cm]</label>
  <input type="number" class="pas_diam_Br_sup" value="20" />
  <span>Distanța între bare perpendicular pe B<sub>r</sub> superior </span>
</div>

<div class="form-row">
  <label>Ø<sub>vert</sub> [mm]</label>
  <input type="number" class="diam_bara_vert" value="12" />
  <span>Diametru bară verticala pereti </span>
</div>

<div class="form-row">
  <label>Pas Ø<sub>vert</sub> [cm]</label>
  <input type="number" class="pas_diam_bara_vert" value="20" />
  <span>Distanța între bare verticale pereti</span>
</div>

<div class="form-row">
  <label>Ø<sub>oriz</sub> [mm]</label>
  <input type="number" class="diam_bara_oriz" value="12" />
  <span>Diametru bară orizontala pereti </span>
</div>

<div class="form-row">
  <label>Pas Ø<sub>oriz</sub> [cm]</label>
  <input type="number" class="pas_diam_bara_oriz" value="20" />
  <span>Distanța între bare orizontale pereti </span>
</div>

<div class="form-row">
  <label>Ø<sub>etr</sub> [mm]</label>
  <input type="number" class="diam_etr" value="12" />
  <span>Diametru etrier colturi pereti</span>
</div>

<div class="form-row">
  <label>Pas Ø<sub>etr</sub> [cm]</label>
  <input type="number" class="pas_diam_etr" value="20" />
  <span>Distanța între etrieri colturi pereti</span>
</div>

        <button onclick="calculeazaIndividual(this)">Calculează</button>
        <button onclick="stergeBazin(this)">🗑️ Șterge Bazin</button>
        <div class="output cantitati_bazin"></div>
      </div>

   <div style="width: 100%; max-width: 1000px; height: auto;">  
    <svg id="svgBazin" width="100%" height="100%" preserveAspectRatio="xMidYMid meet">

<defs>
<pattern id="ansi31" patternUnits="userSpaceOnUse" width="8" height="8">
<path d="M-1,1 l2,-2
            M0,8 l8,-8
            M3,5 l2,-2"
         stroke="#000" stroke-width="0.5"/>
</pattern>
</defs>

<polygon id="radier" points="" fill="#ffe081" stroke="#000" />
<polygon id="bazinext" points="" fill="#ffe081" stroke="#000" />
<polygon id="bazinint" points="" fill="#ffe081" stroke="#000" />
<polygon id="excavatie" points="" fill="#8d6e63" stroke="#000" />
<polygon id="betonsimplu" points="" fill="#ffe081" stroke="#000" />
<polygon id="bazin" points="" fill="#ffe081" stroke="#000" />
<polygon id="bazinhasurat" points="" fill="url(#ansi31)" stroke="#000" />
<polygon id="bazingol" points="" fill="#ffffff" stroke="#000" />

</svg>

<script>
function initDesenareBazin(sheet) {
  const svg = sheet.querySelector('svg');
  const getVal = sel => parseFloat(sheet.querySelector(sel).value);

  const deseneaza = () => {
    const Lib = getVal('.lungime_int_bazin');
    const Bib = getVal('.latime_int_bazin');
    const Cota0 = getVal('.cota_0');
    const Hpb = getVal('.inaltime_perete_bazin');
    const tpb = getVal('.grosime_perete_bazin');
    const trb = getVal('.grosime_radier_bazin');
    const evr = getVal('.evazare_radier');
    const hbs = getVal('.grosime_bs');
    const evexc = getVal('.evazare_excavatie');
    const panta = getVal('.panta_excavatie');

    const Df = Hpb - Cota0 + trb + hbs;
    const Leb=Lib+2*tpb;
    const Beb=Bib+2*tpb;
    const Lr=Leb+2*evr;
    const Br=Beb+2*evr;

    if ([Lib, Bib, Df, Hpb, tpb, trb, evr, hbs, evexc, panta].some(isNaN)) return;

    const scale = 10;
    const lungimebazin = Leb * scale;
    const latimebazin = Beb * scale;
    const lungimeintbazin = Lib * scale;
    const latimeintbazin = Bib * scale;
    const cotazero=Cota0 * scale;
    const adancimebazin = Hpb * scale;
    const adancimefundare = Df * scale;
    const grosimeperete = tpb * scale;
    const grosimeradier = trb * scale;
    const evazareradier = evr * scale;
    const lungimeradier=Lr * scale;
    const latimeradier=Br * scale;
    const betonsimplu = hbs * scale;
    const evazareexcavatie = evexc * scale;
    const pantaexcavatie = panta;
    const evazaresuperioara = pantaexcavatie * adancimefundare;

    const x0 = 10, y0 = 10;
    const x1 = 10, y1 = 10+cotazero;

    const polygon = (pts) => pts.map(p => p.join(',')).join(' ');

    const pointsRadier = polygon([
      [x0+evazaresuperioara+evazareexcavatie, y0],
      [x0+evazaresuperioara+evazareexcavatie+lungimeradier, y0],
      [x0+evazaresuperioara+evazareexcavatie+lungimeradier, y0+latimeradier],
      [x0+evazaresuperioara+evazareexcavatie, y0+latimeradier]
    ]);

    const pointsBazinext = polygon([
      [x0+evazaresuperioara+evazareexcavatie+evazareradier, y0+evazareradier],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+lungimebazin, y0+evazareradier],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+lungimebazin, y0+evazareradier+latimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier, y0+evazareradier+latimebazin]
    ]);

    const pointsBazinint = polygon([
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete, y0+evazareradier+grosimeperete],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+evazareradier+grosimeperete],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+evazareradier+grosimeperete+latimeintbazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete,y0+evazareradier+grosimeperete+latimeintbazin]
    ]);

    const pointsBazin = polygon([
      [x0+evazaresuperioara+evazareexcavatie+evazareradier, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete+evazareradier, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete+evazareradier, y0+latimeradier+y1-cotazero+adancimebazin+grosimeradier],
      [x0+evazaresuperioara+evazareexcavatie, y0+latimeradier+y1-cotazero+adancimebazin+grosimeradier],
      [x0+evazaresuperioara+evazareexcavatie, y0+latimeradier+y1-cotazero+adancimebazin],
     [x0+evazaresuperioara+evazareexcavatie+evazareradier, y0+latimeradier+y1-cotazero+adancimebazin]
    ]);

const pointsBazinhasurat = polygon([
      [x0+evazaresuperioara+evazareexcavatie+evazareradier, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete+evazareradier, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin+grosimeperete+evazareradier, y0+latimeradier+y1-cotazero+adancimebazin+grosimeradier],
      [x0+evazaresuperioara+evazareexcavatie, y0+latimeradier+y1-cotazero+adancimebazin+grosimeradier],
      [x0+evazaresuperioara+evazareexcavatie, y0+latimeradier+y1-cotazero+adancimebazin],
     [x0+evazaresuperioara+evazareexcavatie+evazareradier, y0+latimeradier+y1-cotazero+adancimebazin]
    ]);

    const pointsBazingol = polygon([
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+latimeradier+y1-cotazero],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete+lungimeintbazin, y0+latimeradier+y1-cotazero+adancimebazin],
      [x0+evazaresuperioara+evazareexcavatie+evazareradier+grosimeperete, y0+latimeradier+y1-cotazero+adancimebazin],
    ]);

    const pointsBetonsimplu = polygon([
      [x0+evazaresuperioara+evazareexcavatie-1, y0+latimeradier+y1+adancimefundare-betonsimplu],
      [x0+evazaresuperioara+evazareexcavatie+lungimeradier+1, y0+latimeradier+y1+adancimefundare-betonsimplu],
      [x0+evazaresuperioara+evazareexcavatie+lungimeradier+1, y0+latimeradier+y1+adancimefundare],
      [x0+evazaresuperioara+evazareexcavatie-1,y0+latimeradier+y1+adancimefundare]
    ]);

    const pointsExcavatie = polygon([
      [x0, y0+latimeradier+y1],
      [x0+evazaresuperioara+evazareexcavatie+lungimeradier+evazareexcavatie+evazaresuperioara, y0+latimeradier+y1],
      [x0+evazaresuperioara+evazareexcavatie+lungimeradier+evazareexcavatie, y0+latimeradier+y1+adancimefundare],
      [x0+evazaresuperioara, y0+latimeradier+y1+adancimefundare]
    ]);

    svg.querySelector('#radier').setAttribute('points', pointsRadier);
    svg.querySelector('#bazinext').setAttribute('points', pointsBazinext);
    svg.querySelector('#bazinint').setAttribute('points', pointsBazinint);
    svg.querySelector('#bazin').setAttribute('points', pointsBazin);
    svg.querySelector('#bazinhasurat').setAttribute('points', pointsBazinhasurat);
    svg.querySelector('#bazingol').setAttribute('points', pointsBazingol);
    svg.querySelector('#betonsimplu').setAttribute('points', pointsBetonsimplu);
    svg.querySelector('#excavatie').setAttribute('points', pointsExcavatie);
    
    
  
    const totalWidth = lungimeradier + 2 * Math.max(evazaresuperioara, evazareexcavatie) + 100;
    const totalHeight = latimeradier + adancimefundare + betonsimplu + 100;
    svg.setAttribute('viewBox', `0 0 ${totalWidth} ${totalHeight}`);
  };

  sheet.querySelectorAll('input').forEach(inp => inp.addEventListener('input', deseneaza));
}
function stergeBazin(button) {
  const sheet = button.closest('.sheet');
  const id = sheet.id;

  const confirmare = confirm(`⚠️ Ești sigur că vrei să ștergi Bazinul ${id}?`);
  if (!confirmare) return;

  // Elimină sheet-ul și tab-ul asociat
  sheet.remove();
  [...tabs.children].forEach(btn => {
    if (btn.textContent === id) btn.remove();
  });

  // Activează ultimul tab rămas (sau Centralizator dacă nu mai e nimic)
  const lastTab = [...tabs.children].reverse().find(b => b.textContent !== 'Centralizator');
  if (lastTab) {
    schimbaSheet(lastTab.textContent);
  } else {
    schimbaSheet('centralizator_sheet');
  }

  salveazaDate(); // actualizează localStorage
actualizeazaContorBazine();
}</script>

</template>

  <!-- === CENTRALIZATOR === -->
  <div class="sheet" id="centralizator_sheet">
    <h2>Centralizator Cantități</h2>
    <button onclick="calculeazaTotal()">🔢 Calculează Total</button>
    <button onclick="exportJSON()">💾 Export JSON</button>
    <button onclick="importJSON()">📂 Import JSON</button>
    <button onclick="printeazaTot()">🖨️ Printează Tot</button>
    <input type="file" id="import_file" accept=".json" style="display:none">
    <div id="central_output" class="output"></div>
      </div>

  <script>
    const STORAGE_KEY = 'BazineData';

    let counter = 0;
    const tabs      = document.getElementById('tabs');
    const container = document.getElementById('sheets_container');

    /* ---------- UTILITARE ---------- */
    function addInputListeners(sheet) {
      sheet.querySelectorAll('input').forEach(inp =>
        inp.addEventListener('input', salveazaDate)
      );
    }

    function gatherData() {
      const data = [];
      document.querySelectorAll('.sheet:not(#centralizator_sheet)').forEach(sheet => {
        data.push({
          nb   : +sheet.querySelector('.numar_bazine'  ).value || 0,
          Lib   : +sheet.querySelector('.lungime_int_bazin').value || 0,
          Bib   : +sheet.querySelector('.latime_int_bazin' ).value || 0,
          Cota0   : +sheet.querySelector('.cota_0').value || 0,
          Hpb   : +sheet.querySelector('.inaltime_perete_bazin'   ).value || 0,
          tpb   : +sheet.querySelector('.grosime_perete_bazin').value || 0,
          trb   : +sheet.querySelector('.grosime_radier_bazin' ).value || 0,
          evr   : +sheet.querySelector('.evazare_radier' ).value || 0,
          hbs  : +sheet.querySelector('.grosime_bs'      ).value || 0,
          evexc   : +sheet.querySelector('.evazare_excavatie').value || 0,
          pexc : +sheet.querySelector('.panta_excavatie' ).value || 0
        });
      });
      return data;
    }

    function salveazaDate() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(gatherData()));
    }

function actualizeazaContorBazine() {
  const bazine = [...document.querySelectorAll('.sheet')].filter(s => s.id.startsWith('B'));
  if (bazine.length === 0) {
    counter = 0;
    return;
  }

  const numere = bazine.map(s => parseInt(s.id.replace('B', ''))).filter(n => !isNaN(n));
  counter = Math.max(...numere);
}
    function clearBazine() {
      // șterge tab-urile și fișele (exceptând centralizatorul)
      [...tabs.children].forEach(b => { if (b.textContent !== 'Centralizator') b.remove(); });
      document.querySelectorAll('.sheet:not(#centralizator_sheet)').forEach(s => s.remove());
      counter = 0;
    }

    /* ---------- ADĂUGARE FIȘĂ ---------- */
       function adaugaBazin() {
      counter++;
      const id = `B${counter}`;

      // tab
      const btn = document.createElement('button');
      btn.textContent = id;
      btn.className   = 'tab-btn';
      btn.onclick     = () => schimbaSheet(id);
      tabs.appendChild(btn);

      // sheet
      const clone = document.getElementById('template_bazin').content.cloneNode(true);
      const sheet = clone.querySelector('.sheet');
      sheet.id = id;
      sheet.querySelector('.titlu').textContent = `Bazin ${id}`;
// *** MODIFICARE PENTRU HASURĂ UNICĂ ***
 // cautare si schimbare pattern id
 const pattern = sheet.querySelector('pattern');
 const patternId = `ansi31_${id}`; // ex: ansi31_B1, ansi31_B2
pattern.id = patternId;
 // actualizare fill-ul poligonului hasurat
 const polyHasurat = sheet.querySelector('#bazinhasurat');
 polyHasurat.setAttribute('fill', `url(#${patternId})`);
 // *** SFÂRȘIT MODIFICARE ***
      container.appendChild(sheet);
      initDesenareBazin(sheet);

      addInputListeners(sheet);
      schimbaSheet(id);
}

    /* ---------- SCHIMBĂ FIȘA VIZIBILĂ ---------- */
    function schimbaSheet(id) {
      document.querySelectorAll('.sheet').forEach(s => s.classList.remove('active'));
      document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
      document.getElementById(id)?.classList.add('active');
      [...tabs.children].find(b => b.textContent === id)?.classList.add('active');
    }

    /* ---------- CALCULE PE FIȘĂ ---------- */
    function calculeazaIndividual(button) {
      const bazin = button.closest('.sheet');
      const id    = bazin.id;

      const nb   = parseFloat(bazin.querySelector('.numar_bazine').value);
      const Lib   = parseFloat(bazin.querySelector('.lungime_int_bazin').value);
      const Bib   = parseFloat(bazin.querySelector('.latime_int_bazin').value);
      const Cota0   = parseFloat(bazin.querySelector('.cota_0').value);
      const Hpb   = parseFloat(bazin.querySelector('.inaltime_perete_bazin').value);
      const tpb   = parseFloat(bazin.querySelector('.grosime_perete_bazin').value);
      const trb   = parseFloat(bazin.querySelector('.grosime_radier_bazin').value);
      const evr   = parseFloat(bazin.querySelector('.evazare_radier').value);
      const hbs  = parseFloat(bazin.querySelector('.grosime_bs').value);
      const mV   = parseFloat(bazin.querySelector('.consum_specific_armatura').value);
      const evexc   = parseFloat(bazin.querySelector('.evazare_excavatie').value);
      const pexc = parseFloat(bazin.querySelector('.panta_excavatie').value);


// Inițializări greutăți
let greutateTotala_diam_Lbv = 0,
greutateTotala_diam_Lbh = 0,
greutateTotala_diam_Bbv = 0,
greutateTotala_diam_Bbh = 0,
greutateTotala_diam_Lr_sup = 0,
greutateTotala_diam_Lr_inf = 0,
greutateTotala_diam_Br_sup = 0,
greutateTotala_diam_Br_inf = 0,
greutateTotala_etr = 0;

 if ([nb, Lib, Bib, Hpb,tpb,trb,evr, hbs,mV, evexc, pexc].some(x => isNaN(x) || x <= 0)) {
        bazin.querySelector('.cantitati_bazin').innerText =
          "⚠️ Completează toate câmpurile cu valori pozitive!";
        return;
      }

const Leb=tpb+Lib+tpb;
const Beb=tpb+Bib+tpb;

      /* Dimensiuni săpătură */
      const hexc = Hpb- Cota0 +trb+hbs;
      const Lr=evr+tpb+Lib+tpb+evr;
      const Br=evr+tpb+Bib+tpb+evr;
      const Liex = Lr + 2 * evexc;
      const Biex = Br + 2 * evexc;
      const Lsex = Liex + 2 * hexc * pexc;
      const Bsex = Biex + 2 * hexc * pexc;

      /* Volum pe bazin */
      const V_sap_unit   = (1/6) * hexc *(Lsex * Bsex + (Lsex + Liex) * (Bsex + Biex) + Liex * Biex);
      const V_bs_unit    = (0.1 + Lr + 0.1) * (0.1 + Br + 0.1) * hbs;
      const V_radier_unit = Lr * Br * trb;
      const V_pereti_unit = (Leb*Beb-Lib*Bib)*Hpb;
      const V_bazin_unit = V_radier_unit+V_pereti_unit;

     /* Armatura pe bazin */
     const m_V_bazin_unit=mV*(V_radier_unit +V_pereti_unit);

      /* Total pentru n_b */
      const V_sap_tot   = V_sap_unit   * nb;
      const V_bs_tot    = V_bs_unit    * nb;
      const V_radier_tot = V_radier_unit * nb;
      const V_pereti_tot = V_pereti_unit * nb;
      const V_bazin_tot = V_radier_tot+V_pereti_tot;

     /* Armatura pe n_b */
     const m_V_bazin_tot=m_V_bazin_unit* nb;
 

const diam_Lr = parseFloat(bazin.querySelector('.diam_Lr').value);
const pas_diam_Lr = parseFloat(bazin.querySelector('.pas_diam_Lr').value);
const diam_Br = parseFloat(bazin.querySelector('.diam_Br').value);
const pas_diam_Br = parseFloat(bazin.querySelector('.pas_diam_Br').value);
const diam_Lr_sup = parseFloat(bazin.querySelector('.diam_Lr_sup').value);
const pas_diam_Lr_sup = parseFloat(bazin.querySelector('.pas_diam_Lr_sup').value);
const diam_Br_sup = parseFloat(bazin.querySelector('.diam_Br_sup').value);
const pas_diam_Br_sup = parseFloat(bazin.querySelector('.pas_diam_Br_sup').value);
const diam_bara_vert = parseFloat(bazin.querySelector('.diam_bara_vert').value);
const pas_diam_bara_vert = parseFloat(bazin.querySelector('.pas_diam_bara_vert').value);
const diam_bara_oriz = parseFloat(bazin.querySelector('.diam_bara_oriz').value);
const pas_diam_bara_oriz = parseFloat(bazin.querySelector('.pas_diam_bara_oriz').value);
const diam_etr = parseFloat(bazin.querySelector('.diam_etr').value);
const pas_diam_etr = parseFloat(bazin.querySelector('.pas_diam_etr').value);

if (!isNaN(Lib) && !isNaN(Bib) && !isNaN(trb) && !isNaN(Hpb) && !isNaN(tpb) && !isNaN(evr) && !isNaN(diam_Lr) && !isNaN(pas_diam_Lr) && !isNaN(diam_Br) && !isNaN(pas_diam_Br)&& !isNaN(diam_Lr_sup) && !isNaN(pas_diam_Lr_sup)&& !isNaN(diam_Br_sup) && !isNaN(pas_diam_Br_sup)&& !isNaN(diam_bara_vert) && !isNaN(pas_diam_bara_vert) && !isNaN(diam_bara_oriz) && !isNaN(pas_diam_bara_oriz)&& !isNaN(diam_etr)&& !isNaN(pas_diam_etr)&& pas_diam_Lr > 0&& pas_diam_Br > 0&& pas_diam_Lr_sup > 0&& pas_diam_Br_sup > 0&& pas_diam_bara_vert > 0 && pas_diam_bara_oriz > 0 && pas_diam_etr > 0) {

  const bruteNr_Lr_inf= ((Lr* 100 - 10) / pas_diam_Lr)+1;
  const frac_Lr_inf = bruteNr_Lr_inf - Math.floor(bruteNr_Lr_inf);
  const nr_diam_Lr_inf= frac_Lr_inf>= 0.5 ? Math.ceil(bruteNr_Lr_inf) : Math.floor(bruteNr_Lr_inf);

  const lungime_diam_Lr_inf = (Br * 100 + 2*40*diam_Lr/10)/100;
  const lungimeTotala_diam_Lr_inf = nr_diam_Lr_inf* lungime_diam_Lr_inf;
  const greutateTotala_diam_Lr_inf=lungimeTotala_diam_Lr_inf*Math.PI*Math.pow(diam_Lr/1000,2)/4*7850

  const bruteNr_Br_inf = ((Br * 100 - 10) / pas_diam_Br)+1;
  const frac_Br_inf = bruteNr_Br_inf - Math.floor(bruteNr_Br_inf);
  const nr_diam_Br_inf = frac_Br_inf >= 0.5 ? Math.ceil(bruteNr_Br_inf) : Math.floor(bruteNr_Br_inf);

  const lungime_diam_Br_inf = (Lr * 100 + 2*40*diam_Br/10)/100;
  const lungimeTotala_diam_Br_inf = nr_diam_Br_inf * lungime_diam_Br_inf;
  const greutateTotala_diam_Br_inf=lungimeTotala_diam_Br_inf*Math.PI*Math.pow(diam_Br/1000,2)/4*7850

  const bruteNr_Lr_sup = ((Lr* 100 - 10) / pas_diam_Lr_sup)+1;
  const frac_Lr_sup = bruteNr_Lr_sup - Math.floor(bruteNr_Lr_sup);
  const nr_diam_Lr_sup = frac_Lr_sup >= 0.5 ? Math.ceil(bruteNr_Lr_sup) : Math.floor(bruteNr_Lr_sup);

  const lungime_diam_Lr_sup = (Br * 100 + 2*40*diam_Lr_sup/10)/100;
  const lungimeTotala_diam_Lr_sup = nr_diam_Lr_sup * lungime_diam_Lr_sup;
  const greutateTotala_diam_Lr_sup=lungimeTotala_diam_Lr_sup*Math.PI*Math.pow(diam_Lr/1000,2)/4*7850

  const bruteNr_Br_sup = ((Br * 100 - 10) / pas_diam_Br_sup)+1;
  const frac_Br_sup = bruteNr_Br_sup - Math.floor(bruteNr_Br_sup);
  const nr_diam_Br_sup = frac_Br_sup >= 0.5 ? Math.ceil(bruteNr_Br_sup) : Math.floor(bruteNr_Br_sup);

  const lungime_diam_Br_sup = (Lr * 100 + 2*40*diam_Br_sup/10)/100;
  const lungimeTotala_diam_Br_sup = nr_diam_Br_sup * lungime_diam_Br_sup;
  const greutateTotala_diam_Br_sup=lungimeTotala_diam_Br_sup*Math.PI*Math.pow(diam_Br_sup/1000,2)/4*7850

  const bruteNr_Lbv= ((Lib* 100 - 10)/ pas_diam_bara_vert)+1;
  const frac_Lbv = bruteNr_Lbv - Math.floor(bruteNr_Lbv);
  const nr_Lbv= frac_Lbv>= 0.5 ? Math.ceil(bruteNr_Lbv) : Math.floor(bruteNr_Lbv);
  const nr_Lbv_tot=nr_Lbv*2*2

  const lungime_Lbv = ( Hpb*100+2*40*diam_bara_vert/10)/100;
  const lungimeTotala_Lbv = nr_Lbv_tot * lungime_Lbv;
  const greutateTotala_diam_Lbv=lungimeTotala_Lbv*Math.PI*Math.pow(diam_bara_vert/1000,2)/4*7850

  const bruteNr_Bbv= ((Bib* 100 - 10)/ pas_diam_bara_vert)+1;
  const frac_Bbv = bruteNr_Bbv - Math.floor(bruteNr_Bbv);
  const nr_Bbv= frac_Bbv>= 0.5 ? Math.ceil(bruteNr_Bbv) : Math.floor(bruteNr_Bbv);
  const nr_Bbv_tot=nr_Bbv*2*2+16

  const lungime_Bbv = ( Hpb*100+2*40*diam_bara_vert/10)/100;
  const lungimeTotala_Bbv = nr_Bbv_tot * lungime_Bbv;
  const greutateTotala_diam_Bbv=lungimeTotala_Bbv*Math.PI*Math.pow(diam_bara_vert/1000,2)/4*7850


  const bruteNr_Lbh= ((Hpb*100-10) / pas_diam_bara_oriz)+1;
  const frac_Lbh = bruteNr_Lbh - Math.floor(bruteNr_Lbh);
  const nr_Lbh= frac_Lbh>= 0.5 ? Math.ceil(bruteNr_Lbh) : Math.floor(bruteNr_Lbh);
  const nr_Lbh_tot=nr_Lbh*2*2

  const lungime_Lbh = (Lib*100 + 2*40*diam_bara_oriz/10)/100;
  const lungimeTotala_Lbh= nr_Lbh_tot * lungime_Lbh;
  const greutateTotala_diam_Lbh=lungimeTotala_Lbh*Math.PI*Math.pow(diam_bara_oriz/1000,2)/4*7850

  const bruteNr_Bbh= ((Hpb*100-10) / pas_diam_bara_oriz)+1;
  const frac_Bbh = bruteNr_Bbh - Math.floor(bruteNr_Bbh);
  const nr_Bbh= frac_Bbh>= 0.5 ? Math.ceil(bruteNr_Bbh) : Math.floor(bruteNr_Bbh);
  const nr_Bbh_tot=nr_Bbh*2*2

  const lungime_Bbh = (Bib*100 + 2*40*diam_bara_oriz/10)/100;
  const lungimeTotala_Bbh= nr_Bbh_tot * lungime_Bbh;
  const greutateTotala_diam_Bbh=lungimeTotala_Bbh*Math.PI*Math.pow(diam_bara_oriz/1000,2)/4*7850
  
  const bruteNr_etr= ((Hpb*100-10) / pas_diam_etr)+1;
  const frac_etr = bruteNr_etr - Math.floor(bruteNr_etr);
  const nr_etr= frac_etr>= 0.5 ? Math.ceil(bruteNr_etr) : Math.floor(bruteNr_etr);
  const nr_etr_tot=nr_etr*2*2

  const lungime_etr = (4*tpb*100-4*3.5+20)/100;
  const lungimeTotala_etr= nr_etr_tot * lungime_etr;
  const greutateTotala_etr=lungimeTotala_etr*Math.PI*Math.pow(diam_etr/1000,2)/4*7850

  const bruteNr_capre= (Lr*Br);
  const frac_capre = bruteNr_capre - Math.floor(bruteNr_capre);
  const nr_capre= frac_capre>= 0.5 ? Math.ceil(bruteNr_capre) : Math.floor(bruteNr_capre);

  const lungime_capra = (3*35+2*(trb*100-(10+diam_Lr/10+diam_Br/10+diam_Lr_sup/10+diam_Br_sup/10)))/100;
  const lungimeTotala_capre= nr_capre * lungime_capra;
  const greutateTotala_capre=lungimeTotala_capre*Math.PI*Math.pow(14/1000,2)/4*7850

  const bruteNr_agrafe= Lib*Hpb+Bib*Hpb;
  const frac_agrafe = bruteNr_agrafe - Math.floor(bruteNr_agrafe);
  const nr_agrafe= frac_agrafe>= 0.5 ? Math.ceil(bruteNr_agrafe) : Math.floor(bruteNr_agrafe);
  const nr_agrafe_tot=2 * 4 * nr_agrafe

  const lungime_agrafa = (tpb*100-8+20)/100;
  const lungimeTotala_agrafe= nr_agrafe_tot * lungime_agrafa;
  const greutateTotala_agrafe=lungimeTotala_agrafe*Math.PI*Math.pow(6/1000,2)/4*7850

const greutateTotala_armatura_bazin=greutateTotala_diam_Lr_inf+greutateTotala_diam_Br_inf+greutateTotala_diam_Lr_sup+greutateTotala_diam_Br_sup+greutateTotala_diam_Lbv+greutateTotala_diam_Bbv+greutateTotala_diam_Lbh+greutateTotala_diam_Bbh+greutateTotala_etr+greutateTotala_capre+greutateTotala_agrafe
     /* Greutate armatura bazine (efectiv) n_b */
     const greutateTotala_armatura_bazine=greutateTotala_armatura_bazin* nb;


     bazin.querySelector('.cantitati_bazin').innerText =
`n_b (număr bazine asemenea): ${nb}

Cantități pentru un bazin ${id}:
 • Adancime săpătură: ${hexc.toFixed(2)} m³
 • Volum săpătură: ${V_sap_unit.toFixed(2)} m³
 • Volum beton simplu (egalizare): ${V_bs_unit.toFixed(2)} m³
 • Volum beton radier: ${V_radier_unit.toFixed(2)} m³
 • Volum beton pereti: ${V_pereti_unit.toFixed(2)} m³
 • Volum beton bazin: ${V_bazin_unit.toFixed(2)} m³
 • Armatura bazin: ${m_V_bazin_unit.toFixed(2)} kg
 • Greutate armatura bazin (efectiv): ${greutateTotala_armatura_bazin.toFixed(2)} kg

Cantități TOTALE pentru n_b = ${nb} bazine asemenea:
 • Volum săpătură total ${id}: ${V_sap_tot.toFixed(2)} m³
 • Volum beton simplu total ${id}: ${V_bs_tot.toFixed(2)} m³
 • Volum beton radier total ${id}: ${V_radier_tot.toFixed(2)} m³
 • Volum beton pereti total ${id}: ${V_pereti_tot.toFixed(2)} m³
 • Volum beton bazine total ${id}: ${V_bazin_tot.toFixed(2)} m³
 • Armatura bazine total ${id}: ${m_V_bazin_tot.toFixed(2)} kg
 • Greutate armatura bazine (efectiv) ${id}: ${greutateTotala_armatura_bazine.toFixed(2)} kg

⚙️ Armare longitudinală radier inferior:
 • Număr bare: ${nr_diam_Lr_inf}
 • Lungime bară: ${lungime_diam_Lr_inf.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_diam_Lr_inf.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Lr_inf.toFixed(2)} kg

⚙️ Armare transversala radier inferior:
 • Număr bare: ${nr_diam_Br_inf}
 • Lungime bară: ${lungime_diam_Br_inf.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_diam_Br_inf.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Br_inf.toFixed(2)} kg

⚙️ Armare longitudinală radier superior:
 • Număr bare: ${nr_diam_Lr_sup}
 • Lungime bară: ${lungime_diam_Lr_sup.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_diam_Lr_sup.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Lr_sup.toFixed(2)} kg

⚙️ Armare transversala radier inferior:
 • Număr bare: ${nr_diam_Br_sup}
 • Lungime bară: ${lungime_diam_Br_sup.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_diam_Br_sup.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Br_sup.toFixed(2)} kg

⚙️ Armare verticala pereti pe L:
 • Număr bare: ${nr_Lbv_tot}
 • Lungime bară: ${lungime_Lbv.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_Lbv.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Lbv.toFixed(2)} kg

⚙️ Armare verticala pereti pe B:
 • Număr bare: ${nr_Bbv_tot}
 • Lungime bară: ${lungime_Bbv.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_Bbv.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Bbv.toFixed(2)} kg

⚙️ Armare orizontala pereti pe L:
 • Număr bare: ${nr_Lbh_tot}
 • Lungime bară: ${lungime_Lbh.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_Lbh.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Lbh.toFixed(2)} kg

⚙️ Armare orizontala pereti pe B:
 • Număr bare: ${nr_Bbh_tot}
 • Lungime bară: ${lungime_Bbh.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_Bbh.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_diam_Bbh.toFixed(2)} kg

⚙️ Armare etrieri:
 • Număr bare: ${nr_etr_tot}
 • Lungime bară: ${lungime_etr.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_etr.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_etr.toFixed(2)} kg

⚙️ Armare capre (1Ø14/mp):
 • Număr bare: ${nr_capre}
 • Lungime bară: ${lungime_capra.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_capre.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_capre.toFixed(2)} kg

⚙️ Armare agrafe (4Ø6/mp):
 • Număr bare: ${nr_agrafe_tot}
 • Lungime bară: ${lungime_agrafa.toFixed(2)} m
 • Lungime totală bare: ${lungimeTotala_agrafe.toFixed(2)} m
 • Greutate totală bare: ${greutateTotala_agrafe.toFixed(2)} kg

`;
}



salveazaDate();
    }

   /* ---------- CENTRALIZATOR ---------- */
    function calculeazaTotal() {
      let total_sapatura = 0,
          total_beton    = 0,
          total_radier    = 0,
          total_pereti    = 0,
          total_bazine    = 0,
          total_armatura    = 0,
          total_armatura_efectiv   = 0;

      document.querySelectorAll('.sheet:not(#centralizator_sheet)').forEach(sheet => {
        const txt = sheet.querySelector('.cantitati_bazin').innerText;
        const mSap   = txt.match(/Volum săpătură total.*?:\s([\d.]+)/);
        const mBeton = txt.match(/Volum beton simplu total.*?:\s([\d.]+)/);
        const mRadier = txt.match(/Volum beton radier total.*?:\s([\d.]+)/);
        const mPereti = txt.match(/Volum beton pereti total.*?:\s([\d.]+)/);
        const mBazine = txt.match(/Volum beton bazine total.*?:\s([\d.]+)/);
        const mArmatura= txt.match(/Armatura bazine total.*?:\s([\d.]+)/);
        const mArmatura_efectiv = txt.match(/Greutate armatura bazine \(efectiv\)(?:\s+\w+)?:\s([\d.]+)/);
   

   
        if (mSap)   total_sapatura += parseFloat(mSap[1]);
        if (mBeton) total_beton    += parseFloat(mBeton[1]);
        if (mRadier) total_radier    += parseFloat(mRadier[1]);
        if (mPereti) total_pereti    += parseFloat(mPereti[1]);
        if (mBazine) total_bazine    += parseFloat(mBazine[1]);
        if (mArmatura) total_armatura    += parseFloat(mArmatura[1]);
        if (mArmatura_efectiv) total_armatura_efectiv    += parseFloat(mArmatura_efectiv[1]);
      
      });

      document.getElementById('central_output').innerText =
`📊 CENTRALIZATOR TOTAL:
 • Volum săpătură (toate bazinele): ${total_sapatura.toFixed(2)} m³
 • Volum beton simplu (toate bazinele): ${total_beton.toFixed(2)} m³
 • Volum beton radier (toate bazinele): ${total_radier.toFixed(2)} m³
 • Volum beton pereti (toate bazinele): ${total_pereti.toFixed(2)} m³
 • Volum beton bazine (toate bazinele): ${total_bazine.toFixed(2)} m³
 • Armatura bazine (toate bazinele): ${total_armatura.toFixed(2)} kg
 • Armatura bazine (toate bazinele, efectiv): ${total_armatura_efectiv.toFixed(2)} kg

`;
    }

    /* ---------- EXPORT / IMPORT JSON ---------- */
    function exportJSON() {
      const blob = new Blob([JSON.stringify(gatherData(), null, 2)], {type:'application/json'});
      const url  = URL.createObjectURL(blob);
      const a    = document.createElement('a');
      a.href = url; a.download = 'bazine.json';
      document.body.appendChild(a); a.click();
      document.body.removeChild(a); URL.revokeObjectURL(url);
    }

    function importJSON() {
      document.getElementById('import_file').click();
    }

function printeazaTot() {
  const toateFisele = [...document.querySelectorAll('.sheet:not(#centralizator_sheet)')];
  const centralizator = document.getElementById('centralizator_sheet');

  let htmlPrint = `
    <html>
    <head>
      <title>Print Bazine</title>
      <style>
        @media print {
          body { font-family: Arial; font-size: 12px; margin: 20mm; }
          h2 { margin-top: 30px; }
          .form-row { display: flex; flex-wrap: wrap; margin-bottom: 5px; }
          .form-row label { width: 120px; font-weight: bold; }
          .form-row span { margin-left: 10px; color: #444; }
          .output { margin-top: 10px; white-space: pre-line; background: #eef; padding: 10px; border: 1px solid #ccc; }
          svg { margin-top: 10px; max-width: 100%; height: auto; }
          .page-break { page-break-after: always; display: block; height: 0; }
        }
      </style>
    </head>
    <body>
      <h1>📄 Fișe bazine și centralizator</h1>
  `;

  // Fișe individuale
  toateFisele.forEach(sheet => {
    const titlu = sheet.querySelector('.titlu')?.outerHTML || "";
    const inputuri = sheet.querySelectorAll('.form-row');
    const svg = sheet.querySelector('svg')?.outerHTML || "";
    const output = sheet.querySelector('.cantitati_bazin')?.outerHTML || "";

    htmlPrint += `<div>${titlu}`;
    
    inputuri.forEach(row => {
      const label = row.querySelector('label')?.innerText || '';
      const input = row.querySelector('input');
      const valoare = input ? input.value : '';
      const unit = row.querySelector('span')?.innerText || '';
      htmlPrint += `<div><strong>${label}</strong>= ${valoare}, <em>${unit}</em></div>`;
    });

    htmlPrint += svg;
    htmlPrint += output;
    htmlPrint += `</div><div class="page-break"></div>`;
  });

  // Centralizator
  htmlPrint += `<div><h2>📊 Centralizator</h2>`;
  htmlPrint += centralizator.querySelector('#central_output')?.outerHTML || "";
  htmlPrint += `</div>`;

  htmlPrint += `</body></html>`;

  const printWindow = window.open('', '_blank');
  printWindow.document.write(htmlPrint);
  printWindow.document.close();
  printWindow.focus();
  setTimeout(() => {
    printWindow.print();
    printWindow.close();
  }, 500);
}
    document.getElementById('import_file').addEventListener('change', e => {
      const file = e.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = ev => {
        try {
          const data = JSON.parse(ev.target.result);
          if (!Array.isArray(data)) throw Error('Format JSON invalid');
          clearBazine();
          data.forEach(obj => {
            adaugaBazin();
            const sheet = container.lastElementChild;
            sheet.querySelector('.numar_bazine').value   = obj.nb   ?? '';
            sheet.querySelector('.lungime_int_bazin').value = obj.Lib   ?? '';
            sheet.querySelector('.latime_int_bazin' ).value = obj.Bib   ?? '';
            sheet.querySelector('.cota_0').value = obj.Cota0   ?? '';
            sheet.querySelector('.inaltime_perete_bazin'   ).value = obj.Hpb   ?? '';
            sheet.querySelector('.grosime_perete_bazin' ).value = obj.tpb   ?? '';
            sheet.querySelector('.grosime_radier_bazin'  ).value = obj.trb   ?? '';
            sheet.querySelector('.evazare_radier' ).value = obj.evr   ?? '';
            sheet.querySelector('.grosime_bs'      ).value = obj.hbs  ?? '';
            sheet.querySelector('.evazare_excavatie').value= obj.evexc   ?? '';
            sheet.querySelector('.panta_excavatie' ).value = obj.pexc?? '';
            calculeazaIndividual(sheet.querySelector('button'));
          });
          salveazaDate();
          schimbaSheet('centralizator_sheet');
        } catch(err) {
          alert('Eroare import: ' + err.message);
        }
        e.target.value = ''; // reset input
      };
      reader.readAsText(file);
    });

    /* ---------- ÎNCĂRCARE DATE ---------- */
    function incarcaDate() {
      const saved = localStorage.getItem(STORAGE_KEY);
      if (!saved) { adaugaBazin(); return; }

      try {
        const data = JSON.parse(saved);
        if (!Array.isArray(data) || data.length === 0) throw Error();
        data.forEach(obj => {
          adaugaBazin();
          const sheet = container.lastElementChild;
          sheet.querySelector('.numar_bazine').value   = obj.nb   ?? '';
            sheet.querySelector('.lungime_int_bazin').value = obj.Lib   ?? '';
            sheet.querySelector('.latime_int_bazin' ).value = obj.Bib   ?? '';
            sheet.querySelector('.cota_0').value = obj.Cota0   ?? '';
            sheet.querySelector('.inaltime_perete_bazin'   ).value = obj.Hpb   ?? '';
            sheet.querySelector('.grosime_perete_bazin' ).value = obj.tpb   ?? '';
            sheet.querySelector('.grosime_radier_bazin'  ).value = obj.trb   ?? '';
            sheet.querySelector('.evazare_radier' ).value = obj.evr   ?? '';
            sheet.querySelector('.grosime_bs'      ).value = obj.hbs  ?? '';
            sheet.querySelector('.evazare_excavatie').value= obj.evexc   ?? '';
            sheet.querySelector('.panta_excavatie' ).value = obj.pexc ?? '';
            calculeazaIndividual(sheet.querySelector('button'));
        });
      } catch {
        adaugaBazin();
      }
    }

    /* ---------- INIT ---------- */
    window.onload = () => {
      // tab centralizator
      const centralBtn = document.createElement('button');
      centralBtn.textContent = 'Centralizator';
      centralBtn.className   = 'tab-btn active';
      centralBtn.onclick     = () => schimbaSheet('centralizator_sheet');
      tabs.appendChild(centralBtn);
      document.getElementById('centralizator_sheet').classList.add('active');

      incarcaDate();
    
};

  </script>
</body>
</html>

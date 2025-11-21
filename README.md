<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Digital Communication Dashboard</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--accent:#06b6d4;--muted:#94a3b8;--glass: rgba(255,255,255,0.03)}
    html,body{height:100%;margin:0;font-family:Inter,Segoe UI,Roboto,Arial,sans-serif;background:linear-gradient(180deg,#071426 0%, #081827 100%);color:#e6eef6}
    header{display:flex;align-items:center;gap:12px;padding:16px 24px;background:linear-gradient(90deg,rgba(255,255,255,0.02),transparent)}
    header h1{margin:0;font-size:18px;letter-spacing:0.4px}
    .container{display:grid;grid-template-columns:260px 1fr;gap:18px;padding:18px}
    nav{background:var(--card);border-radius:12px;padding:14px;height:calc(100vh - 120px);box-shadow:0 6px 18px rgba(2,6,23,0.6)}
    nav button{display:block;width:100%;text-align:left;padding:10px;border-radius:8px;background:transparent;border:0;color:var(--muted);margin-bottom:6px;cursor:pointer}
    nav button.active{background:linear-gradient(90deg,rgba(6,182,212,0.08),transparent);color:var(--accent);font-weight:600}
    main{background:var(--glass);padding:18px;border-radius:12px;min-height:70vh}
    .module{display:none}
    .module.active{display:block}
    .controls{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:12px}
    .card{background:rgba(255,255,255,0.03);padding:12px;border-radius:10px;margin-bottom:12px}
    canvas{width:100%;height:220px;border-radius:8px;background:#021124;display:block}
    label{font-size:13px;color:var(--muted)}
    input[type=range]{width:240px}
    .row{display:flex;gap:12px}
    .col{flex:1}
    .small{font-size:13px;color:var(--muted)}
    footer{padding:12px 24px;color:var(--muted);font-size:13px}
    .btn{background:var(--accent);color:#022;padding:8px 12px;border-radius:8px;border:0;cursor:pointer}
    .link{color:var(--accent);text-decoration:underline;cursor:pointer}
    .legend{display:flex;gap:10px;align-items:center}
    .dot{width:12px;height:12px;border-radius:50%}
  </style>
</head>
<body>
  <header>
    <img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='36' height='36'><rect width='36' height='36' rx='6' fill='%2306b6d4'/><text x='50%' y='57%' font-size='18' font-family='Arial' fill='%2301141a' text-anchor='middle'>DC</text></svg>" alt="logo" style="width:36px;height:36px;border-radius:6px;">
    <div>
      <h1 style="margin:0;font-size:18px;letter-spacing:0.4px">Digital Communication — Interactive Dashboard (Single file)</h1>
      <div style="font-size:13px;color:rgba(255,255,255,0.7);margin-top:4px">Project: <strong>Digital Communication Dashboard</strong> &nbsp; | &nbsp; Owner: <strong>PIYUSH MISHRA</strong></div>
    </div>
  </header>

  <div class="container">
    <nav>
      <button class="active" data-target="sampling">Sampling</button>
      <button data-target="linecoding">Line Coding</button>
      <button data-target="modulation">Modulation</button>
      <button data-target="pcm">PCM</button>
      <button data-target="delta">Delta Modulation</button>
      <hr style="border:none;border-top:1px solid rgba(255,255,255,0.03);margin:12px 0">
      <div style="font-size:13px;color:var(--muted)">Tips</div>
      <ul style="padding-left:18px;color:var(--muted)">
        <li>Open this file in VS Code and press Live Server or open in browser.</li>
        <li>Use controls to change amplitude, frequency & sampling rate.</li>
        <li>Click "Run" for each module to draw signals & see effects.</li>
      </ul>
    </nav>

    <main>
      <!-- Sampling Module -->
      <section id="sampling" class="module active">
        <h2>Sampling (Unit I)</h2>
        <div class="card controls">
          <div class="row">
            <div class="col">
              <label>Analog signal freq (Hz)</label>
              <input id="s_sig_freq" type="range" min="1" max="50" value="5"><div class="small"><span id="s_sig_freq_v">5</span> Hz</div>
            </div>
            <div class="col">
              <label>Sampling freq (Hz)</label>
              <input id="s_samp_freq" type="range" min="2" max="200" value="50"><div class="small"><span id="s_samp_freq_v">50</span> Hz</div>
            </div>
            <div class="col">
              <label>Sampling type</label>
              <select id="s_type"><option value="ideal">Ideal</option><option value="natural">Natural</option><option value="flattop">Flat-top</option></select>
            </div>
            <div><button class="btn" id="s_run">Run</button></div>
          </div>
        </div>
        <div class="card">
          <canvas id="s_canvas" width="1000" height="260"></canvas>
        </div>
      </section>

      <!-- Line Coding Module -->
      <section id="linecoding" class="module">
        <h2>Line Coding (Unit II)</h2>
        <div class="card controls">
          <div style="flex:1">
            <label>Bit sequence (spaces allowed)</label>
            <input id="lc_bits" type="text" value="1 0 0 1 1 0 1" style="width:100%">
            <div class="small">Examples: "1 0 1 1 0" or "10110"</div>
          </div>
          <div>
            <label>Scheme</label>
            <select id="lc_scheme"><option>NRZ</option><option>RZ</option><option>Manchester</option><option>AMI</option></select>
          </div>
          <div><button class="btn" id="lc_run">Run</button></div>
        </div>
        <div class="card">
          <canvas id="lc_canvas" width="1000" height="160"></canvas>
        </div>
      </section>

      <!-- Modulation Module -->
      <section id="modulation" class="module">
        <h2>Modulation — ASK / BPSK / QPSK (Unit III)</h2>
        <div class="card controls">
          <div class="col">
            <label>Carrier freq (Hz)</label>
            <input id="m_carrier" type="range" min="10" max="200" value="60"><div class="small"><span id="m_carrier_v">60</span> Hz</div>
          </div>
          <div class="col">
            <label>Message freq (Hz)</label>
            <input id="m_msg" type="range" min="1" max="50" value="5"><div class="small"><span id="m_msg_v">5</span> Hz</div>
          </div>
          <div>
            <label>Modulation</label>
            <select id="m_type"><option>ASK</option><option>BPSK</option><option>QPSK</option></select>
          </div>
          <div><button class="btn" id="m_run">Run</button></div>
        </div>
        <div class="card"><canvas id="m_canvas" width="1000" height="220"></canvas></div>
      </section>

      <!-- PCM Module -->
      <section id="pcm" class="module">
        <h2>PCM (Pulse Code Modulation) — (Unit II)</h2>
        <div class="card controls">
          <div class="col">
            <label>Analog signal freq (Hz)</label>
            <input id="p_sig_freq" type="range" min="1" max="50" value="4"><div class="small"><span id="p_sig_freq_v">4</span> Hz</div>
          </div>
          <div class="col">
            <label>Sampling freq (Hz)</label>
            <input id="p_samp_freq" type="range" min="8" max="200" value="64"><div class="small"><span id="p_samp_freq_v">64</span> Hz</div>
          </div>
          <div class="col">
            <label>Quantization bits</label>
            <input id="p_bits" type="range" min="2" max="8" value="4"><div class="small">Bits: <span id="p_bits_v">4</span></div>
          </div>
          <div><button class="btn" id="p_run">Run</button></div>
        </div>
        <div class="card"><canvas id="p_canvas" width="1000" height="220"></canvas></div>
        <div class="card small" id="p_info">PCM Bits: <code id="p_bits_out">-</code></div>
      </section>

      <!-- Delta Modulation Module -->
      <section id="delta" class="module">
        <h2>Delta Modulation (Unit II)</h2>
        <div class="card controls">
          <div class="col">
            <label>Signal freq (Hz)</label>
            <input id="d_sig_freq" type="range" min="1" max="30" value="3"><div class="small"><span id="d_sig_freq_v">3</span> Hz</div>
          </div>
          <div class="col">
            <label>Step size (&Delta;)</label>
            <input id="d_step" type="range" min="0.01" max="1" step="0.01" value="0.2"><div class="small">&Delta; = <span id="d_step_v">0.2</span></div>
          </div>
          <div><button class="btn" id="d_run">Run</button></div>
        </div>
        <div class="card"><canvas id="d_canvas" width="1000" height="220"></canvas></div>
      </section>

    </main>
  </div>
  <footer>Built for RGPV ECE 5th Sem topics. Save this file as <code>digital-dashboard.html</code> and open in browser (or use Live Server in VS Code).</footer>

<script>
// Helper: draw axes
function clearCanvas(c){const ctx=c.getContext('2d');ctx.clearRect(0,0,c.width,c.height);ctx.fillStyle='#021124';ctx.fillRect(0,0,c.width,c.height);}
function drawAxes(c){const ctx=c.getContext('2d');ctx.strokeStyle='rgba(255,255,255,0.06)';ctx.lineWidth=1;ctx.beginPath();ctx.moveTo(40,10);ctx.lineTo(40,c.height-30);ctx.lineTo(c.width-10,c.height-30);ctx.stroke();}
function mapY(c,y,amin,amax){const h=c.height-40;return 10 + ( (amax - y) / (amax - amin) ) * h;}

// NAV
document.querySelectorAll('nav button').forEach(b=>{b.addEventListener('click',()=>{document.querySelectorAll('nav button').forEach(x=>x.classList.remove('active'));b.classList.add('active');const t=b.dataset.target;document.querySelectorAll('.module').forEach(m=>m.classList.remove('active'));document.getElementById(t).classList.add('active');})});

// SAMPLING
const s_canvas=document.getElementById('s_canvas');const s_ctx=s_canvas.getContext('2d');
function runSampling(){const f = +document.getElementById('s_sig_freq').value;const fs = +document.getElementById('s_samp_freq').value;const type = document.getElementById('s_type').value;clearCanvas(s_canvas);drawAxes(s_canvas);
 const t0=0, t1=1; const samples=1000; const dt=(t1-t0)/samples; const A=1; let arr=[]; for(let i=0;i<=samples;i++){const t=t0+i*dt;const val=A*Math.sin(2*Math.PI*f*t);arr.push({t,val});}
 // draw analog
 s_ctx.beginPath();s_ctx.strokeStyle='#8bd3e6';s_ctx.lineWidth=1.5; for(let i=0;i<arr.length;i++){const x=40 + (arr[i].t - t0)/(t1-t0)*(s_canvas.width-60);const y=mapY(s_canvas,arr[i].val,-1.5,1.5); if(i===0) s_ctx.moveTo(x,y); else s_ctx.lineTo(x,y);} s_ctx.stroke();
 // draw samples
 const sampPeriod=1/fs; s_ctx.fillStyle='#ffd166'; s_ctx.strokeStyle='#ffd166'; for(let n=0;;n++){const t=n*sampPeriod; if(t>t1) break; const val=A*Math.sin(2*Math.PI*f*t); const x=40 + (t - t0)/(t1-t0)*(s_canvas.width-60); const y=mapY(s_canvas,val,-1.5,1.5);
   if(type==='ideal'){s_ctx.beginPath();s_ctx.moveTo(x,y); s_ctx.lineTo(x,s_canvas.height-30); s_ctx.stroke(); s_ctx.beginPath(); s_ctx.arc(x,y,3,0,Math.PI*2); s_ctx.fill();}
   else if(type==='natural'){ // draw short pulse
     const w=6; s_ctx.fillRect(x-w/2,y, w, s_canvas.height-30 - y);
   } else { // flattop
     const w=12; s_ctx.fillRect(x-w/2,y-3,w,6);
   }
 }
 // annotate
 s_ctx.fillStyle='rgba(255,255,255,0.7)'; s_ctx.font='12px Arial'; s_ctx.fillText('Analog signal (blue) and samples (yellow)',50,18);
}
// links
['s_sig_freq','s_samp_freq'].forEach(id=>{document.getElementById(id).addEventListener('input',e=>{document.getElementById(id+'_v').textContent=e.target.value;})});
['s_type'].forEach(id=>document.getElementById(id).addEventListener('change',runSampling)); document.getElementById('s_run').addEventListener('click',runSampling);
runSampling();

// LINE CODING
const lc_canvas=document.getElementById('lc_canvas');const lc_ctx=lc_canvas.getContext('2d');
function parseBits(s){s=s.replace(/\s+/g,''); if(!/^[01]*$/.test(s)) s=s.replace(/[^01]/g,''); if(s.length===0) return [1,0,1,1,0,1]; return s.split('').map(x=>+x);} 
function runLineCoding(){const bits=parseBits(document.getElementById('lc_bits').value); const scheme=document.getElementById('lc_scheme').value; clearCanvas(lc_canvas); drawAxes(lc_canvas);
 const unitWidth=(lc_canvas.width-60)/bits.length; const ymid=lc_canvas.height/2; lc_ctx.strokeStyle='#a6e3a1'; lc_ctx.lineWidth=2; lc_ctx.beginPath(); let x0=40; for(let i=0;i<bits.length;i++){const b=bits[i]; if(scheme==='NRZ'){const y= ymid - (b?60:-60); lc_ctx.moveTo(x0,y); lc_ctx.lineTo(x0+unitWidth,y); x0+=unitWidth;}
 else if(scheme==='RZ'){const y= ymid - (b?60:-60); lc_ctx.moveTo(x0,y); lc_ctx.lineTo(x0+unitWidth*0.6,y); lc_ctx.moveTo(x0+unitWidth*0.6,ymid); x0+=unitWidth;}
 else if(scheme==='Manchester'){ // transition in middle: 1 -> high->low, 0 low->high
   const mid=x0+unitWidth/2; const y1=ymid-60; const y0=ymid+60; if(b===1){lc_ctx.moveTo(x0,y1); lc_ctx.lineTo(mid,y1); lc_ctx.lineTo(mid,y0); lc_ctx.lineTo(x0+unitWidth,y0);} else {lc_ctx.moveTo(x0,y0); lc_ctx.lineTo(mid,y0); lc_ctx.lineTo(mid,y1); lc_ctx.lineTo(x0+unitWidth,y1);} x0+=unitWidth;
 }
 else if(scheme==='AMI'){ if(b===1){lc_ctx.moveTo(x0,ymid-60); lc_ctx.lineTo(x0+unitWidth,ymid-60);} else {lc_ctx.moveTo(x0,ymid); lc_ctx.lineTo(x0+unitWidth,ymid);} x0+=unitWidth; }
 }
 lc_ctx.stroke(); lc_ctx.fillStyle='rgba(255,255,255,0.8)'; lc_ctx.font='12px Arial'; lc_ctx.fillText('Bits: '+bits.join(' '),50,18);
}
document.getElementById('lc_run').addEventListener('click',runLineCoding);
runLineCoding();

// MODULATION
const m_canvas=document.getElementById('m_canvas');const m_ctx=m_canvas.getContext('2d');
['m_carrier','m_msg'].forEach(id=>document.getElementById(id).addEventListener('input',e=>document.getElementById(id+'_v').textContent=e.target.value));
function runModulation(){const fc=+document.getElementById('m_carrier').value;const fm=+document.getElementById('m_msg').value;const type=document.getElementById('m_type').value; clearCanvas(m_canvas); drawAxes(m_canvas);
 const t0=0,t1=0.05; const samples=2000; const A=1; const Ac=1; const twoPI=2*Math.PI; m_ctx.lineWidth=1.2;
 // message
 let msg=[]; for(let i=0;i<=samples;i++){const t=t0+(t1-t0)*i/samples; msg.push({t, v:Math.sin(twoPI*fm*t)});} m_ctx.beginPath(); m_ctx.strokeStyle='rgba(120,180,255,0.5)'; for(let i=0;i<msg.length;i++){const x=40 + (msg[i].t-t0)/(t1-t0)*(m_canvas.width-60); const y=mapY(m_canvas,msg[i].v,-1.5,1.5); if(i===0) m_ctx.moveTo(x,y); else m_ctx.lineTo(x,y);} m_ctx.stroke();
 // carrier and modulated
 let car=[]; for(let i=0;i<=samples;i++){const t=t0+(t1-t0)*i/samples; car.push({t, v:Math.sin(twoPI*fc*t)});} if(type==='ASK'){m_ctx.beginPath(); m_ctx.strokeStyle='#ffd166'; for(let i=0;i<car.length;i++){const t=car[i].t; const x=40 + (t-t0)/(t1-t0)*(m_canvas.width-60); const mval=Math.sin(twoPI*fm*t); const y=mapY(m_canvas,(1+ mval)/2 * Math.sin(twoPI*fc*t), -1.5,1.5); if(i===0) m_ctx.moveTo(x,y); else m_ctx.lineTo(x,y);} m_ctx.stroke();}
 else if(type==='BPSK'){m_ctx.beginPath(); m_ctx.strokeStyle='#ffd166'; for(let i=0;i<car.length;i++){const t=car[i].t; const x=40 + (t-t0)/(t1-t0)*(m_canvas.width-60); const mval=Math.sin(twoPI*fm*t); const phase = mval>=0?0:Math.PI; const y=mapY(m_canvas, Math.sin(twoPI*fc*t + phase), -1.5,1.5); if(i===0) m_ctx.moveTo(x,y); else m_ctx.lineTo(x,y);} m_ctx.stroke();}
 else if(type==='QPSK'){ // draw two carriers with pi/2 shift mapping
   m_ctx.beginPath(); m_ctx.strokeStyle='#ffd166'; for(let i=0;i<car.length;i++){const t=car[i].t; const x=40 + (t-t0)/(t1-t0)*(m_canvas.width-60); const I = Math.sign(Math.cos(twoPI*fm*t)); const Q = Math.sign(Math.sin(twoPI*fm*t)); const ph = (I>=0?0:Math.PI) + (Q>=0?0:Math.PI/2); const y=mapY(m_canvas, Math.cos(twoPI*fc*t + ph), -1.5,1.5); if(i===0) m_ctx.moveTo(x,y); else m_ctx.lineTo(x,y);} m_ctx.stroke();}
 m_ctx.fillStyle='rgba(255,255,255,0.85)'; m_ctx.font='12px Arial'; m_ctx.fillText('Blue: message, Yellow: modulated',50,18);
}
document.getElementById('m_run').addEventListener('click',runModulation); runModulation();

// PCM
const p_canvas=document.getElementById('p_canvas');const p_ctx=p_canvas.getContext('2d');
['p_sig_freq','p_samp_freq','p_bits'].forEach(id=>document.getElementById(id).addEventListener('input',e=>document.getElementById(id+'_v').textContent=e.target.value));
function runPCM(){const fm=+document.getElementById('p_sig_freq').value; const fs=+document.getElementById('p_samp_freq').value; const bits=+document.getElementById('p_bits').value; clearCanvas(p_canvas); drawAxes(p_canvas);
 const t0=0,t1=1; const samples=1000; const A=1; let analog=[]; for(let i=0;i<=samples;i++){const t=t0+(t1-t0)*i/samples; analog.push({t,v:Math.sin(2*Math.PI*fm*t)});} p_ctx.beginPath(); p_ctx.strokeStyle='rgba(120,180,255,0.6)'; for(let i=0;i<analog.length;i++){const x=40+(analog[i].t-t0)/(t1-t0)*(p_canvas.width-60); const y=mapY(p_canvas,analog[i].v,-1.5,1.5); if(i===0) p_ctx.moveTo(x,y); else p_ctx.lineTo(x,y);} p_ctx.stroke();
 // sampling
 const sampPeriod=1/fs; let samplesArr=[]; for(let n=0;;n++){const t=n*sampPeriod; if(t>t1) break; const v=Math.sin(2*Math.PI*fm*t); samplesArr.push({t,v});}
 // quantize
 const L = Math.pow(2,bits); const qLevels=[]; for(let k=0;k<L;k++){qLevels.push(-1 + (2*k+1)/L);} let bitstream=''; for(let i=0;i<samplesArr.length;i++){const v=samplesArr[i].v; // nearest level
   let bestIdx=0; let bestDiff=1e9; for(let k=0;k<qLevels.length;k++){const diff=Math.abs(v - qLevels[k]); if(diff<bestDiff){bestDiff=diff;bestIdx=k;}}
   const q=qLevels[bestIdx]; const x=40 + (samplesArr[i].t-t0)/(t1-t0)*(p_canvas.width-60); const y=mapY(p_canvas,q,-1.5,1.5); p_ctx.fillStyle='#ffd166'; p_ctx.beginPath(); p_ctx.arc(x,y,3,0,Math.PI*2); p_ctx.fill(); // draw stair
   p_ctx.strokeStyle='#ffd166'; p_ctx.beginPath(); p_ctx.moveTo(x,y); p_ctx.lineTo(x + (p_canvas.width-60)/samplesArr.length, y); p_ctx.stroke(); bitstream += bestIdx.toString(2).padStart(bits,'0'); }
 document.getElementById('p_bits_out').textContent = bitstream.substring(0,200) + (bitstream.length>200? '...':'');
 p_ctx.fillStyle='rgba(255,255,255,0.85)'; p_ctx.font='12px Arial'; p_ctx.fillText('Yellow: quantized samples (stair), Blue: analog',50,18);
}
document.getElementById('p_run').addEventListener('click',runPCM); runPCM();

// DELTA MODULATION
const d_canvas=document.getElementById('d_canvas');const d_ctx=d_canvas.getContext('2d');
['d_sig_freq','d_step'].forEach(id=>document.getElementById(id).addEventListener('input',e=>document.getElementById(id+'_v').textContent=e.target.value));
function runDelta(){const fm=+document.getElementById('d_sig_freq').value; const step=+document.getElementById('d_step').value; clearCanvas(d_canvas); drawAxes(d_canvas);
 const t0=0, t1=1, samples=1000; let analog=[]; for(let i=0;i<=samples;i++){const t=t0+(t1-t0)*i/samples; analog.push({t,v:Math.sin(2*Math.PI*fm*t)});} d_ctx.beginPath(); d_ctx.strokeStyle='rgba(120,180,255,0.6)'; for(let i=0;i<analog.length;i++){const x=40+(analog[i].t-t0)/(t1-t0)*(d_canvas.width-60); const y=mapY(d_canvas,analog[i].v,-1.5,1.5); if(i===0) d_ctx.moveTo(x,y); else d_ctx.lineTo(x,y);} d_ctx.stroke();
 // delta mod
 let recon=0; let prevX=40; let bitseq=''; d_ctx.strokeStyle='#ffd166'; d_ctx.lineWidth=1.2; d_ctx.beginPath(); for(let i=0;i<analog.length;i+=10){const a=analog[i]; const x=40+(a.t-t0)/(t1-t0)*(d_canvas.width-60); const err=a.v - recon; const bit = err>=0?1:0; bitseq += bit; recon += (bit? step: -step); const y=mapY(d_canvas,recon,-1.5,1.5); if(i===0) d_ctx.moveTo(x,y); else d_ctx.lineTo(x,y);}
 d_ctx.stroke(); d_ctx.fillStyle='rgba(255,255,255,0.85)'; d_ctx.font='12px Arial'; d_ctx.fillText('Blue: analog, Yellow: delta reconstructed (coarse)',50,18);
}
document.getElementById('d_run').addEventListener('click',runDelta); runDelta();

</script>
</body>
</html>

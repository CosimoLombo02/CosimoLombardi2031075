# CosimoLombardi2031075
## Introduction to RSA Cryptography

The **RSA algorithm** (named after its inventors *Rivest, Shamir, and Adleman*, 1977) is one of the most widely used public-key cryptographic systems. It enables **secure data transmission** and **digital signatures** through the use of asymmetric key pairs — a **public key** for encryption and a **private key** for decryption.

## Theoretical Background

RSA is based on the mathematical properties of **prime numbers** and the difficulty of **integer factorization**. The core idea is that while it is easy to multiply two large primes together, it is computationally infeasible to determine those primes given only their product.

### Key Concepts

1. **Key Generation**
   - Two large prime numbers `p` and `q` are chosen.
   - Their product `n = p × q` forms part of both the public and private keys.
   - The **Euler’s totient function** is calculated as:

     ```
     φ(n) = (p - 1)(q - 1)
     ```

   - A public exponent `e` is selected such that `1 < e < φ(n)` and `gcd(e, φ(n)) = 1`.
   - The private exponent `d` is computed as the modular inverse of `e` modulo `φ(n)`:

     ```
     d × e ≡ 1 (mod φ(n))
     ```

2. **Encryption**
   - A plaintext message `M` is converted into an integer `m` such that `0 < m < n`.
   - The ciphertext `c` is computed as:

     ```
     c ≡ m^e (mod n)
     ```

3. **Decryption**
   - The original message is recovered using the private key:

     ```
     m ≡ c^d (mod n)
     ```

## Security Considerations

The security of RSA relies on the **computational hardness** of factoring large composite numbers. While modern computers can factor small numbers efficiently, factoring a 2048-bit RSA modulus is currently beyond feasible computational limits. However, potential advances in **quantum computing** (e.g., *Shor’s algorithm*) pose a future threat to RSA’s security.

## RSA + Recovery Demo

The following interactive demo illustrates the **RSA encryption process** and a **toy plaintext recovery attempt**.  
It uses small prime numbers and limited alphabets to show, in real time, how brute-force enumeration assisted by a **reference distribution** (English letter frequencies) can prioritize likely plaintext candidates.

<meta charset="utf-8" />
  <title>RSA  + Recovery Demo</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    :root {
      --bg: #f6fbff;
      --card: #ffffff;
      --accent: #2563eb;
      --muted: #6b7280;
      --danger: #b45309;
    }
    body { font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background: var(--bg); color:#0b2440; padding:28px; }
    .container { max-width:900px; margin:0 auto; }
    .card { background: var(--card); border-radius:14px; padding:20px; box-shadow: 0 8px 30px rgba(9,20,40,0.06); }
    h1 { margin:0 0 8px; font-size:20px; }
    p.lead { margin:0 0 16px; color:var(--muted); }
    label { display:block; margin-top:12px; font-weight:600; font-size:14px; color:#0b2440; }
    input[type="text"], input[type="number"] {
      width:100%; padding:10px 12px; border-radius:10px; border:1px solid #e6eefc; font-size:14px;
      box-sizing:border-box;
    }
    .controls { display:flex; gap:12px; margin-top:10px; align-items:center; flex-wrap:wrap; }
    button { margin-top:14px; padding:10px 14px; border-radius:10px; border:0; background:var(--accent); color:white; font-weight:700; cursor:pointer; }
    .small { font-size:13px; color:var(--muted); margin-top:10px; }
    .warning { color:var(--danger); font-weight:700; }
    .progress {
      margin-top:12px; height:12px; background:#e6eefc; border-radius:999px; overflow:hidden;
    }
    .progress > .bar { height:100%; width:0%; background:linear-gradient(90deg,#60a5fa,#2563eb); transition:width 200ms linear; }
    .meta { display:flex; gap:12px; margin-top:8px; font-size:13px; color:var(--muted); }
    .footer { margin-top:18px; font-size:13px; color:var(--muted); }
  </style>

<body>
  <div class="container">
    <div class="card">
      <h1>RSA Toy Encryption + Recovery Demo</h1>
      <p class="lead">
        Enter plaintext, then encrypt it with a tiny RSA key and try to recover it using a frequency-based candidate enumeration.
      </p>

      <label for="plaintext">Plaintext (ASCII only)</label>
      <input id="plaintext" type="text" value="hi" maxlength="32" />

      <!-- TOY PRIMES -->
      <div class="controls">
        <div style="flex:1">
          <label for="primeP">Toy Prime p</label>
          <input id="primeP" type="number" value="353" />
        </div>
        <div style="flex:1">
          <label for="primeQ">Toy Prime q</label>
          <input id="primeQ" type="number" value="359" />
        </div>
      </div>

      <div class="controls">
        <div style="flex:1">
          <label for="alphabet">Alphabet (candidate characters)</label>
          <input id="alphabet" type="text" value="abcdefghijklmnopqrstuvwxyz " />
        </div>
        <div style="max-width:140px;">
          <label for="maxLen">Max candidate length</label>
          <input id="maxLen" type="number" min="1" max="6" value="3" />
        </div>
        <div style="max-width:160px;">
          <label for="batchSize">Batch size</label>
          <input id="batchSize" type="number" min="100" max="200000" value="2000" />
        </div>
      </div>

      <button id="runBtn">Encrypt & Try Recover</button>

      <div class="progress" style="display:none;margin-top:12px;">
        <div class="bar" id="progressBar"></div>
      </div>

      <div class="meta">
        <div id="status">Idle</div>
        <div id="tried"></div>
        <div id="remaining"></div>
      </div>

      <div class="small">
        <p class="warning">⚠️ Important:</p>
        <ul>
          <li>This demo uses <strong>tiny primes</strong> for educational purposes only.</li>
          <li>Your plaintext converted to integer must be &lt; n = p × q.</li>
          <li>Large alphabets or lengths increase runtime exponentially.</li>
        </ul>
      </div>

      <div class="footer">If it’s too slow, lower maxLen or alphabet size.</div>
    </div>
  </div>

  <script>
  // ---------- math helpers ----------
  function egcd(a, b) {
    if (b === 0n) return [a, 1n, 0n];
    const [g, x1, y1] = egcd(b, a % b);
    return [g, y1, x1 - (a / b) * y1];
  }
  function modInv(a, m) {
    const [g, x] = egcd(a, m);
    if (g !== 1n) throw new Error('No modular inverse');
    return (x % m + m) % m;
  }
  function modPow(base, exp, mod) {
    base %= mod;
    let res = 1n;
    while (exp > 0n) {
      if (exp & 1n) res = (res * base) % mod;
      base = (base * base) % mod;
      exp >>= 1n;
    }
    return res;
  }

  // ---------- string helpers ----------
  function bigintFromString(s) {
    let x = 0n;
    for (const ch of s) {
      const code = ch.charCodeAt(0);
      if (code > 255) throw new Error('ASCII only.');
      x = (x << 8n) + BigInt(code);
    }
    return x;
  }
  function stringFromBigint(x) {
    const bytes = [];
    while (x > 0n) {
      bytes.unshift(Number(x & 0xFFn));
      x >>= 8n;
    }
    return String.fromCharCode(...bytes);
  }

  // ---------- English frequency scoring ----------
  const freq = {
    'a': 0.08167,'b':0.01492,'c':0.02782,'d':0.04253,'e':0.12702,'f':0.02228,
    'g':0.02015,'h':0.06094,'i':0.06966,'j':0.00153,'k':0.00772,'l':0.04025,
    'm':0.02406,'n':0.06749,'o':0.07507,'p':0.01929,'q':0.00095,'r':0.05987,
    's':0.06327,'t':0.09056,'u':0.02758,'v':0.00978,'w':0.02360,'x':0.00150,
    'y':0.01974,'z':0.00074,' ':0.13
  };
  function score(s){
    let sc=0;
    for(const c of s.toLowerCase()){
      sc+=Math.log(freq[c]||0.0001);
    }
    return sc;
  }

  // ---------- candidate generation ----------
  function generateCandidates(alph, maxLen, cap=500000){
    const out=[];
    const L=alph.length;
    for(let len=1; len<=maxLen; len++){
      const idx=Array(len).fill(0);
      while(true){
        out.push(idx.map(i=>alph[i]).join(''));
        let pos=len-1;
        while(pos>=0){
          idx[pos]++;
          if(idx[pos]<L) break;
          idx[pos]=0;
          pos--;
        }
        if(pos<0) break;
        if(out.length>cap) return out;
      }
    }
    return out;
  }

  // ---------- UI elements ----------
  const runBtn=document.getElementById('runBtn');
  const plaintext=document.getElementById('plaintext');
  const primeP=document.getElementById('primeP');
  const primeQ=document.getElementById('primeQ');
  const alphabet=document.getElementById('alphabet');
  const maxLen=document.getElementById('maxLen');
  const batchSize=document.getElementById('batchSize');

  const bar=document.getElementById('progressBar');
  const progress=document.querySelector('.progress');
  const statusEl=document.getElementById('status');
  const triedEl=document.getElementById('tried');
  const remEl=document.getElementById('remaining');

  runBtn.onclick=()=>{
    const p=BigInt(primeP.value);
    const q=BigInt(primeQ.value);
    const n=p*q;
    const phi=(p-1n)*(q-1n);
    const e=17n;
    let d;
    try{ d=modInv(e,phi);}catch{alert('Invalid primes (no inverse)'); return;}

    const msg=plaintext.value;
    let m;
    try{ m=bigintFromString(msg);}catch(err){alert(err.message); return;}
    if(m>=n){alert('Plaintext integer >= modulus n. Use smaller primes or shorter text.');return;}

    const c=modPow(m,e,n);
    alert(`Ciphertext (numeric):\n${c}`);

    const alph=[...new Set(alphabet.value.split(''))];
    const maxL=parseInt(maxLen.value)||3;
    const batch=parseInt(batchSize.value)||2000;

    statusEl.textContent='Generating candidates...';
    const totalEst=Math.min(1_000_000, alph.length**maxL);
    let cand=generateCandidates(alph,maxL,1_000_000);
    const scored=cand.map(s=>({s,sc:score(s)}));
    scored.sort((a,b)=>b.sc-a.sc);
    cand=null;

    progress.style.display='block';
    bar.style.width='0%';
    let tested=0;
    let found=null;
    function loop(){
      const end=Math.min(tested+batch,scored.length);
      for(let i=tested;i<end;i++){
        const s=scored[i].s;
        let big;
        try{big=bigintFromString(s);}catch{continue;}
        if(big>=n) continue;
        const enc=modPow(big,e,n);
        if(enc===c){found=s;tested=i+1;break;}
      }
      tested=end;
      const pct=Math.round(100*tested/scored.length);
      bar.style.width=pct+'%';
      triedEl.textContent='Tried: '+tested.toLocaleString();
      remEl.textContent='Remaining: '+(scored.length-tested).toLocaleString();
      if(found||tested>=scored.length){
        const plain=stringFromBigint(modPow(c,d,n));
        if(found){
          alert('Recovered plaintext:\n'+found+'\n\nVerification with private key:\n'+plain);
          statusEl.textContent='Recovered!';
        }else{
          alert('Not recovered in search.\nVerification:\n'+plain);
          statusEl.textContent='Done';
        }
        bar.style.width='100%';
        return;
      }
      setTimeout(loop,0);
    }
    setTimeout(loop,0);
  };
  </script>
</body>

# Code and explanation
![Code of the demo](./carbon.png)




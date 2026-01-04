<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<title>INFECTED'S IP LOOKUP</title>  
  
<style>  
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');  
  
:root {  
    --neon: #b026ff;  
    --neon-soft: #7a1ccf;  
}  
  
body {  
    margin: 0;  
    height: 100vh;  
    background: black;  
    overflow: hidden;  
    font-family: 'Share Tech Mono', monospace;  
    color: var(--neon);  
}  
  
/* MATRIX BACKGROUND */  
canvas {  
    position: fixed;  
    top: 0;  
    left: 0;  
    z-index: -1;  
}  
  
/* TERMINAL */  
.terminal {  
    background: rgba(0, 0, 0, 0.85);  
    border: 2px solid var(--neon);  
    border-radius: 10px;  
    box-shadow: 0 0 25px var(--neon);  
    width: 390px;  
    padding: 20px;  
    margin: auto;  
    margin-top: 10vh;  
}  
  
h2 {  
    text-align: center;  
    margin-bottom: 15px;  
    letter-spacing: 2px;  
    text-shadow: 0 0 10px var(--neon);  
}  
  
input {  
    width: 100%;  
    background: black;  
    border: 1px solid var(--neon);  
    color: var(--neon);  
    padding: 10px;  
    font-size: 16px;  
    outline: none;  
    box-shadow: inset 0 0 10px var(--neon-soft);  
}  
  
input::placeholder {  
    color: #c77dff;  
}  
  
button {  
    width: 100%;  
    margin-top: 10px;  
    padding: 10px;  
    background: black;  
    color: var(--neon);  
    border: 1px solid var(--neon);  
    font-size: 16px;  
    cursor: pointer;  
    box-shadow: 0 0 10px var(--neon-soft);  
}  
  
button:hover {  
    background: var(--neon);  
    color: black;  
    box-shadow: 0 0 20px var(--neon);  
}  
  
.result {  
    margin-top: 15px;  
    font-size: 14px;  
    line-height: 1.6;  
    white-space: pre-line;  
    text-shadow: 0 0 5px var(--neon-soft);  
}  
  
.error {  
    color: #ff4d6d;  
    margin-top: 10px;  
    text-align: center;  
    text-shadow: 0 0 8px #ff4d6d;  
}  
  
/* BLINKING CURSOR */  
.cursor {  
    display: inline-block;  
    width: 10px;  
    animation: blink 1s infinite;  
}  
  
@keyframes blink {  
    50% { opacity: 0; }  
}  
</style>  
</head>  
<body>  
  
<canvas id="matrix"></canvas>  
  
<div class="terminal">  
    <h2>INFECTED'S IP LOOKUP</h2>  
  
    <input type="text" id="ipInput" placeholder="ENTER TARGET IP">  
    <button onclick="lookupIP()">INFECT</button>  
  
    <div class="result" id="result"></div>  
    <div class="error" id="error"></div>  
</div>  
  
<script>  
/* PURPLE MATRIX EFFECT */  
const canvas = document.getElementById("matrix");  
const ctx = canvas.getContext("2d");  
  
canvas.width = window.innerWidth;  
canvas.height = window.innerHeight;  
  
const chars = "10101010100111001010";  
const fontSize = 16;  
const columns = canvas.width / fontSize;  
const drops = [];  
  
for (let i = 0; i < columns; i++) drops[i] = 1;  
  
function drawMatrix() {  
    ctx.fillStyle = "rgba(0, 0, 0, 0.07)";  
    ctx.fillRect(0, 0, canvas.width, canvas.height);  
  
    ctx.fillStyle = "#b026ff";  
    ctx.font = fontSize + "px monospace";  
  
    for (let i = 0; i < drops.length; i++) {  
        const text = chars[Math.floor(Math.random() * chars.length)];  
        ctx.fillText(text, i * fontSize, drops[i] * fontSize);  
  
        if (drops[i] * fontSize > canvas.height && Math.random() > 0.98) {  
            drops[i] = 0;  
        }  
        drops[i]++;  
    }  
}  
setInterval(drawMatrix, 33);  
  
/* IP LOOKUP */  
async function lookupIP() {  
    const ip = document.getElementById("ipInput").value.trim();  
    const result = document.getElementById("result");  
    const error = document.getElementById("error");  
  
    result.textContent = "";  
    error.textContent = "";  
  
    if (!ip) {  
        error.textContent = "NO TARGET DETECTED";  
        return;  
    }  
  
    result.textContent = "INFECTING TARGET...\nBREACHING NETWORK...\n\n";  
  
    try {  
        const res = await fetch(`https://ipapi.co/${ip}/json/`);  
        const data = await res.json();  
  
        if (data.error) {  
            error.textContent = "INFECTION FAILED";  
            return;  
        }  
  
        result.textContent +=  
`IP: ${data.ip}  
CITY: ${data.city}  
REGION: ${data.region}  
COUNTRY: ${data.country_name}  
ISP: ${data.org}  
TIMEZONE: ${data.timezone}  
LAT: ${data.latitude}  
LON: ${data.longitude}  
  
STATUS: COMPLETE ▮`;  
    } catch {  
        error.textContent = "CONNECTION LOST";  
    }  
}  
</script>  
  
</body>  
</html>  

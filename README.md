<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Outlook - Simulación</title>

<style>
body { margin:0; font-family:Segoe UI; background:#f4f4f4; }

.topbar {
    background:#0078D4;
    color:white;
    padding:12px;
    display:flex;
    justify-content:space-between;
}

.container { display:flex; height:92vh; }

.inbox {
    width:30%;
    background:white;
    border-right:1px solid #ccc;
    overflow:auto;
}

.mail {
    padding:12px;
    border-bottom:1px solid #eee;
    cursor:pointer;
}

.mail:hover { background:#f2f2f2; }

.content { width:70%; padding:20px; background:white; }

.btn {
    background:#0078D4;
    color:white;
    padding:10px 15px;
    border:none;
    cursor:pointer;
    margin-top:10px;
}

input {
    width:90%;
    padding:10px;
    margin-top:5px;
}

.msg { padding:10px; margin-top:10px; border-radius:5px; }
.error { background:#fde7e9; }
.success { background:#dff6dd; }

@media(max-width:768px){
    .container{flex-direction:column;}
    .inbox{width:100%;height:40%;}
    .content{width:100%;height:60%;overflow:auto;}
}
</style>
</head>

<body>

<div class="topbar">
    <span>📧 Outlook - Simulación</span>
    <span id="score">Errores: 0</span>
</div>

<div class="container">

<div class="inbox">
    <div class="mail" onclick="mailCEO()">🔴 CEO urgente</div>
    <div class="mail" onclick="mailTI()">🟢 Equipo TI</div>
</div>

<div class="content" id="content">
    <h3>Seleccione un correo</h3>
</div>

</div>

<script>

let errores = 0;
let currentOTP = "";

/* ===== CORREOS ===== */

function mailCEO(){
    setContent(`
    <p><b>De:</b> gerente@gmail.com</p>
    <h2>Pago urgente</h2>
    <p style="color:red;"><b>URGENTE:</b> requiere transferencia inmediata</p>
    <button class="btn" onclick="transferForm()">Responder</button>
    `);
}

function mailTI(){
    setContent(`
    <p><b>De:</b> ti@empresa.com</p>
    <h2>Mantenimiento programado</h2>
    <button class="btn" onclick="success()">Es legítimo</button>
    `);
}

/* ===== GENERAR CUENTA PERÚ ===== */

function generarCuentaPeru(){

    let bancos = [
        {nombre:"BCP", codigo:"002"},
        {nombre:"BBVA", codigo:"011"},
        {nombre:"Interbank", codigo:"003"}
    ];

    let banco = bancos[Math.floor(Math.random()*bancos.length)];

    let cuenta = Math.floor(Math.random()*100000000000)
        .toString().padStart(10,'0');

    let cci = banco.codigo +
        Math.floor(Math.random()*100000000000000000)
        .toString().padStart(17,'0');

    return {
        banco: banco.nombre,
        cuenta: cuenta,
        cci: cci
    };
}

/* ===== FORMULARIO ===== */

function transferForm(){

    let data = generarCuentaPeru();
    let referencia = "REF-" + Math.random().toString(36).substring(2,8).toUpperCase();

    setContent(`
    <h2>Transferencia urgente</h2>

    <p><b>Banco destino:</b> ${data.banco}</p>
    <p><b>Beneficiario:</b> Proveedor Externo SAC</p>

    <label>Número de cuenta</label><br>
    <input value="${data.cuenta}"><br><br>

    <label>CCI</label><br>
    <input value="${data.cci}"><br><br>

    <label>Monto (PEN)</label><br>
    <input id="monto" placeholder="Ej: 5000"><br><br>

    <label>Referencia</label><br>
    <input value="${referencia}"><br><br>

    <button class="btn" onclick="confirmTransfer()">Continuar</button>

    <div id="res"></div>
    `);
}

/* ===== CONFIRMAR → OTP ===== */

function confirmTransfer(){

    let monto = document.getElementById("monto").value;

    if(!monto){
        alert("⚠️ Ingresa monto válido");
        return;
    }

    currentOTP = Math.floor(100000 + Math.random()*900000);

    document.getElementById("res").innerHTML = `
    <div class="msg">
    Enviando código OTP...<br>
    <b>(Simulación OTP: ${currentOTP})</b>
    </div><br>

    <input id="otp" placeholder="Ingrese OTP"><br><br>

    <button class="btn" onclick="validateOTP()">Confirmar transferencia</button>
    `;
}

/* ===== VALIDAR OTP ===== */

function validateOTP(){

    let otp = document.getElementById("otp").value;

    if(otp == currentOTP){

        errores++;

        document.getElementById("res").innerHTML = `
        <div class="msg error">
        💀 FRAUDE COMPLETADO<br><br>
        Transferencia enviada a atacante.<br><br>

        Señales ignoradas:
        - Correo Gmail
        - Urgencia
        - Cambio de cuenta
        - Falta de validación
        </div>
        `;

        update();

    } else {
        document.getElementById("res").innerHTML =
        "<div class='msg error'>⚠️ OTP incorrecto</div>";
    }
}

/* ===== RESULTADOS ===== */

function success(){
    setContent(`<div class="msg success">✅ Correcto, correo legítimo</div>`);
}

function update(){
    document.getElementById("score").innerText = "Errores: " + errores;
}

/* ===== UTIL ===== */

function setContent(html){
    document.getElementById("content").innerHTML =
    html + "<br><button onclick='tips()'>🔍 Ver pistas</button>";
}

function tips(){
    alert("🚩 Señales:\n- Correo externo\n- Urgencia\n- Solicitud de dinero\n- Cuenta cambiante");
}

</script>

</body>
</html>

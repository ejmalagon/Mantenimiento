<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Disponibilidad – Salón del Reino</title>
  <p>Gracias por su disposición para colaborar en este proyecto.

Entre todos realizaremos un trabajo importante que ayudará a proteger el Salón del Reino y las edificaciones vecinas. Se abrirán zanjas para instalar tubos perforados, gravilla y mallas de geotextil, con el fin de recoger las aguas lluvias y dirigirlas a la caja recolectora principal, evitando así daños en las estructuras y paredes.

Agradecemos sinceramente su apoyo; cuando trabajamos juntos, el esfuerzo es menor y los resultados son mejores.</p>
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      padding: 20px;
    }

    h1, h2 {
      text-align: center;
    }

    .formulario {
      background: white;
      max-width: 500px;
      margin: auto;
      padding: 20px;
      border-radius: 10px;
    }

    label {
      display: block;
      margin-top: 10px;
      font-weight: bold;
    }

    input, select, textarea, button {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
    }

    button {
      margin-top: 15px;
      cursor: pointer;
    }

    table {
      width: 100%;
      margin-top: 30px;
      border-collapse: collapse;
      background: white;
    }

    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: center;
      vertical-align: top;
    }

    th {
      background: #eee;
    }
  </style>
</head>

<body>

<h1>Disponibilidad para trabajar en el Salón del Reino</h1>

<div class="formulario">
  <label>Nombre</label>
  <input type="text" id="nombre">

  <label>Día</label>
  <select id="dia">
    <option value="martes-13">Martes 13</option>
    <option value="miercoles-14">Miércoles 14</option>
    <option value="jueves-15">Jueves 15</option>
    <option value="viernes-16">Viernes 16</option>
  </select>

  <label>Turno</label>
  <select id="turno">
    <option value="mañana">Mañana</option>
    <option value="tarde">Tarde</option>
    <option value="todo">Todo el día</option>
  </select>

  <label>Herramientas que puede llevar</label>
  <textarea id="herramientas" placeholder="Ej: pala, palín, pica"></textarea>

  <button onclick="inscribirse()">Inscribirme</button>
</div>

<h2>Resumen de inscritos</h2>

<table>
  <thead>
    <tr>
      <th>Día</th>
      <th>Turno</th>
      <th>Personas inscritas</th>
    </tr>
  </thead>
  <tbody id="tablaResumen"></tbody>
</table>

<!-- 🔥 FIREBASE -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
  import { getDatabase, ref, push, onValue }
    from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBBJwB7goplMb2WJpBvJU5rsvoueD84glg",
    authDomain: "despedida-ba71a.firebaseapp.com",
    databaseURL: "https://despedida-ba71a-default-rtdb.firebaseio.com",
    projectId: "despedida-ba71a",
    storageBucket: "despedida-ba71a.firebasestorage.app",
    messagingSenderId: "30414791076",
    appId: "1:30414791076:web:08d0d7494477ca5f912131"
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  window.inscribirse = function() {
    const nombre = document.getElementById("nombre").value.trim();
    const dia = document.getElementById("dia").value;
    const turno = document.getElementById("turno").value;
    const herramientas = document.getElementById("herramientas").value.trim();

    if (!nombre) {
      alert("Por favor escribe tu nombre");
      return;
    }

    push(ref(db, `disponibilidad/${dia}/${turno}`), {
      nombre,
      herramientas
    });

    document.getElementById("nombre").value = "";
    document.getElementById("herramientas").value = "";
  };

  const tabla = document.getElementById("tablaResumen");

  onValue(ref(db, "disponibilidad"), snapshot => {
    const data = snapshot.val() || {};
    tabla.innerHTML = "";

    for (let dia in data) {
      for (let turno in data[dia]) {
        const personas = Object.values(data[dia][turno])
          .map(p => `${p.nombre}<br><small>${p.herramientas || ""}</small>`)
          .join("<hr>");

        const fila = document.createElement("tr");
        fila.innerHTML = `
          <td>${dia.replace("-", " ")}</td>
          <td>${turno}</td>
          <td>${personas}</td>
        `;
        tabla.appendChild(fila);
      }
    }
  });
</script>

</body>
</html>

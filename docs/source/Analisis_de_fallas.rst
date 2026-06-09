Unidades en analisis
===========================================

.. raw:: html

    <style>
     .units_table {
       width: 100%;
       margin: 20px auto;
       border-collapse: collapse;
       border-collapse: collapse;
       border: 2px solid #333;
     }
     .units_table th, .units_table td {
       padding: 12px;
       text-align: center;        /* centrado horizontal */
       vertical-align: middle;    /* centrado vertical */
       border: 1px solid #ddd;
       border: 2px solid #333;
     }
     .units_table th {
       background-color: #2c3e50;
       color: white;
       font-weight: bold;
     }
   </style>

   <table class="units_table" id="tabla-datos">
     <tr>
       <th>Numero_de_parte</th>
       <th>Descripcion_de_la_falla</th>
       <th>Cantidad</th>
       <th>Imagen</th>
     </tr>
   </table>

   <script>
     // --- Protección por contraseña ---
     const CONTRASENA_CORRECTA = "carlogavazzi2024";

     function verificarAcceso() {
       const acceso = sessionStorage.getItem('acceso_portal');
       if (acceso === 'ok') return;

       let intento = prompt("🔒 Portal Eléctrico — Ingresa la contraseña:");
       if (intento === null) {
         document.body.innerHTML = "<h2 style='text-align:center;margin-top:100px;'>Acceso cancelado.</h2>";
         return;
       }
       if (intento !== CONTRASENA_CORRECTA) {
         document.body.innerHTML = "<h2 style='text-align:center;margin-top:100px;color:red;'>❌ Contraseña incorrecta.</h2>";
         return;
       }
       sessionStorage.setItem('acceso_portal', 'ok');
     }

     verificarAcceso();
     // --- Fin protección ---

     const GITHUB_URL = "https://raw.githubusercontent.com/edumorcazCG/portal-electrico/main/datos.json";

     fetch(GITHUB_URL)
       .then(response => {
         if (!response.ok) throw new Error('No se pudo cargar el archivo de GitHub');
         return response.json();
       })
       .then(datos => {
         const tabla = document.getElementById('tabla-datos');
         datos.forEach(fila => {
           const tr = document.createElement('tr');
           tr.innerHTML = `
             <td>${fila.numero_de_parte || ''}</td>
             <td>${fila.descripcion_de_falla || ''}</td>
             <td>${fila.cantidad || ''}</td>
             <td><img src="_static/${fila.imagen || ''}" width="100" onerror="this.style.display='none'"></td>
           `;
           tabla.appendChild(tr);
         });
       })
       .catch(error => {
         console.error('Error al cargar datos:', error);
         const tabla = document.getElementById('tabla-datos');
         const tr = document.createElement('tr');
         tr.innerHTML = `<td colspan="4" style="color:red; padding:20px;">
           ⚠️ No se pudieron cargar los datos. Verifica que el repositorio de GitHub sea público y que el archivo datos.json exista.
         </td>`;
         tabla.appendChild(tr);
       });
   </script>
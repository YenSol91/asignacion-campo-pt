# Asignación de Campo PT

Sitio (GitHub Pages) para que los maestros de obra — dentro o fuera de Parque Tempisque —
asignen colaboradores a proyectos, casas y actividades diarias.

**Sitio publicado:** https://yensol91.github.io/asignacion-campo-pt/

Es una copia independiente del artifact de Claude "Asignación de Campo PT" — mismo diseño y
funciones (Asignar, Mapa, Resumen, Colaboradores, Proyectos/Códigos, Exportar Excel), con su
propia base de datos en Firebase Firestore. Lo que se registre aquí no aparece en el artifact,
y viceversa.

## Acceso

Se entra con **correo y contraseña**. Las cuentas las crea el administrador en Firebase; no
hay auto-registro. Sin sesión iniciada no se puede leer ni modificar ningún dato.

La `apiKey` de `firebase-config.js` no es una contraseña: identifica el proyecto y es visible
en cualquier sitio que use Firebase. La protección la dan las reglas de Firestore, que solo
permiten el acceso a usuarios con sesión iniciada.

Los datos de los colaboradores no se guardan en este repositorio: solo existen en Firestore.
Por privacidad, el sitio no guarda cédulas.

## Administrar usuarios (Firebase console → proyecto `asignacion-campo-pt`)

- **Agregar un maestro de obra:** Seguridad → Authentication → pestaña **Usuarios** →
  **Agregar usuario** → correo + contraseña temporal. Pásele el enlace del sitio, el correo y
  la contraseña; desde la pantalla de inicio puede usar "¿Olvidó su contraseña?" para ponerse
  una propia.
- **Quitar acceso:** en la misma lista, menú ⋮ del usuario → **Inhabilitar cuenta** (o
  Borrar).

## Configuración de seguridad (una sola vez)

1. **Authentication → Comenzar → Método de acceso → Correo electrónico/contraseña** →
   habilitar (solo la primera opción) → **Guardar**.
2. **Authentication → Configuración → Acciones del usuario** → desmarcar **Habilitar
   creación (registro)** → **Guardar**. Así nadie puede crearse una cuenta por su cuenta.
3. **Firestore Database → Reglas** → reemplazar por lo siguiente y **Publicar**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

## Estructura del repositorio

- `index.html` — la aplicación completa (una sola página) con pantalla de inicio de sesión.
- `firebase-config.js` — configuración pública del proyecto de Firebase.
- `favicon.svg`, `favicon.ico`, `apple-touch-icon.png` — íconos.
- `img/` — planos de La Pampa y Los Parques usados en la pestaña Mapa.

## Notas técnicas

- Firebase SDK compat v10 (App, Firestore, Auth). Colecciones: `colaboradores`, `proyectos`,
  `codigos_presupuestarios`, `asignaciones`.
- "Exportar Excel" se genera en el navegador con ExcelJS (CDN).
- Sin build ni dependencias: HTML/JS plano servido por GitHub Pages.

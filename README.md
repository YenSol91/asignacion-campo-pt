# Asignación de Campo PT

Sitio público (GitHub Pages) para que cualquier persona con el enlace — dentro o fuera de
Parque Tempisque — pueda asignar colaboradores a proyectos, casas y actividades diarias.

**Sitio publicado:** https://yensol91.github.io/asignacion-campo-pt/

Es una copia independiente del artifact de Claude "Asignación de Campo PT" — mismo diseño y
funciones (Asignar, Mapa, Resumen, Colaboradores, Proyectos/Códigos, Exportar Excel), pero con
su propia base de datos (Firebase Firestore) en vez de la base de datos del artifact. Los dos
sistemas quedan separados a propósito: lo que se registre aquí no aparece en el artifact de
Claude, y viceversa.

## ⚠️ Aviso de seguridad

Este sitio está configurado para que **cualquier persona con el enlace pueda leer y escribir
datos sin necesidad de iniciar sesión** (así se pidió explícitamente). Eso significa que
cualquiera que obtenga la URL podría modificar o borrar colaboradores, proyectos y
asignaciones, sin quedar registrado quién lo hizo. Si en algún momento se quiere más control,
la opción más simple es agregar una regla de Firestore que exija un código compartido, o pasar
a autenticación real (Firebase Auth) — puedo ayudar a hacerlo cuando se decida.

## Configuración inicial (una sola vez)

### 1. Crear el proyecto de Firebase

1. Entra a https://console.firebase.google.com con tu cuenta de Google.
2. **Agregar proyecto** → nómbralo, por ejemplo, `asignacion-campo-pt` → sigue el asistente
   (puedes desactivar Google Analytics, no se necesita).
3. Dentro del proyecto, en el menú izquierdo: **Compilación → Firestore Database** →
   **Crear base de datos** → elige **Modo de producción** → selecciona la región más cercana
   (`us-central` o `southamerica-east1` están bien) → **Habilitar**.

### 2. Configurar las reglas de acceso (abierto, sin login)

En Firestore Database → pestaña **Reglas**, reemplaza todo el contenido por:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

Clic en **Publicar**.

### 3. Registrar la app web y obtener la configuración

1. En el ícono de engranaje (⚙️) junto a "Descripción general del proyecto" → **Configuración
   del proyecto**.
2. Sección **Tus apps** → ícono `</>` (Web) → nombra la app (ej. "Sitio público") →
   **Registrar app** (no hace falta el hosting de Firebase, solo el registro).
3. Copia el objeto `firebaseConfig` que te muestra (apiKey, authDomain, projectId, etc.).

### 4. Completar `firebase-config.js`

Abre `firebase-config.js` en este repositorio y reemplaza cada valor `"PEGA_..."` con los
datos reales que copiaste. Guarda y sube el cambio (`git add`, `git commit`, `git push`, o
edítalo directamente en GitHub desde el navegador).

### 5. Cargar los datos iniciales

Abre `https://yensol91.github.io/asignacion-campo-pt/migrate.html` y presiona el botón. Carga
una sola vez los ~410 colaboradores, 68 proyectos y 103 códigos presupuestarios que ya existen
en el artifact de Claude, para no tener que digitarlos de nuevo. Se puede correr más de una vez
sin duplicar nada (sobrescribe los mismos documentos), y no borra asignaciones diarias ya
registradas.

### 6. Listo

Abre `https://yensol91.github.io/asignacion-campo-pt/` — ya debería mostrar "Conectado · datos
en vivo" y funcionar igual que el artifact de Claude.

## Estructura del repositorio

- `index.html` — la aplicación completa (una sola página).
- `firebase-config.js` — configuración de tu proyecto de Firebase (edítala tú).
- `migrate.html` + `seed-data.json` — herramienta de carga inicial de datos (opcional borrarla
  después de usarla).
- `img/` — planos de La Pampa y Los Parques usados en la pestaña Mapa.

## Notas técnicas

- Usa Firebase Firestore (SDK compat v10) con la misma forma de colecciones que el artifact de
  Claude: `colaboradores`, `proyectos`, `codigos_presupuestarios`, `asignaciones`.
- El botón "Exportar Excel" genera el archivo en el propio navegador (librería ExcelJS vía
  CDN) y lo descarga directo — no depende de ningún backend.
- No hay build ni dependencias que instalar: es HTML/JS plano servido tal cual por GitHub
  Pages.

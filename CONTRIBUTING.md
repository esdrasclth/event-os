# Contribuir a EventOS

El código es visible pero **no es de uso libre**: la licencia es propietaria y
las contribuciones se ceden a su titular (ver [`LICENSE`](LICENSE)). Esta guía
es para quien vaya a proponer un cambio.

---

## Entorno

Requisitos: **Node 20+**.

```bash
npm install
npm run dev      # http://localhost:5173
```

Para trabajar sin Firebase, en `src/data/seed.ts`:

```ts
export const USE_MOCK_DATA = true;
```

Con eso `src/services/firebase.ts` devuelve las cinco órdenes de ejemplo en
memoria y no toca la base.

**El interruptor no cubre la autenticación.** `AuthContext` sigue llamando a
Firebase Auth aunque los datos sean ficticios, así que para recorrer la app sin
cuenta hay que saltarse también ese contexto. Es una arista conocida del modo
de desarrollo; si la resuelves, que sea detrás del mismo interruptor y nunca
activada por defecto.

Para trabajar contra Firebase, copia `.env.example` a `.env.local` y rellénalo.
`.env.local` está en `.gitignore` y ahí se queda.

---

## Cómo está hecho

- **Móvil primero.** La app se usa de pie, en el terreno y con una mano: la
  navegación es una barra inferior y los objetivos táctiles son grandes. Una
  pantalla nueva se piensa a 414 px de ancho, no a 1440.
- **Funciona sin conexión.** Es una PWA con Workbox. Un cambio que dependa de
  estar en línea para mostrar algo que ya se descargó rompe la premisa del
  producto.
- **El estado vive en contextos**, uno por dominio: `AuthContext`,
  `OrdenesContext`, `ProductosContext`, `UsersContext`. Los datos se piden en
  `services/firebase.ts`, no desde los componentes.
- **Los roles son `admin`, `staff` y `delivery`,** y `ProtectedRoute` exige
  además `activo`. Un usuario sin perfil o desactivado ve la pantalla de
  «Sin acceso», no una versión reducida.
- **La actividad es auditoría.** Cada creación y modificación registra quién y
  cuándo. No la conviertas en un campo opcional.
- **CSS Modules y variables CSS.** Los colores salen de `styles/variables.css`,
  no de valores sueltos en el componente.

---

## Antes del pull request

```bash
npm run lint
npm run build
```

Y prueba en un viewport de teléfono, no solo en el escritorio.

---

## Estilo

- **Español** en la interfaz, los comentarios y los mensajes de commit. El
  vocabulario es el del negocio: `orden`, `carga del día`, `retiro`.
- **Conventional Commits**: `feat:`, `fix:`, `refactor:`, `docs:`.
- Comenta el **porqué**, no el qué.

---

## Pull requests

1. Rama descriptiva: `feat/carga-por-camion`, `fix/estado-retirado`.
2. Un pull request, un tema.
3. En la descripción: qué problema resuelve y cómo lo probaste.
4. Nunca subas `.env.local` ni capturas con clientes reales: las de este
   repositorio salen del modo de datos ficticios.

---

## Seguridad

Una vulnerabilidad no se reporta en un issue público. Escribe a
<Esdras.Clother@outlook.com> con los pasos para reproducirla.

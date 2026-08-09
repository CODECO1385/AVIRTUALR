# El Tequila — Demo de Menú en Realidad Aumentada

Demo funcional (MVP) de un menú de restaurante con visualización de
platillos en realidad aumentada, construida con **HTML5 + CSS3 +
JavaScript vanilla**, sin frameworks, sin backend y sin base de datos.

El usuario abre el sitio desde su celular, explora el menú de **El
Tequila**, selecciona un platillo, presiona **"Ver en AR"** y ve la
fotografía del platillo superpuesta en tiempo real sobre la cámara
de su teléfono, con controles para moverlo, escalarlo, rotarlo,
cambiar de platillo, tomar una foto y compartirla.

---

## 1. Qué es este proyecto

Es un **prototipo de demostración** para mostrarle a un restaurante
cómo se sentiría un menú con realidad aumentada. No es un producto
de producción: no incluye login, backend, base de datos ni servicios
en la nube. Todo corre localmente en el navegador.

La "realidad aumentada" de esta demo es una **simulación
convincente**: superpone una fotografía 2D sobre el video en vivo de
la cámara, con sombra, animación de aparición y gestos táctiles. No
realiza detección real de superficies 3D (ver sección 9).

---

## 2. Estructura del proyecto

```
el-tequila-ar-demo/
│
├── index.html
├── README.md
│
├── css/
│   └── styles.css
│
├── js/
│   └── app.js
│
└── assets/
    ├── logo/
    │   └── README.txt
    │
    └── dishes/
        ├── dish-01.jpg   (agregar)
        ├── dish-02.jpg   (agregar)
        ├── dish-03.jpg   (agregar)
        ├── dish-04.jpg   (agregar)
        ├── dish-05.jpg   (agregar)
        ├── dish-06.jpg   (agregar)
        └── README.txt
```

---

## 3. Cómo instalarlo

No requiere instalación de dependencias ni `npm install`. Solo
necesitas los archivos del proyecto y un servidor local simple.

Clona o descarga la carpeta `el-tequila-ar-demo/` tal cual.

---

## 4. Cómo ejecutarlo

**La cámara del navegador requiere HTTPS o `localhost`.** Abrir
`index.html` directamente con doble clic (protocolo `file://`) **no
permitirá usar la cámara** — la app lo detecta y te lo explica en
pantalla, pero igual necesitas un servidor local para probarlo bien.

### Opción A — Python (ya instalado en la mayoría de sistemas)
```bash
cd el-tequila-ar-demo
python3 -m http.server 8000
```
Abre: `http://localhost:8000`

### Opción B — Node.js (paquete `serve`)
```bash
cd el-tequila-ar-demo
npx serve .
```

### Opción C — Extensión "Live Server" de VS Code
Clic derecho sobre `index.html` → **"Open with Live Server"**.

---

## 5. Cómo agregar las fotografías

1. Consigue 6 fotografías de los platillos (formato JPG recomendado).
2. Colócalas dentro de `assets/dishes/` con estos nombres exactos:

```
assets/dishes/dish-01.jpg   → Enchiladas
assets/dishes/dish-02.jpg   → Tacos
assets/dishes/dish-03.jpg   → Fajitas
assets/dishes/dish-04.jpg   → Carne Asada
assets/dishes/dish-05.jpg   → Enchiladas Suizas
assets/dishes/dish-06.jpg   → Quesadillas
```

3. Recarga el sitio. No necesitas tocar ni una línea de código: la
   app las detecta automáticamente. Si una imagen todavía no existe,
   la app muestra un aviso elegante en su lugar, sin romperse.

---

## 6. Cómo cambiar nombres, descripciones y precios

Todo el contenido de los platillos vive en un solo lugar:
`js/app.js`, dentro del arreglo `dishes` (sección 1 del archivo,
al inicio). Cada platillo es un objeto simple:

```js
{
  id: 1,
  name: 'Enchiladas',
  description: 'Tortillas de maíz rellenas de pollo...',
  price: '$12.99',
  category: 'Platillo fuerte',
  image: 'assets/dishes/dish-01.jpg'
}
```

Edita `name`, `description`, `price` o `image` y guarda: el menú, el
detalle y la experiencia AR se actualizan solos.

---

## 7. Cómo probarlo desde un teléfono

La cámara solo funciona sobre HTTPS o `localhost`, así que para
probar desde tu celular en la misma red Wi-Fi necesitas exponer tu
servidor local con HTTPS. Dos formas sencillas:

**Con ngrok** (recomendado, genera HTTPS automáticamente):
```bash
# con el servidor local corriendo en el puerto 8000
ngrok http 8000
```
Abre en tu celular la URL `https://...ngrok-free.app` que te entregue.

**Con la IP local (solo funciona en algunos navegadores/Android
sin HTTPS, con más restricciones):**
```bash
python3 -m http.server 8000
```
Luego en el celular (misma red Wi-Fi): `http://TU_IP_LOCAL:8000`.
Nota: muchos navegadores móviles bloquean la cámara sin HTTPS, por
lo que `ngrok` (u otro túnel HTTPS) es la vía más confiable.

---

## 8. Cómo habilitar HTTPS local si es necesario

Si prefieres HTTPS 100% local (sin túneles), puedes usar `mkcert`:

```bash
mkcert -install
mkcert localhost 127.0.0.1
```

Y servir los archivos con cualquier servidor estático que soporte
certificados TLS (por ejemplo `http-server -S -C localhost.pem -K
localhost-key.pem`). Para la demo, `ngrok` suele ser más rápido de
configurar.

---

## 9. Qué navegadores son compatibles

| Navegador                  | Cámara AR | Captura | Compartir (Web Share) |
|-----------------------------|:---------:|:-------:|:----------------------:|
| Chrome (Android)            | ✅        | ✅      | ✅                      |
| Safari (iOS 14.3+)          | ✅        | ✅      | ✅                      |
| Chrome / Edge (desktop)     | ✅ / demo | ✅      | ⚠️ variable            |
| Firefox (desktop / Android) | ✅        | ✅      | ⚠️ variable            |

En **desktop sin cámara**, la app entra automáticamente en un **modo
demo**: puedes seguir moviendo, escalando y rotando el platillo
sobre un fondo simulado para probar la interacción.

---

## 10. Qué funciones son una simulación

Para ser transparentes sobre el alcance de este MVP:

- **No hay detección real de superficies 3D.** La app no reconoce
  mesas, suelos ni geometría del entorno; superpone la fotografía
  del platillo en el centro de la pantalla y el usuario la posiciona
  manualmente con gestos.
- **El platillo es una imagen 2D**, no un modelo 3D. Se le aplican
  sombra, escala y rotación en 2D para simular profundidad.
- **El código QR es un marcador visual**, no genera ni escanea
  códigos reales.
- **La captura de foto** combina el frame actual del video con la
  imagen superpuesta mediante `<canvas>`; en algunos navegadores con
  restricciones de seguridad esta función puede fallar, y la app lo
  informa en pantalla en vez de romperse.

---

## 11. Qué se agregaría para convertirlo en AR real

Roadmap sugerido para una fase posterior:

1. **Modelos 3D reales (.glb / .gltf)** en lugar de fotografías 2D,
   renderizados con **Three.js** o directamente con **WebXR**
   (`navigator.xr`), usando detección de planos (`hit-test`) para
   anclar el modelo a una superficie real. El archivo `js/app.js` ya
   deja marcados los puntos de integración con el comentario
   `INTEGRACIÓN 3D`, donde hoy se aplican `x/y/scale/rotation` a la
   imagen 2D.
2. **Anclaje persistente** del objeto en el espacio (WebXR Anchors),
   para que el platillo no se mueva si el usuario mueve el celular.
3. **Panel de administración** para que el restaurante suba fotos y
   edite precios sin tocar código (requeriría backend + base de
   datos + autenticación, explícitamente fuera del alcance de esta
   demo).
4. **Generación real del código QR** apuntando a la URL pública del
   menú.
5. **Analítica de uso** (qué platillos se visualizan más en AR).

---

## 12. Flujo de la demo

```
EL TEQUILA
   ↓
VER MENÚ
   ↓
Seleccionar un platillo (p. ej. Enchiladas)
   ↓
VER EN AR
   ↓
Permitir acceso a la cámara
   ↓
Cámara trasera se activa
   ↓
El platillo aparece sobre la cámara
   ↓
Mover / Rotar / Escalar
   ↓
Cambiar platillo (sin cerrar la cámara)
   ↓
Tomar foto
   ↓
Compartir
   ↓
Cerrar AR (la cámara se detiene por completo)
```

---

## 13. Limitaciones conocidas

- Requiere HTTPS o `localhost` para la cámara (limitación de los
  navegadores, no de esta app).
- `navigator.share()` con archivos no está disponible en todos los
  navegadores de escritorio; en esos casos la app ofrece descarga
  directa de la captura como alternativa.
- Sin fotografías reales en `assets/dishes/`, el menú y la AR
  muestran un aviso de "foto pendiente" en vez de romperse — esto es
  intencional, no un error.

# Parque Collico — Mapa Interactivo
### COLLICO 01 · Valdivia · ARAUCO Bosque Abierto

> ⚠️ **VERSIÓN PILOTO** — Primera versión funcional de demostración. Datos y funcionalidades sujetos a validación en terreno antes del despliegue oficial.

---

## Descripción

Aplicación web móvil para navegación de senderos del **Parque Collico, Valdivia**. Funciona desde cualquier teléfono sin necesidad de instalar una app. Sin backend — todo corre en el browser del visitante.

**Stack:** HTML · CSS · JavaScript · MapLibre GL JS  
**Hosting:** GitHub Pages  
**URL:** `https://horta30.github.io/COLLICO-01/`

---

## Estructura

```
COLLICO-01/
├── index.html            → Redirect a landing
├── landing.html          → Presentación del parque + CTA
├── registro.html         → Opt-in visitante + selección de actividad
├── mapa.html             → Mapa interactivo con GPS y notificaciones
└── assets/
    └── senderos.js       → Fuente única de verdad
```

### Flujo de usuario

```
landing.html  →  registro.html  →  mapa.html
```

El registro activa el GPS automáticamente al completarse. El visitante entra al mapa con su posición en tiempo real y las notificaciones de proximidad activas.

---

## Senderos — Piloto

Trazados procesados desde KMZ depurado por Valdivia Trail. Perfiles altimétricos validados manualmente.

| Sendero | Distancia | D+ | D+/km | Dificultad |
|---|---|---|---|---|
| 💧 La Cascada | 469 m | +58 m | 124 m/km | Difícil |
| 🏃 Vietnam | 788 m | +84 m | 107 m/km | Difícil |
| 🔭 Mirador Kunstmann | 944 m | +88 m | 93 m/km | Moderado |
| 🌿 Collico 1 | 6.30 km | +438 m | 70 m/km | Moderado |
| 🐆 Mirador del Puma | 6.99 km | +416 m | 60 m/km | Moderado |

**Red total piloto: 15.4 km · 5 senderos · Trekking + Trail Running**

---

## Criterio de dificultad

La dificultad se asigna usando el estándar internacional de trail running basado en la métrica **D+/km** — desnivel positivo acumulado dividido por la distancia total del sendero.

Esta métrica permite comparar senderos de distancias muy distintas en igualdad de condiciones. Un sendero corto puede ser más exigente que uno largo: La Cascada (469m, +58m) tiene D+/km de 124, más intenso que Collico 1 (6.3km, +438m) con D+/km de 70. La distancia total sin considerar desnivel por km lleva a error de percepción para el visitante.

### Tabla de referencia

| Nivel | D+/km | Descripción |
|---|---|---|
| **Fácil** | < 30 m/km | Terreno plano, sin pendiente significativa |
| **Moderado** | 30–90 m/km | Subidas progresivas, recuperación posible |
| **Difícil** | 90–120 m/km | Pendiente sostenida, exige condición física |
| **Muy Difícil** | > 120 m/km | Técnico, desnivel continuo y pronunciado |

> Criterio basado en estándares ITRA (International Trail Running Association) y federaciones de montaña europeas.

---

## Funcionalidades

### ✅ Piloto actual

- Mapa vectorial con MapLibre GL JS sobre base OpenStreetMap
- 5 senderos con trazado GPS real (1,701 puntos totales)
- Camino de ripio de acceso marcado
- Tótem de acceso georreferenciado
- Marcadores de inicio y fin por sendero
- Panel de sendero con datos técnicos al tocarlo
- Sistema de capas por actividad — Trekking / Trail Running activos
- Capa MTB visible con estado **"Próximamente"**
- GPS en tiempo real con punto pulsante en el mapa
- Geofencing de proximidad — radio 80m por inicio y fin de cada sendero
- Notificación en pantalla + vibración al entrar en zona de sendero
- Registro de senderos visitados en localStorage
- Deep link QR por sendero y por actividad
- Opt-in de registro con nombre, email, teléfono y actividad seleccionada

### 🔜 Pendiente

- [ ] KMZ MTB depurado → activar capa Mountain Bike
- [ ] Validación de geofences en terreno (radio 80m es valor inicial)
- [ ] Validación de descripciones de senderos con equipo en terreno
- [ ] Video para landing page
- [ ] Tile de mapa personalizado para el parque

---

## Fuente de datos — senderos.js

`assets/senderos.js` es la fuente única de verdad. Contiene:

```javascript
COLLICO_CONFIG    // Configuración general del parque
SENDEROS[]        // Array con los senderos
RIPIO_ACCESO[]    // Coordenadas del camino de acceso
```

Cada objeto en `SENDEROS`:

```javascript
{
  id, nombre, emoji, color,
  dificultad,          // Fácil / Moderado / Difícil / Muy Difícil
  descripcion,
  dist_km,             // Distancia real del trazado
  elevacion_min,       // Altitud mínima (m)
  elevacion_max,       // Altitud máxima (m)
  desnivel_pos,        // D+ acumulado (m)
  desnivel_neg,        // D- acumulado (m)
  pendiente_max,       // Pendiente máxima (%)
  dpkm,                // D+/km — base del criterio de dificultad
  actividades[],       // ['trekking', 'trail_running']
  geofence: {
    inicio: { lng, lat, radio },
    fin:    { lng, lat, radio }
  },
  coords[][]           // Coordenadas del trazado [lng, lat]
}
```

Para agregar un sendero: añadir un objeto al array `SENDEROS`. El mapa lo renderiza automáticamente.

---

## Deploy

```bash
# 1. Crear repo en GitHub
#    github.com/horta30/COLLICO-01

# 2. Subir archivos
git clone https://github.com/horta30/COLLICO-01
# copiar contenido del zip
git add .
git commit -m "COLLICO 01 — piloto inicial"
git push origin main

# 3. Activar GitHub Pages
#    Settings → Pages → Source: main / root
```

### Deep links

```
/mapa.html?actividad=trekking          → Capa trekking activa
/mapa.html?actividad=trail_running     → Capa trail activa
/mapa.html?trail=cascada               → Abre panel La Cascada
/mapa.html?trail=vietnam               → Abre panel Vietnam
/mapa.html?trail=mirador_kunstmann     → Abre panel Mirador Kunstmann
/mapa.html?trail=collico1              → Abre panel Collico 1
/mapa.html?trail=mirador_puma          → Abre panel Mirador del Puma
```

---

## Notas del piloto

1. Los radios de geofencing (80m) son valores iniciales. Requieren validación en terreno con GPS.
2. Las descripciones de senderos son textos preliminares pendientes de revisión en terreno.
3. Los datos de elevación fueron obtenidos desde perfiles altimétricos externos — el KMZ original tiene altitud = 0 en todos los puntos.
4. La capa MTB está estructurada en el código. Solo requiere el KMZ depurado para activarse.

---

## Créditos

| Rol | |
|---|---|
| Cliente | ARAUCO |
| Programa | ARAUCO Bosque Abierto |
| Datos KMZ | Valdivia Trail |
| Desarrollo | Gravitas Solutions |
| Versión | COLLICO 01 — Piloto · Abril 2026 |

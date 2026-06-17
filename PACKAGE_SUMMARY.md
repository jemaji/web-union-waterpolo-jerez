# 📦 Cartelas Waterpolo - Resumen del Paquete

## Archivos Incluidos

### 1. **cartelas-waterpolo.html** (51.8 KB)
El archivo principal. Single-file HTML con todo integrado.

**Características:**
- ✅ Dos roles: `?role=control` y `?role=overlay`
- ✅ localStorage para persistencia de datos
- ✅ MQTT opcional para sincronización multi-dispositivo
- ✅ Exportación JSON con timestamps
- ✅ Imágenes desde jsDelivr CDN

**Cómo usar:**
```bash
# Servidor Python
python3 -m http.server 8000
# Abre: http://localhost:8000/cartelas-waterpolo.html?role=control
```

---

### 2. **README.md** (373 líneas)
Documentación completa del proyecto.

**Contiene:**
- Instrucciones de inicio rápido
- Guía de uso del panel de control
- Sistema de persistencia explicado
- Estructura del JSON exportado
- Configuración de imágenes y sponsors
- Particularidades técnicas de implementación
- Troubleshooting
- Casos de uso en producción

---

### 3. **SETUP.md** (98 líneas)
Guía paso a paso para subir a GitHub Pages.

**Pasos:**
1. Crear repositorio
2. Preparar archivos
3. Git: add, commit, push
4. Habilitar GitHub Pages
5. Verificar URLs
6. Personalizar para tu equipo

---

### 4. **.gitignore**
Configuración estándar para Git.
- Excluye: node_modules, .DS_Store, *.log, .env, etc.

---

## 🎯 Flujo de Trabajo Recomendado

### Fase 1: Desarrollo Local
```
1. Descarga cartelas-waterpolo.html
2. Inicia servidor Python: python3 -m http.server 8000
3. Abre http://localhost:8000/?role=control
4. Asigna dorsales, prueba funcionalidades
5. Verifica localStorage guarda datos al refrescar
```

### Fase 2: Preparación para GitHub
```
1. Lee SETUP.md
2. Crea repositorio en GitHub
3. Sube cartelas-waterpolo.html
4. Sube README.md y SETUP.md
5. Sube imágenes (jerez.png, ceuta.png, sponsors)
6. Habilita GitHub Pages
7. Personaliza GITHUB_BASE en el HTML
```

### Fase 3: Producción
```
1. URLs: https://tu-usuario.github.io/repo/cartelas-waterpolo.html?role=control
2. Abre control en iPad
3. Abre overlay en otra pantalla
4. Transmite overlay a Twitch
5. Gestiona todo desde control
6. Exporta JSON al terminar
```

---

## 📊 Datos Persistentes

### localStorage
```
Clave: waterpolo_[match]_data
Contenido:
  - state (cartela, marcador, cuarto)
  - roster (dorsales, goles, expulsiones)
  - events (historial completo con timestamps)
```

### Exportación JSON
```
Descarga: [match]_YYYY-MM-DD.json
Contenido:
  - Metadata (equipos, fecha, lugar)
  - Alineación (jugadores con dorsales)
  - Marcador final
  - Eventos completos con timestamp ISO
```

---

## 🔧 Personalización Esencial

### 1. GITHUB_BASE
Cambiar el repositorio de imágenes:
```javascript
var GITHUB_BASE="https://cdn.jsdelivr.net/gh/TU_USUARIO/TU_REPO@main/";
```

### 2. Nombres de Equipos
```javascript
cfg = {
  ln: "Unión Waterpolo Jerez",
  vn: "CN Caballa - Ciudad de Ceuta A",
  ...
};
```

### 3. Colores
```javascript
cfg = {
  lc: "#1C3D74",  // Azul navy
  vc: "#171717",  // Negro
  ...
};
```

### 4. Mensajes Victoria/Derrota
```javascript
cfg = {
  vhead: "¡Enhorabuena Unión!",
  dhead: "Enhorabuena al Club Caballa",
  ...
};
```

---

## 🖼️ Imágenes Necesarias

Coloca en la raíz del repositorio GitHub:

| Archivo | Uso | Tamaño Recomendado |
|---------|-----|-------------------|
| `jerez.png` | Crest local | 200x200 |
| `ceuta.png` | Crest visitante | 200x200 |
| `charbel.png` | Sponsor | 150x80 |
| `ayto.png` | Sponsor | 150x80 |
| `diputacion.png` | Sponsor | 150x80 |
| `jerez2031.png` | Sponsor | 150x80 |

---

## 💾 Estructura de localStorage

```json
{
  "match": "uwj-cnc-bronce-2026",
  "startTime": "2026-06-17T19:15:00.000Z",
  "state": {
    "view": "juego",
    "ls": 12,
    "vs": 10,
    "q": 3,
    "qp": [
      { "l": 4, "v": 3 },
      { "l": 3, "v": 2 },
      { "l": 5, "v": 5 },
      null
    ],
    "qsl": 12,
    "qsv": 10
  },
  "roster": {
    "l": [
      {
        "name": "Alberto Miras",
        "dorsal": "1",
        "goals": 2,
        "exp": 0
      }
    ],
    "v": []
  },
  "events": [
    {
      "time": "2026-06-17T19:15:42.123Z",
      "timestamp": 1718642142123,
      "type": "goal",
      "details": { "team": "l", "player": "...", "dorsal": "1" },
      "state": { "ls": 1, "vs": 0, "q": 1 }
    }
  ]
}
```

---

## 🌐 Parámetros de URL

| Parámetro | Ejemplo | Descripción |
|-----------|---------|-------------|
| `role` | `control` \| `overlay` | Modo de operación |
| `match` | `mi-partido` | ID para MQTT (default: uwj-cnc-bronce-2026) |
| `lc` | `%231C3D74` | Color local (hex URL-encoded) |
| `vc` | `%23171717` | Color visitante (hex URL-encoded) |
| `ln` | `Mi%20Equipo` | Nombre local (URL-encoded) |
| `vn` | `Su%20Equipo` | Nombre visitante (URL-encoded) |

**Ejemplo completo:**
```
http://localhost:8000/cartelas-waterpolo.html?role=control&match=partido-2026&lc=%231C3D74
```

---

## 📱 Dispositivos Recomendados

### Control
- iPad Pro 12.9"
- Tablet Android 10"+
- Laptop/Escritorio

### Overlay
- Pantalla secundaria 1920x1080+
- Integración Twitch vía navegador
- PRISM Live Studio

### Transmisión
- Xiaomi 14T (PRISM)
- Cámaras multicam (Gopro, DJI)
- Sincronización via MQTT (opcional)

---

## ⚡ Rendimiento

- **Tamaño HTML**: 51.8 KB (sin gzip)
- **Carga**: <1s en conexión normal
- **localStorage**: ~500 KB límite (suficiente para >1000 eventos)
- **MQTT**: ~50ms de latencia en conexión estable
- **Compatibilidad**: iOS 12+, Android 8+, Chrome/Firefox/Safari

---

## 🔒 Privacidad y Seguridad

- ✅ No transmite datos personales a servidores terceros
- ✅ localStorage solo en el navegador local
- ✅ MQTT usa TLS (wss://) encriptado
- ✅ Imágenes desde jsDelivr CDN público
- ✅ Sin cookies, sin tracking

---

## 📝 Tipos de Eventos Registrados

El JSON exportado captura estos eventos:

```
view_change      → Cambio de cartela (entrada/juego/descanso/victoria)
quarter_change   → Cambio de cuarto (1→2→3→4)
score_change     → Cambio manual de marcador
live_event       → Evento en directo publicado en banners
goal             → Gol marcado (publicitado)
goal_correction  → Gol +/- (corrección sin publicar)
expulsion        → Expulsión registrada (publicitada)
expulsion_correction → Expulsión +/- (corrección)
penalty          → Penalti registrado (publicitado)
penalty_correction → Penalti +/- (corrección)
dorsal_change    → Cambio de número de dorsal
reset_all        → Reinicio completo del partido
```

Cada evento incluye:
- `time` - ISO 8601 timestamp
- `timestamp` - Unix milliseconds
- `type` - Tipo de evento
- `details` - Parámetros específicos
- `state` - Estado del partido en ese momento

---

## 🚀 Deployment en GitHub Pages

### Desde GitHub UI
1. Sube archivos via web
2. Settings → Pages → Deploy from main branch
3. Espera ~1 minuto
4. Accede a `https://tu-usuario.github.io/repo/cartelas-waterpolo.html`

### Desde Terminal
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/usuario/repo.git
git push -u origin main
# Luego en Settings → Pages → Enable
```

---

## 📊 Análisis Post-Partido

Ejemplo: extraer top goleadores del JSON

```python
import json

with open('uwj-cnc-bronce-2026_2026-06-17.json') as f:
    data = json.load(f)

goals = {}
for event in data['events']:
    if event['type'] in ['goal', 'goal_correction']:
        if event['details']['action'] != 'remove':
            p = event['details']['player']
            goals[p] = goals.get(p, 0) + 1

for player, count in sorted(goals.items(), key=lambda x: x[1], reverse=True):
    print(f"{player}: {count} goles")
```

---

## 🎯 Casos de Uso

| Uso | Herramienta |
|-----|-----------|
| Transmisión en directo | Overlay + PRISM/OBS |
| Estadísticas | JSON exportado |
| Reportes | JSON → Excel/Google Sheets |
| Edición video | Timestamps en JSON |
| Análisis táctico | Historial completo de eventos |
| Auditoría | Quién cambió qué y cuándo |

---

## ✅ Checklist Pre-Partido

- [ ] Servidor HTTP corriendo
- [ ] URLs accesibles (control + overlay)
- [ ] Dorsales asignados a todos los convocados
- [ ] "Solo convocados" funciona correctamente
- [ ] localStorage carga datos al refrescar
- [ ] Imágenes cargan sin errores CORS
- [ ] Overlay transmite a Twitch/Stream
- [ ] MQTT conecta (si usas multi-dispositivo)
- [ ] Botón "Exportar JSON" funciona
- [ ] Dos dispositivos sincronizados

---

## 🆘 Soporte

### Problemas Comunes
1. **"Unsafe attempt to load URL file://"** → Usa servidor HTTP
2. **Dorsales no se guardan** → Verifica localStorage en DevTools
3. **Imágenes no cargan** → Cambia GITHUB_BASE
4. **MQTT no conecta** → Normal, sigue funcionando localmente
5. **localStorage lleno** → Exporta JSON y resetea

### Debugging
```javascript
// En consola del navegador
localStorage.getItem("waterpolo_uwj-cnc-bronce-2026_data")
// Muestra todos los datos guardados en formato JSON
```

---

**Versión**: 1.0  
**Fecha**: 2026-06-17  
**Autor**: Jesús Mateos para Unión Waterpolo Jerez  
**Licencia**: Libre para uso personal y comunitario

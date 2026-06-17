# Cartelas Waterpolo - Sistema de Transmisión en Directo

Sistema de gestión de gráficas en tiempo real para transmisiones de partidos de waterpolo. Genera cartelas profesionales (entrada, descanso, juego, victoria/derrota) con control de marcador, roster de jugadores, dorsales y registro completo de eventos.

## 📋 Características

### Vistas/Cartelas
- **🎬 Entrada** - Animación inicial con crests, "¡Vamos por el BRONCE!" y sponsor strip
- **🏊 Juego** - Scorebug transparente + banners de eventos en directo
- **📊 Descanso** - Marcador grande, cuartos, parciales, rosters lado a lado
- **🥇 Victoria/Derrota** - Pantalla final honrando al ganador

### Gestión de Partido
- Marcador manual (±)
- 4 cuartos con parciales por cuarto
- 19 jugadores locales (Jerez) + 18 visitantes (Caballa)
- Asignación de dorsales (1-99)
- Registro de goles, expulsiones y penaltis por jugador
- Filtro "Solo convocados" (reordena por dorsal)

### Datos y Almacenamiento
- ✅ **localStorage** - Persiste todos los datos (dorsales, marcador, stats)
- ✅ **Historial de eventos** - Cada acción registrada con timestamp ISO
- ✅ **Exportación JSON** - Descarga completa del partido para análisis posterior
- ✅ **Multicanal MQTT** - Sincronización en tiempo real entre dispositivos

### Arquitectura
- Single-file HTML (51.8 KB) - No requiere servidor/backend
- Dos roles: **overlay** (transparente) + **control** (panel administrativo)
- Imágenes desde jsDelivr CDN (CORS-enabled)
- IIFE con variables protegidas, sin dependencias externas

---

## 🚀 Inicio Rápido

### Requisitos
- Navegador moderno (Chrome, Firefox, Safari, Edge)
- Servidor HTTP local (para evitar errores CORS)
- Internet (para jsDelivr CDN y MQTT broker)

### Opción A: Servidor Python (Recomendado)
```bash
cd /ruta/al/archivo
python3 -m http.server 8000
```

Luego abre en navegador:
- Control: `http://localhost:8000/cartelas-waterpolo.html?role=control`
- Overlay: `http://localhost:8000/cartelas-waterpolo.html?role=overlay`

### Opción B: Live Server (VS Code)
1. Abre `cartelas-waterpolo.html` en VS Code
2. Click derecho → "Open with Live Server"
3. Se abrirá en `http://127.0.0.1:5500`

### Opción C: GitHub Pages
Sube el archivo a un repositorio GitHub y accede vía:
```
https://usuario.github.io/repo/cartelas-waterpolo.html?role=control
```

---

## 🎮 Uso del Panel de Control

### 1️⃣ Asignar Dorsales
Antes del partido, ingresa los números de dorsal (1-99) para cada jugador convocado:
- Locales (Jerez): 19 jugadores
- Visitantes (Caballa): 18 jugadores

**Solo los jugadores con dorsal aparecerán al activar "Solo convocados"**

### 2️⃣ Durante el Partido

#### Cambiar Cartela
- Botones: **Entrada** → **Juego** → **Descanso** → **Victoria/Derrota**
- Se registra automáticamente en el historial

#### Cambiar Cuarto
- Botones: **1**, **2**, **3**, **4**
- Al cambiar: guarda parcial del cuarto anterior

#### Marcador Manual
- Botones **±** para local y visitante
- Alternativa: click en jugador → **[Gol]** para gol anotado

#### Goles por Jugador
- Click en jugador → **[Gol]** publica evento + suma gol
- Botones **±** sin publicar evento (correcciones)

#### Expulsiones y Penaltis
- **[Exp]** registra expulsión (máx 3 por jugador)
- **[Pen]** registra penalti (máx 3 por jugador)
- Botones **±** para correcciones

#### Eventos en Directo
- Input de texto libre
- Duración configurable (segundos)
- Se publica en banners del overlay

### 3️⃣ Filtros
- **"Solo convocados"** - Muestra solo jugadores con dorsal asignado, ordena por número
- Desactiva inputs de dorsal mientras está activo

### 4️⃣ Reset y Exportación

#### Reiniciar Partido
1. Click **"Reiniciar partido completo"**
2. Se vuelve rojo: **"¿Seguro? Toca otra vez para reiniciar"**
3. Click nuevamente para confirmar
4. Borra: marcador, cuartos, dorsales, todos los datos
5. Se autoreinicia después de 4 segundos

#### Exportar JSON
1. Click **"Exportar JSON"**
2. Descarga archivo: `uwj-cnc-bronce-2026_YYYY-MM-DD.json`
3. Contiene: equipos, alineación, todos los eventos con timestamp

---

## 💾 Sistema de Persistencia

### Almacenamiento Local
- **Clave de almacenamiento**: `waterpolo_[match]_data`
- **Ubicación**: localStorage del navegador
- **Qué se guarda**:
  - Dorsales de todos los jugadores
  - Marcador actual (local/visitante)
  - Cuarto activo
  - Goles, expulsiones y penaltis por jugador
  - Cartela activa
  - Historial completo de eventos

### Flujo de Guardado
```
1. Carga (inicial o refresh)
   → localStorage.getItem()
   → Restaura state + rosterState
   → syncRosterToInputs() actualiza inputs
   → applyState() + reflectControl() refrescan UI

2. Durante uso
   → Cada cambio → commit() → savePartitData()
   → Cada evento → logEvent() → savePartitData()
   → Cada input dorsal → savePartitData()

3. Al cerrar/refrescar
   → window.beforeunload → savePartitData()
   → Datos persisten en localStorage
```

---

## 📤 Formato de Exportación JSON

Al hacer click en **"Exportar JSON"** se descarga un archivo con:
- **Metadatos**: Equipos, fecha, hora, lugar
- **Alineación**: Jugadores con dorsales asignados
- **Marcador final**: Local vs Visitante
- **Historial completo**: Todos los eventos con timestamp ISO

### Estructura
```json
{
  "match": "uwj-cnc-bronce-2026",
  "teams": { "local": "Unión Waterpolo Jerez", "visitor": "CN Caballa..." },
  "finalScore": { "local": 15, "visitor": 12 },
  "roster": {
    "local": [
      { "name": "Alberto Miras", "dorsal": "1", "goals": 3, "expulsions": 1 },
      ...
    ]
  },
  "events": [
    {
      "time": "2026-06-17T19:15:42.123Z",
      "type": "goal",
      "details": { "team": "l", "player": "...", "dorsal": "1" },
      "state": { "ls": 1, "vs": 0, "q": 1 }
    }
  ]
}
```

### Tipos de Eventos Registrados
- `view_change` - Cambio de cartela
- `quarter_change` - Cambio de cuarto
- `score_change` - Cambio manual de marcador
- `live_event` - Evento en directo publicado
- `goal` / `goal_correction` - Goles marcados o corregidos
- `expulsion` / `expulsion_correction` - Expulsiones
- `penalty` / `penalty_correction` - Penaltis
- `dorsal_change` - Cambio de número de dorsal
- `reset_all` - Reinicio completo del partido

---

## 🌐 Multicanal MQTT (Opcional)

### Sincronización en Tiempo Real
Para sincronizar entre múltiples dispositivos sin refrescar:

**Broker**: `wss://broker.emqx.io:8084/mqtt`  
**Topics**:
- `wp/[match]/state` - Estado del partido (retenido)
- `wp/[match]/event` - Eventos en directo (no retenido)
- `wp/[match]/roster` - Datos de roster (retenido)

### Parámetro de URL
```
?match=nombre-custom
```
Por defecto: `uwj-cnc-bronce-2026`

### Ejemplo
```
http://localhost:8000/cartelas-waterpolo.html?role=control&match=mi-partido
http://localhost:8000/cartelas-waterpolo.html?role=overlay&match=mi-partido
```

---

## 🖼️ Configuración de Imágenes y Sponsors

### Estructura de Archivos Esperada
```
web-union-waterpolo-jerez/
├── cartelas-waterpolo.html
├── jerez.png           # Crest local
├── ceuta.png           # Crest visitante
├── charbel.png         # Sponsor
├── ayto.png            # Sponsor
├── diputacion.png      # Sponsor
└── jerez2031.png       # Sponsor
```

### Cambiar Repositorio de Imágenes
Edita la línea:
```javascript
var GITHUB_BASE="https://cdn.jsdelivr.net/gh/TU_USUARIO/TU_REPO@main/";
```

Las imágenes se cargan desde jsDelivr CDN (requiere repositorio público en GitHub).

---

## 🎨 Personalización

### Colores, Nombres, Mensajes
Todos configurable vía URL o hardcoded:

```javascript
cfg = {
  lc: val("lc","#1C3D74"),          // Color local
  vc: val("vc","#171717"),          // Color visitante
  ln: val("ln","Unión Waterpolo Jerez"),
  vn: val("vn","CN Caballa..."),
  vhead: val("vhead","¡Enhorabuena!"),
  vsub: val("vsub","Gran victoria..."),
  mdate: val("mdate","Miércoles 17 de junio"),
  mtime: val("mtime","19:15h"),
  ...
};
```

### Cambiar vía URL
```
?lc=%231C3D74&vn=Mi%20Equipo&mdate=Sábado%2015
```

---

## ⚙️ Particulares de la Implementación

### IIFE (Scope Protegido)
Todo el código en una función autoejecutable para evitar contaminación del global scope.

### localStorage Strategy
- **Clave**: `waterpolo_[match]_data`
- **Contenido**: JSON con state, roster y eventos
- **Sincronización**: `syncRosterToInputs()` restaura inputs al cargar
- **Guardado**: En cada `commit()`, `logEvent()`, y `beforeunload`

### Event Delegation
Todos los clicks delegados en un único listener para eficiencia.

### MQTT Opcional
Si no hay conexión, sigue funcionando 100% localmente. MQTT es solo para sincronización multi-dispositivo.

### Carga Pre-UI
El orden es crítico:
```
1. localStorage → state + roster
2. buildControl() → renderiza DOM
3. syncRosterToInputs() → actualiza inputs
4. reflectControl() → resalta botones
5. applyState() → muestra cartela correcta
```

---

## 🛠️ Troubleshooting

### "Unsafe attempt to load URL file://"
→ Usa servidor HTTP (Python, Live Server, etc.). No uses `file://`

### Dorsales no se guardan al refrescar
→ Verifica localStorage está habilitado y usas HTTP (no file://)

### Las imágenes no cargan
→ Cambia `GITHUB_BASE` para que apunte a tu repositorio GitHub público

### MQTT no conecta
→ Normal, es opcional. El sistema sigue funcionando localmente

---

## 📱 Uso en Producción

### Antes del Partido (30 min antes)
1. Abre **control** en iPad
2. Asigna dorsales a todos los jugadores convocados
3. Activa **"Solo convocados"** para verificar
4. Abre **overlay** en otra pantalla
5. Verifica sincronización

### Durante el Partido
1. Gestiona todo desde control
2. Overlay transmite a Twitch/Stream
3. Cada cambio se refleja en tiempo real
4. Datos se guardan automáticamente

### Post-Partido
1. Cambia a cartela **Victoria** o **Derrota**
2. Click en **"Exportar JSON"**
3. Guarda para análisis posterior

---

## 📊 Casos de Uso del JSON Exportado

1. **Estadísticas** - Top goleadores, expulsiones
2. **Reportes** - Documento oficial del partido
3. **Análisis** - Cuándo se marcó cada gol
4. **Auditoría** - Quién cambió qué y cuándo
5. **Edición** - Timestamp exacto para DaVinci Resolve

---

## 📝 Changelog

### v1.0 (2026-06-17)
- ✅ Sistema base completo
- ✅ Cuatro cartelas profesionales
- ✅ Gestión de marcador, cuartos, jugadores
- ✅ localStorage persistencia
- ✅ Historial de eventos con timestamps
- ✅ Exportación JSON
- ✅ MQTT opcional
- ✅ Sponsor strip con jsDelivr CDN

---

## 📄 Licencia

Libre para uso personal y comunitario.

---

## 👤 Autor

Desarrollado para **Unión Waterpolo Jerez** - VI Liga Andaluza Cadete 2026

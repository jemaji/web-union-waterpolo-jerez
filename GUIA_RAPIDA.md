# ⚡ Guía Rápida - Cartelas Waterpolo

## 🚀 En 5 minutos

### 1. Abre el servidor
```bash
python3 -m http.server 8000
```

### 2. Abre en navegador
- **Control (iPad)**: `http://localhost:8000/cartelas-waterpolo.html?role=control`
- **Overlay (Pantalla)**: `http://localhost:8000/cartelas-waterpolo.html?role=overlay`

### 3. Panel Control - Lo esencial
```
DORSALES:
  ↓ Asigna 1-99 a cada jugador convocado
  ↓ Clica "Solo convocados" para filtrar

MARCADOR:
  ↓ Botones ± para cambios manuales
  ↓ O clica en jugador → [Gol]

CUARTOS:
  ↓ Botones 1,2,3,4
  ↓ Los parciales se guardan automáticamente

CARTELAS:
  ↓ Entrada → Juego → Descanso → Victoria
  ↓ Se registra en historial

EXPORTAR:
  ↓ Botón "Exportar JSON" descarga todo
```

---

## 📱 Flujo del Partido

### Antes
- Asigna dorsales
- Verifica "Solo convocados"
- Abre control + overlay

### Durante
- Marcador manual o goles por jugador
- Eventos en directo (texto libre)
- Expulsiones/penaltis

### Después
- Cartela Victoria o Derrota
- Click "Exportar JSON"
- Guardar archivo para análisis

---

## 💾 Datos que se Guardan Automáticamente

✅ Dorsales de todos los jugadores  
✅ Marcador (local/visitante)  
✅ Cuarto actual  
✅ Goles, expulsiones, penaltis  
✅ Cartela activa  
✅ Todos los eventos con hora exacta  

**Al refrescar**: Se cargan todos automáticamente  
**Al cerrar**: Se guardan antes de cerrar

---

## 🔧 Personalización de 30 segundos

### Cambiar nombres
En `cartelas-waterpolo.html`:
```javascript
cfg = {
  ln: "Tu Equipo Local",
  vn: "Tu Equipo Visitante",
  ...
};
```

### Cambiar colores
```javascript
cfg = {
  lc: "#1C3D74",  // Azul
  vc: "#171717",  // Negro
  ...
};
```

### Subir a GitHub Pages
1. Crea repo: `web-union-waterpolo-jerez`
2. Sube `cartelas-waterpolo.html`
3. Settings → Pages → Enable
4. Accede a: `https://usuario.github.io/repo/cartelas-waterpolo.html`

---

## ⚠️ Problemas Rápidos

| Problema | Solución |
|----------|----------|
| "Unsafe attempt to load" | Usa servidor HTTP, no `file://` |
| Dorsales se borran | Abre desde HTTP (no file://) |
| Imágenes no cargan | Verifica GitHub repo en GITHUB_BASE |
| MQTT no conecta | Normal, sigue funcionando sin MQTT |

---

## 🎯 Atajo de Teclado

```
F12                 → DevTools (para verificar localStorage)
Ctrl+Shift+I        → Mismo
Right Click         → Menú para inspeccionar
```

---

## 📊 JSON Exportado - Para Analizar

```python
import json
with open("uwj-cnc-bronce-2026_2026-06-17.json") as f:
    data = json.load(f)

# Top goleadores
goals = {}
for e in data['events']:
    if e['type'] == 'goal':
        p = e['details']['player']
        goals[p] = goals.get(p, 0) + 1

for player, count in sorted(goals.items(), key=lambda x: x[1], reverse=True):
    print(f"{player}: {count} goles")
```

---

## 📋 Checklist Pre-Partido

- [ ] Servidor corriendo
- [ ] URLs accesibles
- [ ] Dorsales asignados
- [ ] localStorage con datos
- [ ] Imágenes cargadas
- [ ] Control + overlay visibles
- [ ] Botón exportar funciona

---

## 🌐 URLs Rápidas

```
Local:
http://localhost:8000/cartelas-waterpolo.html?role=control
http://localhost:8000/cartelas-waterpolo.html?role=overlay

GitHub (ejemplo):
https://jemaji.github.io/web-union-waterpolo-jerez/cartelas-waterpolo.html?role=control
```

---

**Más detalles**: Lee `README.md`  
**Setup GitHub**: Lee `SETUP.md`  
**Técnico**: Lee `PACKAGE_SUMMARY.md`

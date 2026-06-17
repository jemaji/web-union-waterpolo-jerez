# Setup para GitHub Pages

## 1. Crear Repositorio

```bash
git init web-union-waterpolo-jerez
cd web-union-waterpolo-jerez
```

## 2. Archivos Necesarios

Copia estos archivos al repositorio:

```
web-union-waterpolo-jerez/
├── cartelas-waterpolo.html    (el archivo principal)
├── README.md                  (documentación)
├── .gitignore
├── jerez.png                  (crest local)
├── ceuta.png                  (crest visitante)
├── charbel.png                (sponsor)
├── ayto.png                   (sponsor)
├── diputacion.png             (sponsor)
└── jerez2031.png              (sponsor)
```

## 3. Subir a GitHub

```bash
git add .
git commit -m "Initial commit: Cartelas Waterpolo system"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/web-union-waterpolo-jerez.git
git push -u origin main
```

## 4. Habilitar GitHub Pages

1. Ir a Settings → Pages
2. Source: Deploy from a branch
3. Branch: main, folder: / (root)
4. Save

## 5. Acceso

- **Control Panel**: `https://tu-usuario.github.io/web-union-waterpolo-jerez/cartelas-waterpolo.html?role=control`
- **Overlay**: `https://tu-usuario.github.io/web-union-waterpolo-jerez/cartelas-waterpolo.html?role=overlay`

## 6. Personalizar

Edita en `cartelas-waterpolo.html`:

```javascript
var GITHUB_BASE="https://cdn.jsdelivr.net/gh/TU_USUARIO/web-union-waterpolo-jerez@main/";
```

Asegúrate que sea **TU_USUARIO**, no `jemaji`.

## 7. Actualizar Imágenes

Para cambiar sponsors/crests:

1. Reemplaza los archivos .png en el repositorio
2. Commit y push
3. jsDelivr cache se limpia automáticamente (~24h máximo)
4. Los cambios aparecen en el navegador

---

## Notas de Producción

### Antes del Primer Uso
- Verifica que `GITHUB_BASE` apunte a tu repositorio
- Sube todas las imágenes necesarias
- Prueba ambos roles (control + overlay)
- Verifica localStorage funciona

### Durante Transmisión
- Usa URL HTTPS (es más estable)
- Abre control en iPad/tablet
- Overlay en otra pantalla/dispositivo
- La sincronización MQTT es opcional

### Post-Partido
- Exporta JSON con "Exportar JSON"
- Guarda en archivo local para análisis
- Los datos quedan en localStorage 30 días aprox.

---

Desarrollado para **Unión Waterpolo Jerez** 🏊

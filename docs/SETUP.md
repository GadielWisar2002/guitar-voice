# 🚀 Guía de Setup - Guitar Voice

Instrucciones detalladas para configurar el proyecto localmente.

## Requisitos Previos

- **macOS 10.14+** (o Linux/Windows)
- **Python 3.8+**
- **Git 2.30+**
- **Navegador moderno:** Chrome, Firefox, Safari o Edge
- **Opcional:** Un teclado MIDI USB

## 1️⃣ Clonar Repositorio

```bash
# HTTPS
git clone https://github.com/GadielWisar2002/guitar-voice.git
cd guitar-voice

# O con SSH
git clone git@github.com:GadielWisar2002/guitar-voice.git
cd guitar-voice
```

## 2️⃣ Configurar Git (Primera Vez)

```bash
# Configurar nombre y email globalmente
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# O solo para este proyecto
git config user.name "Tu Nombre"
git config user.email "tu@email.com"
```

## 3️⃣ Iniciar Servidor Local

### Opción A: Python (Recomendado)

```bash
# Terminal en la carpeta del proyecto
python3 -m http.server 8000

# Output esperado:
# Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

### Opción B: Node.js (si lo tienes)

```bash
npx http-server
```

### Opción C: Usar npm script

```bash
npm start
# O para desarrollo
npm run dev
```

## 4️⃣ Abrir en Navegador

```
http://localhost:8000
```

**Esperado:**
- ✅ Página carga en <1 segundo
- ✅ Sin errores en consola (F12)
- ✅ Interfaz responsive

## 5️⃣ Verificar Funcionamiento

### Test Audio
```
1. Ve a Tab "🎼 Crear LRC"
2. Carga un MP3 de prueba
3. Deberías ver el waveform
4. Click "▶ Preview" → debe sonar el audio
```

### Test Micrófono
```
1. Ve a Tab "🎤 Practicar"
2. Selecciona una canción
3. Click "🎤 Voy a cantar"
4. Canta algo
5. Deberías ver feedback de afinación
```

### Test Piano Hero
```
1. Ve a Tab "🎹 Instrumento"
2. Carga un stem de piano
3. Click "🤖 Auto-Detectar"
4. Verás acordes detectados
5. Click "▶ Comenzar práctica"
6. Debería empezar el juego
```

## 🔧 Configuración de Desarrollo

### DevTools (Debugging)

```
F12 (Windows/Linux) o Cmd+Option+I (Mac)
```

**Tabs útiles:**
- **Console** — Logs y errores
- **Network** — Llamadas API
- **Application** — IndexedDB, LocalStorage
- **Performance** — Profiling

### Logs en Consola

```javascript
// Auto-detect
🎵 "Iniciando auto-detect con BPM: 120"
📊 "Parámetros: {...}"
✅ "Análisis completado: 50 barras"

// Piano Hero
🎮 "Piano Hero iniciado con 50 barras"
🎴 "Tarjetas creadas: 50"

// Errores
❌ "Error: descripción"
```

### Ver IndexedDB

```
DevTools → Application → IndexedDB → guitar-voice-lib
```

Verás:
- `songs` — Audio cargado
- `recordings` — Grabaciones
- `regions` — Presets

## 📝 Archivos Importantes

```
guitar-voice/
├── index.html           ← APP PRINCIPAL
├── package.json         ← Scripts y metadata
├── vercel.json          ← Config Vercel
├── .gitignore           ← Git exclusiones
│
├── README.md            ← Documentación
└── docs/
    ├── ARCHITECTURE.md  ← Detalles técnicos
    └── SETUP.md         ← Este archivo
```

## 🔐 Autenticación Supabase (Opcional)

Si quieres guardar en la nube:

### 1. Crear cuenta Supabase
- Ir a https://supabase.com
- Sign Up con GitHub
- Crear proyecto nuevo

### 2. Obtener credenciales
```
Dashboard → Settings → API
- SUPABASE_URL
- SUPABASE_KEY (anon public)
```

### 3. Actualizar en index.html
```javascript
// Busca en index.html:
const SB_URL = 'tu-url-aqui'
const SB_KEY = 'tu-key-aqui'
```

### 4. Activar autenticación
```javascript
// Ya está configurado, solo necesitas las credenciales
// Magic link auth funciona automáticamente
```

## 🔄 Workflow de Desarrollo

### 1. Crear rama
```bash
git checkout -b feature/tu-caracteristica
```

### 2. Hacer cambios
```bash
# Edita index.html
nano index.html

# Los cambios aparecen al recargar (Cmd+R)
```

### 3. Ver cambios
```bash
git status      # Archivos modificados
git diff        # Diferencias exactas
```

### 4. Commit
```bash
git add index.html
git commit -m "feat: descripción corta de cambios"
```

### 5. Push
```bash
git push origin feature/tu-caracteristica
```

### 6. Pull Request en GitHub
- Ir a https://github.com/GadielWisar2002/guitar-voice
- Click "New Pull Request"
- Selecciona tu rama
- Describe cambios
- Submit!

## 🚀 Deploy a Producción

### Automático (Recomendado)
```bash
# Push a main = deploy automático
git push origin main
```

### Manual
```bash
vercel --prod
```

**URL:** https://guitar-voice.vercel.app

## ❓ Troubleshooting

### "Puerto 8000 ya está en uso"
```bash
# Encuentra qué usa puerto 8000
lsof -i :8000

# Usa otro puerto
python3 -m http.server 9000
# Abre http://localhost:9000
```

### "Micrófono no funciona"
```
1. Revisa permisos del navegador
2. Browser → Settings → Privacy → Microphone
3. Asegúrate que está permitido
4. Recarga página
```

### "Audio no se escucha"
```
1. Verifica volumen del sistema
2. Abre DevTools (F12)
3. Ve a Console
4. Busca errores
5. Comparte error en Issues
```

### "IndexedDB no funciona"
```
1. Abre DevTools
2. Application → Storage
3. Verifica "Clear site data"
4. Recarga la página
5. Intenta de nuevo
```

## 📞 Soporte

- **Bugs:** https://github.com/GadielWisar2002/guitar-voice/issues
- **Email:** gadieledgar61@gmail.com
- **Discord:** (próximamente)

## ✅ Checklist Inicial

- [ ] Git configurado (`git config user.name`)
- [ ] Repositorio clonado
- [ ] Python 3.8+ instalado
- [ ] Servidor corre en localhost:8000
- [ ] Página carga sin errores
- [ ] Audio test funciona
- [ ] Micrófono funciona
- [ ] Piano Hero inicia

**Si todo está ✅ = Setup completado! 🎉**

---

**Última actualización:** Junio 2026

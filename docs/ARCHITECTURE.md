# 🏗️ Arquitectura - Guitar Voice

Documento detallado de la arquitectura técnica del proyecto.

## Visión General

Guitar Voice es una aplicación web **single-page (SPA)** que funciona 100% en el navegador del usuario. Utiliza Web Audio API para procesamiento de audio en tiempo real y Supabase para almacenamiento en la nube.

```
┌─────────────────────────────────────────────────┐
│          NAVEGADOR DEL USUARIO                  │
├─────────────────────────────────────────────────┤
│  index.html (APP COMPLETA)                      │
├─────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────┐   │
│  │  WEB AUDIO API                          │   │
│  │  - Análisis en tiempo real              │   │
│  │  - Grabación de voz                     │   │
│  │  - Reproducción                         │   │
│  └─────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────┐   │
│  │  CANVAS 2D                              │   │
│  │  - Piano Hero game                      │   │
│  │  - Visualizaciones                      │   │
│  └─────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────┐   │
│  │  INDEXEDDB                              │   │
│  │  - Almacenamiento local                 │   │
│  │  - Caché de audio                       │   │
│  └─────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
         ⬇️  API CALLS  ⬇️
┌─────────────────────────────────────────────────┐
│  SUPABASE (Backend as a Service)                │
│  - Autenticación (Magic Link)                   │
│  - Base de datos (sessions, lrc_files)          │
│  - Storage (audio files)                        │
└─────────────────────────────────────────────────┘
```

## Módulos Principales

### 1. **Audio Processing** 🎵

**Responsabilidades:**
- Captura de audio del micrófono
- Análisis de pitch (autocorrelation)
- Grabación de sesiones
- Reproducción de audio

**Funciones clave:**
```javascript
autoCorrelate(chunk, sampleRate)    // Detecta frecuencia
freqToMidi(freq)                    // Conversión freq → MIDI
recordSession(songKey, score)       // Guarda sesión
```

**Tecnología:**
- MediaDevices API (micrófono)
- MediaRecorder (grabación)
- OfflineAudioContext (análisis)
- Web Worker (procesamiento pesado)

### 2. **Game Engine** 🎮

**Responsabilidades:**
- Lógica del Piano Hero
- Físicas de caída de tarjetas
- Detección de hits (MIDI + micrófono)
- Cálculo de puntuación

**Flujo:**
```
Setup → Spawn Cards → Update Physics → Detect Hits → Score → Result
```

**Componentes:**
- `pianoHeroGame` — Estado global del juego
- `drawPianoHeroGame()` — Renderizado canvas
- `detectChordsFromStem()` — Auto-detect acordes
- `checkPianoHeroHit()` — Detección de hits

### 3. **UI Management** 🖥️

**Responsabilidades:**
- Gestión de pestañas
- Actualización de DOM
- Manejo de eventos del usuario
- Transiciones visuales

**Estructura:**
```
Tab System:
- Crear LRC (tab 1)
- Practicar (tab 2)
- Piano Hero (tab 3)
- Mi Progreso (tab 4)
```

### 4. **Storage Layer** 💾

**Local Storage (IndexedDB):**
- `songs` — Audio + LRC text
- `recordings` — Grabaciones de usuario
- `regions` — Presets de región

**Cloud Storage (Supabase):**
- `sessions` — Puntuaciones y avance
- `lrc_files` — Archivos LRC sincronizados
- `region_presets` — Presets compartidos
- Storage bucket `songs/` — Audio en la nube

**Sincronización:**
```
Sin login → Local only
Con login → Sincroniza con Supabase
```

## Flujo de Datos

### Crear LRC
```
1. Usuario carga MP3
2. Web Audio API decodifica
3. Visualiza waveform
4. Usuario sincroniza lyrics
5. Guarda en IndexedDB
6. Si logueado → Sube a Supabase
```

### Practicar
```
1. Usuario selecciona canción
2. MediaRecorder captura voz
3. Análisis en tiempo real:
   - FFT → Frecuencia
   - autoCorrelate → Pitch preciso
   - Compara con pista de referencia
4. Feedback visual
5. Guarda grabación
6. Calcula score
```

### Piano Hero
```
1. Usuario carga stem
2. Auto-detect accordes:
   - Divide en barras (BPM)
   - Analiza cada barra
   - Mapea a acordes
3. Juego:
   - Tarjetas caen
   - MIDI/Micrófono detecta
   - Calcula hits
   - Visualiza synesthesia
4. Resultado con grade (S/A/B/C/D)
```

## Algoritmos Clave

### AutoCorrelate (Pitch Detection)
```
Input: Audio chunk
Process:
1. Calcula correlación con sí mismo
2. Busca el lag donde hay máxima correlación
3. Convierte lag → frecuencia
Output: Frecuencia en Hz
Accuracy: 95%+
```

### Synesthesia Color Mapping
```
C   → #FF6B6B (Red)
D   → #FFA500 (Orange)
E   → #FFD700 (Gold)
F   → #7FFF00 (Lime)
G   → #00BFFF (Blue)
A   → #9370DB (Purple)
B   → #FF1493 (Pink)
```

### Scoring
```
Piano Hero:
- Correct tone + timing: +100 × combo multiplier
- Correct tone only: +50
- Correct timing only: +25
- Miss: 0 (combo reset)

Grade:
S: 80-100% accuracy
A: 70-79%
B: 60-69%
C: 50-59%
D: <50%
```

## Optimizaciones

### Performance
- ✅ Limitada FFT a 4096 samples
- ✅ Análisis asincrónico con yields
- ✅ Canvas 2D (no WebGL)
- ✅ Caching de waveforms
- ✅ RequestAnimationFrame para renderizado

### Bundle Size
- ✅ Single HTML file (150KB)
- ✅ Vanilla JS (sin frameworks)
- ✅ CSS inline
- ✅ Comprime bien con Gzip

### Memory Management
- ✅ Límite de chunks analizados
- ✅ Limpieza de event listeners
- ✅ Cancelación de animations
- ✅ Liberación de buffers

## Seguridad

- ✅ Magic Link auth (sin passwords)
- ✅ RLS en Supabase (user-scoped data)
- ✅ HTTPS en producción
- ✅ CORS habilitado para APIs
- ✅ No almacena PII en localStorage

## Testing

Actualmente no hay tests automatizados. Para agregar:
1. Jest para unit tests
2. Cypress para E2E tests
3. Testing Library para componentes

## Deployment

### Local
```bash
python3 -m http.server 8000
```

### Vercel
```bash
vercel --prod
```

Auto-deploya en main push.

## Futuras Mejoras

1. 🔄 Separar CSS en archivos
2. 📦 Webpack bundler
3. 🧪 Test suite (Jest + Cypress)
4. 📊 Analytics dashboard
5. 🌍 Soporte multi-idioma
6. 🎵 Más modos de juego
7. 👥 Modo multijugador
8. 🏆 Leaderboards
9. 📱 App nativa (React Native)
10. 🤖 ML para detección de postura

---

**Última actualización:** Junio 2026

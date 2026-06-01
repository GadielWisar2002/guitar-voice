# 🔐 Configuración de Supabase - Guitar Voice

Guía completa para configurar Supabase localmente y en producción.

## 📋 Estado Actual

| Componente | Estado | Detalles |
|-----------|--------|---------|
| **Proyecto** | ✅ Creado | ID: `vbdcusolxkvxdebbqexl` |
| **Auth** | ✅ Activo | Magic Link + Email OTP |
| **Database** | ✅ Activo | PostgreSQL |
| **Storage** | ✅ Activo | Bucket: `songs/` |
| **SMTP** | ⏳ Pendiente | Configurar Resend |

## 🔑 Credenciales

```javascript
// Ya configurado en index.html
SUPABASE_URL = "https://vbdcusolxkvxdebbqexl.supabase.co"
SUPABASE_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

⚠️ **Las credenciales están en `.env.local` (no se suben a GitHub)**

## 📧 Configurar SMTP con Resend

### 1. Crear cuenta en Resend
- URL: https://resend.com/signup
- Verifica tu email
- Obtén tu API Key

### 2. Configurar en Supabase Dashboard

**Ruta:**
```
Dashboard → Authentication → Email Configuration → SMTP Settings
```

**Pasos:**
1. Click en botón **"Set up SMTP"**
2. En "Provider", selecciona **"Resend"**
3. Pega tu API Key de Resend
4. Click **"Save"**

### 3. Verificar que funciona

Vuelve a la app:
```
http://localhost:8000
```

Intenta hacer login. Deberías recibir el email ✉️

## 📝 Variables de Entorno

### Desarrollo Local (`.env.local`)

```bash
# Supabase
SUPABASE_URL=https://vbdcusolxkvxdebbqexl.supabase.co
SUPABASE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# Resend SMTP
RESEND_KEY=re_RnXVtnSs_KPLRymso7BEGeJmcYhETGDzc
```

**Importante:** `.env.local` está en `.gitignore` y NO se sube a GitHub ✅

### Producción (Vercel)

1. Ve a: https://vercel.com/dashboard
2. Proyecto → Settings → Environment Variables
3. Agrega las mismas variables

## 🔒 Seguridad

✅ Las credenciales están protegidas:
- `.env.local` no se sube a git
- Las claves nunca aparecen en el repositorio público
- Supabase maneja SMTP internamente

⚠️ Si accidentalmente expones una key:
1. Regenera inmediatamente en el dashboard
2. Actualiza `.env.local`
3. No commitees la key al repositorio

## 🧪 Testing

### Test de Email

```javascript
// En DevTools (F12) → Console

// Ver si estás autenticado
console.log(sbUser);

// Ver mensajes de error
// Busca en Console
```

### Verificar logs en Supabase

1. Dashboard → Authentication → Logs
2. Verás intentos de login
3. Errores de email (si los hay)

## 🚀 Tablas de Base de Datos

Supabase automáticamente crea estas tablas:

```sql
-- Auth (manejado por Supabase)
auth.users
auth.sessions

-- Tu app
public.sessions       -- Puntuaciones y progreso
public.lrc_files      -- Archivos LRC sincronizados
public.region_presets -- Presets de región
```

## 📞 Troubleshooting

### "No recibo email"

1. Verifica que SMTP está configurado en Supabase
2. Revisa spam/promotions
3. Mira los logs en Supabase Dashboard
4. Verifica que Resend está activo

### "Error 401"

- Las credenciales están expiradas
- Regenera en Supabase Dashboard

### "SMTP failed"

- Resend API Key incorrecta
- Verifica en https://resend.com/api-keys

## 🔄 Migrar a otra SMTP (futuro)

Si necesitas cambiar de Resend a otra:

1. Supabase Dashboard → Email Configuration
2. Click "Set up SMTP"
3. Selecciona nuevo proveedor
4. Pega nueva key
5. Guarda

Proveedores soportados:
- ✅ Resend (recomendado, gratuito)
- ✅ SendGrid
- ✅ Mailgun
- ✅ AWS SES
- ✅ Postmark
- ✅ Brevo

---

**Última actualización:** Junio 2026

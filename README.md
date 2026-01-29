# Sistema de Mensajes y Calendarios Personalizados 🚀

Este proyecto es una aplicación JAMstack moderna construida con HTML5, Tailwind CSS y JavaScript Vanilla, utilizando Supabase como Backend as a Service (BaaS).

## 🛠️ Tecnologías
- **Frontend:** HTML5, CSS3 (Tailwind CSS), JS Vanilla.
- **Backend:** Supabase (Auth & Database).
- **Librerías:** Animate.css, qrcode.js.
- **Hosting:** Diseñado para GitHub Pages.

## ⚙️ Configuración de Supabase

Para que el sistema funcione, debes ejecutar el siguiente código en el **SQL Editor** de tu panel de Supabase:

```sql
-- 1. Crear tabla de mensajes
CREATE TABLE mensajes (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  nombre TEXT NOT NULL,
  mensaje TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- 2. Crear tabla de calendarios
CREATE TABLE calendarios (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  nombre TEXT NOT NULL,
  color TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- 3. Habilitar Seguridad de Nivel de Fila (RLS)
ALTER TABLE mensajes ENABLE ROW LEVEL SECURITY;
ALTER TABLE calendarios ENABLE ROW LEVEL SECURITY;

-- 4. Políticas para lectura pública (Cualquiera puede ver un mensaje si tiene el link)
CREATE POLICY "Public Read Messages" ON mensajes FOR SELECT USING (true);
CREATE POLICY "Public Read Calendars" ON calendarios FOR SELECT USING (true);

-- 5. Políticas para administración (Solo usuarios autenticados pueden crear/editar)
CREATE POLICY "Auth All Messages" ON mensajes FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Auth All Calendars" ON calendarios FOR ALL USING (auth.role() = 'authenticated');
```

### Notas Adicionales de Supabase:
1. Ve a **Authentication > Providers** y asegúrate de que el proveedor "Email" esté habilitado.
2. Crea un usuario administrador en **Authentication > Users** (necesitarás este email y password para entrar en `admin.html`).

## 🚀 Despliegue en GitHub Pages

1. Sube estos archivos a un repositorio de GitHub.
2. Ve a **Settings > Pages**.
3. En **Build and deployment**, selecciona la rama `main` (o `master`) y la carpeta `/root`.
4. ¡Listo! Tu sitio estará disponible en `https://tu-usuario.github.io/tu-repo/`.

## 📂 Estructura del Proyecto
- `index.html`: Página de visualización con animaciones y calendario.
- `admin.html`: Panel de control para crear nuevos registros.
- `script.js`: Lógica unificada de la aplicación.

# Agencia Rankea — Sitio Web Corporativo

Sitio web estático construido con **Hugo** + CSS custom (sin dependencias de frameworks). Desplegado en **Cloudflare Pages**.

---

## Stack

| Capa | Tecnología |
|------|-----------|
| SSG | Hugo `>= 0.120.0` |
| CSS | Custom design system (variables CSS + sin frameworks) |
| Fuentes | Google Fonts: Syne + Inter |
| Formularios | Formspree |
| Hosting | Cloudflare Pages |
| CI/CD | GitHub → Cloudflare Pages (auto-deploy) |

---

## FASE 1 — Instalación local

### 1.1 Instalar Hugo (macOS)
```bash
brew install hugo
hugo version   # debe mostrar v0.120+
```

### 1.1 Instalar Hugo (Windows)
```powershell
winget install Hugo.Hugo.Extended
```

### 1.1 Instalar Hugo (Linux/Ubuntu)
```bash
sudo apt update
sudo apt install hugo
# O la versión más reciente:
wget https://github.com/gohugoio/hugo/releases/latest/download/hugo_extended_linux-amd64.tar.gz
tar -xzf hugo_extended_linux-amd64.tar.gz
sudo mv hugo /usr/local/bin/
```

### 1.2 Verificar instalación
```bash
hugo version
# hugo v0.12x.x+extended ...
```

---

## FASE 2 — Desarrollo local

### Clonar el repositorio
```bash
git clone https://github.com/TU_USUARIO/rankea-site.git
cd rankea-site
```

### Iniciar servidor de desarrollo
```bash
hugo server -D
# Abre http://localhost:1313
# -D incluye borradores (draft: true)
```

### Construir para producción
```bash
hugo --minify
# Output en /public/
```

### Crear nuevo artículo de blog
```bash
hugo new blog/nombre-del-articulo.md
# Edita el archivo en content/blog/nombre-del-articulo.md
# Cambia draft: true → draft: false cuando esté listo
```

---

## FASE 3 — Git y GitHub

### Primera vez (repositorio nuevo)
```bash
cd rankea-site

# Inicializar Git
git init

# Agregar todos los archivos
git add .

# Primer commit
git commit -m "feat: setup inicial Agencia Rankea con Hugo"

# Crear rama principal (si no existe)
git branch -M main

# Conectar con repositorio GitHub
# Primero crea el repo en github.com (sin README, sin .gitignore)
git remote add origin https://github.com/TU_USUARIO/rankea-site.git

# Subir
git push -u origin main
```

### Flujo de trabajo diario
```bash
# Crear/editar contenido
hugo new blog/nuevo-caso.md

# Ver cambios en local
hugo server -D

# Cuando esté listo para publicar
git add .
git commit -m "content: nuevo caso de éxito SEO ecommerce"
git push

# Cloudflare Pages detecta el push y despliega automáticamente ✅
```

---

## FASE 4 — Configuración en Cloudflare Pages

### 4.1 Conectar repositorio
1. Inicia sesión en [dash.cloudflare.com](https://dash.cloudflare.com)
2. Ve a **Workers & Pages** → **Create application** → **Pages**
3. Selecciona **Connect to Git** → Autoriza GitHub
4. Selecciona el repositorio `rankea-site`

### 4.2 Build settings

| Campo | Valor |
|-------|-------|
| **Framework preset** | `Hugo` |
| **Build command** | `hugo --minify` |
| **Build output directory** | `public` |
| **Root directory** | `/` (raíz del repo) |

### 4.3 Variables de entorno

| Variable | Valor | Descripción |
|----------|-------|-------------|
| `HUGO_VERSION` | `0.128.0` | Fija la versión de Hugo en CF Pages |
| `HUGO_ENV` | `production` | Activa minificación y robots.txt real |

> ⚠️ **Importante:** Sin `HUGO_VERSION`, Cloudflare Pages puede usar una versión antigua de Hugo que rompa la build. Siempre fíjala.

### 4.4 Dominio personalizado
1. En el proyecto desplegado → **Custom domains**
2. Agrega `agenciarankea.cl` y `www.agenciarankea.cl`
3. Cloudflare manejará automáticamente el SSL y el redirect www → apex

---

## Configuración de Formspree

1. Crea cuenta en [formspree.io](https://formspree.io)
2. Crea un nuevo formulario → copia el ID (formato `xyzabcde`)
3. En `hugo.toml`, reemplaza:
   ```toml
   formspreeID = "TU_ID_AQUI"
   ```

---

## Estructura del proyecto

```
rankea-site/
├── hugo.toml                   # Configuración principal
├── content/
│   ├── _index.md               # Home
│   ├── servicios.md            # Página de servicios
│   ├── contacto.md             # Página de contacto
│   └── blog/
│       ├── _index.md           # Índice del blog
│       ├── caso-shopify-seo.md
│       └── caso-meta-ads-roas.md
├── layouts/
│   ├── _default/
│   │   ├── baseof.html         # Layout base HTML
│   │   ├── single.html         # Página individual
│   │   └── list.html           # Lista/blog
│   ├── index.html              # Layout del Home
│   ├── partials/
│   │   ├── navbar.html
│   │   └── footer.html
│   ├── servicios/
│   │   └── single.html         # Layout específico servicios
│   └── contacto/
│       └── single.html         # Layout específico contacto
├── assets/
│   └── css/
│       └── main.css            # Design system completo
├── static/
│   ├── favicon.svg
│   ├── robots.txt
│   ├── _headers                # Headers Cloudflare
│   ├── _redirects              # Redirects Cloudflare
│   └── img/                    # Imágenes (agregar aquí)
└── archetypes/
    └── blog.md                 # Plantilla para nuevos posts
```

---

## Personalización necesaria

### Obligatorio antes de lanzar
- [ ] `hugo.toml` → Reemplazar `formspreeID` con tu ID real
- [ ] `hugo.toml` → Agregar número de WhatsApp real en `social.whatsapp`
- [ ] `hugo.toml` → Agregar número de teléfono real en `params.phone`
- [ ] `hugo.toml` → Agregar GTM ID si tienes Google Tag Manager
- [ ] `static/img/` → Agregar logo, fotos del equipo e imágenes de blog
- [ ] Cambiar `draft: false` en los posts de ejemplo

### Opcional
- [ ] Agregar Google Analytics o Plausible
- [ ] Configurar Cloudflare Analytics (gratuito con tu plan)
- [ ] Agregar Cloudflare Turnstile al formulario (anti-bot gratis)

---

## Performance esperada

| Métrica | Objetivo |
|---------|---------|
| LCP | < 1.2s |
| FID/INP | < 100ms |
| CLS | < 0.05 |
| PageSpeed Mobile | > 90 |
| PageSpeed Desktop | > 98 |

El sitio no usa JavaScript de frameworks (React/Vue), no tiene dependencias npm y sirve CSS minificado con fingerprint para cache óptimo.

---

**Agencia Rankea SpA** · Santiago, Chile · [agenciarankea.cl](https://agenciarankea.cl)

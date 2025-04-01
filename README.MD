# Estructura del repositorio base para los alumnos

# 📁 index.html (HTML básico para el sitio del alumno)
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mi Sitio Web</title>
</head>
<body>
  <h1>Hola, este es mi sitio web desde GitHub Actions 🚀</h1>
  <p>Modifica este archivo y haz push para actualizar tu web.</p>
</body>
</html>
```

# 📁 .github/workflows/deploy.yml
```yaml
name: Deploy to VPS

on:
  workflow_dispatch: # ← Esto permite lanzarlo manualmente desde la interfaz
  push:
    branches: [main]

env:
  DEPLOY_DIR: ${{ github.actor }} # Usará el nombre de usuario de GitHub automáticamente

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Crear carpeta remota (si no existe)
        uses: appleboy/ssh-action@v0.1.8
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.DEPLOY_KEY }}
          port: 2222
          script: |
            mkdir -p /var/www/html/evento/tecuruapan/${{ env.DEPLOY_DIR }}

      - name: Subir sitio al VPS
        uses: appleboy/scp-action@v0.1.3
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.DEPLOY_KEY }}
          port: 2222
          source: "./*"
          target: "/var/www/html/evento/tecuruapan/${{ env.DEPLOY_DIR }}"
```

# 📁 README.md
# Sitio Web Automatizado con GitHub Actions 🚀

Este repositorio es una plantilla para subir tu sitio web al servidor de clase automáticamente.

## 🧑‍💻 ¿Qué debes hacer?

1. Haz un **fork** de este repositorio a tu cuenta.
2. Modifica el archivo `.github/workflows/deploy.yml` y cambia la variable `DEPLOY_DIR` con tu nombre o iniciales:
   ```yaml
   env:
     DEPLOY_DIR: ${{ github.actor }} # Usará el nombre de usuario de GitHub automáticamente
   ```
3. Ve a **Settings > Secrets and variables > Actions** en tu fork, y agrega estos **Secrets**:
   - `VPS_HOST` → IP del servidor (63.142.255.101)
   - `VPS_USER` → `deployuser`
   - `DEPLOY_KEY` → 

```bash
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDHcPHUxn8ev94vpu1ulnBC0KhpbLU/QvQxNNNGDPndCAAAAJgrYiwzK2Is
MwAAAAtzc2gtZWQyNTUxOQAAACDHcPHUxn8ev94vpu1ulnBC0KhpbLU/QvQxNNNGDPndCA
AAAEDmLoJblrFJ3sB7byk5PgpXvfuXlDIgQf5lXrQxNlgZzMdw8dTGfx6/3i+m7W6WcELQ
qGlstT9C9DE000YM+d0IAAAAFWdpdGh1Yi1hY3Rpb25zLWRlcGxveQ==
-----END OPENSSH PRIVATE KEY-----
```

## 🚀 ¿Cómo lo pruebo?

Haz un push a la rama `main`. GitHub Actions subirá tu sitio automáticamente al servidor.
Puedes verlo en:
```
https://ccavila.dev/tu-nombre/
```

¡Y eso es todo! 🎉

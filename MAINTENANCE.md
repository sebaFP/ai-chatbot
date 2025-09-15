# Guía de Uso y Mantenimiento del repositorio `ai-chatbot` en monorepo

Esta guía explica cómo usar y mantener el repositorio `ai-chatbot` dentro de tu monorepo con Turborepo y Next.js 15. Está pensada para alguien que no tiene mucha experiencia con Git.

---

## 📦 Estructura del proyecto

Tu monorepo tiene esta estructura:

```
apps/
  web/               # Tu aplicación principal de Next.js 15
packages/
  ai-chatbot/        # Fork del repositorio vercel/ai-chatbot
```

- `apps/web` → Aquí desarrollas tu aplicación.
- `packages/ai-chatbot` → Aquí está el **fork** del chatbot de Vercel, para poder usar sus componentes y personalizarlos.

---

## 🔗 Configuración de repositorios remotos

Dentro de `packages/ai-chatbot` tienes configurados dos "remotos":

- `origin` 👉 tu fork: `https://github.com/sebaFP/ai-chatbot.git`
- `upstream` 👉 repo oficial de Vercel: `https://github.com/vercel/ai-chatbot.git`

Esto te permite trabajar sobre tu fork, pero también traer actualizaciones de Vercel.

Puedes comprobarlo con:

```bash
cd packages/ai-chatbot
git remote -v
```

---

## 🔄 Mantener tu fork actualizado

1. **Ir a la carpeta del chatbot:**

   ```bash
   cd packages/ai-chatbot
   ```

2. **Traer los últimos cambios del repo oficial (Vercel):**

   ```bash
   git fetch upstream
   ```

3. **Actualizar tu rama `main`:**

   ```bash
   git checkout main
   git merge upstream/main
   ```

   (Si prefieres un historial limpio, puedes usar `git rebase upstream/main`).

4. **Enviar los cambios a tu fork (GitHub personal):**

   ```bash
   git push origin main
   ```

---

## 🚀 Usar el chatbot en tu app

En tu aplicación `apps/web` puedes importar componentes desde tu fork:

```tsx
import { Chat } from 'ai-chatbot/components/chat';

export default function Page() {
  return <Chat />;
}
```

Si hiciste cambios en los componentes del chatbot, solo tienes que volver a correr tu app y verlos reflejados.

---

## 🛠️ Script de actualización automática

Para que no tengas que escribir todos los comandos cada vez, puedes usar un script.

En la raíz de tu monorepo crea `scripts/update-ai-chatbot.sh`:

```bash
#!/bin/bash
set -e

cd packages/ai-chatbot

echo "📥 Fetching upstream..."
git fetch upstream

echo "🔄 Merging upstream/main into main..."
git checkout main
git merge upstream/main

echo "📤 Pushing to your fork..."
git push origin main

cd ../..
echo "✅ ai-chatbot updated!"
```

Hazlo ejecutable:

```bash
chmod +x scripts/update-ai-chatbot.sh
```

Y úsalo así:

```bash
./scripts/update-ai-chatbot.sh
```

---

## ⚠️ Consejos prácticos para principiantes

- Siempre trabaja en una **rama separada** cuando hagas cambios importantes. Ejemplo:

  ```bash
  git checkout -b feature/mejora-chat-ui
  ```

- Haz `commit` seguido, con mensajes claros:

  ```bash
  git add .
  git commit -m "Agregué nuevo estilo al input del chat"
  ```

- Si al actualizar desde `upstream` aparecen conflictos:
  - Git te mostrará los archivos en conflicto.
  - Abre esos archivos, resuelve los cambios manualmente.
  - Luego:

    ```bash
    git add <archivo>
    git commit
    ```

- Nunca borres la carpeta `.git` dentro de `packages/ai-chatbot`.
- Si algo sale mal, siempre puedes volver a un estado estable con:

  ```bash
  git reset --hard origin/main
  ```

---

## ✅ Resumen

- `origin` = tu fork. `upstream` = repo oficial.
- Usa `git fetch upstream` + `git merge upstream/main` para traer mejoras.
- Haz `git push origin main` para mantener tu fork en GitHub sincronizado.
- Usa el script `update-ai-chatbot.sh` para simplificar.
- Haz commits claros y resuelve conflictos con calma.

Con esto puedes mantener el chatbot actualizado y adaptado a tu proyecto sin problemas.

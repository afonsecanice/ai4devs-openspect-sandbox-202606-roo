# OpenSpec Sandbox — Ejercicio Spec-Driven Development

Aquí practicas la instalación, la exploración guiada del framework y aplicas una vez más los **3 Pilares** (Herramienta, Contexto, Prompt) antes de la sesión en vivo.

---

## 🎯 Objetivo

Llegar a la sesión con OpenSpec **instalado y funcionando**, habiendo aplicado los 3 Pilares de forma consciente en una micro-tarea y registrado tus primeras observaciones del framework.

---

## ✅ Prerrequisitos

- **Node.js v20.19.0 o superior** (requisito de OpenSpec)
- Acceso a un copiloto de IA de tu elección (Claude, Cursor, Copilot…)

Verifica tu versión antes de empezar:

```bash
node --version
```

Si es inferior a 20.19.0, actualízala con `nvm`, el instalador oficial o el gestor que prefieras.

---

## 🚀 Cómo empezar

> Si llegaste aquí con **"Use this template"**, ya tienes tu copia limpia. Clónala y trabaja sobre ella.

```bash
git clone <tu-repo>
cd <tu-repo>
```

### 1. Instala la CLI de OpenSpec

```bash
npm install -g @fission-ai/openspec@latest
openspec --version   # debe responder con un número de versión
```

### 2. Inicializa OpenSpec en el sandbox

```bash
openspec init
```

En el asistente: selecciona tu copiloto (Claude Code, Cursor, Copilot…) y **acepta los defaults** del resto de prompts.

### 3. Verifica la estructura

```bash
ls -la
ls -R openspec/
```

Deberías ver el directorio `openspec/`, la carpeta de skills de tu copiloto (`.claude/skills/` o equivalente) y posiblemente `openspec/project.md`.

### 4. Explora (no escribas specs todavía)

- Lee `openspec/project.md` — la "constitución" del proyecto.
- Recorre el árbol de `openspec/`: ¿dónde van las propuestas? ¿los specs? ¿lo archivado?
- Date una vuelta por el workflow **propose → apply → archive** (OPSX) en la doc oficial. Solo familiarízate con el vocabulario.

---

## 📦 Entregable

Completa el archivo [`ENTREGA.md`](./ENTREGA.md) con:

1. **Evidencia** de que OpenSpec está operativo — pega el output de `openspec --version` y de `ls -R openspec/`.
2. **Plantilla de los 3 Pilares** rellenada con tu micro-tarea.
3. **3 observaciones** de la exploración.

La propia estructura `openspec/` commiteada en el repo cuenta como prueba viva de la instalación. Cuando termines:

```bash
git add .
git commit -m "Ejercicio OpenSpec completado"
git push
```

Comparte el link de tu repo en el canal del grupo.

---

## 🗂️ Estructura esperada al terminar

```
.
├── openspec/              # generado por openspec init (evidencia)
├── .claude/skills/        # o el equivalente de tu copiloto
├── ENTREGA.md             # tu entregable
└── README.md
```

---

## 📚 Recursos

- **Doc oficial:** https://openspec.pro
- **Repo:** `fission-ai/openspec` en GitHub
- **Guía en español:** *Spec Driven Development con OpenSpec* en webreactiva.com
- **Vídeo (opcional):** "Getting Started with OpenSpec | Spec Driven Development | Setup Tutorial" en YouTube

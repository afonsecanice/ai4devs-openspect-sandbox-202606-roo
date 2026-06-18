# Ejercicio previo — Instalar SDD en un proyecto limpio

**Alumno:** Ariel Fonseca Sandoval (`afonsecanice`)  
**Repositorio:** [afonsecanice/ai4devs-openspect-sandbox-202606-roo](https://github.com/afonsecanice/ai4devs-openspect-sandbox-202606-roo)  
**Implementación Parte A (local):** `D:\LIDR\Modulo2\ParteA`  
**Sandbox OpenSpec (local):** `D:\LIDR\Modulo2\ParteB\test-app\openspec-sandbox`

---

## Parte A — Repaso de los 3 Pilares

*Micro-tarea:* Crear una página web para convertir un JSON array a formato TOON (Token-Oriented Object Notation).

*Pilar 1 — Herramienta:* ¿Cuál eliges?

Cursor (implementación) + Claude (diseño del prompt y contexto).

¿Por qué esta y no otra?

Cursor permite iterar rápido sobre código Next.js/TypeScript, configurar agentes y skills con poca fricción. Claude ayuda a estructurar prompts largos con reglas de negocio (formato TOON). No usé Copilot + VS Code porque la configuración de agentes y el flujo spec→código me resultó más directo en Cursor para esta micro-tarea.

*Pilar 2 — Contexto:* ¿Qué información estás aportando?

- Stack: Next.js 14, App Router, TypeScript strict, CSS Modules.
- Archivos: `app/page.tsx`, `app/page.module.css`, `lib/convertToToon.ts`.
- Comportamiento: textarea JSON, botón **Procesar**, panel TOON read-only, botón **Copy**.
- Reglas TOON: header `[N]:`, serialización de valores, orden de campos, omisión de `null`.
- Validación: el input debe ser un array JSON; cada elemento debe ser un objeto plano; errores inline en rojo.
- Ejemplos de entrada JSON y salida TOON esperada.

¿Hay algo del contexto que has decidido omitir conscientemente?

Sí: diseño visual detallado (colores, branding), pruebas unitarias, formateo previo del JSON pegado y despliegue a producción.

*Pilar 3 — Prompt:* ¿Cómo lo estructuras?

Por secciones: Task, Tech, TOON Format Rules, Behavior, File structure, ejemplos input/output, pseudocódigo de `convertToToon.ts`, layout UI y constraints. Salida esperada: archivos listos para copiar en un proyecto Next.js.

**Prompt final:** (archivo completo en `D:\LIDR\Modulo2\ParteA\json-to-tons-prompt.md`)

> **Task:** Build a single-page Next.js 14 (App Router) app in TypeScript that lets the user paste a JSON array, click **Procesar**, and see it converted to **TOON** side-by-side. Validate array + plain objects before conversion; reject invalid shapes with inline error.
>
> **Tech:** Next.js 14, App Router, TypeScript strict, CSS Modules, `app/page.tsx`, `app/page.module.css`, `lib/convertToToon.ts`.
>
> **Behavior:** textarea JSON, botón Procesar (disabled si vacío), panel TOON read-only, botón Copy, errores inline en rojo.
>
> **Constraints:** sin `any`, `convertToToon` puro, sin dependencias extra.

*Resultado:* ¿Funcionó a la primera o tuviste que iterar?

Funcionó a la primera ejecución. La app corre en `D:\LIDR\Modulo2\ParteA` con validación de array y objetos planos en `page.tsx`.

Una mejora que harías si volvieras a hacerlo:

- Agregar pruebas unitarias para `convertToToon.ts`.
- Mejorar el diseño visual.
- Añadir validador y formateador de JSON antes de convertir.

---

## Parte B — Instalación de OpenSpec en sandbox

### Prerrequisitos verificados

```bash
node --version
```

```text
v22.16.0
```

(Node ≥ v20.19.0 — requisito cumplido.)

### Paso 2 — CLI instalada

```bash
npm install -g @fission-ai/openspec@latest
openspec --version
```

```text
1.4.1
```

### Paso 3 — Inicialización en sandbox

Ejecutado `openspec init` en `D:\LIDR\Modulo2\ParteB\test-app\openspec-sandbox`.

- Asistente elegido: **Cursor** (skills en `.cursor/`).
- También generó `.claude/` (defaults del init).
- Valores por defecto aceptados en el resto de prompts.

### Evidencia — estructura del proyecto (`ls`)

Raíz del sandbox:

```text
D:\LIDR\Modulo2\ParteB\test-app\openspec-sandbox
├── .claude/
├── .cursor/
├── openspec/
└── ENTREGA.md
```

Árbol interno de `openspec/`:

```text
openspec/
├── config.yaml
├── changes/
│   └── archive/
└── specs/
```

Estructura commiteada en este repositorio (fork):

```text
.
├── .claude/
│   ├── commands/opsx/     (apply, archive, explore, propose, sync)
│   └── skills/            (openspec-*)
├── .cursor/
│   ├── commands/          (opsx-*)
│   └── skills/            (openspec-*)
├── openspec/
│   ├── config.yaml
│   ├── changes/archive/
│   └── specs/
├── ENTREGA.md
└── README.md
```

### Paso 4 — Exploración guiada (resumen)

- **Constitución del proyecto:** en OpenSpec 1.4.1 el archivo principal es `openspec/config.yaml` (no se generó `openspec/project.md` en esta versión).
- **Propuestas:** `openspec/changes/` (activas) y `openspec/changes/archive/` (archivadas).
- **Specs:** `openspec/specs/`.
- **Workflow OPSX:** propose → apply → archive, reflejado en comandos/skills `opsx-propose`, `opsx-apply`, `opsx-archive`, `opsx-explore`, `opsx-sync`.

---

## Observaciones de la exploración OpenSpec

1. Aún no entiendo dónde se configura qué tecnologías se usan para que OpenSpec pueda entender mejor mi desarrollo. En este caso estoy usando Next.js + TypeScript.
2. No veo algo donde se use un grafo para documentar los archivos `.md` y poder visualizarlos en herramientas como Obsidian o Logseq.
3. No veo uso de agentes.
4. Me gustan los comandos. Son básicos, pero potentes: aplicar, archivar, explorar, proponer y sincronizar.
5. No veo algo de validación en los comandos (validar la tarea).

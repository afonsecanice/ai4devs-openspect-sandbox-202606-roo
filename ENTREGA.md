# Entrega — OpenSpec Sandbox

*Micro-tarea:* Crea un pagina web para formatear un json a formato TOON / Token-Oriented Object Notation

*Pilar 1 — Herramienta:* Uso de herramientas Cursor para generar el trabajo, y claude para generar el contexto.
por que son herramientas top y puedo generar agentes, y la configuracon es mas sencilla que copilot y vscode.

*Pilar 2 — Contexto:* Generar un web Next.js + TypeScript+html+css, con un boton para procesar, pego el json y al dar click en procesar cambia a TOON. El input debe ser un array JSON de objetos planos; si hay numeros, null o strings sueltos, mostrar error inline sin crashear.

Omito colores, diseño especificco de la ventana y pruebas unitarias

*Pilar 3 — Prompt:*

## Prompt: JSON → TOON Converter — Next.js + TypeScript

### Task

Build a single-page Next.js 14 (App Router) app in TypeScript that lets the user paste a JSON array, click **Procesar**, and see it converted to **TOON (Token-Oriented Object Notation)** in a read-only output panel side-by-side. Before conversion, validate that the parsed payload is an array and that every element is a plain object (not `null`, a number, a string, or a nested array); reject invalid shapes with an inline error instead of calling `Object.entries` on non-objects.

### Tech

- Next.js 14, App Router, TypeScript strict
- Plain CSS Modules (no Tailwind, no UI library)
- Single page: `app/page.tsx` + `app/page.module.css`
- Pure utility: `lib/convertToToon.ts`

### TOON Format Rules

Given a JSON array of N objects:

1. **Header:** `[N]:` — the count of items, followed by colon.
2. **Each object** becomes a block of lines:
   - First field: `  - key: value` (2 spaces + dash + space)
   - Remaining fields: `    key: value` (4 spaces)
3. **Value serialization:**
   - `null` or missing field → **omit the line entirely**
   - `boolean` → lowercase literal: `true` / `false`
   - `number` → as-is, no quotes
   - `string` containing `:` → wrap in double quotes: `"2020-05-09T19:08:00"`
   - `string` without `:` → no quotes: `posArea`, `Memo Test 2`, `test.Area/Post`
4. **Field order:** preserve original JSON key order.
5. **No trailing commas, no braces, no brackets** inside the body.

### Behavior

1. Left panel: `<textarea>` — user pastes raw JSON array.
2. **Procesar** button (centered, disabled when textarea is empty).
3. Right panel: read-only `<pre>` — shows TOON output.
4. If JSON is invalid **or** the parsed array contains non-object elements (e.g. numbers, `null`, strings), show inline error below textarea (red text, no alert/modal).
5. Output panel shows a **Copy** button (copies TOON text to clipboard).

### File structure

```text
app/
  page.tsx
  page.module.css
lib/
  convertToToon.ts
```

### Example — JSON input

```json
[{"AreaID":1,"Name":"Memo Test 2","IsActive":true,"CreatedBy":"posArea","ModifiedBy":"test.Area\/Post","CreatedUserID":1,"ModifiedUserID":27642,"CreatedDate":"2020-05-09T19:08:00","ModifiedDate":"2024-06-21T14:02:44.237"},{"AreaID":2,"Name":"Configuracion","IsActive":true,"CreatedBy":"posArea","CreatedUserID":1,"CreatedDate":"2020-05-09T19:08:00"},{"AreaID":3,"Name":"Operacion","IsActive":true,"CreatedBy":"posArea","CreatedUserID":1,"CreatedDate":"2020-05-09T23:16:28.697"}]
```

### Example — expected TOON output

```text
[3]:
  - AreaID: 1
    Name: Memo Test 2
    IsActive: true
    CreatedBy: posArea
    ModifiedBy: test.Area/Post
    CreatedUserID: 1
    ModifiedUserID: 27642
    CreatedDate: "2020-05-09T19:08:00"
    ModifiedDate: "2024-06-21T14:02:44.237"
  - AreaID: 2
    Name: Configuracion
    IsActive: true
    CreatedBy: posArea
    CreatedUserID: 1
    CreatedDate: "2020-05-09T19:08:00"
  - AreaID: 3
    Name: Operacion
    IsActive: true
    CreatedBy: posArea
    CreatedUserID: 1
    CreatedDate: "2020-05-09T23:16:28.697"
```

### Key conversion logic (pseudocode for `convertToToon.ts`)

```typescript
function formatValue(value: unknown): string | null
  if value === null or value === undefined → return null   // omit line
  if typeof boolean → return value.toString()             // "true"/"false"
  if typeof number  → return value.toString()
  if typeof string  → return value.includes(":") ? `"${value}"` : value

function convertToToon(json: unknown[]): string
  for each obj in json
    if typeof obj !== "object" or obj === null or Array.isArray(obj) →
      throw new Error("Array elements must be plain objects")
  lines = [`[${json.length}]:`]
  for each obj in json
    entries = Object.entries(obj).filter(([,v]) => v !== null && v !== undefined)
    entries.forEach(([key, val], i) =>
      prefix = i === 0 ? "  - " : "    "
      formatted = formatValue(val)
      if formatted !== null → lines.push(`${prefix}${key}: ${formatted}`)
    )
  return lines.join("\n")
```

`page.tsx` must catch errors from `convertToToon` and show them inline (e.g. input `[1, null]`).

### UI layout

```text
┌──────────────────────────────────────────────────────────────┐
│  JSON Input            [ Procesar ]       TOON Output  [Copy]│
│  ┌─────────────────┐                   ┌──────────────────┐  │
│  │  <textarea>     │                   │  <pre>           │  │
│  │                 │                   │  [3]:            │  │
│  └─────────────────┘                   │    - AreaID: 1   │  │
│  ← error if invalid JSON               │    ...           │  │
│                                        └──────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

### Constraints

- No `any` types — use `unknown` + type guards.
- `convertToToon.ts` must be a pure function with no side effects.
- Button disabled while textarea is empty.
- No external dependencies beyond Next.js defaults.

*Resultado:* Funciono a la 1 ejecucion

*Mejoras:*

- Agregar pruebas unitarias
- Un mejor diseño
- Un Validador de json
- Un formateo de Json

## Entregable B

### openspec --version

- 1.4.1

### Directorio

```text
D:.
│   ENTREGA.md
│
├───.claude
│   ├───commands
│   │   └───opsx
│   │           apply.md
│   │           archive.md
│   │           explore.md
│   │           propose.md
│   │           sync.md
│   │
│   └───skills
│       ├───openspec-apply-change
│       │       SKILL.md
│       │
│       ├───openspec-archive-change
│       │       SKILL.md
│       │
│       ├───openspec-explore
│       │       SKILL.md
│       │
│       ├───openspec-propose
│       │       SKILL.md
│       │
│       └───openspec-sync-specs
│               SKILL.md
│
├───.cursor
│   ├───commands
│   │       opsx-apply.md
│   │       opsx-archive.md
│   │       opsx-explore.md
│   │       opsx-propose.md
│   │       opsx-sync.md
│   │
│   └───skills
│       ├───openspec-apply-change
│       │       SKILL.md
│       │
│       ├───openspec-archive-change
│       │       SKILL.md
│       │
│       ├───openspec-explore
│       │       SKILL.md
│       │
│       ├───openspec-propose
│       │       SKILL.md
│       │
│       └───openspec-sync-specs
│               SKILL.md
│
└───openspec
    │   config.yaml
    │
    ├───changes
    │   └───archive
    └───specs
```

### 3 Observaciones

1. Aún no entiendo dónde se configura qué tecnologías se usan para que OpenSpec pueda entender mejor mi desarrollo. En este caso estoy usando Next.js + TypeScript.
2. No veo algo donde se use un grafo para documentar los archivos .md y poder visualizarlos en herramientas como Obsidian o Logseq.
3. No veo uso de agentes.
4. Me gustan los comandos. Son básicos, pero potentes: aplicar, archivar, explorar , proponer y sincronizar.
5. No veo algo de validación en los comandos (validar la tarea).

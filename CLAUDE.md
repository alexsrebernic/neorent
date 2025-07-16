# CLAUDE.md - Reglas Maestras Unificadas

## 🌐 Reglas Globales

### Principios Fundamentales
- **Server-first**: Minimizar JS cliente, preferir Server Components/Actions
- **Type-safe**: TypeScript estricto siempre
- **Seguridad**: RLS obligatorio, validación server-side, nunca exponer keys
- **Selección de modelo**: Gemini Pro (arquitectura), Flash (validaciones), O3 (debugging)

### Estructura de Respuestas
```markdown
1. Código COMPLETO y funcional (no fragmentos)
2. Path exacto del archivo
3. Imports necesarios incluidos
4. Comentarios del "por qué", no del "qué"
```

## 🛠️ Herramientas de Desarrollo

### zen-mcp-server
```bash
# Análisis arquitectónico
zen analyze src/ --model=gemini:pro --thinking=high

# Debug sistemático
zen debug --model=o3 --context=backend/

# Revisión pre-merge
zen codereview --severity=high+ --model=flash
```


```sql
-- Tablas SIEMPRE con RLS
ALTER TABLE items ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own" ON items
  FOR SELECT USING (auth.uid() = user_id);
```

## 📋 Gestión de Proyectos

### Task Master
```bash
# Flujo estándar
task-master parse-prd docs/prd.txt    # 1. Generar tareas
task-master next                       # 2. Siguiente tarea
task-master set-status --id=X --status=done  # 3. Actualizar
task-master expand --id=X              # 4. Dividir si complejo
```

### NEORENT - Referencias de Contexto
| Necesidad | Archivo | Cuándo Usar |
|-----------|---------|-------------|
| User Stories & UX | `@ai-docs/USER-STORIES.md` | Implementar features |
| Lista de Features | `@ai-docs/FEATURES.md` | Desarrollo de funcionalidades |
| Estilos y Diseño | `@ai-docs/STYLES.md` | UI/UX desarrollo |
| Especificaciones Técnicas | `@ai-docs/TECH-SPECS.md` | Decisiones técnicas |
| Documento de Requisitos | `@ai-docs/PDR.md` | Contexto general del producto |

## ⚡ Referencia Rápida

| Contexto | Herramienta | Comando/Patrón |
|----------|-------------|----------------|
| Arquitectura | zen + gemini pro | `zen analyze --thinking=high` |
| Debug complejo | zen + o3 | `zen debug --model=o3` |
| Validación rápida | zen + flash | `zen validate --model=flash` |
| Nueva feature | task-master | `parse-prd → next → implement` |
| Auth Next.js | Supabase | `getUser()` nunca `getSession()` |
| Mutaciones | Server Actions | `'use server'` + `revalidatePath()` |

## 🔄 Precedencia y Compatibilidad

1. **Seguridad > Performance > DX**
2. **Específico > General**: Reglas de herramienta > globales
3. **Server > Client**: Default a Server Components
4. **Modelo por tarea**:
   - Planificación → Opus 4 (tú)
   - Implementación → Sonnet 4
   - Análisis masivo → Gemini (1M tokens)
   - Debug puro → O3

## ❌ Prohibiciones Absolutas

- `any` sin justificación
- RLS deshabilitado
- `getSession()` server-side
- Client Components innecesarios
- Exponer API keys
- Modificar auth base del template
- Crear carpeta `pages/` en Next.js 15
- Rutas dinámicas sin nesting explícito
- Params directos sin `React.use()`
- URLs hardcodeadas (usar `lib/routes.ts`)

---
*Optimizado para zen-mcp v2.0+, Task Master v2.4.2+, Next.js 15 + Supabase, NEORENT MVP*

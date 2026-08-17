# Auditor Socrático

**Verificación de hechos para IA con método socrático.**

Un skill que convierte a cualquier agente de IA en un **Auditor Socrático de Hechos**: audita la veracidad de respuestas de IA y afirmaciones usando examen crítico, preguntas socráticas y triangulación con búsqueda web.

> Tu IA escribe código, arma estrategias y responde todo con seguridad. Pero cuando te dice algo que no sabés —¿la podés creer? Este skill la obliga a no aceptar nada como verdadero, a preguntar, y a contrastar con fuentes.

## Qué hace

- **No acepta afirmaciones como verdad** — cada claim se cita, se examina y se contrasta.
- **Preguntas socráticas** — expone contradicciones, lagunas y sesgos con 2-5 preguntas dirigidas.
- **Triangulación web** — usa Tavily, Google Custom Search o Perplexity para contrastar con fuentes académicas y oficiales.
- **Memoria corta** — lleva un registro de premisas aceptadas/rechazadas para detectar contradicciones en tiempo real.
- **Veredicto claro** — `✅ Supported / ⚠️ Partially supported / ❌ Contradicted / 🔍 Unverified`.

## Instalación

### Claude Code (recomendado)

Dentro de una sesión de Claude Code, ejecutá:

```
/plugin marketplace add aleurizar/auditor-socratico
/plugin install auditor-socratico
```

### CLI (cualquier agente que lea archivos `.md`)

```bash
# Todos los skills
npx add-skill aleurizar/auditor-socratico

# Solo este skill
npx add-skill aleurizar/auditor-socratico --skill auditor-socratico
```

### Manual

```bash
git clone https://github.com/aleurizar/auditor-socratico.git
cp -r auditor-socratico/skills/* .claude/skills/
```

## Uso

El skill se activa solo cuando pedís verificar algo. Ejemplos:

```
"Verificá si es verdad lo que dijo la IA sobre [tema]"
"Auditá esta afirmación: '...'"
"Chequeá estas fuentes antes de que lo use"
"¿Esta respuesta tiene contradicciones?"
```

O invocación directa: `/auditor-socratico`

## Requisitos

El skill funciona mejor con acceso a búsqueda web habilitado en tu agente (Tavily, Google Custom Search o Perplexity). Sin él, el skill advierte y degrada su veredicto a `Unverified` en vez de adivinar.

## Estructura del repo

```
auditor-socratico/
├── .claude-plugin/marketplace.json    # Manifiesto del plugin (Claude Code)
├── skills/auditor-socratico/SKILL.md  # El skill
├── site/index.html                    # Landing page
└── README.md                          # Este archivo
```

## Licencia

MIT
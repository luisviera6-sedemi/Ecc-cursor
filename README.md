# ECC en Cursor

Este repositorio es el proyecto de Cursor de Luis con [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC) instalado en local.

## Qué es ECC

ECC es el arnés de agentes de [affaan-m/ECC](https://github.com/affaan-m/ECC): reglas, agentes, comandos y skills para planificar, implementar, revisar y comprobar código. La fuente oficial es solo ese repositorio (y los canales que el propio proyecto publica: npm `ecc-universal`, la GitHub App y [ecc.tools](https://ecc.tools)). No uses mirrors.

Aquí está el adaptador de **Cursor**, perfil **minimal**, ECC **2.2.3** (`b38be6a7f6350964d59328b52f0624e1d1d552d1`). El comando, ejecutado con este repo como directorio de trabajo y el checkout de ECC solo como origen, fue:

```bash
./install.sh --profile minimal --target cursor
```

Ese perfil trae reglas, agentes, comandos, configuración de plataforma y skills de calidad. No instala el runtime de hooks y no añade el plugin de Claude Code (`ecc@ecc`). No hay claves de API en este repo.

## Cómo abrirlo en Cursor

1. Clona este repositorio.
2. En Cursor: **File → Open Folder** y elige la carpeta del clon.
3. Abre un chat de Agent en esa carpeta. Cursor lee el proyecto local `.cursor/` (reglas, agentes, skills y comandos).
4. El comportamiento exacto de agentes y comandos depende de la versión de Cursor. Si un agente no aparece en la lista, igual puedes pedirlo por nombre: los archivos están en `.cursor/agents/ecc-*.md`.

## Qué quedó instalado

| Ruta | Contenido |
|---|---|
| `.cursor/rules/` | Reglas `.mdc` (comunes y por lenguaje) |
| `.cursor/agents/` | Agentes `ecc-*.md` (planner, tdd-guide, security-reviewer, code-reviewer, …) |
| `.cursor/commands/` | Comandos (`plan`, `build-fix`, `security-scan`, `test-coverage`, …) |
| `.cursor/skills/` | Skills de flujo, entre ellas `tdd-workflow`, `e2e-testing`, `verification-loop` |
| `.cursor/mcp.json` | Único MCP activo: `chrome-devtools` (sin secretos) |
| `.cursor/mcp-configs/` | Catálogo de ejemplo. Los valores son placeholders de ECC, no credenciales |
| `.cursor/scripts/` | Utilidades que el perfil minimal copia con los comandos |
| `.cursor/ecc-install-state.json` | Registro de esta instalación (perfil, versión, hashes) |

El estado de instalación guarda las rutas absolutas de la máquina donde se aplicó. Si más adelante vuelves a ejecutar el instalador dentro de tu clon, ECC actualiza ese registro. No reescribas el JSON a mano.

## Siguientes prompts

Empieza por planificar. El comando `/plan` espera tu confirmación antes de tocar código.

**Plan**

```text
/plan Describe la funcionalidad. Reformula requisitos, riesgos y fases. No escribas código hasta que yo confirme el plan.
```

**Build**

```text
El plan está confirmado. Implementa con la skill tdd-workflow: primero un test que falle, luego el mínimo código que pase y después el refactor. Si el build ya está roto, usa /build-fix.
```

**Secure**

```text
/security-scan Revisa este cambio con el agente security-reviewer: secretos, entradas, auth y datos sensibles. No inventes claves ni las escribas en el repo.
```

**Test**

```text
/test-coverage Comprueba cobertura y casos límite de este cambio. Si falta un flujo crítico de usuario, usa la skill e2e-testing.
```

## Autonomía acotada: modo seguro primero

Empieza en modo seguro. Deja que Cursor pida confirmación antes de ampliar permisos. ECC trata la autonomía como algo con frenos: el flujo sigue necesitando aprobación humana antes de una shell fuera del sandbox, salida a la red, leer rutas con secretos, escribir fuera del repo, o lanzar un workflow o un despliegue. Auto-aprobar todo eso no es autonomía; es quitarse los frenos.

Este perfil no instala hooks. Activarlos sería otro paso explícito (`--enable-hooks` o `--no-hooks`), no parte de esta instalación. No apiles encima una instalación del plugin de Claude.

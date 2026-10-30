# market-analysis

**English:** [README.md](README.md)

Un skill para Claude Code que produce análisis de mercado para alta dirección con el rigor de una consultoría estratégica de primer nivel: tamaño y crecimiento con fuentes, segmentación MECE, motores y frenos, panorama competitivo, oportunidades y riesgos, y una recomendación honesta que puede ser "no entrar" o "todavía no". El resultado es un informe en pirámide con títulos de acción, introducción SCQ, nivel de confianza declarado, una sección de "argumentos en contra" y tres anexos (tabla de datos clave, datos no disponibles, preguntas abiertas). Se entrega en Word por defecto.

El skill está escrito en español. Funciona con peticiones en español o inglés y para cualquier sector y geografía.

## Qué lo hace distinto

- **Integridad de datos por encima de todo.** Cada afirmación relevante va etiquetada como `[HECHO]` (publicado por fuente identificable, con fecha), `[ESTIMACIÓN]` (proyección de un tercero, con el autor) o `[INFERENCIA]` (conclusión propia, con el razonamiento). Lo que no se ha visto en una fuente consultada en la sesión se escribe como `[DATO NO DISPONIBLE]` con una nota de qué se buscó. Nunca inventa fuentes, títulos, autores, fechas ni URLs.
- **Honesto por diseño.** Las malas noticias se dicen de forma directa. La recomendación puede ser negativa. Cada informe incluye el caso más sólido contra su propia recomendación y las señales que demostrarían que está equivocada.
- **Hipótesis antes de buscar.** El skill escribe 3-5 hipótesis que responderían a la pregunta central y después busca datos para refutarlas, no para confirmarlas.
- **Formato pirámide.** El resumen ejecutivo responde a la pregunta en su primer párrafo. Los títulos de sección son frases completas con la conclusión; leídos seguidos, cuentan la historia entera. Cada cifra lleva su "¿y qué?" para la decisión del lector.
- **Recogida guiada del encargo.** El encargo se pregunta una cosa cada vez mediante el cuadro modal de preguntas de Claude Code (`AskUserQuestion`): mercado y alcance, geografías, quién es el lector, qué decisión depende del informe, pregunta central, horizonte y formato. Las opciones se adaptan a las respuestas anteriores y se saltan las preguntas ya contestadas en el mensaje.

## Instalación

### Claude Code (nivel de usuario, todos los proyectos)

```bash
git clone https://github.com/Claudio-C/market-analysis-skill.git
cp -r market-analysis-skill/market-analysis ~/.claude/skills/
```

### Claude Code (nivel de proyecto)

```bash
cp -r market-analysis-skill/market-analysis .claude/skills/
```

### Archivo `.skill` empaquetado

El archivo `dist/market-analysis.skill` de este repositorio se puede importar allí donde Claude admita paquetes de skills.

Reinicia Claude Code después de instalar. El skill se activa solo con peticiones como "estudio de mercado", "market sizing", "¿entramos en X?", "TAM/SAM/SOM", "panorama competitivo" o "informe para el comité", y se puede forzar con `/market-analysis`.

## Uso

Escribe una petición, o invoca el skill directamente:

```
/market-analysis
```

También puedes pasar una URL como argumento. El skill lee primero la página (por ejemplo, la de servicios de tu empresa) y adapta las preguntas del encargo a ese negocio:

```
/market-analysis https://ejemplo.com/servicios
```

Después pregunta, un cuadro cada vez:

| # | Parámetro | Ejemplo |
|---|---|---|
| 1 | Mercado y alcance | "Ciberseguridad: servicios gestionados, hardware de seguridad, productos con IA" |
| 2 | Geografías (selección múltiple) | Global, Europa, España, MENA, LATAM |
| 3 | Quién es el lector | Director de estrategia, inversor, emprendedor, consejo |
| 4 | Qué decisión depende del informe | Entrar o no, elegir segmento, valorar una adquisición |
| 5 | Pregunta central | "¿Es España un mercado suficiente para arrancar y escalar después?" |
| 6 | Horizonte y formato | 3-5 años, Word (por defecto) · Markdown |

Tras la última respuesta resume el encargo y arranca: hipótesis, búsqueda web con fuentes primarias primero, tabla de datos que se va rellenando durante la investigación, informe en pirámide y revisión final que comprueba que cada cifra tiene fuente y fecha.

## Estructura del informe

```
Título
Introducción (SCQ: Situación, Complicación, Pregunta)
Resumen ejecutivo (respuesta + recomendación, nivel de confianza, 3-4 argumentos clave)
1. [Título de acción sobre tamaño y crecimiento]
2. [Título de acción sobre segmentación]
3. [Título de acción sobre motores y frenos]
4. [Título de acción sobre competencia]
5. [Título de acción sobre oportunidades y riesgos para el lector]
6. Recomendación y próximos pasos
   Argumentos en contra de esta recomendación
   Qué tendría que ocurrir para que esta recomendación sea incorrecta
Anexo 1. Tabla de datos clave: dato | valor | fuente | fecha | tipo | confianza
Anexo 2. Datos no disponibles: qué se buscó y no se encontró
Anexo 3. Preguntas abiertas antes de decidir
Fuentes consultadas (con URL)
```

La salida en Word usa estilos de título reales, tablas nativas y numeración de páginas. Requiere el skill `docx` (incluido en los skills de Anthropic para Claude Code); sin él, el informe se entrega en Markdown.

## Contenido del repositorio

```
market-analysis/
├── SKILL.md                      # el skill: encargo, cobertura, reglas de integridad, formato, proceso, entrega
├── references/
│   └── report-template.md        # estructura exacta del informe, ejemplos de títulos buenos y malos, checklist final
└── evals/                        # prompts de prueba (todavía no ejecutados como benchmark formal)
    ├── ciberseguridad-espana-nuevo-entrante/prompt.md
    └── no-entrar-mercado-saturado/prompt.md
dist/
└── market-analysis.skill         # skill empaquetado
```

## Consejos

- La investigación funciona mejor cuando el modelo puede hacer búsquedas web y, en encargos grandes, delegar la investigación por geografía o tema a subagentes y quedarse solo con los hallazgos verificados.
- Espera huecos declarados. Un informe con entradas `[DATO NO DISPONIBLE]` visibles está funcionando como debe; uno sin huecos y sin fuentes, no.
- Si la recomendación es "no entrar", lee la sección de argumentos en contra antes de discutirla: está escrita para ser el argumento más fuerte del otro lado.

## Licencia

MIT. Ver [LICENSE](LICENSE).

---
name: cv-ats-optimizer
description: Optimiza curriculums (CV) para superar filtros ATS (Applicant Tracking Systems) frente a una vacante especifica, usando como fuente el CV maestro del usuario (references/cv-maestro.md) o el CV que el usuario suba. Usala siempre que el usuario pegue o mencione una vacante, oferta de trabajo o job description y quiera un CV adaptado, o cuando suba un CV y quiera mejorarlo, pasar el ATS o aumentar su match. Frases tipicas son 'optimiza mi CV', 'adaptalo a este puesto', 'aqui esta la vacante' o 'quiero aplicar a esto'. Entrega dos cosas. Primero, un CV optimizado en PDF de texto seleccionable, una sola columna y parseable. Segundo, un informe con puntuacion, analisis de keywords y cambios realizados. Activala aunque el usuario no diga literalmente la palabra ATS.
---

# Optimizador de CV para ATS

Esta habilidad transforma el CV de una persona en una versión optimizada para sistemas ATS **contra una vacante concreta**, y entrega un informe explicando qué se cambió y por qué.

## Qué es un ATS (contexto rápido)

Un ATS (Applicant Tracking System) es el software que las empresas usan para filtrar CVs antes de que un humano los lea. Funciona en dos fases: (1) **parsea** el archivo extrayendo texto plano y clasificándolo en campos (nombre, puesto, fechas, habilidades…), y (2) **puntúa/rankea** el CV según cuánto coincide con la vacante, sobre todo por palabras clave. Un CV puede ser excelente y aun así ser descartado si el parser no lo lee bien o si no contiene las keywords de la oferta. El objetivo de esta habilidad es resolver ambas mitades: **parseabilidad** y **coincidencia de keywords**.

## Insumos necesarios

1. **El CV fuente** — puede venir de dos sitios:
   - **CV maestro** (`references/cv-maestro.md`): la fuente de verdad del candidato, con datos de contacto, resúmenes por clúster de rol, el banco completo de bullets etiquetados, proyectos, educación, certificaciones y habilidades. Si está rellenado, úsalo por defecto y **no pidas al usuario que suba nada**.
   - **CV subido por el usuario**: si el maestro sigue siendo la plantilla vacía (contiene marcadores tipo `[TU NOMBRE COMPLETO]`), pide al usuario que suba su CV actual y trabaja desde ahí. Al terminar, **ofrécele volcar su información al maestro** para que las siguientes optimizaciones sean instantáneas.
2. **La descripción de la vacante (job description)** — el texto de la oferta a la que se postula.

Si falta la vacante, **pídela** — esta habilidad optimiza *contra un puesto concreto*, y sin ella solo podrías hacer una revisión ATS-genérica (dilo explícitamente si el usuario prefiere seguir sin vacante).

## Flujo de trabajo

Sigue estos pasos en orden. No te saltes el paso 1.

### Paso 1 — Cargar el CV fuente

**Caso normal (maestro relleno):** lee `references/cv-maestro.md` completo. Fíjate en:
- Las **etiquetas de rol** de cada bullet — selecciona el clúster que corresponda a la vacante usando la sección "Mapa rápido: rol → qué incluir".
- Los **resúmenes por clúster** — parte del que corresponda y ajusta 2–3 keywords con la redacción literal de la oferta.
- La sección **"Reglas de integridad al derivar"** al final del maestro — es vinculante.
- Las notas de estrategia del maestro (ubicación según modalidad de la vacante, qué perfiles públicos incluir solo en roles técnicos, qué formación complementaria añadir según el puesto).

**Caso alterno (el usuario subió un CV):** extrae el texto del PDF con `pdfplumber` (preserva mejor el orden de lectura) o `pdftotext -layout`:

```python
import pdfplumber
with pdfplumber.open("/mnt/user-data/uploads/cv.pdf") as pdf:
    texto = "\n".join(p.extract_text() or "" for p in pdf.pages)
print(texto)
```

**Diagnóstico de parseabilidad mientras extraes** — anota señales de que el ATS tendrá problemas:
- Si `extract_text()` devuelve poco o nada → probablemente es un **PDF de imagen** (escaneado o exportado desde Canva/InDesign como imagen). Es el peor caso: el ATS lee cero texto. Márcalo como problema crítico en el informe.
- Si el texto sale desordenado o mezclado entre secciones → probablemente hay **columnas o tablas** que rompen el orden de lectura.
- Revisa si el contacto (email/teléfono) aparece en el texto extraído; si no, puede estar en un **encabezado/pie de página**, que muchos ATS ignoran.
- Si el CV subido pertenece al mismo candidato del maestro y lo contradice, **el maestro tiene prioridad** (es la versión curada más reciente); menciona la discrepancia en el informe.

Lee `references/ats-rules.md` para la lista completa de reglas y el sistema de puntuación antes de analizar.

### Paso 2 — Analizar el CV y extraer keywords de la vacante

Haz dos análisis en paralelo:

**a) Auditoría de formato/parseabilidad** — evalúa el CV contra las reglas de `references/ats-rules.md` (columnas, tablas, fuentes, encabezados de sección estándar, formato de fechas, contacto en el cuerpo, etc.).

**b) Análisis de brecha de keywords (keyword gap)** — este es el corazón de la optimización contra vacante:
1. Extrae de la job description los términos que el ATS buscará: título del puesto, hard skills, herramientas/tecnologías, certificaciones, metodologías y frases exactas repetidas.
2. Compara con lo que aparece en el CV fuente (el maestro contiene MÁS de lo que cabe en un CV: el trabajo es seleccionar, no solo añadir).
3. Clasifica cada keyword en: **presente**, **ausente pero el candidato la cumple** (hay que añadirla usando la redacción literal de la oferta), o **ausente y no aplica** (no inventar experiencia — respétalo; el maestro lista las brechas conocidas del candidato).
4. Regla de acrónimos: incluye forma larga + corta la primera vez, p. ej. "Project Management Professional (PMP)", porque distintos ATS indexan formas distintas.

> **Integridad ante todo:** nunca inventes experiencia, títulos, fechas ni habilidades que el candidato no tenga. Optimizar es reordenar, reescribir y hacer visible lo que ya es cierto usando el lenguaje de la oferta — no fabricar. Si el candidato no cumple una keyword clave, dilo en el informe como brecha real, no la metas en el CV.

### Paso 3 — Generar el CV optimizado (PDF parseable)

Reescribe el CV aplicando las reglas ATS y las keywords. Usa `assets/cv_template.html` como base y conviértelo a PDF. El template ya está diseñado para ser ATS-safe (una columna, fuentes de sistema, sin tablas de maquetación, contacto en el cuerpo).

Método recomendado (produce PDF de **texto seleccionable**):

```bash
pip install weasyprint --break-system-packages -q
python -c "from weasyprint import HTML; HTML('cv.html').write_pdf('/mnt/user-data/outputs/CV_Optimizado.pdf')"
```

Si `weasyprint` no está disponible, usa `reportlab` con `SimpleDocTemplate` (ver skill `pdf`) — también genera texto seleccionable. **Nunca** generes el PDF como imagen.

**Verificación obligatoria** tras generar: vuelve a extraer el texto del PDF creado con `pdfplumber` y confirma que sale limpio y en orden (nombre, contacto, secciones). Es el equivalente al "copy-paste test": si tú no puedes extraer el texto, el ATS tampoco.

Nombra el archivo `Nombre_Apellido_CV.pdf` cuando conozcas el nombre (los ATS y reclutadores lo prefieren).

Reglas no negociables del CV generado (detalle en `references/ats-rules.md`):
- Una sola columna, orden cronológico inverso.
- Fuentes estándar (Arial, Calibri, Helvetica, Georgia), cuerpo 10–12pt.
- Encabezados de sección estándar: *Resumen profesional, Experiencia, Habilidades, Educación, Certificaciones*.
- Contacto en el cuerpo del documento, nunca en encabezado/pie.
- Sin tablas de maquetación, cuadros de texto, columnas, iconos, gráficos, barras de nivel ni foto.
- Fechas en formato consistente (p. ej. "Ene 2023 – Mar 2025").
- Viñetas de logro cuantificadas siempre que se pueda ("Reduje costos 23%").

### Paso 4 — Escribir el informe

Entrega el informe en el chat (y opcionalmente como archivo .md si el usuario lo pide). Usa **exactamente** esta estructura:

```markdown
# Informe de optimización ATS

## Puntuación
- **ATS de origen:** X/100
- **ATS optimizado:** Y/100
- **Match con la vacante:** antes Z% → después W%

## Problemas de parseabilidad detectados
(lista de lo que rompía el parser en el CV original y cómo se corrigió)

## Análisis de keywords
- **Keywords de la vacante ya presentes:** ...
- **Keywords añadidas** (que el candidato sí cumplía): ...
- **Brechas reales** (keywords que el candidato NO cumple): ...  ← honesto, no inventado

## Cambios aplicados
(bullets concretos: reestructuración, reescritura de logros, formato de fechas, etc.)

## Recomendaciones para el candidato
(acciones que solo la persona puede hacer: obtener una certificación, cuantificar un logro que falta, etc.)

## Nota sobre el formato de archivo
El CV se entrega en PDF de texto seleccionable, que los ATS modernos (Workday, Greenhouse, Lever, iCIMS) parsean bien en 2026. Si la vacante o el portal es de un sistema heredado (Taleo antiguo) o pide "Word", ofrece generar también una versión .docx equivalente.
```

Calcula las puntuaciones con la rúbrica de `references/ats-rules.md` (no inventes números; deriva cada punto de criterios verificables).

### Paso 5 — Entregar los archivos

Guarda el CV optimizado en `/mnt/user-data/outputs/` y preséntalo con `present_files`. Si generaste el informe como archivo, preséntalo también. Ofrece la versión .docx como siguiente paso si aplica.

## Recordatorio central

Dos mitades: **que el ATS lo lea** (parseabilidad) y **que el ATS lo puntúe alto** (keywords de la vacante). Optimiza ambas, con integridad absoluta sobre el contenido: haz visible lo verdadero, nunca fabriques lo falso. Al derivar desde el maestro, el CV final debe caber en **1–2 páginas**: selecciona 4–6 bullets por puesto (los del clúster correspondiente) y solo las categorías de habilidades relevantes — el maestro es un banco, no una plantilla para copiar entero.

## Mantenimiento del CV maestro

`references/cv-maestro.md` es la fuente de verdad y debe mantenerse actualizada. **La primera vez que se usa la skill viene como plantilla vacía**: si detectas marcadores tipo `[TU NOMBRE COMPLETO]`, ofrece al usuario rellenarla a partir de su CV actual y entrégale el `cv-maestro.md` completo para que lo reemplace dentro de la skill. Si durante la conversación el usuario menciona experiencia nueva, certificaciones, proyectos o correcciones que no están en el maestro:
1. Úsalas en el CV derivado de esa sesión.
2. Avísale que el maestro dentro de la skill quedó desactualizado y ofrécele generar la versión actualizada de `cv-maestro.md` (y el paquete `.skill` completo) para que la reemplace: la skill se actualiza subiendo el paquete nuevo en **Ajustes → Capacidades → Habilidades** (o guardando el archivo .skill presentado en el chat).

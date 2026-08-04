# Reglas ATS y rúbrica de puntuación (2026)

Referencia detallada para auditar y reescribir un CV. Basado en el comportamiento real de los ATS más usados (Workday, Greenhouse, Lever, iCIMS, Taleo, SmartRecruiters).

## Índice
1. Reglas de parseabilidad (formato)
2. Reglas de contenido y keywords
3. Reglas por sección
4. Formato de archivo
5. Rúbrica de puntuación (0–100)

---

## 1. Reglas de parseabilidad (formato)

Esto determina si el ATS puede *leer* el CV. Es un problema de integridad de datos, no de estética.

- **Una sola columna.** Regla más importante. Los diseños a dos columnas se leen fuera de orden en la mayoría de parsers (Taleo e iCIMS son los peores): mezclan la columna izquierda con la derecha y las habilidades/experiencia pierden contexto. Todo el texto en un flujo lineal de arriba a abajo.
- **Sin tablas para maquetar.** Las tablas se aplanan y se descolocan. Si necesitas alinear algo, usa tabuladores/espaciado del propio flujo de texto, no celdas.
- **Sin cuadros de texto (text boxes).** Muchos parsers los saltan por completo.
- **Sin encabezado/pie de página para información vital.** ~2 de cada 3 ATS ignoran el contenido de headers/footers. El **contacto (nombre, email, teléfono) va en el cuerpo**, arriba.
- **Sin gráficos, iconos, logos, fotos ni barras de nivel de habilidad.** El parser ve una imagen = hueco vacío = cero keywords. Una barra de "Excel ●●●●○" aporta cero coincidencias; escribe "Excel" como texto.
- **Fuentes estándar de sistema:** Arial, Calibri, Helvetica, Georgia, Garamond, Times New Roman. Fuentes personalizadas/decorativas pueden no renderizar o corromper caracteres al extraer texto.
- **Tamaños:** cuerpo 10–12pt, encabezados de sección 13–14pt, nombre 16–18pt. No bajar de 10pt para comprimir (no ayuda al parser y da mala señal al humano).
- **Un solo color de acento como máximo** para encabezados; texto del cuerpo en oscuro sobre fondo blanco.
- **Formato de orden cronológico inverso** (reverse-chronological). El formato "funcional" (solo habilidades, sin cronología clara) confunde a los parsers y levanta sospechas.

## 2. Reglas de contenido y keywords

Esto determina cómo el ATS *puntúa* el CV frente a la vacante.

- **Copia las frases exactas de la oferta.** El matching suele ser literal: si la oferta dice "cross-functional collaboration", usa esa frase, no "coordinación de equipos". Coincidencia exacta > sinónimo.
- **Lista las habilidades explícitamente.** No confíes en que el ATS infiera skills de la narrativa. Agrúpalas por categoría: Habilidades técnicas, Herramientas y plataformas, Certificaciones, Idiomas.
- **Acrónimos: forma larga + corta la primera vez.** "Search Engine Optimization (SEO)". Distintos ATS indexan formas distintas.
- **Título del puesto alineado.** Si tu puesto real es equivalente al de la oferta, refleja el lenguaje de la oferta en el resumen (sin mentir sobre el cargo que tuviste).
- **Logros cuantificados.** Métricas en el 60–70% de las viñetas. "Reduje costos 23%" puntúa y convence más que "reduje costos". Números concretos: %, $, volúmenes, plazos, tamaño de equipo.
- **Verbos de acción** al inicio de cada viña (Lideré, Diseñé, Automaticé, Incrementé…).
- **Densidad honesta.** Repetir una keyword clave 2–3 veces en contextos reales está bien; el "keyword stuffing" (listas ocultas, texto en blanco) es penalizado y detectable.

## 3. Reglas por sección

Orden recomendado (tendencia 2026 hacia screening basado en habilidades):
**Contacto → Resumen profesional → Habilidades → Experiencia → Educación → Certificaciones**
(El clásico Resumen → Experiencia → Educación → Habilidades también es válido.)

- **Contacto:** nombre, teléfono, email, ciudad, LinkedIn. Texto plano en el cuerpo. Sin iconos.
- **Resumen profesional:** 3–4 líneas. Posiciona al candidato para *esta* vacante e incluye 2–3 keywords principales de la oferta. No es un objetivo genérico.
- **Habilidades:** lista de texto agrupada. Aquí es donde más rápido se cierra la brecha de keywords.
- **Experiencia:** cronológico inverso. Cada entrada: Puesto — Empresa — Ubicación — Fechas. Viñetas de logro cuantificadas. Encabezado de sección literal "Experiencia" o "Experiencia profesional" (evita nombres creativos tipo "Mi trayectoria").
- **Educación:** título, institución, año. Encabezado literal "Educación".
- **Certificaciones:** nombre completo + acrónimo + año/emisor.
- **Encabezados estándar en general:** el parser clasifica por el nombre de la sección. Usa nombres convencionales, no ingeniosos.

### Fechas
- **Formato consistente en todo el CV.** El parser calcula antigüedad y detecta huecos con las fechas; mezclar "05/2023", "'23", "mayo 23" produce cálculos erróneos.
- Formato recomendado: "Ene 2023 – Mar 2025" o "Enero 2023 – Presente". Elige uno y mantenlo.
- No omitir fechas para ocultar huecos: los ATS y reclutadores las esperan; su ausencia es sospechosa.

### Longitud
- Los ATS modernos manejan 2 páginas sin penalización. Deja que el contenido decida (1 pág. perfil junior, 2 págs. senior). No comprimas por debajo de 10pt para forzar 1 página.

## 4. Formato de archivo (2026)

- **PDF de texto seleccionable** es parseado de forma fiable por los ATS modernos (Workday, Greenhouse, Lever, iCIMS) en 2026. Es válido y preserva el diseño para el humano que lo lee después.
- **El PDF debe generarse desde texto, no como imagen.** Los PDFs de Canva/InDesign/escaneados con el texto rasterizado puntúan ~0 porque el parser no ve caracteres. Prueba: intenta seleccionar/copiar el texto; si se resalta, es válido.
- **.docx sigue siendo lo más seguro para sistemas heredados** (Taleo antiguo) y cuando la oferta pide "Word". Marginalmente más compatible universalmente. Ofrece generar una versión .docx si el destino lo requiere.
- **Sigue siempre las instrucciones explícitas de la oferta.** Si pide "Word document only", entrega .docx aunque el PDF sea técnicamente mejor.
- **Nunca** protejas el PDF con contraseña (el parser no puede abrirlo) ni uses formatos que requieran software especial (.pages, .jpg, .png).
- Nombre de archivo: `Nombre_Apellido_CV.pdf`.

## 5. Rúbrica de puntuación (0–100)

Deriva la puntuación de criterios verificables; no inventes el número. Reparte 100 puntos en dos bloques.

### Parseabilidad — 50 pts
| Criterio | Pts |
|---|---|
| Texto extraíble (no es imagen) | 15 |
| Una sola columna / orden de lectura correcto | 10 |
| Sin tablas, text boxes, gráficos, barras | 8 |
| Contacto en el cuerpo (no en header/footer) | 6 |
| Fuentes estándar y tamaños correctos | 5 |
| Encabezados de sección estándar | 6 |

### Contenido y match con la vacante — 50 pts
| Criterio | Pts |
|---|---|
| Match de keywords con la job description (proporcional; ~75%+ = máximo) | 20 |
| Habilidades listadas explícitamente y agrupadas | 8 |
| Logros cuantificados (60–70% de viñetas) | 10 |
| Fechas consistentes y completas | 6 |
| Resumen profesional alineado a la vacante | 6 |

**Match con la vacante (%)** = (keywords de la oferta presentes en el CV) / (keywords relevantes que el candidato realmente cumple) × 100. Repórtalo antes y después. Objetivo práctico: ≥75%.

Calcula "ATS de origen" con el CV original y "ATS optimizado" con la versión reescrita, usando la misma rúbrica, para que la mejora sea comparable y honesta.

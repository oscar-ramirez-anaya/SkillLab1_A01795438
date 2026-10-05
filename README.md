<div style="text-align: center;">

# Skill Lab 1 | Análisis y selección de modelos para un caso de negocio

### Tokens, costo, efecto de posición y evaluación con criterio previo

**Tecnológico de Monterrey** · Maestría en Inteligencia Artificial Aplicada
**IA5004.10 — Sistemas multiagente**

**Alumno:** Oscar Alberto Ramírez Anaya · **Matrícula:** A01795438 · **Correo:** A01795438@tec.mx
**Modalidad:** Individual · **Fecha:** Octubre 2026

</div>

---

## Descripción

Recomendación de modelo para dos tareas del asistente de inteligencia comercial de **Distribuidora
Cuauhtémoc**, con perfiles opuestos:

| Tarea | Qué exige | Decisión |
|---|---|---|
| **Consulta de campo** (preventista frente a la tienda) | Respuesta en segundos, costo bajo, error tolerable | **rápido** (provisional) |
| **Resumen nocturno de incidentes** (gerente comercial) | Máxima calidad de síntesis, latencia irrelevante | **capaz** |

El laboratorio trabaja en **modo precargado**: datos didácticos simulados del conjunto común `LAB1-A`,
sin llamadas a modelos ni a una API. Se analizan conteos y costos, el efecto de posición, una comparación
con criterio de aceptación definido **antes** de ver los resultados y se documenta la decisión en dos
**Model Decision Cards**.

## Contenido

| Archivo | Descripción |
|---|---|
| **`SkillLab1_A01795438.pdf`** | Entregable: resultados, interpretaciones, criterio y sus límites, cards y conclusiones |
| `SkillLab1_A01795438.ipynb` | Notebook del laboratorio con las celdas DECISIÓN completadas y ejecutado con salidas |
| `skill_lab1_variants.json` | Conjunto común del laboratorio (`LAB1-A`) |
| `figuras/` | Gráficas exportadas de las secciones A, B y C |
| `criterio_previo.txt` | Criterio de aceptación con la hora en que se registró, antes de consultar `eval_20` |
| `reporte/` | Fuente del PDF: plantilla HTML, figuras y `generar_pdf.py` (portada, índice y numeración) |

## Resultados principales

**Sección A — Tokens y costo** (solo entrada, 1000 llamadas/día; tarifas didácticas 0.15 y 1.25 USD por millón de tokens)

| Texto | Tokens | USD/día rápido | USD/día capaz |
|---|---:|---:|---:|
| consulta_campo | 47 | 0.00705 | 0.05875 |
| reporte_incidentes | 449 | 0.06735 | 0.56125 |
| catalogo | 1580 | 0.23700 | 1.97500 |

El **catálogo** no entra completo a cada consulta de campo: se recuperan solo los renglones pertinentes.
Razón español/inglés: 1.32.

**Sección B — Efecto de posición:** inicio 0.86, medio 0.71, final 0.88. Enterrar la evidencia a media
ventana cuesta **19 %** del máximo; la evidencia decisiva va al inicio o al final del paquete de contexto.

**Sección C — Comparación con criterio previo** (20 casos simulados)

| Modelo | Aciertos | USD por acierto | Latencia media |
|---|---:|---:|---:|
| rápido | 13/20 | 0.084 | 1.4 s |
| capaz | 18/20 | 0.396 | 6.3 s |

Criterio de campo: latencia <= 2.0 s y al menos 15/20 aciertos. **Ningún modelo lo cumple**; se documenta
sin reescribir el criterio y se adopta el rápido como asistente sugerido con vigilancia semanal. Para el
resumen nocturno el capaz cumple (18/20) y la latencia no aplica.

## Cómo ejecutar

Requiere Python 3 con `pandas` y `matplotlib`. Coloca el notebook y `skill_lab1_variants.json` en la misma
carpeta y ejecuta las celdas en orden (en Colab, sube el JSON al panel de archivos de la sesión).

Para regenerar el PDF se necesitan Google Chrome y los paquetes `pypdf` y `reportlab`:

```bash
cd reporte
python3 generar_pdf.py
```

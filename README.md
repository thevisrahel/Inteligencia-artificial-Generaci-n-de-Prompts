# Espejos Coloniales

### POC de Fast Prompting — Racismo internalizado (autoracismo) en el Perú y su raíz colonial

**Proyecto Final — Preentrega 2 (Fast Prompting en Acción)**
Autor: Elvis Labaca · Curso: Prompt Engineering · Comisión N.º 96090

> 📓 El desarrollo completo (prompts + código) está en [`Espejos_Coloniales_POC.ipynb`](./Espejos_Coloniales_POC.ipynb).

---

## Introducción

### 1. Nombre del proyecto
**Espejos Coloniales** — generador de piezas educativas (texto + imagen) sobre racismo internalizado en el Perú.

### 2. Presentación del problema

El racismo en el Perú no es un fenómeno aislado ni reciente: es un sistema heredado del orden
colonial que organizó a la sociedad virreinal en jerarquías según el origen étnico y el color de
piel. Aunque la Colonia terminó hace dos siglos, esa jerarquía se transformó y persiste hoy en
dinámicas de discriminación estructural y, de forma particularmente invisibilizada, en el modo en
que muchas personas indígenas y afroperuanas se perciben a sí mismas.

A ese fenómeno se lo conoce como **racismo internalizado o autoracismo**: el proceso mediante el
cual una persona perteneciente a un grupo históricamente racializado adopta, sin cuestionarlas, las
jerarquías raciales impuestas durante la Colonia, y termina rechazando o avergonzándose de rasgos
de su propia identidad.

Es importante precisar que este autoracismo **no tiene una única raíz colonial**, sino al menos
dos, distintas y a la vez entrelazadas en la sociedad peruana actual:

- El **sistema de castas del Virreinato**, que jerarquizó a la población indígena y estableció la
  hispanización como condición de ascenso social (de ahí derivan, por ejemplo, el rechazo al
  quechua/aimara y la vergüenza del apellido u origen andino).
- El **régimen esclavista** y sus normas de "buena presencia" y moralidad pública impuestas a la
  población negra desde el siglo XVIII (de ahí deriva específicamente la discriminación hacia el
  cabello afro-rizado y la práctica del alisado como estrategia de adaptación laboral,
  documentada en estudios recientes de la Pontificia Universidad Católica del Perú).

Esta problemática es relevante porque el autoracismo reproduce la desigualdad racial sin necesidad
de un agresor externo explícito, lo que lo vuelve más difícil de identificar y combatir que la
discriminación directa; porque afecta a una parte muy amplia de la sociedad peruana, muchas veces
sin que las personas lo reconozcan como tal; y porque el propio Estado peruano —a través del Censo
Nacional de 2017, que incorporó por primera vez una pregunta de autoidentificación étnica— ha
reconocido la necesidad de visibilizar esta dimensión de la identidad nacional.

### 3. Desarrollo de la propuesta de solución

La propuesta consiste en un generador, mediante ingeniería de prompts, de una serie de **cinco
piezas educativas para redes sociales** (texto breve + imagen conceptual), cada una centrada en
una manifestación actual del autoracismo, conectada explícitamente con su raíz colonial específica.
Dirigido a jóvenes y adultos peruanos de 18 a 30 años, activos en redes sociales.

La solución se vincula al desarrollo de modelos de IA generativa en dos frentes, correspondientes
a los dos modelos trabajados en el curso:

- **Modelo texto-texto** (`gpt-4o-mini` vía API de OpenAI): genera, en una sola consulta por pieza,
  el texto educativo y el prompt de imagen asociado, usando salida estructurada en JSON.
- **Modelo texto-imagen** (`dall-e-3` vía API de OpenAI, o alternativamente Nightcafe u otra
  herramienta gratuita disponible): genera la pieza visual a partir del prompt producido en el
  paso anterior.

Las cinco manifestaciones trabajadas, con su raíz colonial específica, son:

| Manifestación | Raíz colonial |
|---|---|
| Rechazo o vergüenza al hablar quechua/aimara | Sistema de castas indígena |
| Alisado capilar como condición de "buena presencia" laboral | Régimen esclavista |
| La expresión "mejorar la raza" al elegir pareja | Ambos sistemas coloniales |
| Vergüenza del apellido o el origen andino/amazónico | Sistema de castas indígena |
| Idealización de rasgos europeos en medios y publicidad | Ambos sistemas coloniales |

El detalle de los prompts empleados en cada etapa está desarrollado en la sección
[Implementación](#implementación) y, en su versión ejecutable, en el notebook.

### 4. Justificación de la viabilidad del proyecto

El proyecto es técnicamente viable porque no requiere infraestructura propia ni desarrollo de
software complejo: se apoya en la API de OpenAI (documentada y estable) y en un notebook ejecutable
en cualquier entorno con Python 3.9+. No depende de que una herramienta puntual sea gratuita —el
código admite alternar entre DALL·E y otra herramienta de generación de imagen sin cambiar el
diseño del prompt de texto.

En términos de tiempo y recursos, el proyecto se estructura en un número acotado de entregables
(cinco piezas), lo que permite estimar el trabajo de forma realista: preparación y prueba de
prompts en modo demo (sin costo), una corrida real controlada de 10 llamadas a la API en total
(ver [Costos](#costos-y-optimización)), y una revisión final antes de publicar.

La principal limitación identificada es el riesgo de que el modelo de texto genere datos
históricos imprecisos, mitigado con restricciones explícitas en el prompt (no mezclar raíces
coloniales) y con la función de revisión opcional incluida en el notebook.

---

## Objetivos

- Demostrar la comprensión de los principios y técnicas de Fast Prompting (Role Prompting,
  Few-Shot, salida estructurada, Zero-Shot, prompting negativo) aplicados a un caso real.
- Experimentar con distintas configuraciones de prompt para optimizar la eficacia y consistencia
  de las piezas generadas.
- Preparar una demostración funcional y reproducible en Jupyter Notebook, ejecutable sin costo
  en modo demo y con costo controlado en modo real.
- Analizar si las técnicas de Fast Prompting permiten mejorar, en términos de costo y consistencia,
  la propuesta planteada en la Preentrega 1.

## Metodología

El proyecto se desarrolló en cuatro pasos:

1. **Relevamiento de la Preentrega 1**: se revisaron los cuatro prompts manuales originales
   (investigación, redacción, imagen, revisión) y se identificó que las etapas de investigación y
   redacción podían fusionarse sin perder rigor, ya que ambas dependen del mismo insumo (la
   manifestación y su raíz colonial).
2. **Diseño de prompts con salida estructurada**: se reescribieron los prompts para que el modelo
   devuelva JSON con claves fijas (`texto`, `raiz_colonial`, `prompt_imagen`), lo que permite
   encadenar automáticamente el resultado de texto hacia el generador de imagen sin intervención
   manual.
3. **Implementación con control de costo**: se separó el código en funciones puras
   (`generate_piece`, `generate_image`, `review_piece`) con un parámetro `dry_run` que permite
   probar y depurar todo el pipeline sin llamar a la API, dejando el gasto real acotado a una
   única corrida final y explícita.
4. **Validación end-to-end**: el notebook se ejecutó de punta a punta en modo demo (sin API key)
   para confirmar que no hay errores de código antes de habilitar la corrida real.

## Herramientas y tecnologías

**Lenguaje y entorno:** Python 3 sobre Jupyter Notebook, con `ipywidgets` para la interfaz
interactiva y `python-dotenv` para el manejo seguro de la API key.

**Modelos:** `gpt-4o-mini` (texto-texto) y `dall-e-3` (texto-imagen), vía API de OpenAI.

**Técnicas de prompting utilizadas y justificación:**

- **Role Prompting**: el componente `<rol>` del prompt maestro fija el rol de "historiador +
  redactor de contenido educativo", lo que mejora la precisión histórica y evita un tono genérico
  de blog.
- **Few-Shot Prompting**: el componente `<ejemplo>` incluye un caso completo (tema → raíz colonial
  → texto final) que ancla el formato y la extensión esperada, reduciendo la necesidad de
  corrección manual entre piezas.
- **Salida estructurada (JSON mode)**: el componente `<formato_salida>` permite fusionar en una
  sola llamada lo que en la Preentrega 1 eran dos etapas separadas, y consumir la respuesta
  directamente en código sin parseo frágil de texto libre.
- **Zero-Shot Prompting**: usado en la función de revisión de sensibilidad, una tarea de
  verificación puntual que no se beneficia de ejemplos adicionales y donde agregarlos solo
  aumentaría el costo sin mejorar el resultado.
- **Prompting negativo (restricciones explícitas)**: el componente `<restricciones>` lista
  explícitamente las cosas a evitar (mezclar raíces coloniales, estereotipos visuales, tono
  acusatorio), más eficaz que describir solo lo que sí se quiere obtener.
- **Prompt estructurado con etiquetas (XML)**: el prompt maestro completo se organiza en bloques
  etiquetados (`<rol>`, `<contexto>`, `<reglas>`, `<restricciones>`, `<formato_salida>`) en lugar
  de un párrafo corrido, lo que reduce la ambigüedad para el modelo y facilita mantener o editar
  cada componente por separado.

### Ejemplo de resultado esperado

Para dejar demostrada la relación `prompt → salida → objetivo`, este es el resultado esperado
para la pieza del tema "apellido" (el mismo usado como Few-Shot en el prompt maestro):

| Campo de salida | Valor esperado | Objetivo que cumple |
|---|---|---|
| `texto` | "Cambiar de apellido o esconder el pueblo de origen para 'encajar' en la ciudad no es casualidad. [...] ¿Alguna vez sentiste que tu apellido o tu acento decían más de vos de lo que quisiste?" | Situación cotidiana + raíz colonial + pregunta reflexiva, en ≤80 palabras, tono no acusatorio. |
| `raiz_colonial` | "sistema de castas indígena" | Permite verificar por código que no se mezcló con la raíz esclavista. |
| `prompt_imagen` | "Ilustracion digital conceptual [...] Evitar estereotipos caricaturescos de rasgos indigenas." | Prompt reutilizable en la herramienta de imagen, sin una nueva llamada a un modelo de texto. |

### Indicadores de validación

Cada iteración de una pieza se valida contra estos indicadores antes de darla por aprobada:

| Indicador | Cómo se verifica |
|---|---|
| **Precisión histórica** | La `raiz_colonial` devuelta coincide con la definida en `TEMAS` para ese tema (verificación automática, función `validar_pieza()`). |
| **Adecuación al límite de palabras** | El campo `texto` no supera las 80 palabras pedidas (verificación automática, `validar_pieza()`). |
| **Accesibilidad del lenguaje** | Se revisa con la función `review_piece()` (Zero-Shot), que señala lenguaje poco accesible. |
| **Ausencia de estereotipos** | El `prompt_imagen` no describe personas reales ni rasgos caricaturescos; se revisa manualmente antes de generar la imagen. |

## Implementación

El desarrollo completo —prompts, funciones y demostración— está en
[`Espejos_Coloniales_POC.ipynb`](./Espejos_Coloniales_POC.ipynb), organizado en las siguientes
secciones:

1. Instalación y configuración (incl. control de costo).
2. Técnicas de Fast Prompting utilizadas (tabla resumen).
3. Base de conocimiento del proyecto (los 5 temas como datos, no como texto repetido).
4. Generador de texto — `generate_piece()`: una llamada por pieza, Role + Few-Shot + JSON.
5. Generador de imagen — `generate_image()`: reutiliza el prompt generado en el paso anterior.
6. Función de revisión — `review_piece()`: Zero-Shot, opcional, bajo demanda.
7. Demostración en modo demo (sin costo).
8. Widget interactivo para generar piezas sin editar código.
9. Celda de ejecución real (desactivada por defecto, para evitar consumo accidental de créditos).
10. Análisis de costos.
11. Conclusiones.

El notebook incluido en este repositorio ya fue **ejecutado en modo demo** (sin ninguna llamada
real a la API), por lo que sus salidas son visibles directamente en GitHub sin necesidad de
correrlo.

### Costos y optimización

| Enfoque | Llamadas por pieza | Llamadas para las 5 piezas |
|---|---|---|
| Preentrega 1 (manual, 4 etapas por pieza) | 4 | 20 |
| Esta POC (texto + imagen fusionados, revisión opcional) | 2 | 10 |

La reducción se logra fusionando las etapas de investigación y redacción de la Preentrega 1 en una
sola llamada con salida JSON, y dejando la revisión de sensibilidad como una función bajo demanda
en vez de un paso automático sobre las 5 piezas. El modo `dry_run` permite además iterar sobre el
prompt sin ninguna llamada real, concentrando el gasto de créditos únicamente en la corrida final.

### Cómo ejecutar en modo real

```bash
git clone <URL-de-este-repositorio>
cd espejos-coloniales
pip install -r requirements.txt
cp .env.example .env        # completar OPENAI_API_KEY=sk-...
jupyter notebook Espejos_Coloniales_POC.ipynb
```

En el notebook, cambiar `dry_run=True` por `dry_run=False` en la celda que se quiera ejecutar de
forma real (Secciones 7, 8 o 9).

## Conclusiones

Aplicar Fast Prompting sobre la propuesta de la Preentrega 1 permitió reducir a la mitad el número
de consultas necesarias por pieza, aumentar la consistencia del formato entre las cinco piezas
gracias al ejemplo Few-Shot, estructurar el prompt maestro con etiquetas explícitas (rol, contexto,
reglas, restricciones, formato de salida) en vez de un párrafo corrido, y definir indicadores de
validación concretos para cada iteración. Todo el pipeline pudo además construirse y probarse sin
gastar créditos gracias al modo demo, dejando el gasto real acotado a una única corrida final y
controlada.

## Referencias

- Kogan, L., Fuchs, R. & Lay, P. (2020). *No sabía que existía tanto racismo hasta que entré a
  trabajar ahí: un estudio sobre discriminación en procesos de selección laboral en Lima
  Metropolitana.* Pontificia Universidad Católica del Perú.
- Instituto Nacional de Estadística e Informática (INEI). *Censo Nacional 2017: Perú — Perfil
  Sociodemográfico*, primera pregunta de autoidentificación étnica.
- OpenAI. *API Reference — Chat Completions.* https://platform.openai.com/docs/api-reference/chat
- OpenAI. *API Reference — Images.* https://platform.openai.com/docs/api-reference/images

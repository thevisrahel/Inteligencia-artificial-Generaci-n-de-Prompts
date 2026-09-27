# Espejos Coloniales

### Racismo internalizado (autoracismo) en el Perú y su raíz colonial

**Proyecto Final — Entrega 3**
Autor: Elvis Labaca · Curso: Prompt Engineering · Comisión N.º 96090

> 📓 Notebook: [`Espejos_Coloniales_Final.ipynb`](./Espejos_Coloniales_Final.ipynb)
> 🖼️ Imágenes generadas: [`imagenes/`](./imagenes)

---

## Resumen

**Espejos Coloniales** es un generador de piezas educativas (texto + imagen) sobre
racismo internalizado (autoracismo) en el Perú, que conecta cinco manifestaciones
actuales de este fenómeno con su raíz colonial específica —el sistema de castas
indígena o el régimen esclavista, según corresponda—, dirigido a jóvenes peruanos de
18 a 30 años activos en redes sociales. El proyecto combina un modelo de texto-texto
(API de OpenAI, `gpt-4o-mini`) para generar el contenido escrito de cada pieza, y una
herramienta gratuita de generación de imagen para producir la pieza visual asociada a
partir de un prompt diseñado específicamente para evitar estereotipos. El desarrollo
aplicó técnicas de Fast Prompting (Role Prompting, Few-Shot, salida estructurada en
JSON y prompting negativo) para reducir a la mitad el número de consultas necesarias
respecto al enfoque manual planteado en la primera entrega del proyecto.

## Introducción

### Nombre del proyecto
**Espejos Coloniales**

### Presentación del problema

El racismo en el Perú no es un fenómeno aislado ni reciente: es un sistema heredado del
orden colonial que organizó a la sociedad virreinal en jerarquías según el origen étnico
y el color de piel. Aunque la Colonia terminó hace dos siglos, esa jerarquía se
transformó y persiste hoy en dinámicas de discriminación estructural y, de forma
particularmente invisibilizada, en el modo en que muchas personas indígenas y
afroperuanas se perciben a sí mismas.

A ese fenómeno se lo conoce como **racismo internalizado o autoracismo**: el proceso
mediante el cual una persona perteneciente a un grupo históricamente racializado adopta,
sin cuestionarlas, las jerarquías raciales impuestas durante la Colonia, y termina
rechazando o avergonzándose de rasgos de su propia identidad.

Este autoracismo no tiene una única raíz colonial, sino al menos dos, distintas y a la
vez entrelazadas en la sociedad peruana actual:

- El **sistema de castas del Virreinato**, que jerarquizó a la población indígena y
  estableció la hispanización como condición de ascenso social (de ahí derivan, por
  ejemplo, el rechazo al quechua/aimara y la vergüenza del apellido u origen andino).
- El **régimen esclavista** y sus normas de "buena presencia" y moralidad pública
  impuestas a la población negra desde el siglo XVIII (de ahí deriva específicamente la
  discriminación hacia el cabello afro-rizado y la práctica del alisado, documentada en
  estudios recientes de la Pontificia Universidad Católica del Perú).

Esta problemática es relevante porque el autoracismo reproduce la desigualdad racial sin
necesidad de un agresor externo explícito, lo que lo vuelve más difícil de identificar y
combatir que la discriminación directa; porque afecta a una parte muy amplia de la
sociedad peruana, muchas veces sin que las personas lo reconozcan como tal; y porque el
propio Estado peruano —a través del Censo Nacional de 2017, que incorporó por primera
vez una pregunta de autoidentificación étnica— ha reconocido la necesidad de visibilizar
esta dimensión de la identidad nacional.

### Desarrollo de la propuesta de solución

La propuesta consiste en un generador, mediante ingeniería de prompts, de una serie de
**cinco piezas educativas para redes sociales** (texto breve + imagen conceptual), cada
una centrada en una manifestación actual del autoracismo, conectada explícitamente con
su raíz colonial específica.

La solución se vincula al desarrollo de modelos de IA generativa en dos frentes,
correspondientes a los dos modelos trabajados en el curso:

- **Modelo texto-texto** (`gpt-4o-mini`, vía API de OpenAI): genera, en una sola consulta
  por pieza, el texto educativo y el prompt de imagen asociado, usando salida
  estructurada en JSON.
- **Modelo texto-imagen**: dado que DALL·E dejó de ser gratuita, se optó por una
  herramienta gratuita de generación de imagen (Nightcafe), a la que se le pegó
  manualmente el prompt producido en el paso anterior, sin uso de API, siguiendo la
  indicación de esta entrega.

Las cinco manifestaciones trabajadas, con su raíz colonial específica, son:

| Manifestación | Raíz colonial |
|---|---|
| Rechazo o vergüenza al hablar quechua/aimara | Sistema de castas indígena |
| Alisado capilar como condición de "buena presencia" laboral | Régimen esclavista |
| La expresión "mejorar la raza" al elegir pareja | Ambos sistemas coloniales |
| Vergüenza del apellido o el origen andino/amazónico | Sistema de castas indígena |
| Idealización de rasgos europeos en medios y publicidad | Ambos sistemas coloniales |

### Justificación de la viabilidad del proyecto

El proyecto es técnicamente viable porque no requiere infraestructura propia: se apoya
en la API de OpenAI para el texto (documentada, estable, de bajo costo para el volumen
de este proyecto) y en una herramienta gratuita de generación de imagen para lo visual,
sin depender de que una plataforma puntual sea gratuita de forma permanente.

En términos de tiempo y recursos, el proyecto se estructura en un número acotado de
entregables (cinco piezas), lo que permitió estimar el trabajo de forma realista: diseño
y prueba de prompts en modo demo (sin costo), una corrida real de 5 llamadas a la API
para el texto (ver [Resultados](#resultados)), y la generación manual de las 5 imágenes.

La principal limitación identificada es el riesgo de que el modelo de texto genere datos
históricos imprecisos, mitigado con restricciones explícitas en el prompt (no mezclar
raíces coloniales) y con la función de revisión de sensibilidad incluida en el notebook.

## Objetivos

- Identificar una problemática social y plantear una solución basada en generación de
  prompts, usando OpenAI para texto y una herramienta gratuita para imagen.
- Plantear una solución factible, evaluando la disponibilidad real de recursos (tiempo,
  costo de API, herramientas gratuitas).
- Optimizar el uso de los prompts, minimizando el número de consultas necesarias.
- Aplicar y justificar técnicas de Fast Prompting concretas sobre un caso real.

## Metodología

El proyecto se desarrolló en las siguientes etapas:

1. **Definición del problema y su alcance** (Entrega 1): selección del autoracismo en el
   Perú como tema, acotado a cinco manifestaciones concretas con raíz colonial
   documentada.
2. **Diseño de prompts con salida estructurada** (Entrega 2): reescritura de los prompts
   manuales de la Entrega 1 en un formato que devuelve JSON con claves fijas (`texto`,
   `raiz_colonial`, `prompt_imagen`), fusionando etapas que antes eran manuales y
   separadas.
3. **Implementación con control de costo** (Entrega 2): funciones con parámetro
   `dry_run` que permiten probar y depurar todo el pipeline sin llamar a la API,
   acotando el gasto real a una única corrida final.
4. **Generación de resultados reales** (Entrega 3): ejecución real de la generación de
   texto (5 llamadas a la API), y generación manual de las 5 imágenes en una herramienta
   gratuita, a partir de los prompts producidos por el paso anterior.
5. **Revisión y documentación de resultados**: verificación de que cada texto respeta el
   formato pedido y que la raíz colonial asignada es correcta, documentación de las 5
   piezas finales en este README.

## Herramientas y tecnologías

**Lenguaje y entorno:** Python 3 sobre Jupyter Notebook, con `python-dotenv` para el
manejo seguro de la API key.

**Modelos y herramientas:**
- `gpt-4o-mini` (texto-texto), vía API de OpenAI.
- Nightcafe (texto-imagen), usada de forma manual, sin integración por API.

**Técnicas de Fast Prompting utilizadas y justificación:**

- **Role Prompting**: el prompt de sistema fija el rol de "historiador + redactor de
  contenido educativo", lo que mejora la precisión histórica y evita un tono genérico.
- **Few-Shot Prompting**: se incluye un ejemplo completo (tema → raíz colonial → texto
  final) que ancla el formato y la extensión esperada.
- **Salida estructurada (JSON mode)**: permite obtener en una sola llamada tanto el
  texto como el prompt de imagen, listo para reutilizar en la herramienta manual.
- **Zero-Shot Prompting**: usado en la función de revisión de sensibilidad, una tarea
  puntual que no se beneficia de ejemplos adicionales.
- **Prompting negativo (restricciones explícitas)**: se listan explícitamente las cosas
  a evitar (mezclar raíces coloniales, estereotipos visuales, tono acusatorio), tanto en
  el prompt de texto como en el de imagen.

## Implementación

El desarrollo completo está en
[`Espejos_Coloniales_Final.ipynb`](./Espejos_Coloniales_Final.ipynb), organizado en:

1. Instalación y configuración.
2. Técnicas de Fast Prompting utilizadas (tabla resumen).
3. Base de conocimiento del proyecto (los 5 temas como datos).
4. Generador de texto (`generate_piece()`): una llamada por pieza, Role + Few-Shot + JSON.
5. Flujo manual de generación de imagen (sin API, según lo indicado en la consigna).
6. Función de revisión (`review_piece()`): Zero-Shot, opcional.
7. **Ejecución real**: generación de las 5 piezas de texto con la API (resultados abajo).
8. Prompts de imagen listos para copiar en la herramienta gratuita elegida.
9. Resultados (resumen; el detalle está en la sección siguiente de este README).

### Prompts de imagen utilizados

| Tema | Prompt de imagen utilizado en Nightcafe |
|---|---|
| quechua | Una puerta antigua entreabierta, con una luz dorada que brilla desde el interior, simbolizando la conexión con las raíces culturales. La puerta está decorada con patrones andinos, y el espacio detrás es difuso, sugiriendo historia y pertenencia. |
| alisado | Un peinado de cabello afro estilizado, representado como una nube que se transforma en una cadena rota, sobre un fondo suave y abstracto en tonos cálidos. Sin rostros humanos, enfatizando la conexión entre libertad y autoexpresión. |
| mejorar_raza | Una balanza en equilibrio, con un lado lleno de flores de distintos colores y el otro lado vacío, simbolizando la diversidad y la búsqueda de equilibrio en las relaciones. Composición centrada, estilo editorial. |
| apellido | Una cadena rota que se transforma en una flor, simbolizando la liberación y el florecimiento de la identidad, en tonos verdes y tierra, con un fondo abstracto que evoca la naturaleza andina. |
| medios | Un espejo roto que refleja fragmentos de paisajes andinos y europeos, simbolizando la diversidad y la fragmentación de la identidad, en una paleta de colores suaves y armoniosos. |

## Resultados

Resultados reales de la corrida final (`DRY_RUN = False`, Sección 7 del notebook), con
`gpt-4o-mini`, más las 5 imágenes generadas manualmente. Total de llamadas reales a la
API: **5** (una por pieza), tal como estaba previsto.

### Pieza 1 — Quechua/aimara
**Texto:** ¿Te has sentido incómodo hablando quechua o aimara en público? Este rechazo
tiene raíces en la Colonia, donde la hispanización era un requisito para mejorar tu
estatus social. Las lenguas indígenas eran vistas como un símbolo de inferioridad.
¿Cómo influye esto en tu forma de relacionarte con tus raíces hoy?
**Raíz colonial:** sistema de castas indígena
**Imagen:** `imagenes/quechua.png`

### Pieza 2 — Alisado capilar
**Texto:** ¿Te has sentido presionado a alisar tu cabello para "verse bien" en el
trabajo? Esta presión tiene raíces en el siglo XVIII, cuando la sociedad impuso normas
de moralidad que desvalorizaban el cabello afro, asociándolo con la esclavitud. ¿Nos
hemos preguntado qué significa realmente "buena presencia"?
**Raíz colonial:** régimen esclavista
**Imagen:** `imagenes/alisado.png`

### Pieza 3 — "Mejorar la raza"
**Texto:** ¿Alguna vez escuchaste que la elección de pareja se basa en "mejorar la
raza"? Esta idea proviene de una lógica colonial que buscaba blanquear la población,
tanto en el sistema de castas como en la esclavitud. La mezcla de razas se consideraba
un camino hacia la "superioridad". ¿Qué crees que se oculta detrás de esta creencia?
**Raíz colonial:** ambos sistemas coloniales
**Imagen:** `imagenes/mejorar_raza.png`

### Pieza 4 — Apellido/origen andino
**Texto:** ¿Alguna vez te has sentido incómodo por tu apellido o acento en un entorno
urbano? Esta vergüenza tiene raíces en el sistema de castas del periodo colonial, donde
el origen étnico definía tu lugar en la sociedad. Es un legado que persiste hoy. ¿Qué
significaría para ti abrazar tu identidad sin miedo?
**Raíz colonial:** sistema de castas indígena
**Imagen:** `imagenes/apellido.png`

### Pieza 5 — Medios y publicidad
**Texto:** Cuando ves comerciales o redes sociales, ¿te has dado cuenta de cuántas veces
los modelos tienen rasgos europeos? Esta búsqueda de belleza "ideal" proviene de la
Conquista, donde los estándares europeos se impusieron como sinónimo de valor. En el
Perú actual, seguimos lidiando con estos cánones que desvalorizan nuestra diversidad.
¿Qué tanto influye esto en cómo te ves a ti mismo y en cómo ves a los demás?
**Raíz colonial:** ambos sistemas coloniales
**Imagen:** `imagenes/medios.png`

### ¿Se logró la solución esperada?

Sí, en líneas generales. Las 5 raíces coloniales asignadas por el modelo coinciden
exactamente con las documentadas en la base de conocimiento del notebook (Sección 3):
sistema de castas indígena para quechua y apellido, régimen esclavista para alisado, y
ambos sistemas para mejorar_raza y medios, lo que confirma que la restricción de "no
mezclar raíces salvo que ambas apliquen" funcionó correctamente. Los 5 textos siguen la
estructura de tres partes pedida (contexto cotidiano → dato histórico → pregunta
reflexiva) y se mantienen dentro de un rango breve apto para redes sociales, sin tono
acusatorio hacia el lector. Las 5 imágenes generadas manualmente son coherentes con sus
prompts: usan metáforas visuales (puertas, cadenas, balanzas, espejos) en lugar de
representar personas, lo que evita estereotipos raciales o físicos. El total de llamadas
reales a la API fue de 5, una por pieza, según lo previsto.

## Conclusiones

Aplicar Fast Prompting sobre la propuesta planteada en la Entrega 1 permitió reducir a
la mitad el número de consultas necesarias por pieza de texto (de un enfoque manual de 4
etapas a una sola llamada con salida estructurada), aumentar la consistencia del formato
entre las cinco piezas gracias al ejemplo Few-Shot, y separar por completo el desarrollo
del costo real gracias al modo demo utilizado durante las Entregas 2 y 3: todo el
pipeline pudo construirse y probarse sin gastar créditos, dejando el gasto real acotado a
una única corrida final de 5 llamadas a la API.

Revisando los resultados reales, el objetivo de generar piezas educativas rigurosas y
no estigmatizantes se cumplió: las cinco piezas conectan una manifestación cotidiana con
su raíz colonial documentada, sin errores de atribución y sin caer en un tono
acusatorio hacia quien lee. Como mejora para una futura iteración, se podría ampliar el
proyecto a más manifestaciones del autoracismo (por ejemplo, el trato diferencial en
espacios turísticos o educativos), incorporar el modelo de texto-audio como contenido
adicional para generar una narración de cada pieza, y sumar la función de revisión de
sensibilidad (`review_piece`) a la corrida real, hoy disponible pero no ejecutada en
modo real en esta entrega.

## Referencias

- Instituto Nacional de Estadística e Informática (INEI). Censo Nacional 2017: primera
  pregunta de autoidentificación étnica.
- Pontificia Universidad Católica del Perú (PUCP). Estudios sobre discriminación laboral
  y cabello afro-rizado en Lima.
- OpenAI. Documentación de la API — [platform.openai.com/docs](https://platform.openai.com/docs)
- Nightcafe — herramienta gratuita de generación de imagen: [creator.nightcafe.studio](https://creator.nightcafe.studio)

---

## Cómo ejecutar este proyecto

```bash
git clone <URL-de-este-repositorio>
cd espejos-coloniales
pip install -r requirements.txt
cp .env.example .env        # completar OPENAI_API_KEY=sk-...
jupyter notebook Espejos_Coloniales_Final.ipynb
```

En la Sección 7 del notebook, cambiar `DRY_RUN = True` por `DRY_RUN = False` para generar
los resultados reales. Luego, con los prompts de imagen de la Sección 8, generar las 5
imágenes manualmente en Nightcafe (u otra herramienta gratuita) y guardarlas en la
carpeta `imagenes/` de este repositorio con el nombre de cada tema.

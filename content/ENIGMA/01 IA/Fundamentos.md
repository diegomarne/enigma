---
title: Fundamentos IA
---
### LLM: Large Lenguage Model
Modelo de IA entrenado con grandes cantidades de texto con el fin de aprender patrones del lenguaje y generar respuestas mediante probabilidad. 



### Inferencia
Es el proceso que realiza un LLM ya entrenado para generar una respuesta a una petición nueva. **Inferir** significa deducir o sacar una conclusión a partir de otros hechos, datos o premisas.

El proceso sería:

1. **Entrada:** recibe tu pregunta.
    
2. **Inference:**  procesa el texto y genera una respuesta token a token.
    
3. **Salida:** produce una respuesta.



### Parámetros
Valores internos del modelo, aprendidos durante su entrenamiento, que determina como procesa y genera la información.

**Ejemplo** Un módelo de 7.000 millones de parámetros tiene 7.000 millones de valores internos ajustados durante su entrenamiento.

**¿Más es mejor?** Generalmente, más parámetros pueden permitir una mayor capacidad, pero no garantizan mejores resultados. También importan los datos (calidad y cantidad), la arquitectura y el entrenamiento. Capacidad ≠ calidad.

💭 Reflexión: Un modelo mal entrenado bien sea por unos datos o una arquitectura deficiente es como un turismo con el consumo de un todo terreno. El objetivo debería ser conseguir que cada parámetro sea más útil. Es decir, conseguir más capacidad con menos recursos. ¿No sería ideal que durante el propio entrenamiento fuese el propio modelo quien determinase el nº de parámetros que necesita?


##### Arquitectura Dense vs MOE
Existen actualmente dos arquitecturas que conviene distinguir:

- Modelos densos (**Dense**): Utilizan prácticamente todos sus parámetros para procesar cada token.

- Modelos de Expertos (**MoE**): Tienen muchos parámetros distribuidos en diferentes expertos. Un router (gate) decide que experto activar par cada input. Esto permite construir modelos enormes sin tener que utilizar todos sus parámetros en cada operación. Baja su consumo computacional. Ejemplo: **Llama 4 Maverick (MoE)** - 400B / 17B - 400.000 millones totales / 17.000 millones activos por token

##### Clasificación orientativa actualizada (2026)

| Tamaño      | Nº de parámetros | Características                                                         |
| ----------- | ---------------- | ----------------------------------------------------------------------- |
| Ultraligero | Menos de 1B      | Dispositivos móviles, tareas sencillas y ejecución local.               |
| Pequeño     | 1B–8B            | Asistentes locales, tareas generales y consumo reducido de recursos.    |
| Mediano     | 8B–30B           | Buen equilibrio entre capacidad, rendimiento y recursos.                |
| Grande      | 30B–70B          | Mayor capacidad para razonamiento, programación y tareas complejas.     |
| Muy grande  | 70B–200B         | Modelos de gran capacidad que suelen requerir hardware especializado.   |
| Masivo      | Más de 200B      | Modelos de escala extraordinaria, frecuentemente con arquitecturas MoE. |

*Nota: Esta clasificación es orientativa y se refiere principalmente al número de parámetros totales de modelos Dense. En arquitecturas MoE es necesario diferenciar entre parámetros totales y parámetros activos por token, ya que pueden existir modelos con cientos de miles de millones de parámetros totales que utilizan solamente una pequeña parte en cada operación



### Tokens
Son las unidades en las que un **tokenizador** divide el texto para que un modelo de IA pueda procesarlo. Pueden ser palabras, partes de palabras o caracteres. Un token ≠ una palabra.

Antes de entrenar el modelo los desarrolladores preparan un **sistema de tokenización** que determina cómo se divide el texto en tokens antes de introducirlo en el modelo. Este puede dividr cada palabra en un token pero también podría dividir alguna palabra en varias partes (p.ej: palabra poco frecuente) o alguna frase en un token (p.ej: frase muy frecuente). Para construirlo, se utilizan grandes cantidades de texto que permiten identificar las combinaciones de caracteres y palabras más frecuentes. El resultado es la construcción de un vocabulario de tokens que posteriormente utilizará el modelo.

💭 *Si yo todos los días le digo a un modelo "Buenos días, proporcioname una guía rápida de como utilizar makdown que me sirva como referencia para estilizar rapidamente los documentos con los que trabajo." ¿el tokenizador podría acabar convirtiendlo en un token?*
Respuesta de la IA: El **tokenizador** no suele aprender durante las conversaciones. Su vocabulario se establece previamente al entrenamiento y permanece fijo durante el uso normal del modelo. Repetir una frase no hace que necesite menos tokens en futuras interacciones.

#### Tipos de tokens
A la hora de utilizar un modelo hay que distinguir entre tres tipos de tokens:
- **Tokens de entrada**: los que envías en tu pregunta y el contexto de la conversación.
- **Tokens de razonamiento**: los que el modelo utiliza internamente para analizar el problema antes de responder.
- **Tokens de salida visibles**: los que forman la respuesta que recibes.

### Temperatura
Parámetro que controla el grado de aleatoriedad (o creatividad) en la selección de los siguientes tokens. Un valor bajo genera respuestas predecible y uno alto, más variadas.

La temperatura es un parámetro de _inferencia_, es decir, afecta a cómo genera respuestas un modelo ya entrenado, no a cómo aprende durante su entrenamiento.

Aclaración: No todos los modelos ni plataformas permiten modificar este parámetro. Algunos pueden configurarse mediante API o herramientas de ejecución local cuando el modelo lo admite. No está directamente relacionado con que el modelo sea open weight.



### Prompt
Instrucciones o información que se proporcionan a un modelo para inferir en una respuesta.



### Ventana de contexto
Es la cantidad máxima de tokens que el modelo puede tener en cuenta durante una interacción. Cuanto mayor sea el contexto más información va a poder tener en cuenta pero no garantiza una mejor comprensión ni que el modelo recuerde todos los detalles con la misma precisión.

Más ventana de contexto ≠  más inteligencia, solamente indica que permite trabajar con más información. 

Cuando hablas con ChatGPT por ejemplo, el contexto puede incluir:
- Instrucciones iniciales del sistema.
- Tus mensajes anteriores.
- Las respuestas del modelo a tus mensajes.
- La pregunta actual.
- Documentos o imágenes proporcionados.

Además hay que reservar espacio para la respuesta que va a generar el modelo, y en determinados modelos, para los tokens de razonamiento interno.

💭 *Una biblioteca enorme permite almacenar miles de libros, pero en la práctica, tanto espacio, puede ocasionar mayor dificultad para encontrar libros y también propiciar una mayor acumulación de libros de poca utilidad.*

###### ¿Que ocurre cuando se llena el espacio de contexto?
Cuando un modelo ocupa el espacio completo de su ventana de contexto y se necesita seguir introduciendo más información, el sistema necesita gestionar el espacio disponible.

Puede gestionar el espacio lleno de las siguientes formas:
- **Eliminar información antigua**
- **Resumir el historial**
- **Compactar el contexto**
- **Rechazar la petición**

El resumen de la información o la compresión de esta puede llegar a ocasionar una perdida de datos. En estos casos pueden aparecer [[#Alucinaciones]] del modelo ya que puede intentar rellenar o interpretar la información que le falta. Sin embargo, las alucinaciones puede ocurrir sin que exista ningún problema de contexto.


### Multimodalidad
Es la capacidad que tiene un modelo de IA para trabajar con distintos tipos de información, tanto de entrada como salida.

- **Texto**: resumir o crear un texto.
- **Imágen**: identificar objetos en una fotografía o crear una imágen.
- **Audio**: transcribir una conversación o crear un audio a partir de un texto.
- **Video**: analizar lo que sucede en una grabación o generar un video.

Un matiz **importante**, que un modelo sea multimodal no significa que pueda generar todos esos formatos. Por ejemplo, puede analizar imágenes y audio, pero generar únicamente texto.



### Razonamiento
Es la capacidad del modelo para analizar información, relacionar conceptos y resolver problemas paso a paso hasta llegar a una conclusión.

A nivel práctico podemos encontrar distintos ejemplos:

- **Descomponer problemas**: dividir un problema complejo en otros más sencillos.
- **Relacionar información**: conectar diferentes datos para extraer conclusiones.
- **Realizar deducciones**: obtener información nueva a partir de datos conocidos.
- **Comparar alternativas**: evaluar distintas opciones y sus posibles consecuencias.
- **Resolver problemas matemáticos y lógicos**.
- **Planificar tareas**: establecer los pasos necesarios para llegar a un objetivo.

Tomando como ejemplo ChatGPT cuando activas el modo Think este lo que hace es dedicar más esfuerzo computacional al razonamiento. 

Utilizar más **razonamiento** suele **incrementar el** **consumo de tokens** y también ocupar **más espacio de la ventana de contexto**.


### Alucinaciones
Son respuestas que dan los modelos con información incorrecta, inventada o no contrastada. A simple vista estas respuestas parecen igual de convincentes que una correcta.

##### ¿Por qué se producen?
A diferencia de las BDD tradicionales donde la información es fija y la respuesta siempre es la misma, los LLM generan su respuesta a partir de patrones aprendidos y su contexto, por lo tanto pueden generar una respuesta no deseada.

Se pueden producir alucinaciones por diferenntes motivos:
- **Falta de información o información desactualizada.**
- **Ambigüedad**: interpretación erronea de una pregunta.
- **Pérdida de contexto** no dispone de acceso o se ha resumido perdiendo detalles.
- **Errores de razonamiento**: dedudcciones incorrectas o errores en una secuencia de pasos.

##### Ejemplos de alucinaciones
- Datos inventados: atribuir una fecha o cifra incorrecta a un acontecimiento.
- Fuentes inexistentes: inventar un artículo cientifico, una publicación  o una referencia bibliográfica.
- Afirmaciones falsas: describir como cierta una característica que un producto o servicio no tiene.
- Interpretaciones incorrectas: analizar una imágen o un documento y afirmar que contiene algo que realmente no aparece.

##### ¿Como podemos reducirlas?
No podemos eliminarlas completamente pero si reducir su probabilidad:
- Proporcionar contexto: facilitar la información necesaria para responder.
- Ser específicos: formular preguntas claras y concretas.
- Utilizar fuentes fiables: pedir que las respuestas se apoyen en documentación o referencias verificables.
- Consultar información actualizada: pedir que realice búsquedas web o fuentes externas si necesitamos información actualizada. Por ejemplo documentación sobre una versión de software nueva que acaba de salir, comparar o valorar productos del mercado actual.
- Verificar las respuestas: comprobar especialmente fechas, cifras, citas y afirmaciones importantes.
- Permitir que el modelo reconozca la incertidumbre: indicarle que admita cuando no dispone de información suficiente.
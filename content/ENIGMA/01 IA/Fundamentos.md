---
title: Fundamentos IA
---
##### LLM: Large Lenguage Model
Modelo de IA entrenado con grandes cantidades de texto con el fin de aprender patrones del lenguaje y generar respuestas mediante probabilidad. 

#### Inferencia
Es el proceso que realiza un LLM ya entrenado para generar una respuesta a una petición nueva.

El proceso sería:

1. **Entrada:** recibe tu pregunta.
    
2. **Inference:**  procesa el texto y genera una respuesta token a token.
    
3. **Salida:** produce una respuesta.

#### Parámetros
Valores internos del modelo, aprendidos durante su entrenamiento, que determina como procesa y genera la información.

**Ejemplo** Un módelo de 7.000 millones de parámetros tiene 7.000 millones de valores internos ajustados durante su entrenamiento.

**¿Más es mejor?** Generalmente, más parámetros pueden permitir una mayor capacidad, pero no garantizan mejores resultados. También importan los datos (calidad y cantidad), la arquitectura y el entrenamiento. Capacidad ≠ calidad.

💭 Reflexión: Un modelo mal entrenado bien sea por unos datos o una arquitectura deficiente es como un turismo con el consumo de un todo terreno. El objetivo debería ser conseguir que cada parámetro sea más útil. Es decir, conseguir más capacidad con menos recursos. ¿No sería ideal que durante el propio entrenamiento fuese el propio modelo quien determinase el nº de parámetros que necesita?

###### Arquitectura Dense vs MOE
Existen actualmente dos arquitecturas que conviene distinguir:

- Modelos densos (**Dense**): Utilizan prácticamente todos sus parámetros para procesar cada token.

- Modelos de Expertos (**MoE**): Tienen muchos parámetros distribuidos en diferentes expertos, pero solo activan una parte para cada token. Esto permite construir modelos enormes sin tener que utilizar todos sus parámetros en cada operación. Baja su consumo computacional. Ejemplo: **Llama 4 Maverick (MoE)** - 400B / 17B - 400.000 millones totales / 17.000 millones activos por token

###### Clasificación orientativa actualizada (2026)

| Tamaño      | Nº de parámetros | Características                                                         |
| ----------- | ---------------- | ----------------------------------------------------------------------- |
| Ultraligero | Menos de 1B      | Dispositivos móviles, tareas sencillas y ejecución local.               |
| Pequeño     | 1B–8B            | Asistentes locales, tareas generales y consumo reducido de recursos.    |
| Mediano     | 8B–30B           | Buen equilibrio entre capacidad, rendimiento y recursos.                |
| Grande      | 30B–70B          | Mayor capacidad para razonamiento, programación y tareas complejas.     |
| Muy grande  | 70B–200B         | Modelos de gran capacidad que suelen requerir hardware especializado.   |
| Masivo      | Más de 200B      | Modelos de escala extraordinaria, frecuentemente con arquitecturas MoE. |

*Nota: Esta clasificación es orientativa y se refiere principalmente al número de parámetros totales de modelos Dense. En arquitecturas MoE es necesario diferenciar entre parámetros totales y parámetros activos por token, ya que pueden existir modelos con cientos de miles de millones de parámetros totales que utilizan solamente una pequeña parte en cada operación

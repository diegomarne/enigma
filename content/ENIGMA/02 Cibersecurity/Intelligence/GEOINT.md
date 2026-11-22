
# Overpass turbo [link](https://overpass-turbo.eu/)

Es una herramienta gratuita que permite buscar, consultar y extraer datos geográficos de **OpenStreetMap (OSM)** mediante consultas personalizadas.

#### ¿Para qué sirve?

Por ejemplo, puedes utilizar Overpass Turbo para:

- 🏢 Encontrar todos los edificios de un municipio.
    
- 🚲 Localizar carriles bici y rutas ciclistas.
    
- 🅿️ Buscar aparcamientos.
    
- 🏥 Identificar hospitales, farmacias y centros de salud.
    
- 🌳 Localizar parques, fuentes y zonas verdes.
    
- 🛣️ Extraer carreteras, caminos y senderos.
    
- 🏪 Obtener información de establecimientos comerciales.

#### ¿Cómo funciona?
Utiliza un lenguaje de consulta llamado **Overpass QL**, que permite pedir información específica a OpenStreetMap. **OpenStreetMap** es la base de datos geográfica, **Overpass API** permite consultar sus datos y **Overpass Turbo** es la interfaz visual que facilita hacer esas consultas.

![[overpass-turbo-rainbolt.png]]

#### ¿Qué puedes hacer con los resultados?
- Visualizarlos sobre un mapa.
- Exportarlos a formatos como GeoJSON, KML o GPX, según el tipo de datos.
- Utilizarlos en QGIS.
- Analizarlos con Python.
- Crear mapas personalizados.

#### EJEMPLO
El siguiente fragmento es extrado de un [video de YT del canal de RAINBOLT](https://www.youtube.com/watch?v=pO6Hf3SdVcY).

El código de la izquierda permite localizar y extraer información geográfica de los puertos, marinas y zonas portuarias de Palermo. Además, representa los resultados en el mapa  con diferentes colores para distinguir cada tipo de instalación portuaria.

![[overpass-turbo-rainbolt.png]]

El código es el siguiente:

```js

// Los resultados se obtienen en JSON.
// Tiempo máximo de ejecución de 60".
[out:json][timeout:60];


// SELECCIÓN DEL ÁREA GEOGRÁFICA: PALERMO

// Busca el área administrativa de Palermo.
// boundary=administrative: límite administrativo.
// admin_level=6: nivel administrativo asignado.
// name=Palermo: nombre del área buscada.
// ->.palermo: guarda el área encontrada en la variable .palermo.

area["boundary"="administrative"]["admin_level"="6"]["name"="Palermo"]->.palermo;


// BÚSQUEDA DE PUERTOS, INSTALACIONES PORTUARIAS Y MARINAS

// Busca los elementos dentro del área .palermo.
// n = node (punto).
// w = way (línea o polígono).
// r = relation (relación entre elementos).

(
  // 1. Puertos identificados mediante etiquetas marítimas.
  nwr["seamark:type"="harbour"](area.palermo);

  // 2. Elementos que contienen la etiqueta harbour.
  nwr["harbour"](area.palermo);

  // 3. Marinas o puertos deportivos.
  nwr["leisure"="marina"](area.palermo);

  // 4. Instalaciones portuarias industriales.
  nwr["industrial"="port"](area.palermo);

);


// OBTENCIÓN Y RECUPERACIÓN DE LOS RESULTADOS

// Devuelve los elementos encontrados y el centro
// de las geometrías de vías y relaciones.
out center;

// Recupera los elementos integrantes de las geometrías
// seleccionadas, como los nodos que forman las vías.
>;

// Devuelve la información geométrica básica de esos
// elementos, ordenada mediante quadtile.
out skel qt;


// PERSONALIZACIÓN DE LA SIMBOLOGÍA DEL MAPA

// {{style: inicia el bloque de estilos de Overpass Turbo.
// Permite asignar colores diferentes según las etiquetas
// de OpenStreetMap de cada elemento geográfico.
{{style:

  // 1. PUERTOS MARÍTIMOS (SEAMARK) (rojo: #d7191c)
  node[seamark:type=harbour],
  way[seamark:type=harbour],
  relation[seamark:type=harbour] {
    color: #d7191c;       /* Color del contorno */
    fill-color: #d7191c;  /* Color del relleno */
    width: 4;             /* Grosor del contorno */
  }


  // 2. HARBOUR / MARINAS (naranja: #fdae61)
  node[harbour],
  way[harbour],
  relation[harbour] {
    color: #fdae61;       
    fill-color: #fdae61; 
    width: 4;            
  }


  // 3. MARINAS O PUERTOS DEPORTIVOS (azul: #2c7bb6)
  node[leisure=marina],
  way[leisure=marina],
  relation[leisure=marina] {
    color: #2c7bb6;      
    fill-color: #2c7bb6; 
    width: 4;             
  }


  // 4. PUERTOS INDUSTRIALES (morado: #7b3294)
  node[industrial=port],
  way[industrial=port],
  relation[industrial=port] {
    color: #7b3294;       
    fill-color: #7b3294; 
    width: 4;            
  }

}}
```



# SunCalc.org [link](https://www.suncalc.org)

Es una herramienta gratuita online que permite calcular y visualizar la posición del Sol en cualquier lugar del mundo y en cualquier fecha. Es especialmente útil para fotografía, arquitectura, construcción, agricultura, orientación de viviendas y planificación de actividades al aire libre.

#### ¿Que permite hacer SunCalc?

Sus principales funciones son:
- Saber la hora del atardece y anochecer
- Trayectoria solar
- Altitud solar: Saber a que altura está sobre el horizonte.
- Azimut solar: Conocer la dirección exacta del Sol respecto al norte.
- Sombras: Estimar la longitud de las sombras según la altura de un objeto.
- Evolución anual: Comparar la trayectoria solar durante distintas épocas del año.



# Bellingcat Shadow Finder
Gratuita y de código abierto. Permite introducir la altura de un objeto, la longitud de su sombra y una fecha/hora conocidas para calcular zonas geográficas compatibles con esa geometría solar.

Es especialmente útil cuando conoces la hora aproximada pero todavía no la ubicación.

- [Bellingcat: How to Tell When a Photo Was Taken by Measuring Shadows](https://www.bellingcat.com/resources/2021/05/18/unsure-when-a-video-or-photo-was-taken-how-to-tell-by-measuring-the-length-of-shadows/?utm_source=chatgpt.com). Es probablemente el recurso más directamente relacionado con tu objetivo.
    
- [Bellingcat: Shadow Finder](https://colab.research.google.com/github/Bellingcat/ShadowFinder/blob/main/ShadowFinderColab.ipynb?utm_source=chatgpt.com). Cuaderno de investigación para el análisis geográfico inverso de sombras.



# Shademap
Gratuita con funciones de pago. Simula las sombras de edificios y relieves según ubicación, fecha y hora. Muy interesante para contrastar una hipótesis con el entorno real.


# Heliora
Calcula cuándo el Sol aparece o desaparece detrás de montañas y relieves. También incluye planificación de hora dorada, Luna y eventos astronómicos. No tiene en cuenta edificios ni árboles.

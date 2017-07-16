# MapProxy

"MapProxy (http://mapproxy.org/) es un servidor de teselas y proxy WMS Open Source, acelera las aplicaciones de mapas a través de la pregeneración de tiles integrando múltiples fuentes de datos y almacenándolos en una caché."

Cuando empiezas a utilizar herramientas como OpenStreetMap para hacer mapas te das cuenta que parte importante del trabajo es hacer **Foto Interpretación** que consiste en dibujar objetos sobre una foto aerea de una zona para convertirlos en elementos de un mapa.

Con la idea de aprender como funcionan diferentes herramientas que utilizo como voluntario de OSM he estado explorando algunas herramientas que me permitan implementar servidores para servir OrtoFotos (Fotos satelitales o aéreas tomadas con drones o similares). Siguiendo esta idea me encontré con la necesidad de hacer un "cache" o "respaldo" de algunos servicios web que no pueden utilizarse de manera sencilla en las herramientas que usa OSM para hacer el mapa.

El problema especifico es poder servir servicios tipos WMTS que entregan la mayoría de servidores ArcGis que usan las instituciones para crear sus sistemas de información geográfico, el plan es convertir esos WMTS (Web Map Tile Service. es una especificación internacional para proporcionar mapas digitales en la Web utilizando teselas de imágenes en caché) en un TMS (Tile Map Service. Es otro estándar para servir mapas web usado principalmente por OpenStreetMap)

Los TMS son mucho mas fácil de utilizar en diferentes tipos de aplicaciones, ademas es la fuente de imágenes utilizadas por el servidor de tareas (tareas.openstreetmap.co) que se utiliza para coordinar trabajo de cartografía de Zonas.

## Instalación:

MapProxy se puede instalar en cualquier maquina linux o Windows, yo uso linux. Se recomienda utilizar un entorno virtual de Python pero puede instalarse de manera sencilla si tienes instalado Python PIP ejecutando el comando:

`pip install MapProxy` 

Al instalarlo de esta manera instala la ultima versión que es mi recomendación. 
En el proceso de búsqueda encontré un muy buen documento en español que me guio en todo el proceso:

http://mapproxy-workshop.readthedocs.io/es/latest/instalacion-configuracion.html

Dejo el enlace para no irme hasta ese tema.


## Creación del TMS usando un WMTS

La parte mas importante del proceso es entender la API REST de los servidores de ArcGIS a mi me tomo algo de trabajo entender como se debe construir la dirección que se utiliza en la configuración de MapProxy para que este pueda leer sin problema los OrtoFotoMosaicos y hacer el cache de manera exitosa. 

- Lo primero que hay que hacer es encontrar la URL que te lleva a las especificaciones tecnicas del servicio WMTS el formato es de la siguiente forma: **/WMTS/1.0.0/WMTSCapabilities.xml** 
En esta URL encontraremos todas las especificaciones para formar la URL usada por MapProxy

- En ese archivo XML busca la etiqueta ** GetTile ** vas a ver una URL terminada en lo siguiente: **/WMTS/tile/1.0.0/**

- Busca la etiqueta **Layer** va a decirnos el nombre de la capa que vamos a llamar.

- Busca la etiqueta **BoundingBox** en el XML alli vas a encontrar el sistema de proyecciones que utiliza el servidor al que nos estamos conectando en mi caso es **EPSG:3857** el estandar de Mercartor también encontrara las etiquetas **LowerCorner UpperCorner** que son las coordenadas que definen el recuadro de la OrtoFoto o datos a utilizar.

- Busca la etiqueta **Style** al interior de esta etiqueta encontramos la etiqueta **Identifier** que nos dice el nombre del estilo.

- Por ultimo busca la etiqueta **TileMatrixSet** nos entrega el nombre del conjunto de Tiles y sus propiedades, un WMTS puede tener varios conjuntos para diferentes propósitos yo tuve suerte y encontré uno para utilizar con GoogleMaps que es compatible con OSM entonces decidi utilizar ese.

Entonces para formar la URL que vas a poner en la configuración del MapProxy solo debes juntar las partes asi:

**/WMTS/tile/1.0.0/-Title Layer-/Style-Identifier-/TileMatrixSet/%(z)s/%(y)s/%(x)s.png**

Ejemplo como quedó la url del server a usar:

**http://ServidorArcGis:81/arcgis/rest/services/imagenes/Foto_Mercator/ImageServer/WMTS/tile/1.0.0/Foto_OrtoMercartor/default/GoogleMapsCompatible/%(z)s/%(y)s/%(x)s.png**

A continuación dejo una copia de mi archivo de configuración de pruebas en el que integro todo lo encontré en la API que me permitió conectarme al servicio WMTS de un servidor ArcGis:

```
services:
  demo:
  tms:
    use_grid_names: false
    origin: 'sw'
  kml:
      use_grid_names: true
  wmts:
  wms:
    md:
      title: Mapa
      abstract: Pruebas de Mapa

layers:
  - name: osm
    title: OrtofotoMosaico de XYZ
    sources: [osm_cache]

caches:
   osm_cache:
    grids: [webmercator]
    bulk_meta_tiles: true
    sources: [osm_wms]
    cache:
      type: sqlite
      directory: osmcache

sources:
  osm_wms:
    type: tile
    grid: GLOBAL_WEBMERCATOR
    url: http://ServidorArcGis:81/arcgis/rest/services/imagenes/Foto_Mercator/ImageServer/WMTS/tile/1.0.0/Foto_OrtoMercartor/default/GoogleMapsCompatible/%(z)s/%(y)s/%(x)s.png
    coverage:
      bbox: [-8615055.4593,586156.2752010676,-8208030.384301049,1001144.7751999982]
      srs: 'EPSG:3857'

grids:
    webmercator:
        base: GLOBAL_WEBMERCATOR

globals:

```

Como esto no pretende ser un manual avanzado recomiendo leer la documentación oficial de MapProxy en http://mapproxy.org/

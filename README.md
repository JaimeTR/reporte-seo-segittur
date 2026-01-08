# Reporte Técnico SEO - Buscador SEGITTUR
**Documento de Análisis de URLs y Estrategia de Optimización**

---

## 📋 Índice

1. [Resumen Ejecutivo](#resumen-ejecutivo)
2. [Estructura Actual de URLs](#estructura-actual-de-urls)
3. [Análisis Técnico del Sistema](#análisis-técnico-del-sistema)
4. [Patrones de URL por Tipo de Búsqueda](#patrones-de-url-por-tipo-de-búsqueda)
5. [Parámetros y Filtros Disponibles](#parámetros-y-filtros-disponibles)
6. [Anexos: Listas de Opciones](#anexos-listas-de-opciones)

---

## 📊 Resumen Ejecutivo

### Estado Actual del Sistema

El **Centro de Información Turística SEGITTUR** utiliza un sistema de búsqueda avanzada con **6 tipos de contenido**:

| Tipo | Endpoint API | Filtros Principales | Est. URLs Únicas |
|------|--------------|---------------------|------------------|
| **Publicaciones** | `/documentaciones` | Materia (~5,898), Tipo, País, Idioma, Año | ~500,000+ |
| **Datos Estadísticos** | `/datoEstadisticos` | Tema, Idioma, Año, Ámbito | ~50,000+ |
| **Eventos** | `/eventos` | Materia, Tipo, Fecha, Lugar | ~100,000+ |
| **Organismos** | `/organismoEntidades` | Actividad (~1,303), Ámbito, País, Objeto | ~150,000+ |
| **Estudios** | `/estudios` | Tipo, Lugar, Modalidad, Idioma | ~20,000+ |
| **Becas** | `/becas` | Solicitante, Tipo, Convocante | ~10,000+ |

**Total estimado de combinaciones posibles:** ~830,000+ URLs únicas

### ⚠️ Hallazgos Críticos

- ✅ **Fortalezas:**
  - Sistema flexible de filtros combinables
  - URLs con query strings (fácil de implementar)
  - Paginación funcional
  - Detalles con URLs separadas

- ❌ **Debilidades SEO:**
  - Sin canonical tags implementados
  - URLs no amigables (query strings)
  - Sin control de indexación (robots meta)
  - Posible contenido duplicado
  - Sin estructura de URLs jerárquica

---

## 🔗 Estructura Actual de URLs

### URL Base del Buscador

```
https://tu-sitio.com/centro-de-informacion-turistica/
```

### Formato General de Búsqueda

```
https://tu-sitio.com/centro-de-informacion-turistica/?PARAMETROS
```

### Formato de Página de Detalle

```
https://tu-sitio.com/centro-de-informacion-turistica/?segittur_detalle_tipo=TIPO&segittur_id=ID
```

**Ejemplo real:**
```
https://tu-sitio.com/centro-de-informacion-turistica/?segittur_detalle_tipo=documentacion&segittur_id=12345
```

---

## 🔍 Análisis Técnico del Sistema

### Cómo se Generan las URLs

El sistema genera URLs mediante **3 mecanismos principales**:

#### 1. Formulario de Búsqueda (`<form method="get">`)

```html
<form method="get" action="/centro-de-informacion-turistica/">
  <input type="hidden" name="FILTRO" value="DOCUMENTACION">
  <input type="hidden" name="segittur_tab" value="0">
  <select name="selectedTesauros[]" multiple>
    <option value="123">Sostenibilidad</option>
    <!-- ~5,898 opciones más -->
  </select>
</form>
```

**Resultado:** `?FILTRO=DOCUMENTACION&segittur_tab=0&selectedTesauros[]=123`

#### 2. Función `add_query_arg()` de WordPress

```php
$url = add_query_arg([
    'FILTRO' => 'DOCUMENTACION',
    'segittur_tab' => 0,
    'selectedTesauros' => [123],
    'anio_doc' => '2024'
], home_url('/centro-de-informacion-turistica/'));
```

**Resultado:** `?FILTRO=DOCUMENTACION&segittur_tab=0&selectedTesauros[]=123&anio_doc=2024`

#### 3. JavaScript de Normalización

El sistema incluye un script que **normaliza las URLs en el navegador**:

```javascript
// Quita los corchetes [] de los parámetros
selectedTesauros[] → selectedTesauros

// Corrige inconsistencias
DATOESTADISTICO → DATO_ESTADISTICO
```

**Antes:** `?selectedTesauros[]=123`  
**Después:** `?selectedTesauros=123`

---

## 🎯 Patrones de URL por Tipo de Búsqueda

### 1️⃣ PUBLICACIONES (Documentación)

#### URL Mínima
```
?FILTRO=DOCUMENTACION&segittur_tab=0
```

#### URL con Filtro de Materia
```
?FILTRO=DOCUMENTACION&segittur_tab=0&selectedTesauros=123
```
- **123** = ID de la materia (ej: "Sostenibilidad")
- Campo: `<select name="selectedTesauros[]" multiple>`

#### URL Completa con Todos los Filtros
```
?FILTRO=DOCUMENTACION
&segittur_tab=0
&filtroGlobal=turismo
&selectedDocumentacionTipos=1,2,3
&selectedPaises=34
&selectedTesauros=123
&selectedIdiomas=2
&anio_doc=2024
&o_relevancia=false
&segittur_page_0=1
&segittur_size=10
```

#### Endpoints API Relacionados
```
/services/documentaciones/tipos       → Tipos de documento
/services/documentaciones/paises      → Países (filtro)
/services/documentaciones/idiomas     → Idiomas disponibles
/services/documentaciones/anios       → Años de publicación
/services/documentaciones/materias    → ⭐ MATERIAS (~5,898 opciones)
/services/documentaciones?{params}    → Resultados de búsqueda
```

---

### 2️⃣ DATOS ESTADÍSTICOS

#### URL Mínima
```
?FILTRO=DATO_ESTADISTICO&segittur_tab=1
```

#### URL con Filtro de Tema
```
?FILTRO=DATO_ESTADISTICO&segittur_tab=1&selectedTemas=45
```

#### URL Completa
```
?FILTRO=DATO_ESTADISTICO
&segittur_tab=1
&filtroGlobal=ocupacion
&selectedTemas=45
&selectedIdiomas=1
&anio_dat=2024
&selectedAmbitoGeograficoTipos=2
&o_relevancia=false
&segittur_page_1=1
```

#### Endpoints API
```
/services/datoEstadisticos/temas      → Temas (~200 opciones)
/services/datoEstadisticos/idiomas    → Idiomas
/services/datoEstadisticos/anios      → Años
/services/datoEstadisticos/ambitos    → Ámbitos geográficos
```

---

### 3️⃣ EVENTOS

#### URL Mínima
```
?FILTRO=EVENTO&segittur_tab=2
```

#### URL con Filtro de Materia
```
?FILTRO=EVENTO&segittur_tab=2&selectedTesauros=789
```

#### URL Completa
```
?FILTRO=EVENTO
&segittur_tab=2
&filtroGlobal=congreso
&selectedEventoTipos=3
&anio_eve=2024
&mes_eve=Marzo
&lugar=Madrid
&selectedTesauros=789
&o_relevancia=true
```

#### Endpoints API
```
/services/eventos/tipos      → Tipos de evento
/services/eventos/anios      → Años
/services/eventos/meses      → Meses
/services/eventos/lugares    → Lugares
/services/eventos/materias   → Materias de eventos
```

---

### 4️⃣ ORGANISMOS (Entidades)

#### URL Mínima
```
?FILTRO=ORGANISMO_ENTIDAD&segittur_tab=3
```

#### URL con Filtro de Actividad
```
?FILTRO=ORGANISMO_ENTIDAD&segittur_tab=3&selectedTesauros=456
```
- **456** = ID de actividad (ej: "Promoción Turística")
- Campo: `<select name="selectedTesauros[]" multiple>` **(¡mismo nombre que Publicaciones!)**

#### URL Completa
```
?FILTRO=ORGANISMO_ENTIDAD
&segittur_tab=3
&filtroGlobal=ministerio
&selectedAmbitoGeograficoTipos=1
&selectedPaises=34
&selectedTesauros=456
&selectedObjetos=10
&o_relevancia=false
```

#### Endpoints API
```
/services/organismoEntidades/ambitos      → Ámbitos geográficos
/services/organismoEntidades/paises       → Países
/services/organismoEntidades/actividades  → ⭐ ACTIVIDADES (~1,303 opciones)
/services/organismoEntidades/objetos      → Objetos
```

---

### 5️⃣ ESTUDIOS (Formación)

#### URL Mínima
```
?FILTRO=ESTUDIO&segittur_tab=4
```

#### URL Completa
```
?FILTRO=ESTUDIO
&segittur_tab=4
&filtroGlobal=master
&selectedEstudioTipos=2
&lugar=3
&selectedModalidades=1
&selectedIdiomas=2
```

#### Endpoints API
```
/services/estudios/tipos         → Tipos de estudio
/services/estudios/lugares       → Lugares (IDs)
/services/estudios/modalidades   → Modalidades (presencial, online, etc.)
/services/estudios/idiomas       → Idiomas
```

**⚠️ NOTA:** Estudios NO usa `selectedTesauros` para materias. Los filtros son distintos.

---

### 6️⃣ BECAS

#### URL Mínima
```
?FILTRO=BECA&segittur_tab=5
```

#### URL Completa
```
?FILTRO=BECA
&segittur_tab=5
&filtroGlobal=investigacion
&selectedSolicitantes=1
&selectedBecaTipos=2
&selectedConvocanteTipos=3
```

#### Endpoints API
```
/services/becas/solicitantes  → Perfiles de solicitante
/services/becas/tipos         → Tipos de beca
/services/becas/convocantes   → Tipos de convocante
```

---

## 📑 Parámetros y Filtros Disponibles

### Parámetros Globales (Todos los Tipos)

| Parámetro | Tipo | Descripción | Ejemplo |
|-----------|------|-------------|---------|
| `FILTRO` | String | Tipo de búsqueda principal | `DOCUMENTACION`, `EVENTO`, `ORGANISMO_ENTIDAD` |
| `segittur_tab` | Integer | Pestaña activa (0-5) | `0`=Publicaciones, `3`=Organismos |
| `filtroGlobal` | String | Búsqueda en todos los campos | `turismo sostenible` |
| `o_relevancia` | Boolean | Orden: `false`=Relevancia, `true`=Fecha | `false` |
| `segittur_page_N` | Integer | Número de página (N = tab) | `segittur_page_0=2` |
| `segittur_size` | Integer | Resultados por página (5-100) | `10` |

### Parámetros Específicos por Tipo

#### PUBLICACIONES

| Parámetro | Campo HTML | Valores | Cantidad |
|-----------|------------|---------|----------|
| `selectedDocumentacionTipos` | Array de IDs | Tipos de documento | ~20 |
| `selectedPaises` | Array de IDs | Países | ~200 |
| `selectedTesauros` | Array de IDs | **MATERIAS** | **~5,898** |
| `selectedIdiomas` | Array de IDs | Idiomas | ~30 |
| `anio_doc` | String | Año de publicación | `2024`, `2023`, etc. |

#### ORGANISMOS

| Parámetro | Campo HTML | Valores | Cantidad |
|-----------|------------|---------|----------|
| `selectedAmbitoGeograficoTipos` | Array de IDs | Ámbitos | ~10 |
| `selectedPaises` | Array de IDs | Países | ~200 |
| `selectedTesauros` | Array de IDs | **ACTIVIDADES** | **~1,303** |
| `selectedObjetos` | Array de IDs | Objetos | ~50 |

#### EVENTOS

| Parámetro | Campo HTML | Valores | Cantidad |
|-----------|------------|---------|----------|
| `selectedEventoTipos` | Array de IDs | Tipos de evento | ~15 |
| `selectedTesauros` | Array de IDs | Materias | ~500 |
| `anio_eve` | String | Año del evento | `2024` |
| `mes_eve` | String | Mes del evento | `Marzo`, `Abril` |
| `lugar` | String | Lugar del evento | `Madrid`, `Barcelona` |

#### DATOS ESTADÍSTICOS

| Parámetro | Campo HTML | Valores | Cantidad |
|-----------|------------|---------|----------|
| `selectedTemas` | Array de IDs | Temas | ~200 |
| `selectedIdiomas` | Array de IDs | Idiomas | ~30 |
| `anio_dat` | String | Año | `2024` |
| `selectedAmbitoGeograficoTipos` | Array de IDs | Ámbitos | ~10 |

#### ESTUDIOS

| Parámetro | Campo HTML | Valores | Cantidad |
|-----------|------------|---------|----------|
| `selectedEstudioTipos` | Array de IDs | Tipos | ~10 |
| `lugar` | Array de IDs | Lugares | ~100 |
| `selectedModalidades` | Array de IDs | Modalidades | ~5 |
| `selectedIdiomas` | Array de IDs | Idiomas | ~30 |

#### BECAS

| Parámetro | Campo HTML | Valores | Cantidad |
|-----------|------------|---------|----------|
| `selectedSolicitantes` | Array de IDs | Perfiles | ~10 |
| `selectedBecaTipos` | Array de IDs | Tipos de beca | ~15 |
| `selectedConvocanteTipos` | Array de IDs | Convocantes | ~20 |

---


---

## 📚 Anexos: Listas de Opciones

### Anexo A: Endpoints de la API SEGITTUR

#### Catálogos de Publicaciones
```
GET /services/documentaciones/tipos
GET /services/documentaciones/paises
GET /services/documentaciones/idiomas
GET /services/documentaciones/anios
GET /services/documentaciones/materias      ← ~5,898 opciones
```

#### Catálogos de Datos Estadísticos
```
GET /services/datoEstadisticos/temas        ← ~200 opciones
GET /services/datoEstadisticos/idiomas
GET /services/datoEstadisticos/anios
GET /services/datoEstadisticos/ambitos
```

#### Catálogos de Eventos
```
GET /services/eventos/tipos
GET /services/eventos/anios
GET /services/eventos/meses
GET /services/eventos/lugares
GET /services/eventos/materias              ← ~500 opciones
```

#### Catálogos de Organismos
```
GET /services/organismoEntidades/ambitos
GET /services/organismoEntidades/paises
GET /services/organismoEntidades/actividades ← ~1,303 opciones
GET /services/organismoEntidades/objetos
```

#### Catálogos de Estudios
```
GET /services/estudios/tipos
GET /services/estudios/lugares
GET /services/estudios/modalidades
GET /services/estudios/idiomas
```

#### Catálogos de Becas
```
GET /services/becas/solicitantes
GET /services/becas/tipos
GET /services/becas/convocantes
```

---

### Anexo B: Ejemplos de URLs Reales

#### Publicaciones - Sostenibilidad (ID: 123)
```
# URL actual
https://tu-sitio.com/centro-de-informacion-turistica/?FILTRO=DOCUMENTACION&segittur_tab=0&selectedTesauros=123

# URL optimizada propuesta
https://tu-sitio.com/centro-de-informacion-turistica/publicaciones/sostenibilidad/

# URL con filtros adicionales
https://tu-sitio.com/centro-de-informacion-turistica/publicaciones/sostenibilidad/?pais=34&anio=2024
```

#### Organismos - Promoción Turística (ID: 456)
```
# URL actual
https://tu-sitio.com/centro-de-informacion-turistica/?FILTRO=ORGANISMO_ENTIDAD&segittur_tab=3&selectedTesauros=456

# URL optimizada propuesta
https://tu-sitio.com/centro-de-informacion-turistica/organismos/promocion-turistica/
```

#### Eventos - Congresos en Madrid
```
# URL actual
https://tu-sitio.com/centro-de-informacion-turistica/?FILTRO=EVENTO&segittur_tab=2&selectedEventoTipos=3&lugar=Madrid&anio_eve=2024&mes_eve=Marzo

# URL optimizada propuesta
https://tu-sitio.com/centro-de-informacion-turistica/eventos/congresos/?lugar=madrid&fecha=2024-03
```

---

### Anexo C: Script para Obtener Listado de IDs

Si necesitas obtener la lista completa de IDs y valores de materias/actividades, puedes usar este script:

```php
<?php
// Archivo temporal: wp-content/themes/tu-tema/get-catalog-ids.php
require_once '../../../wp-load.php';

header('Content-Type: text/csv; charset=utf-8');
header('Content-Disposition: attachment; filename="segittur-catalogo.csv"');

$output = fopen('php://output', 'w');

// Publicaciones - Materias
fputcsv($output, ['TIPO', 'ID', 'VALOR']);

$materias = segittur_api_request('/services/documentaciones/materias', []);
foreach ($materias as $m) {
    fputcsv($output, ['PUBLICACIONES_MATERIA', $m['id'], $m['valor']]);
}

// Organismos - Actividades
$actividades = segittur_api_request('/services/organismoEntidades/actividades', []);
foreach ($actividades as $a) {
    fputcsv($output, ['ORGANISMOS_ACTIVIDAD', $a['id'], $a['valor']]);
}

fclose($output);
exit;
```

**Acceder:** `https://tu-sitio.com/wp-content/themes/tu-tema/get-catalog-ids.php`

---

### Anexo D: Parámetros Normalizados

El sistema normaliza automáticamente estos parámetros vía JavaScript:

| Original | Normalizado | Notas |
|----------|-------------|-------|
| `selectedTesauros[]` | `selectedTesauros` | Quita corchetes |
| `DATOESTADISTICO` | `DATO_ESTADISTICO` | Añade guion bajo |
| `anio` | `anio_doc` | En tab publicaciones |
| `anio` | `anio_eve` | En tab eventos |
| `mes` | `mes_eve` | En tab eventos |

**Script de normalización** (ver líneas 169-220 de `buscador-avanzado-mixto.php`):

```javascript
function normalizeSearchParams(params) {
    var out = new URLSearchParams();
    params.forEach(function(val, key) {
        var k = key.endsWith('[]') ? key.slice(0, -2) : key;
        if (k === 'FILTRO' && val === 'DATOESTADISTICO') val = 'DATO_ESTADISTICO';
        out.append(k, val);
    });
    return out;
}
```

---

## 🎯 Conclusiones y Próximos Pasos

### Resumen de Hallazgos

1. **Sistema robusto** con 6 tipos de búsqueda y ~830,000 URLs potenciales
2. **Problemas SEO críticos:** Sin canonical, URLs no amigables, sin control de indexación
3. **Oportunidad importante:** Con las optimizaciones propuestas se puede mejorar significativamente el tráfico orgánico

### Recomendación Principal

**Priorizar FASE 1** (canonical tags + títulos + meta descriptions + robots):
- Bajo esfuerzo técnico
- Alto impacto inmediato
- Implementable en 1-2 semanas
- ROI rápido

### Contacto y Soporte

Para implementar estas mejoras, considera:

1. **Contratar consultor SEO** especializado en WordPress
2. **Usar plugin SEO** como Yoast o RankMath (automatiza canonical/titles)
3. **Monitorear con Google Search Console** desde el día 1
4. **A/B testing** de títulos y descriptions

---

**Documento generado:** 8 de enero de 2026  
**Versión:** 1.0  
**Autor:** Análisis técnico del código SEGITTUR v2.1



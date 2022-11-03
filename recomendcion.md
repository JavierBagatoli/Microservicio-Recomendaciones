## Microservicio de Recomendacion

### Casos de uso

#### CU: Incrementar etiquetas busqueda
- Precondicion: El usuario esta registrado y logeado

- Camino normal:
    - Al acceder a los detalles de un articulo, el servicio de catalogo incrementa un contador de las etiquetas que tiene el articulo en el microservicio de de Recomendaciones (IdPersona, etiqutas, busquedas)


#### CU: Recomendar articulos
-Camino normal:
    - Al acceder la pagina principal se mostraran tres articulos como recomendados.
    - La GUI enviara una llamada al microservicio de Recomendaciones, se buscara en la base de datos las etiquetas mas buscada y se pedira al microservicio de articulos que entregue una lista de articulos.
        - si alguno de los articulos esta entre los no recomendados del usuario se descarta.

#### CU: Dejar de recomendar articulo
    -En la GUI cada articulo tendra la oportunidad de dejar de recomendar, lo incluye en un lista de no recomendados.
    -Si los articulos comparten etiqueta, se le restara puntaje a la etiqueta para que deje de ser recomendados.

#### CU: Recomendar por articulo relacionado
    -Al acceder al detalle de un articulo, la GUI enviara informacion del articulo como nombre y etiquetas para que el microservicio de recomendaciones pida a articulos elementos similares.

### Modelo de datos

**PersonaGustos**
```json
{
    "id" : "string",
    "idUsuario" : "string",
    "gustosEtiquetas" : [{
        "etiqueta" : "string",
        "contador" : "int"
        }],
    "listaNoRecomendar" : [
        {"etiqueta" : "String",
        "idArticulos": ["id":"String", ...]}]
}
```

### Interfaz REST

**Recomendar articulos**

`GET /{IdUsuario}`

*Headers*
Authorization: Bearer token

*Response*
`200 OK` articulos recomendados
```json
{
    "articulos" : ["etiqueta", "..."]
}
```

**Recomendar por articulo relacionado**

`GET /etiquetas/{etiqueta}`

*Headers*
Authorization: Bearer token

*Response*
`200 OK` articulos recomendados
```json
{
    "articulos" : ["etiqueta", "..."]
}
```

### Interfaz asincronica (rabbit)

**Incrementar etiqueta de busqueda**

Recibe a las `etiquetas` que incrementara

body
```json
{
    "id" : "231",
    "idUsuario" : "235498213",
    "etiquetas" : ["deporte", "neumaticos"]
}
```


**Dejar de recomendar articulo**

Recibe el IdArticulo que dejara de recomendarse

body
```json
{
    "id" : "231",
    "idUsuario" : "235498213",
    "idArticulo" : "321321"
}
```

## Microservicio de Recomendacion

### Casos de uso

#### CU: Incrementar etiquetas busqueda
- Precondicion: El usuario esta registrado y logeado

- Camino normal:
    - Al acceder a los detalles de un articulo, el servicio de catalogo incrementa un contador de las etiquetas que tiene el articulo en el microservicio de de Recomendaciones (IdPersona, etiqutas, busquedas)


#### CU: Recomendar articulos
- Camino normal:
    * La GUI enviara una llamada al microservicio de Recomendaciones, encontrara las etiquetas mas buscadas y las devolvera para que la GUI pueda trabajar.
     * si alguno de los articulos esta entre los no recomendados del usuario se descarta.

#### CU: Dejar de recomendar articulo
 - Camino normal:
    * En la GUI cada articulo tendra la oportunidad de dejar de recomendar, lo incluye en un lista de no recomendados.
    * Si los articulos comparten etiqueta, se le restara puntaje a la etiqueta para que deje de ser recomendados.

#### CU: Recomendar por articulo relacionado
 - Camino normla:
    * Al acceder al detalle de un articulo, la GUI enviara las etiquetas para que el microservicio de recomendaciones devuelva si existen que artculos no se mostraran.

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
    "etiquetasRecomendadas" : ["etiqueta", "..."]
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
    "articulos" : ["idArticulo", "..."]
}
```

### Interfaz asincronica (rabbit)

**Incrementar etiqueta de busqueda**

Recibe a las `etiquetas` que incrementara

- cola: microRecomendaciones - incrementarEtiqueta
- Tipo suscripcion: 
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

- Cola: microRecomendaciones - dejarDeRecomendar
- Tipo suscripcion

body
```json
{
    "id" : "231",
    "idUsuario" : "235498213",
    "idArticulo" : "321321"
}
```

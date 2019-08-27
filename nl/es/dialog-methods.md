---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-12"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Métodos de lenguaje de expresión
{: #dialog-methods}

Puede procesar valores extraídos de expresiones del usuario a los que desea hacer referencia en una variable de contexto, condición o en alguna otra parte de la respuesta.
{: shortdesc}

## Dónde utilizar la sintaxis de la expresión
{: #dialog-methods-evaluation-syntax}

Para expandir los valores de variable dentro de otras variables, o aplicar métodos a textos de salida o variables de contexto, utilice la sintaxis de expresión `<? expression ?>`. Por ejemplo:

- **Cómo hacer referencia a la entrada de un usuario desde una respuesta de texto de nodo de diálogo**

  ```bash
  You said <? input.text ?>.
  ```
  {: codeblock}

- **Incremento de una propiedad numérica desde el editor JSON**

    ```json
    "output":{"number":"<? output.number + 1 ?>"}
    ```
    {: codeblock}

- **Adición de un elemento a una matriz de variables de contexto desde el editor de contexto**

| Nombre de variable de contexto | Valor de variable de contexto |
|-----------------------|------------------------|
| `toppings` | `<? context.toppings.append( 'onions' ) ?>` |

También puede utilizar expresiones SpEL en condiciones y condiciones de respuesta de nodo de diálogo. Cuando se utiliza una expresión en una condición, la sintaxis de `<? ?>` de alrededor no es necesaria.

- **Comprobación de un valor de entidad específico de una condición de nodo de diálogo**

  ```bash
  @city.toLowerCase() == 'paris'
  ```
  {: codeblock}

- **Comprobación de un rango de fechas específico de una condición de respuesta de nodo de diálogo**

  ```bash
  @sys-date.after(today())
  ```
  {: codeblock}

En las siguientes secciones se describen los métodos que puede utilizar para procesar valores. Están organizados por tipo de datos:

- [Matrices](#dialog-methods-arrays)
- [Fecha y hora](#dialog-methods-date-time)
- [Números](#dialog-methods-numbers)
- [Objetos](#dialog-methods-objects)
- [Series](#dialog-methods-strings)

## Matrices
{: #dialog-methods-arrays}

No puede utilizar estos métodos para comprobar un valor de una matriz en una condición de nodo o condición de respuesta dentro del mismo nodo en el que establece los valores de matriz.

### JSONArray.append(object)
{: #dialog-methods-arrays-append}

Este método añade un nuevo valor a JSONArray y devuelve la matriz JSONArray modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: codeblock}

### JSONArray.clear()
{: #dialog-methods-arrays-clear}

Este método borra todos los valores de la matriz y devuelve un valor nulo.

Utilice la expresión siguiente en la salida para definir un campo que borre una matriz que ha guardado en una variable de contexto ($toppings_array) de sus valores.

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

Si luego hace referencia a la variable de contexto $toppings_array, solo devolverá '[]'.

### JSONArray.contains(Object value)
{: #dialog-methods-arrays-contains}

Este método devuelve true si la matriz JSONArray de entrada contiene el valor de entrada.

Para este contexto de tiempo de ejecución del diálogo establecido por un nodo anterior o aplicación externa:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Nodo de diálogo o condición de respuesta:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Resultado: `True` porque la matriz contiene el elemento ham.

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-arrays-containsIntent}

Este método devuelve `true` si la matriz JSON `intents` contiene específicamente la intención especificada, y dicha intención tiene una puntuación de confianza que es igual o mayor que la puntuación mínima especificada. Si lo desea, puede especificar un número para indicar que la intención se debe incluir dentro de ese número de elementos principales en la matriz.

Devuelve `false` si la intención especificada no está en la matriz, si no tiene una puntuación de confianza que igual o mayor que la puntuación de confianza mínima o si la intención es inferior en la matriz que la ubicación de índice especificada.

El servicio genera automáticamente una matriz `intents` que enumera las intenciones que detecta el servicio en la entrada siempre que se envía una entrada de usuario. La matriz muestra todas las intenciones que detecta el servicio, con la de mayor confianza en primer lugar.

Puede utilizar este método en una condición de nodo para comprobar no solo la presencia de una intención, sino también para establecer un umbral de puntuación de confianza que se debe cumplir para que el nodo se pueda procesar y se devuelva su respuesta.

Por ejemplo, utilice la expresión siguiente en una condición de nodo cuando desee desencadenar el nodo de diálogo solo cuando se cumplan las condiciones siguientes:

- Aparezca la intención `#General_Ending`.
- La puntuación de confianza de la intención `#General_Ending` sea superior al 80 %.
- La intención `#General_Ending` sea una de las 2 intenciones principales en la matriz de intenciones.

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-arrays-filter}

Filtra una matriz, comparando cada valor de elemento de la matriz con un valor que especifique. Este método es similar a una [proyección de colección](#dialog-methods-collection-projection). Una proyección de colección devuelve una matriz filtrada basada en un nombre de un par nombre-valor de un elemento de la matriz. El método de filtrado devuelve una matriz filtrada basada en un valor de un par nombre-valor de un elemento de la matriz.

La expresión de filtro consta de los valores siguientes:

- `temp`: Nombre de una variable que se utiliza temporalmente a medida que se evalúa cada elemento de la matriz. Por ejemplo, `city`.
- `property`: Propiedad del elemento que desea comparar con `comparison_value`. Especifique la propiedad como una propiedad de la variable temporal que ha especificado en el primer parámetro. Utilice la sintaxis: `temp.property`. Por ejemplo, si `latitude` es un nombre de elemento válido para un par nombre-valor de la matriz, especifique la propiedad como `city.latitude`.
- `operator`: Operador que se utilizará para comparar el valor de la propiedad con `comparison_value`.

    Los operadores admitidos son:

    <table>
    <caption>Operadores de filtro admitidos</caption>
    <tr>
      <th>Operador</th>
      <th>Descripción</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>Igual que</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>Mayor que</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>Menor que</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>Mayor o igual que</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>Menor o igual que</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>No igual que</td>
    </tr>
    </table>

- `comparison_value`: Valor con el que desea comparar cada valor de propiedad de elemento de la matriz. Para especificar un valor que pueda cambiar en función de la entrada del usuario, utilice una variable de contexto o una entidad como el valor. Si especifica un valor que puede variar, añada lógica para garantizar que el valor de `comparison_value` sea válido en el momento de la evaluación; de lo contrario, se producirá un error.

#### Ejemplo de filtro 1

Por ejemplo, puede utilizar el método de filtro para evaluar una matriz que contenga un conjunto de nombres de ciudad y su población para devolver una matriz de menor tamaño que solo contenga las ciudades con una población de más de 5 millones de habitantes.

La siguiente variable de contexto `$cities` contiene una matriz de objetos. Cada objeto contiene una propiedad `name` y `population`.

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

En el ejemplo siguiente, el nombre de la variable temporal arbitraria es `city`. La expresión SpEL filtra la matriz `$cities`
para incluir solo las ciudades con una población de más de 5 millones:

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

La expresión devuelve la siguiente matriz filtrada:

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

Puede utilizar una proyección de colección para crear una nueva matriz que incluya únicamente los nombres de las ciudades de la matriz que ha devuelto el método de filtro. A continuación, puede utilizar el método `join` para mostrar los dos valores de elemento de nombre de la matriz como una serie de caracteres y separar los valores con una coma y un espacio.

```bash
The cities with more than 5 million people include <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

La respuesta resultante es la siguiente: `The cities with more than 5 million people include Tokyo, Beijing.`

#### Ejemplo de filtro 2

Una de las ventajas del método de filtro que no necesita codificar el valor de `comparison_value`. En este ejemplo, el valor codificado 5000000 se sustituye por una variable de contexto.

En este ejemplo, la variable de contexto `$population_min` contiene el número `5000000`. El nombre de la variable temporal arbitraria es `city`. La expresión SpEL filtra la matriz `$cities`
para incluir solo las ciudades con una población de más de 5 millones:

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

La expresión devuelve la siguiente matriz filtrada:

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

Cuando se comparen valores numéricos, asegúrese de establecer la variable de contexto implicada en la comparación con un valor válido antes de que se active el método de filtro. Tenga en cuenta que `null` puede ser un valor válido si el elemento de la matriz con el que lo está comparando lo puede contener. Por ejemplo, si el par de nombre y valor de población correspondiente a Tokio es `"population":null` y la expresión de comparación es `"city.population == $population_min"`, `null` sería un valor válido para la variable de contexto `$population_min`.
{: tip}

Puede utilizar una expresión de respuesta de nodo de diálogo como la siguiente:

```bash
The cities with more than $population_min people include <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

La respuesta resultante es la siguiente: `The cities with more than 5000000 people include Tokyo, Beijing.`

#### Ejemplo de filtro 3

En este ejemplo, se utiliza un nombre de entidad como `comparison_value`. La entrada del usuario es `What is the population of Tokyo?` El nombre de la variable temporal arbitraria es `y`. Ha creado una entidad llamada `@city` que reconoce nombres de ciudades, incluido `Tokyo`.

```bash
$cities.filter("y", "y.name == @city")
```

La expresión devuelve la siguiente matriz:

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

Puede utilizar un proyecto de recopilación para obtener una matriz únicamente con el elemento population de la matriz original y luego utilizar el método `get` para devolver el valor del elemento population.

```bash
The population of @city is: <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

La expresión devuelve: `The population of Tokyo is 9273000.`

### JSONArray.get(Integer)
{: #dialog-methods-arrays-get}

Este método devuelve el índice de entrada de la matriz JSONArray.

Para este contexto de tiempo de ejecución del diálogo establecido por un nodo anterior o aplicación externa:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

Nodo de diálogo o condición de respuesta:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Resultado: `True` porque la matriz anidada contiene `one` como valor.

Respuesta:

```json
"output": {
  "generic":[
      {
      "values": [
        {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()
{: #dialog-methods-arrays-getRandom}

Este método devuelve un elemento aleatorio procedente de la JSONArray de entrada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Resultado: `"ham is a great choice!"` o bien `"onion is a great choice!"` o bien `"olives is a great choice!"`

**Nota:** El texto de salida resultante se elige aleatoriamente.

### JSONArray.indexOf(value)
{: #dialog-methods-arrays-indexOf}

Este método devuelve el número de índice del elemento de la matriz que coincide con el valor que especifica como un parámetro o `-1`
si el valor no se encuentra en la matriz. El valor puede ser de tipo String (`"School"`), Integer(`8`) o Double (`9.1`). El valor debe ser una coincidencia exacta y distingue entre mayúsculas y minúsculas.

Por ejemplo, las siguientes variables de contexto contienen matrices:

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

Se pueden utilizar las siguientes expresiones para determinar el índice de la matriz en el que se especifica el valor:

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

Este método puede resultar útil, por ejemplo, para obtener el índice de un elemento de una matriz de intenciones. Puede aplicar el método `indexOf` a la matriz de intenciones que se genera cada vez que se evalúa la entrada del usuario para determinar el número de índice de matriz de una intención específica.

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

Si desea conocer la puntuación de confianza de una intención específica, puede pasar la expresión anterior en como el valor *`index`*
a una expresión con la sintaxis `intents[`*`index`*`].confidente`. Por ejemplo:

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)
{: #dialog-methods-arrays-join}

Este método une todos los valores de esta matriz a una serie. Los valores se convierten en una serie y se delimitan mediante el delimitador de entrada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Resultado:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

Si una entrada de usuario menciona varios ingredientes adicionales, y ha definido una entidad llamada `@toppings` que puede reconocer las menciones de arriba, puede utilizar la siguiente expresión en la respuesta para listar los ingredientes que se mencionaron:

```json
So, you'd like <? @toppings.values.join(',') ?>.
```
{: codeblock}

Si define una variable que almacena varios valores en una matriz JSON, puede devolver un subconjunto de valores de la matriz y luego utilizar el método join() para formatearlos correctamente.

#### Proyección de recopilación
{: #dialog-methods-collection-projection}

Una expresión SpEL de `proyección de colección` extrae una subcolección de una matriz que contiene objetos. La sintaxis de una proyección de colección es `matriz_que_contiene_conjuntos_valores.![valor_que_interesa]`.

Por ejemplo, la siguiente variable de contexto define una matriz JSON que almacena información sobre vuelos. Hay dos puntos de datos por vuelo, la hora (time) y el código del vuelo (flight_code).

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

Para devolver únicamente los códigos de vuelo, puede crear una expresión de proyección de colección con la sintaxis siguiente:

```
<? $flights_found.![flight_code] ?>
```

Esta expresión devuelve una matriz de los valores de `flight_code` como `["OK123","LH421","TS4156"]`. Consulte la [documentación sobre proyección de colección de SpEL](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) para obtener más información.

Si aplica el método `join()` a los valores de la matriz devuelta, los códigos de vuelo se muestran en una lista separada por comas. Por ejemplo, puede utilizar la sintaxis siguiente en una respuesta:

```
Los vuelos que cumplen sus criterios son:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

Resultado: `Los vuelos que cumplen sus criterios son: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-arrays-joinToArray}

Este método aplica el formato que defina en una plantilla a la matriz y devuelve una matriz formateada de acuerdo con sus especificaciones. Este método resulta útil, por ejemplo, para aplicar formateo a valores de matriz que desea que se devuelvan en una respuesta de diálogo.

La plantilla se puede especificar como una serie, un objeto JSON o una matriz JSON. Para hacer referencia a valores de la matriz que está editando en la plantilla, siga estos convenios de sintaxis:

- `%`: Representa el principio o el final de un elemento o de una propiedad de elemento que desea que devuelva la matriz que está editando.
- `e`: Representa temporalmente el elemento de matriz al que desea aplicar el formato. Este nombre de variable temporal `e` no se puede modificar.

Por ejemplo, supongamos que tiene una variable de contexto que contiene una matriz con una lista de detalles de vuelos correspondientes a tres vuelos.

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

Desea devolver únicamente la lista de códigos de vuelo. Para extraer únicamente el valor del elemento `flight` de cada matriz y devolverlo en una lista, puede utilizar la expresión siguiente:

```
The available flights are <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

La respuesta del nodo de diálogo es `The available flights are ["DL1040","DL1710","DL4379"].`

Para mostrar la matriz como texto, utilice el método `join` en la expresión de este modo:

```
The available flights are <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

La respuesta es `The available flights are DL1040, DL1710, DL4379.`

#### Plantilla compleja
{: #dialog-methods-complex-template}

Para crear una plantilla más compleja, en lugar de especificar directamente los detalles de la plantilla en el parámetro method, puede crear una variable de contexto.

Esta variable de contexto de plantilla contiene un subconjunto de los elementos de la matriz y añade etiquetas delante de los mismos, de modo que la información se visualizará en una lista legible en la respuesta:

```json
"template": "<br/>Flight number: %e.flight% <br/> Airline: %e.carrier% <br/> Departure date: %e.departure_date% <br/> Departure time: %e.departure_time% <br/> Arrival time: %e.arrival_time% <br/>"
```
{: codeblock}

Algunos canales de integración, incluidos Facebook y Slack, *no* muestran el código HTML `<br/>` correspondiente a salto de línea.
{: note}

Utilice esta expresión en la respuesta del nodo del diálogo para aplicar la plantilla definida en `$template` a la matriz de `$flights`.

```
The flight info is <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

La respuesta se parece a la siguiente:

```
The flight info is
Flight number: DL1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: DL4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

La ventaja de utilizar este método es que no importa la frecuencia con la que cambian los valores de la matriz o si aumenta el número de elementos de la matriz. Siempre que cada elemento de la matriz contenga al menos el subconjunto de propiedades a las que hace referencia la plantilla, la expresión funciona.

#### Ejemplo de plantilla de objetos JSON
{: #dialog-methods-object-template}

En este ejemplo, la variable de contexto de plantilla se define como un objeto JSON que extrae el número de vuelo, y las fechas y horas de llegada y de salida de cada uno de los elementos de vuelo especificados en la matriz en la variable de contexto `$flights`. Puede utilizar este método, por ejemplo, para aplicar el formateo estándar a los detalles de vuelo correspondientes a vuelos gestionados por dos transportistas diferentes que formatean la información de vuelo de forma distinta en sus servicios web.

```json
"template": {
      "departure": "Flight %e.flight% departs on %e.departure_date% at %e.departure_time%.",
      "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
    }
```
{: codeblock}

Quizás desee diseñar una aplicación cliente personalizada para leer los objetos de la matriz devuelta y formatear los valores correctamente para la respuesta de su asistente. La respuesta del nodo de diálogo puede devolver el objeto de detalles de llegada del vuelo como una matriz utilizando esta expresión:

```
<? $flights.joinToArray($template) ?>
```
{: screen}

Esta es la respuesta del nodo de diálogo:

```json
[
  {
    "arrival":"Flight DL1040 arrives on 2019-02-03 at 07:00.",
    "departure":"Flight DL1040 departs on 2019-02-02 at 16:45."
    },
  {
    "arrival":"Flight DL1710 arrives on 2019-02-02 at 10:19.",
    "departure":"Flight DL1710 departs on 2019-02-02 at 07:00."
    },
  {
    "arrival":"Flight DL4379 arrives on 2019-02-03 at 09:05.",
    "departure":"Flight DL4379 departs on 2019-02-02 at 21:40."
    }
  ]
  ```

Observe que el orden de los elementos `arrival` y `departure` se ha intercambiado en la respuesta. El servicio normalmente reordena los elementos en un objeto JSON. Si desea que los elementos se devuelvan en un orden específico, defina la plantilla utilizando un valor de matriz JSON o String en su lugar.

### JSONArray.remove(Integer)
{: #dialog-methods-arrays-remove}

Este método elimina el elemento en la posición de índice de JSONArray y devuelve la matriz JSONArray actualizada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(object)
{: #dialog-methods-arrays-removeValue}

Este método elimina la primera aparición del valor de JSONArray y devuelve la matriz JSONArray actualizada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.set(Integer index, Object value)
{: #dialog-methods-arrays-set}

Este método establece el índice de entrada de JSONArray en el valor de entrada y devuelve la matriz JSONArray modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()
{: #dialog-methods-arrays-size}

Este método devuelve el tamaño de la matriz JSONArray como un entero.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Realice esta actualización:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)
{: #dialog-methods-arrays-split}

Este método divide la serie de entrada utilizando la expresión regular de entrada. El resultado es una matriz JSONArray de series.

Por esta entrada:

```
"bananas;apples;pears"
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### Soporte de com.google.gson.JsonArray
{: #dialog-methods-arrays-com-google-gson-JsonArray}

Además de los métodos incorporados, utilice los métodos estándar de la clase `com.google.gson.JsonArray`.

#### Nueva matriz
{: #dialog-methods-arrays-new}

new JsonArray().append('value')

Para definir una nueva matriz para cumplimentarla con valores proporcionados por los usuarios, cree una instancia de matriz. También debe añadir un valor de marcador a la matriz cuando cree la instancia de la misma. Utilice la siguiente sintaxis para hacerlo:

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## Fecha y hora
{: #dialog-methods-date-time}

Dispone de varios métodos para trabajar con datos de fecha y hora.

Para obtener información sobre cómo reconocer y extraer información de hora y fecha de la entrada del usuario, consulte las [entidades @sys-date y @sys-time](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time).

### .after(String date or time)
{: #dialog-methods-dates-after}

Determina si el valor de fecha y hora es posterior al argumento date/time.

### .before(String date or time)
{: #dialog-methods-dates-before}

Determina si el valor de fecha y hora es anterior al argumento date/time.

Por ejemplo:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- Si se comparan distintos elementos, como por ejemplo `time vs. date`, `date vs. time` y `time vs. date and time`, el método devuelve false y se muestra una excepción en el registro JSON de respuestas `output.log_messages`.

  Por ejemplo, `@sys-date.before(@sys-time)`.
- Si se compara `date and time vs. time`, el método pasa por alto la fecha y solo compara las horas.

### now()
{: #dialog-methods-dates-now}

Devuelve una serie con la fecha y hora actuales en el formato `aaaa-MM-dd HH:mm:ss`.

- Función estática.
- Los otros métodos de fecha y hora se pueden invocar en valores de fecha y hora que devuelve esta función y se pueden pasar como argumento.
- Si se ha definido la variable de contexto `$timezone`, esta función devuelve fechas y horas en el huso horario del cliente.

Ejemplo de un nodo de diálogo en el que se utiliza `now()` en el campo de salida:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "<? now() ?>"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Ejemplo de `now()` en condiciones del nodo (para decidir si aún es por la mañana):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic":[
      {
        "values": [
          {
          "text": "Good morning!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
        }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(String format)
{: #dialog-methods-dates-reformatDateTime}

Formatea las series de fecha y hora con el formato deseado para la salida de usuario.

Devuelve una serie formateada según el formato especificado:

- `MM/dd/aaaa` para 12/31/2016
- `h a` para 10pm

Para devolver el día de la semana:

- `E` para martes
- `u` para índice de días (1 = lunes, ..., 7 = domingo)

Por ejemplo, esta definición variable de contexto crea una variable $time que guarda el valor 17:30:00 como *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Formato que siguen las reglas [SimpleDateFormat ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window} de Java.

**Nota**: Al intentar formatear únicamente la hora, la fecha se trata como `1970-01-01`.

### .sameMoment(String date/time)
{: #dialog-methods-dates-sameMoment}

- Determina si el valor de fecha y hora es igual al argumento date/time.

### .sameOrAfter(String date/time)
{: #dialog-methods-dates-sameOrAfter}

- Determina si el valor de fecha y hora es posterior o igual al argumento date/time.
- Similar a `.after()`.

### .sameOrBefore(String date/time)
{: #dialog-methods-dates-sameOrBefore}

- Determina si el valor de fecha y hora es anterior o igual al argumento date/time.

### today()
{: #dialog-methods-dates-today}

Devuelve una serie con la fecha actual en el formato `aaaa-MM-dd`.

- Función estática.
- Los otros métodos de fecha se pueden invocar en valores de fecha que devuelve esta función y se pueden pasar como argumento.
- Si se ha definido la variable de contexto `$timezone`, esta función devuelve fechas en el huso horario del cliente. De lo contrario, se utiliza el huso horario `GMT`.

Ejemplo de un nodo de diálogo en el que se utiliza `today()` en el campo de salida:

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Today's date is <? today() ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Resultado: `Today's date is 2018-03-09.`

## Cálculos de fecha y hora
{: #dialog-methods-date-time-calculations}

Utilice los métodos siguientes para calcular una fecha.

| Método                  | Descripción |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | Devuelve la fecha del día n días antes de la fecha especificada. |
| `<date>.minusMonths(n)` | Devuelve la fecha del día n meses antes de la fecha especificada. |
| `<date>.minusYears(n)`  | Devuelve la fecha del día n años antes de la fecha especificada. |
| `<date>.plusDays(n)`   | Devuelve la fecha del día n días después de la fecha especificada. |
| `<date>.plusMonths(n)` | Devuelve la fecha del día n meses después de la fecha especificada. |
| `<date>.plusYears(n)`  | Devuelve la fecha del día n años después de la fecha especificada. |

donde `<date>` se especifica en formato `aaaa-MM-dd` o `aaaa-MM-dd HH:mm:ss`.

Para obtener la fecha de mañana, especifique la expresión siguiente:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Resultado si hoy es el 9 de marzo de 2018: `Tomorrow's date is 2018-03-10.`

Para obtener la fecha correspondiente al día a la semana desde hoy, especifique la siguiente expresión:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Next week's date is <? @sys-date.plusDays(7) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Resultado si la fecha que captura la entidad @sys-date es el 9 de marzo de 2018: `Next week's date is 2018-03-16.`

Para obtener la fecha del mes pasado, especifique la expresión siguiente:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Last month the date was <? today().minusMonths(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Resultado si hoy es el 9 de marzo de 2018: `Last month the date was 2018-02-9.`

Utilice los métodos siguientes para calcular la hora.

| Método                  | Descripción |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | Devuelve la hora n horas antes de la hora especificada. |
| `<time>.minusMinutes(n)` | Devuelve la hora n minutos antes de la hora especificada. |
| `<time>.minusSeconds(n)`  | Devuelve la hora n segundos antes de la hora especificada. |
| `<time>.plusHours(n)`   | Devuelve la hora n horas después de la hora especificada. |
| `<time>.plusMinutes(n)` | Devuelve la hora n minutos después de la hora especificada. |
| `<time>.plusSeconds(n)`  | Devuelve la hora n segundos después de la hora especificada. |

donde `<time>` se especifica en el formato `HH:mm:ss`.

Para obtener la hora dentro de una hora a partir de ahora, especifique la siguiente expresión:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "One hour from now is <? now().plusHours(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Resultado si ahora son las 8 AM: `One hour from now is 09:00:00.`

Para obtener la hora hace 30 minutos, especifique la siguiente expresión:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Resultado si la hora que captura la entidad @sys-time es las 8 AM: `A half hour before 08:00:00 is 07:30:00.`

Para volver a formatear la hora que se devuelve, puede utilizar la expresión siguiente:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "6 hours ago was <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Resultado si ahora son las 2:19 PM: `6 hours ago was 8:19 AM.`

### Cómo trabajar con intervalos de tiempo
{: #dialog-methods-time-spans}

Para mostrar una respuesta que dependa de si la fecha de hoy se encuentra dentro de un determinado intervalo de tiempo, puede utilizar una combinación de métodos relacionados con el tiempo. Por ejemplo, si ejecuta una oferta especial durante la temporada navideña cada año, puede comprobar si la fecha de hoy se encuentra entre el 25 de noviembre y el 24 de diciembre de este año. En primer lugar, defina las fechas de interés como variables de contexto.

En las siguientes expresiones de variable de contexto de fecha de inicio y de finalización, la fecha se crea concatenando el valor de año actual obtenido de forma dinámica con valores de mes y día codificados.

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

En la condición de respuesta, puede indicar que desea mostrar la respuesta únicamente si la fecha actual se encuentra entre las fechas de inicio y de finalización que ha definido como variables de contexto.

`now().after($start_date) && now().before($end_date)`

### Soporte de java.util.Date
{: #dialog-methods-dates-java-util-date}

Además de los métodos incorporados, utilice los métodos estándar de la clase `java.util.Date`.

Para obtener la fecha de aquí a una semana, puede utilizar la siguiente sintaxis.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

Esta expresión primero obtiene la fecha actual en milisegundos (desde el 1 de enero de 1970 a las 00:00:00 GMT). También calcula el número de milisegundos en 7 días. (`(24*60*60*1000L)` representa un día en milisegundos). A continuación añade 7 días a la fecha actual. El resultado es la fecha completa del día que cae de aquí una semana a partir de la fecha actual de hoy. Por ejemplo, `Fri Jan 26 16:30:37 UTC 2018`. Observe que la hora está en el huso horario UTC. Siempre es posible cambiar 7 por una variable (`$number_of_days`, por ejemplo) que puede pasar a la expresión. Tan solo debe asegurarse de que se establece un valor antes de evaluar la expresión.

Si con posterioridad desea comparar la fecha con otra fecha generada por el servicio, debe reformatear la fecha. Las entidades del sistema (`@sys-date`) y otros métodos incorporados (`now()`) convierten la fecha al formato `yyyy-MM-dd`.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

Después de reformatear la fecha, el resultado es `2018-01-26`. Ahora, podrá utilizar expresiones como `@sys-date.after($week_from_today)` en una condición de respuesta para comparar una fecha especifica en la entrada de usuario con la fecha guardada en la variable de contexto.

La siguiente expresión calcula la hora de aquí a 3 horas.

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

El valor `(60*60*1000L)` representa una hora en milisegundos. Esta expresión añade 3 horas a la hora actual. A continuación recalcula la hora en un huso horario UTC al huso horario EST restando 5 horas a la misma. También reformatea los valores de fecha para incluir las horas y los minutos con AM o PM.

## Números
{: #dialog-methods-numbers}

Estos métodos ayudan a reformatear valores numéricos.

Para obtener información sobre las entidades de sistema que pueden reconocer y extraer números de la entrada del usuario, consulte la [entidad @sys-number](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number).

Si desea que un servicio reconozca formatos de número específicos en la entrada de usuario como, por ejemplo, número de referencia de pedidos, considere el crear una entidad de patrón para capturarlos. Consulte [Creación de entidades](/docs/services/assistant?topic=assistant-entities) para obtener más detalles.

Si desea cambiar la posición decimal para un número, para volver a formatear un número como, por ejemplo, un valor de moneda, consulte el [método String format()](#java.lang.String).

### toDouble()
{: #dialog-methods-numbers-toDouble}

  Convierte el objeto o campo al tipo de número Double (doble). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

### toInt()
{: #dialog-methods-numbers-toInt}

  Convierte el objeto o campo al tipo de número Integer (entero). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

### toLong()
{: #dialog-methods-numbers-toLong}

  Convierte el objeto o campo al tipo de número Long (largo). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

  Si especifica un tipo de número Long en una expresión SpEL, debe añadir una `L` al número para identificarlo como tal. Por ejemplo, `5000000000L`. Esta sintaxis es necesaria para cualquier número que no encaje en el tipo Integer de 32 bits. Por ejemplo, números mayores que 2^31 (2.147.483.648) o inferiores a -2^31 (-2.147.483.648) se consideran tipos de número Long. Los tipos de números Long tienen un valor mínimo de -2^63 y un valor máximo de 2^63-1.

### Soporte numérico de Java
{: #dialog-methods-numbers-java}

### java.lang.Math()
{: #dialog-methods-numbers-java-lang-math}

Realiza las operaciones numéricas básicas.

Puede utilizar los métodos Class, incluidos estos:

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "The bigger number is $bigger_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "The smaller number is $smaller_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Consulte la [Documentación de referencia de java.lang.Math ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html) para obtener información sobre otros métodos.

### java.util.Random()
{: #dialog-methods-numbers-java-util-random}

Devuelve un número aleatorio. Utilice una de las siguientes opciones de sintaxis:

- Para devolver un valor booleano aleatorio (true o false), utilice `<?new Random().nextBoolean()?>`.
- Para devolver un número doble aleatorio comprendido entre 0 (incluido) y 1 (excluido), utilice `<?new Random().nextDouble()?>`
- Para devolver un entero aleatorio comprendido entre 0 (incluido) y el número que especifique, utilice `<?new Random().nextInt(n)?>` donde n es la parte superior del rango de números que desea + 1.
  Por ejemplo, si desea que se devuelva un número aleatorio comprendido entre 0 y 10, especifique `<?new Random().nextInt(11)?>`.
- Para devolver un entero aleatorio del rango completo de valores enteros (entre -2147483648 y 2147483648), utilice `<?new Random().nextInt()?>`.

Por ejemplo, supongamos que desea crear un nodo de diálogo que se activa mediante la intención #random_number. La primera condición de respuesta se parecería a la siguiente:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Consulte la [documentación de referencia de java.util.Random](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html) para obtener información sobre otros métodos.

También puede utilizar métodos estándar de las siguientes clases:

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Objetos
{: #dialog-methods-objects}

### JSONObject.clear()
{: #dialog-methods-objects-jsonobject-clear}

Este método borra todos los valores del objeto JSON y devuelve un valor nulo.

Por ejemplo, si desea borrar los valores actuales de la variable de contexto $user.

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Utilice la expresión siguiente en la salida para definir un campo que borre el objeto de sus valores.

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

Si luego hace referencia a la variable de contexto $user, esta solo devuelve `{}`.

Puede utilizar el método `clear()` en los objetos JSON `context` u `output` en el cuerpo de la llamada de API `/message`.

#### Cómo borrar contexto
{: #dialog-methods-clearing-context}

Si utiliza el método `clear()` para borrar el objeto `context`, se borran **todas** las variables excepto estas:

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**Aviso**: Todos los valores de variable de contexto significa:

  - Todos los valores predeterminados que se han definido para las variables en los nodos que se han desencadenado durante la sesión actual.
  - Las actualizaciones realizadas en los valores predeterminados con la información proporcionada por el usuario o por servicios externos durante la sesión actual.

Para utilizar el método, puede especificarlo en una expresión en una variable que defina en el objeto de salida. Por ejemplo:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Response for this node."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### Cómo borrar salida
{: #dialog-methods-clearing-output}

Si se utiliza el método `clear()` para borrar el objeto `output`, se borran todas las variables excepto la que utiliza para borrar el objeto de salida y las respuestas de texto que defina en el nodo actual. Tampoco se borran estas variables:

- `output.nodes_visited`
- `output.nodes_visited_details`

Para utilizar el método, puede especificarlo en una expresión en una variable que defina en el objeto de salida. Por ejemplo:

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Have a great day!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

Si un nodo anterior en el árbol define la respuesta de texto `I'm happy to help.` y luego salta al nodo con el objeto de salida JSON definido anteriormente, en la salida solo se muestra `Have a great day.`. La salida `I'm happy to help.` no se muestra porque se ha borrado y se ha sustituido por la respuesta de texto procedente del nodo que llama al método `clear()`.

### JSONObject.has(String)
{: #dialog-methods-objects-jsonobject-has}

Este método devuelve true si el objeto JSONObject complejo tiene una propiedad del nombre de entrada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Resultado: La condición es true (verdadera) porque el objeto de usuario contiene la propiedad `first_name`.

### JSONObject.remove(String)
{: #dialog-methods-objects-jsonobject-remove}

Este método elimina una propiedad del nombre de la entrada `JSONObject`. El elemento `JSONElement` que devuelve este método es el `JSONElement` que se va a eliminar.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Salida del nodo del diálogo:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### Soporte de com.google.gson.JsonObject
{: #dialog-methods-objects-com-google-gson-JsonObject}

Además de los métodos incorporados, utilice los métodos estándar de la clase `com.google.gson.JsonObject`.

## Series
{: #dialog-methods-strings}

Estos métodos ayudan a trabajar con textos.

Para obtener información sobre cómo reconocer y extraer determinados tipos de series como, por ejemplo, nombres y ubicaciones, desde la entrada del usuario, consulte [Entidades del sistema](/docs/services/assistant?topic=assistant-system-entities).

**Nota:** Para los métodos con expresiones regulares, consulte la [referencia de la sintaxis de RE2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/google/re2/wiki/Syntax){: new_window} para obtener detalles sobre la sintaxis a utilizar cuando se especifica una expresión regular.

### String.append(Object)
{: #dialog-methods-strings-append}

Este método añade un objeto de entrada a la serie como serie (string) y devuelve una serie modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(String)
{: #dialog-methods-strings-contains}

Este método devuelve true si la serie contiene la subserie de entrada.

Entrada: "Yes, I'd like to go."

Esta sintaxis:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.endsWith(String)
{: #dialog-methods-strings-endsWith}

Este método devuelve true si la serie termina por la subserie de entrada.

Por esta entrada:

```
"What is your name?".
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.extract(String regexp, Integer groupIndex)
{: #dialog-methods-strings-extract}

Este método devuelve una serie de la entrada que coincide con el patrón de grupo de expresiones regulares que especifique. Devuelve una serie vacía si no se encuentra ninguna coincidencia.

Este método se ha diseñado para extraer coincidencias para distintos grupos de patrones de expresión regular (regex), no diferentes coincidencias para un patrón de expresión regular único. Para buscar coincidencias diferentes, consulte el método [getMatch](#dialog-methods-strings-getMatch).
{: note}

En este ejemplo, la variable de contexto guarda una serie que coincide con el grupo del patrón de expresión regular que se especifica. En la expresión, se definen dos grupos de patrones de expresión regular, cada uno encerrado entre paréntesis. Hay un tercer grupo inherente que está compuesto por los dos grupos juntos. Este es el primer grupo de expresión regular (groupIndex 0); coincide con una serie que contiene el grupo de números completo y el grupo de texto juntos. El segundo grupo de regex (groupIndex 1) coincide con la primera aparición de un grupo de números. El tercer grupo (groupIndex 2) coincide con la primera aparición de un grupo de texto después de un grupo de números.

```json
{
  "context": {
    "number_extract": "<? input.text.extract('([\\d]+)(\\b [A-Za-z]+)',n) ?>"
  }
}
```
{: codeblock}

Cuando especifica la expresión regular en JSON, debe proporcionar dos barras inclinadas invertidas (\\). Si especifica esta expresión en una respuesta de nodo, sólo necesita una barra inclinada invertida. Por ejemplo: 

`<? input.text.extract('([\d]+)(\b [A-Za-z]+)',n) ?>`

Entrada:

```
"Hello 123 this is 456".
```
{: codeblock}

Resultado:

- Si n=`0`, el valor es `123 this`.
- Si n=`1`, el valor es `123`.
- Si n=`2`, el valor es `this`.

### String.find(String regexp)
{: #dialog-methods-strings-find}

Este método devuelve true si algún segmento de la serie coincide con la expresión regular de entrada.  Puede llamar a este método sobre un elemento JSONArray o JSONObject y convertirá la matriz o el objeto en una serie antes de realizar la comparación.

Por esta entrada:

```
"Hello 123456".
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Da como resultado: La condición es true porque la parte numérica de la entrada coincide con la expresión regular `^[^\d]*[\d]{6}[^\d]*$`.

### String.getMatch(String regexp, Integer matchIndex)
{: #dialog-methods-strings-getMatch}

Este método devuelve una serie de la entrada que coincide con la instancia del patrón de grupo de expresiones regulares que especifique. Este método devuelve una serie vacía si no se encuentra ninguna coincidencia.

A medida que se encuentran coincidencias, se añaden a lo que se puede pensar como una *matriz de coincidencias*. Si desea devolver la tercera coincidencia, porque el recuento de elementos de matriz empieza en 0, especifique 2 como el valor `matchIndex`. Por ejemplo, si especifica una serie de texto con tres palabras que coincidan con el patrón especificado, puede devolver la primera, segunda o tercera coincidencia solo especificando el valor de su índice.

En la expresión siguiente, está buscando un grupo de números en la entrada. Esta expresión guarda la segunda serie de coincidencia de patrón en la variable de contexto `$second_number` porque se ha especificado el valor de índice 1.

```json
{
  "context": {
    "second_number": "<? input.text.getMatch('([\\d]+)',1) ?>"
  }
}
```
{: codeblock}

Si especifica la expresión en la sintaxis JSON, debe proporcionar dos barras inclinadas invertidas (\\). Si especifica la expresión en una respuesta de nodo, sólo necesita una barra inclinada invertida. 

Por ejemplo: 

`<? input.text.getMatch('([\d]+)',1) ?>`

- Entrada del usuario:

  ```
  "hello 123 i said 456 and 8910".
  ```
  {: codeblock}

- Resultado: `456`

En este ejemplo, la expresión busca el tercer bloque de texto en la entrada.

`<? input.text.getMatch('(\b [A-Za-z]+)',2) ?>`

Para la misma entrada de usuario, esta expresión devuelve `and`.

### String.isEmpty()
{: #dialog-methods-strings-isEmpty}

Este método devuelve true si la serie termina es una serie vacía pero no nula.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.length()
{: #dialog-methods-strings-length}

Este método devuelve la longitud en caracteres de la serie.

Por esta entrada:

```
"Hello"
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(String regexp)
{: #dialog-methods-strings-matches}

Este método devuelve true si la serie coincide con la expresión regular de entrada.

Por esta entrada:

```
"Hello".
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Da como resultado: La condición es true porque el texto coincide con la expresión regular `\^Hello\$`.

### String.startsWith(String)
{: #dialog-methods-strings-startsWith}

Este método devuelve true si la serie empieza por la subserie de entrada.

Por esta entrada:

```
"What is your name?".
```
{: codeblock}

Esta sintaxis:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Da como resultado: La condición es `true`.

### String.substring(Integer beginIndex, Integer endIndex)
{: #dialog-methods-strings-substring}

Este método obtiene una subserie con el carácter de `beginIndex` y el último carácter establecido en el índice antes de `endIndex`.
El carácter endIndex no se incluye.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()
{: #dialog-methods-strings-toLowerCase}

Este método devuelve la serie original convertida a letras minúsculas.

Por esta entrada:

```
"This is A DOG!"
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "input_lower_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()
{: #dialog-methods-strings-toUpperCase}

Este método devuelve la serie original convierten a letras mayúsculas.

Por esta entrada:

```
"hi there".
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()
{: #dialog-methods-strings-trim}

Este método elimina los espacios al principio y al final de la serie y devuelve la serie modificada.

Para este contexto de tiempo de ejecución del diálogo:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Da como resultado esta salida:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### Soporte de java.lang.String
{: #dialog-methods-strings-java-lang-String-format}

Además de los métodos incorporados, utilice los métodos estándar de la clase `java.lang.String`.

#### java.lang.String.format()
{: #dialog-methods-strings-java-lang-String-format}

Puede aplicar el método de series Java estándar `format()` al texto. Consulte la [información de referencia de java.util.formatter ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono enlace externo")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window} para obtener información sobre la sintaxis a utilizar al especificar los detalles del formato.

Por ejemplo, la siguiente expresión toma tres enteros decimales (1, 1 y 2) y los añade a una sentencia.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Resultado: `1 + 1 equals 2`.

Para cambiar la posición ubicación decimal de un número, utilice la sintaxis siguiente:

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

Por ejemplo, si la variable $number que se tiene que formatear en dólares americanos es `4.5`, una respuesta como `Your total is $<? T(String).format('%.2f',$number) ?>` devuelve `Your total is $4.50.`

## Conversión indirecta de tipos de datos
{: #dialog-methods-indirect-type-conversion}

Cuando por ejemplo se incluye una expresión dentro del texto, como parte de una respuesta de nodo, el valor se representa como String. Si desea que la expresión se represente en su tipo de datos original, no la delimite con texto.

Por ejemplo, puede añadir esta expresión a una respuesta de nodo de diálogo para que devuelva las entidades que se reconocen en la entrada del usuario en formato String:

```json
  The entities are <? entities ?>.
```
{: codeblock}

Si el usuario especifica *Hello now* como entrada, las entidades @sys-date y @sys-time se activan por la referencia a `now` (ahora). El objeto de entidades es una matriz, pero como la expresión se incluye en el texto, las entidades se devuelven en formato String, como aquí:

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

Si no incluye texto en la respuesta, en su lugar se devuelve una matriz. Por ejemplo, si la respuesta se especifica únicamente como una expresión, sin rodearla de texto.

```
  <? entities ?>
```
{: codeblock}

La información de la entidad se devuelve en su tipo de datos nativo, como una matriz.

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

A continuación otro ejemplo. En la siguiente variable de contexto $array es una matriz, pero la variable de contexto $string_array es una serie (string).

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

Si selecciona los valores de estas variables de contexto en el panel Pruébelo, verá sus valores especificados de la siguiente manera:

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

Luego puede ejecutar métodos de matriz sobre la variable $array, como `<? $array.removeValue('two') ?>`, pero no la variable $array_in_string.

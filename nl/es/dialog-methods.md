---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Métodos de lenguaje de expresión

Puede procesar valores extraídos de expresiones del usuario a los que desea hacer referencia en una variable de contexto, condición o en alguna otra parte de la respuesta.
{: shortdesc}

## Sintaxis de evaluación

Para expandir los valores de variable dentro de otras variables, o aplicar métodos a textos de salida o variables de contexto, utilice la sintaxis de expresión `<? expression ?>`.  Por ejemplo:

- **Incremento de una propiedad numérica**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Invocación a un método en un objeto**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

En las siguientes secciones se describen los métodos que puede utilizar para procesar valores. Están organizados por tipo de datos:

- [Matrices](dialog-methods.html#arrays)
- [Fecha y hora](dialog-methods.html#date-time)
- [Números](dialog-methods.html#numbers)
- [Objetos](dialog-methods.html#objects)
- [Series](dialog-methods.html#strings)

## Matrices
{: #arrays}

No puede utilizar estos métodos para comprobar un valor de una matriz en una condición de nodo o condición de respuesta dentro del mismo nodo en el que establece los valores de matriz.

### JSONArray.append(object)

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

### JSONArray.contains(object value)

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

### JSONArray.get(integer)

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
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

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
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Resultado: `"ham is a great choice!"` o bien `"onion is a great choice!"` o bien `"olives is a great choice!"`

**Nota:** El texto de salida resultante se elige aleatoriamente.

### JSONArray.join(string delimiter)

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
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

### JSONArray.remove(integer)

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

### JSONArray.set(integer index, object value)

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
{: #com.google.gson.JsonArray}

Además de los métodos incorporados, utilice los métodos estándar de la clase `com.google.gson.JsonArray`. 

#### Nueva matriz

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
{: #date-time}

Dispone de varios métodos para trabajar con datos de fecha y hora.

Para obtener información sobre cómo reconocer y extraer información de hora y fecha de la entrada del usuario, consulte las [entidades @sys-date y @sys-time](system-entities.html#sys-datetime). 

### .after(String date/time)

- Determina si el valor de fecha y hora es posterior al argumento date/time.
- Similar a `.before()`.

### .before(String date or time)

- Por ejemplo:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Determina si el valor de fecha y hora es anterior al argumento date/time.
- Si se comparan distintos elementos, como por ejemplo `time vs. date`, `date vs. time` y `time vs. date and time`, el método devuelve false y se muestra una excepción en el registro JSON de respuestas `output.log_messages`.
    - Por ejemplo, `@sys-date.before(@sys-time)`.
- Si se compara `date and time vs. time`, el método pasa por alto la fecha y solo compara las horas.

### now()

- Función estática.
- Devuelve una serie con la fecha y hora actuales en el formato `aaaa-MM-dd HH:mm:ss`.
- Los otros métodos de fecha y hora se pueden invocar en valores de fecha y hora que devuelve esta función y se pueden pasar como argumento.
- Si se ha definido la variable de contexto `$timezone`, esta función devuelve fechas y horas en el huso horario del cliente.

Ejemplo de un nodo de diálogo en el que se utiliza `now()` en el campo de salida:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Ejemplo de `now()` en condiciones del nodo (para decidir si aún es por la mañana):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

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

- Determina si el valor de fecha y hora es igual al argumento date/time.

### .sameOrAfter(String date/time)

- Determina si el valor de fecha y hora es posterior o igual al argumento date/time.
- Similar a `.after()`.

### .sameOrBefore(String date/time)

- Determina si el valor de fecha y hora es anterior o igual al argumento date/time.

### Soporte de java.util.Date 
{: #java.util.Date}

Además de los métodos incorporados, utilice los métodos estándar de la clase `java.util.Date`. 

#### Cálculos de fechas

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
{: #numbers}

Estos métodos ayudan a reformatear valores numéricos. 

Para obtener información sobre las entidades de sistema que pueden reconocer y extraer números de la entrada del usuario, consulte la [entidad @sys-number](system-entities.html#sys-number). 

Si desea que un servicio reconozca formatos de número específicos en la entrada de usuario como, por ejemplo, número de referencia de pedidos, considere el crear una entidad de patrón para capturarlos. Consulte [Creación de entidades](entities.html#creating-entities) para obtener más detalles. 

### toDouble()

  Convierte el objeto o campo al tipo de número Double (doble). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

### toInt()

  Convierte el objeto o campo al tipo de número Integer (entero). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

### toLong()

  Convierte el objeto o campo al tipo de número Long (largo). Puede llamar a este método sobre cualquier objeto o campo. Si la conversión falla, se devuelve *null*.

  Si especifica un tipo de número Long en una expresión SpEL, debe añadir una `L` al número para identificarlo como tal. Por ejemplo, `5000000000L`. Esta sintaxis es necesaria para cualquier número que no encaje en el tipo Integer de 32 bits. Por ejemplo, números mayores que 2^31 (2.147.483.648) o inferiores a -2^31 (-2.147.483.648) se consideran tipos de número Long. Los tipos de números Long tienen un valor mínimo de -2^63 y un valor máximo de 2^63-1.

### Soporte numérico de Java
{: #java.lang.Number}

### java.lang.Math()

Realiza las operaciones numéricas básicas.

Puede utilizar los métodos Class, incluidos estos: 

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "text": {
      "values": [
        "The bigger number is $bigger_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "The smaller number is $smaller_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Your number $base to the second power is $power_of_two."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

Consulte la [documentación de referencia de java.lang.Math](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html) para obtener información sobre otros métodos. 

### java.util.Random()

Devuelve un número aleatorio. Utilice una de las siguientes opciones de sintaxis:

- Para devolver un valor booleano aleatorio (true o false), utilice `<?new Random().nextBoolean()?>`.
- Para devolver un número doble aleatorio comprendido entre 0 (incluido) y 1 (excluido), utilice `<?new Random().nextDouble()?>`
- Para devolver un entero aleatorio comprendido entre 0 (incluido) y el número que especifique, utilice `<?new Random().nextInt(n)?>`  donde n es el límite superior del rango de números que desea + 1. 
  Por ejemplo, si desea que se devuelva un número aleatorio entre comprendido entre 0 y 10, especifique `<?new Random().nextInt(11)?>`.
- Para devolver un entero aleatorio del rango completo de valores enteros (entre -2147483648 y 2147483648), utilice `<?new Random().nextInt()?>`.

Por ejemplo, supongamos que desea crear un nodo de diálogo que se activa mediante la intención #random_number. La primera condición de respuesta se parecería a la siguiente:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
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
{: #objects}

### JSONObject.has(string)

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

### JSONObject.remove(string)

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
{: #com.google.gson.JsonObject}

Además de los métodos incorporados, utilice los métodos estándar de la clase `com.google.gson.JsonObject`. 

## Series
{: #strings}

Estos métodos ayudan a trabajar con textos. 

Para obtener información sobre cómo reconocer y extraer determinados tipos de series como, por ejemplo, nombres y ubicaciones, desde la entrada del usuario, consulte [Entidades del sistema](system-entities.html).

**Nota:** Para los métodos con expresiones regulares, consulte la [referencia de la sintaxis de RE2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/google/re2/wiki/Syntax){: new_window} para obtener detalles sobre la sintaxis a utilizar cuando se especifica una expresión regular. 

### String.append(object)

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

### String.contains(string)

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

### String.endsWith(string)

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

Este método devuelve una serie extraída por el índice de grupo especificado de la expresión regular de entrada.

Por esta entrada:

```
"Hello 123456".
```
{: codeblock}

Esta sintaxis:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Importante:** Para procesar `\\d` como expresión regular, debe añadir un escape a ambas barras inclinadas invertidas; para ello debe añadir otro `\\`: `\\\\d`

Resultado:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

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

### String.isEmpty()

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

### String.matches(string regexp)

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

### String.startsWith(string)

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

### String.substring(int beginIndex, int endIndex)

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
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

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
{: #java.lang.String}

Además de los métodos incorporados, utilice los métodos estándar de la clase `java.lang.String`. 

#### java.lang.String.format()

Puede aplicar el método de series Java estándar `format()` al texto. Consulte la [información de referencia de java.util.formatter ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono enlace externo")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window} para obtener información sobre la sintaxis a utilizar al especificar los detalles del formato. 

Por ejemplo, la siguiente expresión toma tres enteros decimales (1, 1 y 2) y los añade a una sentencia. 

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Resultado: `1 + 1 equals 2`.

## Conversión indirecta de tipos de datos

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

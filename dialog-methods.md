---

copyright:
  years: 2015, 2023
lastupdated: "2023-02-08"

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

{{site.data.content.newlink}}

# Expression language methods
{: #dialog-methods}

You can process values extracted from user utterances that you want to reference in a context variable, condition, or elsewhere in the response.
{: shortdesc}

## Where to use the expression syntax
{: #dialog-methods-evaluation-syntax}

To expand variable values inside other variables, or apply methods to output text or context variables, use the `<? expression ?>` expression syntax. For example:

- Referencing a user's input from a dialog node text response

  ```bash
  You said <? input.text ?>.
  ```
  {: codeblock}

- Incrementing a numeric property from the JSON editor

    ```json
    "output":{"number":"<? output.number + 1 ?>"}
    ```
    {: codeblock}

- Checking for a specific entity value from a dialog node condition

  ```bash
  @city.toLowerCase() == 'paris'
  ```
  {: codeblock}

- Checking for a specific date range from a dialog node response condition

  ```bash
  @sys-date.after(today())
  ```
  {: codeblock}

- Adding an element to a context variable array from the context editor

| Context variable name | Context variable value |
|-----------------------|------------------------|
| `toppings` | `<? context.toppings.append( 'onions' ) ?>` |

You can use SpEL expressions in dialog node conditions and dialog node response conditions also. 

When a SpEL expression is used in a node condition, the surrounding `<? ?>` syntax is not required.
{: important}

The following sections describe methods you can use to process values. They are organized by data type:

- [Arrays](#dialog-methods-arrays)
- [Date and Time](#dialog-methods-date-time)
- [Numbers](#dialog-methods-numbers)
- [Objects](#dialog-methods-objects)
- [Strings](#dialog-methods-strings)

## Arrays
{: #dialog-methods-arrays}

You cannot use these methods to check for a value in an array in a node condition or response condition within the same node in which you set the array values.

### JSONArray.addAll(JSONArray)
{: #dialog-methods-arrays-addall}

This method appends one array to another.

For this dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"],
    "more_toppings": ["mushroom","pepperoni"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.addAll($more_toppings) ?>"
  }
}
```
{: codeblock}

Result: The method itself returns `null`. However, the first array is updated to include the values from the second array.

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "mushroom", "pepperoni"]
  }
}
```
{: codeblock}

### JSONArray.append(object)
{: #dialog-methods-arrays-append}

This method appends a new value to the JSONArray and returns the modified JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

Result:

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

This method clears all values from the array and returns null.

Use the following expression in the output to define a field that clears an array that you saved to a context variable ($toppings_array) of its values.

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

If you subsequently reference the $toppings_array context variable, it returns '[]' only.

### JSONArray.contains(Object value)
{: #dialog-methods-arrays-contains}

This method returns true if the input JSONArray contains the input value.

For this Dialog runtime context which is set by a previous node or external application:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node or response condition:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Result: `true` because the array contains the element `ham`.

### JSONArray.containsIgnoreCase(Object value)
{: #dialog-methods-arrays-contains-ignore-case}

This method returns `true` if the input JSONArray contains the input value, regardless of whether the value is specified in uppercase or lowercase letters.

For this Dialog runtime context which is set by a previous node or external application:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node or response condition:

```json
$toppings_array.containsIgnoreCase('HAM')
```
{: codeblock}

Result: `true` because the array contains the element `ham` and the case is ignored.

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-arrays-containsIntent}

This method returns `true` if the `intents` JSONArray specifically contains the specified intent, and that intent has a confidence score that is equal to or higher than the specified minimum score. Optionally, you can specify a number to indicate that the intent must be included within that number of top elements in the array. The `top_n` parameter is ignored if you specify a negative number.

Returns `false` if the specified intent is not in the array, does not have a confidence score that is equal to or greater than the minimum confidence score, or the array index of the intent is lower than the specified index location.

The service automatically generates an `intents` array that lists the intents that the service detects in the input whenever user input is submitted. The array lists all intents that are detected by the service in order of highest confidence first.

You can use this method in a node condition to not only check for the presence of an intent, but to set a confidence score threshold that must be met before the node can be processed and its response returned.

For example, use the following expression in a node condition when you want to trigger the dialog node only when the following conditions are met:

- The `#General_Ending` intent is present.
- The confidence score of the `#General_Ending` intent is over 80%.
- The `#General_Ending` intent is one of the top 2 intents in the intents array.

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}



### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-arrays-filter}

Filters an array by comparing each array element value to a value you specify. This method is similar to a [collection projection](#dialog-methods-collection-projection). A collection projection returns a filtered array based on a name in an array element name-value pair. The filter method returns a filtered array based on a value in an array element name-value pair.

The filter expression consists of the following values:

- `temp`: Name of a variable that is used temporarily as each array element is evaluated. For example, `city`.
- `property`: Element property that you want to compare to the `comparison_value`. Specify the property as a property of the temporary variable that you name in the first parameter. Use the syntax: `temp.property`. For example, if `latitude` is a valid element name for a name-value pair in the array, specify the property as `city.latitude`.
- `operator`: Operator to use to compare the property value to the `comparison_value`.

    Supported operators are:

    <table>
    <caption>Supported filter operators</caption>
    <tr>
      <th>Operator</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>==</code></td>
      <td>Is equal to</td>
    </tr>
    <tr>
      <td><code>></code></td>
      <td>Is greater than</td>
    </tr>
    <tr>
      <td><code><</code></td>
      <td>Is less than</td>
    </tr>
    <tr>
      <td><code>>=</code></td>
      <td>Is greater than or equal to</td>
    </tr>
    <tr>
      <td><code><=</code></td>
      <td>Is less than or equal to</td>
    </tr>
    <tr>
      <td><code>!=</code></td>
      <td>Is not equal to</td>
    </tr>
    </table>

- `comparison_value`: Value that you want to compare each array element property value against. To specify a value that can change depending on the user input, use a context variable or entity as the value. If you specify a value that can vary, add logic to guarantee that the `comparison_value` value is valid at evaluation time or an error will occur.

#### Filter example 1

For example, you can use the filter method to evaluate an array that contains a set of city names and their population numbers to return a smaller array that contains only cities with a population over 5 million.

The following `$cities` context variable contains an array of objects. Each object contains a `name` and `population` property.

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

In the following example, the arbitrary temporary variable name is `city`. The SpEL expression filters the `$cities` array to include only cities with a population of over 5 million:

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

The expression returns the following filtered array:

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

You can use a collection projection to create a new array that includes only the city names from the array returned by the filter method. You can then use the `join` method to display the two name element values from the array as a String, and separate the values with a comma and a space.

```bash
The cities with more than 5 million people include <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

The resulting response is: `The cities with more than 5 million people include Tokyo, Beijing.`

#### Filter example 2

The power of the filter method is that you do not need to hard code the `comparison_value` value. In this example, the hard coded value of 5000000 is replaced with a context variable instead.

In this example, the `$population_min` context variable contains the number `5000000`. The arbitrary temporary variable name is `city`. The SpEL expression filters the `$cities` array to include only cities with a population of over 5 million:

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

The expression returns the following filtered array:

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

When comparing number values, be sure to set the context variable involved in the comparison to a valid value before the filter method is triggered. Note that `null` can be a valid value if the array element you are comparing it against might contain it. For example, if the population name and value pair for Tokyo is `"population":null`, and the comparison expression is `"city.population == $population_min"`, then `null` would be a valid value for the `$population_min` context variable.
{: tip}

You can use a dialog node response expression like this:

```bash
The cities with more than $population_min people include <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

The resulting response is: `The cities with more than 5000000 people include Tokyo, Beijing.`

#### Filter example 3

In this example, an entity name is used as the `comparison_value`. The user input is, `What is the population of Tokyo?` The arbitrary temporary variable name is `y`. You created an entity named `@city` that recognizes city names, including `Tokyo`.

```bash
$cities.filter("y", "y.name == @city")
```

The expression returns the following array:

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

You can use a collection project to get an array with only the population element from the original array, and then use the `get` method to return the value of the population element.

```bash
The population of @city is: <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

The expression returns: `The population of Tokyo is 9273000.`

### JSONArray.get(Integer)
{: #dialog-methods-arrays-get}

This method returns the input index from the JSONArray.

For this Dialog runtime context which is set by a previous node or external application:

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

Dialog node or response condition:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Result:
`True` because the nested array contains `one` as a value.

Response:

```json
"output": {
  "generic" : [
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

This method returns a random item from the input JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "output": {
  "generic" : [
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

Result: `"ham is a great choice!"` or `"onion is a great choice!"` or `"olives is a great choice!"`

**Note:** The resulting output text is randomly chosen.

### JSONArray.indexOf(value)
{: #dialog-methods-arrays-indexOf}

This method returns the index number of the element in the array that matches the value you specify as a parameter or `-1` if the value is not found in the array. The value can be a String (`"School"`), Integer(`8`), or Double (`9.1`). The value must be an exact match and is case sensitive.

For example, the following context variables contain arrays:

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

The following expressions can be used to determine the array index at which the value is specified:

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

This method can be useful for getting the index of an element in an intents array, for example. You can apply the `indexOf` method to the array of intents generated each time user input is evaluated to determine the array index number of a specific intent.

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

If you want to know the confidence score for a specific intent, you can pass the earlier expression in as the *`index`* value to an expression with the syntax `intents[`*`index`*`].confidence`. For example:

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)
{: #dialog-methods-arrays-join}

This method joins all values in this array to a string. Values are converted to string and delimited by the input delimiter.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "output": {
  "generic" : [
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

Result:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

If a user input mentions multiple toppings, and you defined an entity named `@toppings` that can recognize topping mentions, you could use the following expression in the response to list the toppings that were mentioned:

```json
So, you'd like <? @toppings.values.join(',') ?>.
```
{: codeblock}

If you define a variable that stores multiple values in a JSON array, you can return a subset of values from the array, and then use the join() method to format them properly.

#### Collection projection
{: #dialog-methods-collection-projection}

A `collection projection` SpEL expression extracts a subcollection from an array that contains objects. The syntax for a collection projection is `array_that_contains_value_sets.![value_of_interest]`.

For example, the following context variable defines a JSON array that stores flight information. There are two data points per flight, the time and flight code.

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

To return the flight codes only, you can create a collection projection expression by using the following syntax:

```
<? $flights_found.![flight_code] ?>
```

This expression returns an array of the `flight_code` values as `["OK123","LH421","TS4156"]`. See the [SpEL Collection projection documentation](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) for more details.

If you apply the `join()` method to the values in the returned array, the flight codes are displayed in a comma-separated list. For example, you can use the following syntax in a response:

```
The flights that fit your criteria are:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

Result: `The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template, retainDataType)
{: #dialog-methods-arrays-joinToArray}

This method extracts information from each item in the array and builds a new array that is formatted according to the template you specify. The template can be a string, a JSON object, or an array. The method returns an array of strings, an array of objects, or an array of arrays, depending on the type of template.

This method is useful for formatting information as a string you can return as part of the output of a dialog node, or for transforming data into a different structure so you can use it with an external API.

In the template, you can reference values from the source array using the following syntax:

```text
%e.{property}%
```

where `{property}` represents the name of the property in the source array.

For example, suppose your assistant has stored an array containing flight details in a context variable. The stored data might look like this:

```json
"flights": [
      {
        "flight": "AZ1040",
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
        "flight": "VS4379",
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

To build an array of strings that describe these flights in a user-readable form, you might use the following expression:

```text
${Flight_data}.joinToArray("Flight %e.flight% to %e.destination%", true)
```

This expression would return the following array of strings: `["Flight AZ1040 to FCO","Flight DL1710 to LAX","Flight VS4379 to LHR"]`.

The optional `retainDataType` parameter specifies whether the method should preserve the data type of all input values in the returned array. If `retainDataType` is set to `false` or omitted, in some situations, strings in the input array might be converted to numbers in the returned array. For example, if the selected values from the input array are `"1"`, `"2"`, and `"3"`, the returned array might be `[ 1, 2, 3 ]`. To avoid unexpected type conversions, specify `true` for this parameter.

#### Complex templates
{: #join-to-array-complex-template}

A more complex template might contain formatting that displays the information in a legible layout. For a complex template, you might want to store the template in a context variable, which you can then pass to the `joinToArray` method instead of a string.

For example, this complex template contains a subset of the array elements, adding labels and formatting:

```text
<br/>Flight number: %e.flight% <br/> Airline: %e.carrier% <br/> Departure date: %e.departure_date% <br/> Departure time: %e.departure_time% <br/> Arrival time: %e.arrival_time% <br/>
```
{: codeblock}

Make sure any formatting you use in your template is supported by the channel integration that will be displaying the assistant output.
{: note}

If you create a context variable called `Template`, and assign this template as its value, you can then use that variable in your expressions:

```text
${Flight_data}.joinToArray(${Template})
```

At run time, the response would look like this:

```text
Flight number: AZ1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: VS4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

#### JSON Object templates
{: #join-to-array-object-template}

Instead of a string, you can define a template as a JSON object. This provides a way to standardize the formatting of information from different systems, or to transform data into the format required for an external service.

In this example, a template is defined as a JSON object that extracts flight details from the elements specified in the array stored in the `Flight data` context variable:

```json
{
  "departure": "Flight %e.flight% departs on %e.departure_date% at %e.departure_time%.",
  "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
}
```
{: codeblock}

Using this template, the `joinToArray()` method returns a new array of objects with the specified structure.

### JSONArray.remove(Integer)
{: #dialog-methods-arrays-remove}

This method removes the element in the index position from the JSONArray and returns the updated JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Result:

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

This method removes the first occurrence of the value from the JSONArray and returns the updated JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

Result:

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

This method sets the input index of the JSONArray to the input value and returns the modified JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Result:

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

This method returns the size of the JSONArray as an integer.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Result:

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

This method splits the input string by using the input regular expression. The result is a JSONArray of strings.

For this input:

```
"bananas;apples;pears"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### com.google.gson.JsonArray support
{: #dialog-methods-arrays-com-google-gson-JsonArray}

In addition to the built-in methods, you can use standard methods of the `com.google.gson.JsonArray` class.

#### New array
{: #dialog-methods-arrays-new}

new JsonArray().append('value')

To define a new array that will be filled in with values that are provided by users, you can instantiate an array. You must also add a placeholder value to the array when you instantiate it. You can use the following syntax to do so:

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## Date and Time
{: #dialog-methods-date-time}

Several methods are available to work with date and time.

For information about how to recognize and extract date and time information from user input, see [@sys-date and @sys-time entities](/docs/assistant?topic=assistant-system-entities#system-entities-sys-date-time).

The following string formats are supported for date-time literals on which the methods below may be invoked.

- For time only: `HH:mm:ss` or `HH:mm`
- For date only: `yyyy-MM-dd`
- For date and time: `yyyy-MM-dd HH:mm:ss`
- For date and time with time zone: `yyyy-MM-dd HH:mm:ss VV`. The V symbol is from the [DateTimeFormatter](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/format/DateTimeFormatter.html){: external} and represents time zone in IANA Time Zone Database (TZDB) format, for example, Europe/London.

### .after(String date or time)
{: #dialog-methods-dates-after}

Determines whether the date/time value is after the date/time argument.

### .before(String date or time)
{: #dialog-methods-dates-before}

Determines whether the date/time value is before the date/time argument.

For example:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- If comparing different items, such as `time vs. date`, `date vs. time`, and `time vs. date and time`, the method returns false and an exception is printed in the response JSON log `output.log_messages`.

  For example, `@sys-date.before(@sys-time)`.
- If comparing `date and time vs. time` the method ignores the date and only compares times.

### now(String time zone)
{: #dialog-methods-dates-now}

Returns a string with the current date and time in the format `yyyy-MM-dd HH:mm:ss`. Optionally specify a `timezone` value to get the current date and time for a specific time zone, with a returned string in the format `yyyy-MM-dd HH:mm:ss 'GMT'XXX`.

- Static function.
- The other date/time methods can be invoked on date-time values that are returned by this function and it can be passed in as their argument.
- The user interface creates a `$timezone` context variable for you automatically so the correct time is returned when you test from the "Try it out" pane. If you don't pass a time zone, the time zone that is set automatically by the UI is used. Outside of the UI, `GMT` is used as the time zone. To learn about the syntax to use to specify the time zone, see [Time zones supported by system entities](/docs/assistant?topic=assistant-time-zones).

Example of `now()` being used to first check whether it's morning before responding with a morning-specific greeting.

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic": [
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

Example of using now() with a timezone to return the current time (in England):

```json
{
  "output": {
      "generic": [
        {
        "values": [
          {
          "text": "The current date and time is: <? now('Europe/London') ?>"
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

You can substitute the hard-coded time zone value with a context variable to dynamically change the time based on a time zone that is passed to the expression. For example: `<? now('$myzone') ?>`. The `$myzone` context variable might be set to `'Australia/Sydney'` in one conversation and to `'Mexico/BajaNorte'` in another.

### .reformatDateTime(String format)
{: #dialog-methods-dates-reformatDateTime}

Formats date and time strings to the format desired for user output.

Returns a formatted string according to the specified format:

- `MM/dd/yyyy` for 12/31/2016
- `h a` for 10pm

To return the day of the week:

- `E` for Tuesday
- `u` for day index (1 = Monday, ..., 7 = Sunday)

For example, this context variable definition creates a $time variable that saves the value 17:30:00 as *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Format follows the Java [SimpleDateFormat](https://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: external} rules.

**Note**: When trying to format time only, the date is treated as `1970-01-01`.

### .sameMoment(String date/time)
{: #dialog-methods-dates-sameMoment}

- Determines whether the date/time value is the same as the date/time argument.

### .sameOrAfter(String date/time)
{: #dialog-methods-dates-sameOrAfter}

- Determines whether the date/time value is after or the same as the date/time argument.
- Analogous to `.after()`.

### .sameOrBefore(String date/time)
{: #dialog-methods-dates-sameOrBefore}

- Determines whether the date/time value is before or the same as the date/time argument.

### today()
{: #dialog-methods-dates-today}

Returns a string with the current date in the format `yyyy-MM-dd`.

- Static function.
- The other date methods can be invoked on date values that are returned by this function and it can be passed in as their argument.
- If the context variable `$timezone` is set, this function returns dates in the client's time zone. Otherwise, the `GMT` time zone is used.

Example of a dialog node with `today()` used in the output field:

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic": [
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

Result: `Today's date is 2018-03-09.`

## Date and time calculations
{: #dialog-methods-date-time-calculations}

Use the following methods to calculate a date.

| Method                  | Description |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | Returns the date of the day n number of days before the specified date. |
| `<date>.minusMonths(n)` | Returns the date of the day n number of months before the specified date. |
| `<date>.minusYears(n)`  | Returns the date of the day n number of years before the specified date. |
| `<date>.plusDays(n)`   | Returns the date of the day n number of days after the specified date. |
| `<date>.plusMonths(n)` | Returns the date of the day n number of months after the specified date. |
| `<date>.plusYears(n)`  | Returns the date of the day n number of years after the specified date. |

where `<date>` is specified in the format `yyyy-MM-dd` or `yyyy-MM-dd HH:mm:ss`.

To get tomorrow's date, specify the following expression:

```json
{
  "output": {
    "generic": [
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

Result if today is March 9, 2018: `Tomorrow's date is 2018-03-10.`

To get the date for the day a week from today, specify the following expression:

```json
{
  "output": {
    "generic": [
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

Result if the date captured by the @sys-date entity is today's date, March 9, 2018: `Next week's date is 2018-03-16.`

To get last month's date, specify the following expression:

```json
{
  "output": {
    "generic": [
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

Result if today is March 9, 2018: `Last month the date was 2018-02-9.`

Use the following methods to calculate time.

| Method                  | Description |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | Returns the time n hours before the specified time. |
| `<time>.minusMinutes(n)` | Returns the time n minutes before the specified time. |
| `<time>.minusSeconds(n)`  | Returns the time n seconds before the specified time. |
| `<time>.plusHours(n)`   | Returns the time n hours after the specified time. |
| `<time>.plusMinutes(n)` | Returns the time n minutes after the specified time. |
| `<time>.plusSeconds(n)`  | Returns the time n secons after the specified time. |

where `<time>` is specified in the format `HH:mm:ss`.

To get the time an hour from now, specify the following expression:

```json
{
  "output": {
    "generic": [
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

Result if it is 8 AM: `One hour from now is 09:00:00.`

To get the time 30 minutes ago, specify the following expression:

```json
{
  "output": {
    "generic": [
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

Result if the time captured by the @sys-time entity is 8 AM: `A half hour before 08:00:00 is 07:30:00.`

To reformat the time that is returned, you can use the following expression:

```json
{
  "output": {
    "generic": [
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

Result if it is 2:19 PM: `6 hours ago was 8:19 AM.`

### Working with time spans
{: #dialog-methods-time-spans}

To show a response based on whether today's date falls within a certain time frame, you can use a combination of time-related methods. For example, if you run a special offer during the holiday season every year, you can check whether today's date falls between November 25 and December 24 of this year. First, define the dates of interest as context variables.

In the following start and end date context variable expressions, the date is being constructed by concatenating the dynamically-derived current year value with hard-coded month and day values.

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

In the response condition, you can indicate that you want to show the response only if the current date falls between the start and end dates that you defined as context variables.

`now().after($start_date) && now().before($end_date)`

### java.util.Date support
{: #dialog-methods-dates-java-util-date}

In addition to the built-in methods, you can use standard methods of the `java.util.Date` class.

To get the date of the day that falls a week from today, you can use the following syntax.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

This expression first gets the current date in milliseconds (since January 1, 1970, 00:00:00 GMT). It also calculates the number of milliseconds in 7 days. (The `(24*60*60*1000L)` represents one day in milliseconds.) It then adds 7 days to the current date. The result is the full date of the day that falls a week from today. For example, `Fri Jan 26 16:30:37 UTC 2018`. Note that the time is in the UTC time zone. You can always change the 7 to a variable (`$number_of_days`, for example) that you can pass in. Just be sure that its value gets set before this expression is evaluated.

If you want to be able to subsequently compare the date with another date that is generated by the service, then you must reformat the date. System entities (`@sys-date`) and other built-in methods (`now()`) convert dates to the `yyyy-MM-dd` format.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

After reformatting the date, the result is `2018-01-26`. Now, you can use an expression like `@sys-date.after($week_from_today)` in a response condition to compare a date specified in user input to the date saved in the context variable.

The following expression calculates the time 3 hours from now.

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

The `(60*60*1000L)` value represents an hour in milliseconds. This expression adds 3 hours to the current time. It then recalculates the time from a UTC time zone to EST time zone by subtracting 5 hours from it. It also reformats the date values to include hours and minutes AM or PM.

## Numbers
{: #dialog-methods-numbers}

These methods help you get and reformat number values.

For information about system entities that can recognize and extract numbers from user input, see [@sys-number entity](/docs/assistant?topic=assistant-system-entities#system-entities-sys-number).

If you want the service to recognize specific number formats in user input, such as order number references, consider creating a pattern entity to capture it. See [Creating entities](/docs/assistant?topic=assistant-entities) for more details.

If you want to change the decimal placement for a number, to reformat a number as a currency value, for example, see the [String format() method](#dialog-methods-strings-java-lang-String-format).

### toDouble()
{: #dialog-methods-numbers-toDouble}

  Converts the object or field to the Double number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toInt()
{: #dialog-methods-numbers-toInt}

  Converts the object or field to the Integer number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toLong()
{: #dialog-methods-numbers-toLong}

  Converts the object or field to the Long number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

  If you specify a Long number type in a SpEL expression, you must append an `L` to the number to identify it as such. For example, `5000000000L`. This syntax is required for any numbers that do not fit into the 32-bit Integer type. For example, numbers that are greater than 2^31 (2,147,483,648) or lower than -2^31 (-2,147,483,648) are considered Long number types. Long number types have a minimum value of -2^63 and a maximum value of 2^63-1 (or 9,223,372,036,854,775,807).

  If you need to find out if a number is too long to be recognized properly in the dialog, you can check whether there are more than 18 integers in the number by using an expression like this:

  ```
  <? @sys-number.toString().length() > 18 ?>
  ```
  {: codeblock}

  If you need to work with numbers that are longer than 18 integers, consider using a pattern entity (with a regular expression such as `\d{20}`) to work with them instead of using `@sys-number`.

### Standard math
{: #dialog-methods-numbers-standard-math}

Use SpEL expressions to define standard math equations, where the operators are represented by using these symbols:

| Arithmetic operation | Symbol |
|--------|:-----------|
| addition | + |
| division | / |
| multiplication | * |
| subraction | - |

For example, in a dialog node response, you might add a context variable that captures a number specified in the user input (`@sys-number`), and saves it as `$your_number`. You can then add the following text as a text response:

```
I'm doing math. Given the value you specified ($your_number), when I add 5, I get: <? $your_number + 5 ?>. 
When I subtract 5, I get: <? $your_number - 5 ?>. 
When I multiply it by 5, I get: <? $your_number * 5 ?>. 
When I divide it by 5, I get: <? $your_number/5 ?>.
```
{: codeblock}

If the user specifies `10`, then the resulting text response looks like this:

```
I'm doing math. Given the value you specified (10), when I add 5, I get: 15. 
When I subtract 5, I get: 5. 
When I multiply it by 5, I get: 50. 
When I divide it by 5, I get: 2.
```
{: codeblock}

### Java number support
{: #dialog-methods-numbers-java}

### java.lang.Math()
{: #dialog-methods-numbers-java-lang-math}

Performs basic numeric operations.

You can use the the Class methods, including these:

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic": [
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
    "generic": [
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
    "generic": [
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

See the [java.lang.Math reference documentation](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html){: external} for information about other methods.

### java.util.Random()
{: #dialog-methods-numbers-java-util-random}

Returns a random number. You can use one of the following syntax options:

- To return a random boolean value (true or false), use `<?new Random().nextBoolean()?>`.
- To return a random double number between 0 (included) and 1 (excluded), use `<?new Random().nextDouble()?>`
- To return a random integer between 0 (included) and a number you specify, use `<?new Random().nextInt(n)?>`  where n is the top of the number range you want + 1.
  For example, if you want to return a random number between 0 and 10, specify `<?new Random().nextInt(11)?>`.
- To return a random integer from the full Integer value range (-2147483648 to 2147483648), use `<?new Random().nextInt()?>`.

For example, you might create a dialog node that is triggered by the #random_number intent. The first response condition might look like this:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic": [
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

See the [java.util.Random reference documentation](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html){: external} for information about other methods.

You can use standard methods of the following classes also:

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Objects
{: #dialog-methods-objects}

### JSONObject.clear()
{: #dialog-methods-objects-jsonobject-clear}

This method clears all values from the JSON object and returns null.

For example, you want to clear the current values from the $user context variable.

```json
{
  "context": {
    "user": {
      "first_name":"John",
      "last_name":"Snow"
    }
  }
}
```
{: codeblock}

Use the following expression in the output to define a field that clears the object of its values.

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

If you subsequently reference the $user context variable, it returns `{}` only.

You can use the `clear()` method on the `context` or `output` JSON objects in the body of the API `/message` call.

#### Clearing context
{: #dialog-methods-clearing-context}

When you use the `clear()` method to clear the `context` object, it clears **all** variables except these ones:

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**Warning**: All context variable values means:

  - All default values that were set for variables in nodes that have been triggered during the current session.
  - Any updates made to the default values with information provided by the user or external services during the current session.

To use the method, you can specify it in an expression in a variable that you define in the output object. For example:

```json
{
  "output": {
    "generic": [
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

#### Clearing output
{: #dialog-methods-clearing-output}

When you use the `clear()` method to clear the `output` object, it clears all variables except the one you use to clear the output object and any text responses that you define in the current node. It also does not clear these variables:

- `output.nodes_visited`
- `output.nodes_visited_details`

To use the method, you can specify it in an expression in a variable that you define in the output object. For example:

```json
{
  "output": {
    "generic": [
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

If a node earlier in the tree defines a text response of `I'm happy to help.` and then jumps to a node with the JSON output object defined earlier, then  only `Have a great day.` is displayed as the response. The `I'm happy to help.` output is not displayed, because it is cleared and replaced with the text response from the node that is calling the `clear()` method.

### JSONObject.has(String)
{: #dialog-methods-objects-jsonobject-has}

This method returns true if the complex JSONObject has a property of the input name.

For this Dialog runtime context:

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

Dialog node output:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Result: The condition is true because the user object contains the property `first_name`.

### JSONObject.remove(String)
{: #dialog-methods-objects-jsonobject-remove}

This method removes a property of the name from the input `JSONObject`. The `JSONElement` that is returned by this method is the `JSONElement` that is being removed.

For this Dialog runtime context:

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

Dialog node output:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Result:

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

### com.google.gson.JsonObject support
{: #dialog-methods-objects-com-google-gson-JsonObject}

In addition to the built-in methods, some of the standard methods of the `com.google.gson.JsonObject` class are also supported.

## Strings
{: #dialog-methods-strings}

There methods help you work with text.

For information about how to recognize and extract certain types of Strings, such as people names and locations, from user input, see [System entities](/docs/assistant?topic=assistant-system-entities).

**Note:** For methods that involve regular expressions, see [RE2 Syntax reference](https://github.com/google/re2/wiki/Syntax){: external} for details about the syntax to use when you specify the regular expression.

### String.append(Object)
{: #dialog-methods-strings-append}

This method appends an input object to the string as a string and returns a modified string.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Results in this output:

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

This method returns true if the string contains the input substring.

Input: "Yes, I'd like to go."

This syntax:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.endsWith(String)
{: #dialog-methods-strings-endsWith}

This method returns true if the string ends with the input substring.

For this input:

```
"What is your name?".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.equals(String)
{: #dialog-methods-strings-equals}

This method returns `true` if the specified string equals the input string exactly.

Input: "Yes"

This syntax:

```json
{
  "conditions": "input.text.equals('Yes')"
}
```
{: codeblock}

Results: The condition is `true`.

If the input is `Yes.`, then the result is `false` because the user included a period and the expression expects only the exact text, `Yes` without any punctuation.

### String.equalsIgnoreCase(String)
{: #dialog-methods-strings-equals-ignore-case}

This method returns `true` if the specified string equals the input string, regardless of whether the case of the letters match.

Input: "yes"

This syntax:

```json
{
  "conditions": "input.text.equalsIgnoreCase('Yes')"
}
```
{: codeblock}

Results: The condition is `true`.

If the input is `Yes.`, then the result is `false` because the user included a period and the expression expects only the text, `Yes`, in uppercase or lowercase letters without any punctuation.

### String.extract(String regexp, Integer groupIndex)
{: #dialog-methods-strings-extract}

This method returns a string from the input that matches the regular expression group pattern that you specify. It returns an empty string if no match is found.

This method is designed to extract matches for different regex pattern groups, not different matches for a single regex pattern. To find different matches, see the [getMatch](#dialog-methods-strings-getMatch) method.
{: note}

In this example, the context variable is saving a string that matches the regex pattern group that you specify. In the expression, two regex patterns groups are defined, each one enclosed in parentheses. There is an inherent third group that is comprised of the two groups together. This is the first (groupIndex 0) regex group; it matches with a string that contains the full number group and text group together. The second regex group (groupIndex 1) matches with the first occurrence of a number group. The third group (groupIndex 2) matches with the first occurrence of a text group after a number group.

```json
{
  "context": {
    "number_extract": "<? input.text.extract('([\\d]+)(\\b [A-Za-z]+)',n) ?>"
  }
}
```
{: codeblock}

When you specify the regex in JSON, you must provide two backslashes (\\). If you specify this expression in a node response, you need one backslash only. For example: 

`<? input.text.extract('([\d]+)(\b [A-Za-z]+)',n) ?>`

Input:

```
"Hello 123 this is 456".
```
{: codeblock}

Result:

- When n=`0`, the value is `123 this`.
- When n=`1`, the value is `123`.
- When n=`2`, the value is `this`.

### String.find(String regexp)
{: #dialog-methods-strings-find}

This method returns true if any segment of the string matches the input regular expression.  You can call this method against a JSONArray or JSONObject element, and it will convert the array or object to a string before making the comparison.

For this input:

```
"Hello 123456".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Result: The condition is true because the numeric portion of the input text matches the regular expression `^[^\d]*[\d]{6}[^\d]*$`.

### String.getMatch(String regexp, Integer matchIndex)
{: #dialog-methods-strings-getMatch}

This method returns a string from the input that matches the occurrence of the regular expression pattern that you specify. This method returns an empty string if no match is found.

As matches are found, they are added to what you can think of as a *matches array*. If you want to return the third match, because the array element count starts at 0, specify 2 as the `matchIndex` value. For example, if you enter a text string with three words that match the specified pattern, you can return the first, second, or third match only by specifying its index value.

In the following expression, you are looking for a group of numbers in the input. This expression saves the second pattern-matching string into the `$second_number` context variable because the index value 1 is specified.

```json
{
  "context": {
    "second_number": "<? input.text.getMatch('([\\d]+)',1) ?>"
  }
}
```
{: codeblock}

If you specify the expression in JSON syntax, you must provide two backslashes (\\). If you specify the expression in a node response, you need one backslash only. 

For example: 

`<? input.text.getMatch('([\d]+)',1) ?>`

- User input:

  ```
  "hello 123 i said 456 and 8910".
  ```
  {: codeblock}

- Result: `456`

In this example the expression looks for the third block of text in the input.

`<? input.text.getMatch('(\b [A-Za-z]+)',2) ?>`

For the same user input, this expression returns `and`.

### String.isEmpty()
{: #dialog-methods-strings-isEmpty}

This method returns true if the string is an empty string, but not null.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

This syntax:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Results: The condition is `true`.

### String.length()
{: #dialog-methods-strings-length}

This method returns the character length of the string.

For this input:

```
"Hello"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Results in this output:

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

This method returns true if the string matches the input regular expression.

For this input:

```
"Hello".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Result: The condition is true because the input text matches the regular expression `\^Hello\$`.

### String.startsWith(String)
{: #dialog-methods-strings-startsWith}

This method returns true if the string starts with the input substring.

For this input:

```
"What is your name?".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.substring(Integer beginIndex, Integer endIndex)
{: #dialog-methods-strings-substring}

This method gets a substring with the character at `beginIndex` and the last character set to index before `endIndex`.
The endIndex character is not included.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toJson()
{: #dialog-methods-strings-toJson}

This method parses a string that contains JSON data and returns a JSON object or array, as in this example:

```
${json_var}.toJson()
```

If the context variable `${json_var}` contains the following string:

```string
"{ \"firstname\": \"John\", \"lastname\": \"Doe\" }"
```

the `toJson()` method returns the following object:

```json
{
  "firstname": "John",
  "lastname": "Doe"
}
```

### String.toLowerCase()
{: #dialog-methods-strings-toLowerCase}

This method returns the original String converted to lowercase letters.

For this input:

```
"This is A DOG!"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Results in this output:

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

This method returns the original String converted to uppercase letters.

For this input:

```
"hi there".
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Results in this output:

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

This method trims any spaces at the beginning and the end of the string and returns the modified string.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### java.lang.String support
{: #dialog-methods-strings-java-lang-String}

In addition to the built-in methods, you can use standard methods of the `java.lang.String` class.

#### java.lang.String.format()
{: #dialog-methods-strings-java-lang-String-format}

You can apply the standard Java String `format()` method to text. See [java.util.formatter reference](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: external} for information about the syntax to use to specify the format details.

For example, the following expression takes three decimal integers (1, 1, and 2) and adds them to a sentence.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Result: `1 + 1 equals 2`.

To change the decimal placement for a number, use the following syntax:

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

For example, if the $number variable that needs to be formatted in US dollars is `4.5`, then a response such as, `Your total is $<? T(String).format('%.2f',$number) ?>` returns `Your total is $4.50.`

## Indirect data type conversion
{: #dialog-methods-indirect-type-conversion}

When you include an expression within text, as part of a node response, for example, the value is rendered as a String. If you want the expression to be rendered in its original data type, then do not surround it with text.

For example, you can add this expression to a dialog node response to return the entities that are recognized in the user input in String format:

```json
  The entities are <? entities ?>.
```
{: codeblock}

If the user specifies *Hello now* as the input, then the @sys-date and @sys-time entities are triggered by the `now` reference. The entities object is an array, but because the expression is included in text, the entities are returned in String format, like this:

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

If you do not include text in the response, then an array is returned instead. For example, if the response is specified as an expression only, not surrounded by text.

```
  <? entities ?>
```
{: codeblock}

The entity information is returned in its native data type, as an array.

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

As another example, the following $array context variable is an array, but the $string_array context variable is a string.

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

If you check the values of these context variables in the Try it out pane, you will see their values specified as follows:

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

You can subsequently perform array methods on the $array variable, such as `<? $array.removeValue('two') ?>`,but not the $array_in_string variable.

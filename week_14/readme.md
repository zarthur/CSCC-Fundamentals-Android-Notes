# Week 14 - JSON and Objects

## Corresponding Text
*Learn Java for Android Development*, pp. 205-217

## Using External Libraries with Gradle
Often when we want our programs to perform a complicated task, it's useful to
use a library or framework developed by someone else.  A **library** is a
collection of code implementing related behavior with a well defined interface
that is accessible to other code using the library.  

Many tools exist for managing the libraries we might want to use in our Java
code.  Among these are tools like Apache Maven and Gradle.  We'll use Gradle
to manage our programs' dependencies. In addition to managing dependencies,
Gradle is also manages how projects are built and packaged.

For the next two weeks topics, we'll work with a new IntelliJ project separate
from our other weekly code. When creating a new project IntelliJ, we can choose
to use Gradle by selecting *Gradle* as shown below. When specifying the
*Groupid* and *Artifactid*, you can use something similar to a package name
and project name, respectively.  

![Using Gradle](images/intellij-gradle.png)

After creating the new project, you should see many Gradle-related files. We
can specify our program's dependencies by modifying *build.gradle*.  For the
work we'll be doing this week, we'll want to use the Gson library so your
*build.gradle* file will look similar to this:

```
group 'com.myname'
version '1.0-SNAPSHOT'

apply plugin: 'java'
apply plugin: 'application'
apply plugin: 'idea'

mainClassName = 'com.myname.Main'

repositories {
    mavenCentral()
}

dependencies {
    compile 'com.google.code.gson:gson:2.6.1'
    testCompile group: 'junit', name: 'junit', version: '4.11'
}

```

After updating the Gradle settings, refresh the Gradle project settings by
choosing *Gradle* from the right-hand toolbar and then clicking the refresh
icon.

![Refresh Gradle](images/gradle-refresh.png)

If IntelliJ prompts you to enable refreshing the Gradle wrapper with sources,
enable the feature. If prompted to index remote respositories, click *Open
Repositories List"*, click the URL containing *http://repo1.maven.org*, and
click the *Update* button.

We'll have to create a folder to store our code.  Until now, we've been placing
all our code in a module or in a package inside a module.  The standard Java
directory structure that you'll often see, has source code appearing in
`src/main/java` - a *src* folder within the project, a *main* folder within
the *src* folder, and a *java* folder in the *main* folder.  Once we create
those folders, we can create a package and class files like usual.

When we create a run configuration for our project, rather than create a
new application configuration, we'll create a Gradle configuration with
settings similar to those below.

![IntelliJ Run Configuration](images/intellij-config.png)

While we're setting up our project, lets create a *.gitignore* file.  The
following will exclude user-specific configuration files from our Git
repository.

```
## Gradle
.gradle
build/

# Ignore Gradle GUI config
gradle-app.setting

# Avoid ignoring Gradle wrapper jar file (.jar files are usually ignored)
!gradle-wrapper.jar

# Cache of project
.gradletasknamecache

# # Work around https://youtrack.jetbrains.com/issue/IDEA-116898
gradle/wrapper/gradle-wrapper.properties

## IntelliJ
# User-specific stuff:
.idea/workspace.xml
.idea/tasks.xml
.idea/dictionaries
.idea/vcs.xml
.idea/jsLibraryMappings.xml

# Sensitive or high-churn files:
.idea/dataSources.ids
.idea/dataSources.xml
.idea/dataSources.local.xml
.idea/sqlDataSources.xml
.idea/dynamic.xml
.idea/uiDesigner.xml

# Gradle:
.idea/gradle.xml
.idea/libraries

# Mongo Explorer plugin:
.idea/mongoSettings.xml

## File-based project format:
*.iws

## Plugin-specific files:

# IntelliJ
/out/
```

Now, if create a *Main* class file and add the following code, we should be
able to run the project.

```java
package com.myname;


public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

The output will be something similar to the following:

```
9:22:41 AM: Executing external task 'run'...
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
:run
Hello, world!

BUILD SUCCESSFUL

Total time: 0.286 secs
9:22:42 AM: External task execution finished 'run'.
```

The output includes information related to Gradle's build process but we can
see the output of our program as well.

## JSON

**JSON** is a language-independent data format used to exchange data using
human-readable text. JSON is based on JavaScript but can be used with any
language.  Often, JSON is used to send and receive data to/from another
program including a web server.

There are six basic data types that are representable in JSON:

1. *Number*: a signed, decimal number that may contain a fractional part,
2. *String*: a sequence of zero or more unicode characters delimited with
   double quotes,
3. *Boolean*: either *true* or *false*,
4. *Array*: an ordered list of zero or more values each of which can be of
   any type,
5. *Object*: an unordered collection of key/value pairs where the keys are
   strings, and
6. *null*: an empty value.

The following is an example of a JSON array containing both strings and
numbers:

```JSON
[10, "Hello", -4.32, "😀"]
```

Note that arrays are enclosed in square brackets (`[` and `]`) and elements
are separated by commas.

The following is an example of a JSON object:

```JSON
{
  "name": "columbus",
  "temperature": 70,
  "forecast": [70, 65, 82, 54, 60]
}
```

JSON objects are enclosed in curly brackets (`{` and `}`), names are strings
enclosed in double quotes, values are any JSON type, a key is separated from
a value using a colon, and key/value pairs are separated from one another using
commas.

##JSON in Java
Now that we've been introduced to JSON, we'd like to be able to use it in
Java.  For example, the next lecture will include examples of retrieving and
sending data from/to a website; we'll use JSON to to facilitate the transfer.

When working with JSON and other similar formats, we often describe the
process of converting data in our program to JSON as **serialization** or
**marshalling**. The process of converting JSON data into a data structure our
program can work with is known as **deserialization**. We'll look at
deserialization first.

For the following examples, we'll be working with the following JSON data or
some subset:

```JSON
[
    {
        "name":"columbus",
        "forecast":[40, 50, 65, 60, 70]
    },
    {
        "name":"cleveland",
        "forecast":[35, 55, 60, 45, 65]
    },
    {
        "name":"cincinnati",
        "forecast":[35, 60, 65, 45, 65]
    }
]
```

This data consists of a single JSON array where each element is a JSON object.
Each of the three JSON objects have two key/value pairs.  The first key/value
pair has a "name" key and a string value.  The second key/value pair has a
"forecast" key and an array of numbers as the corresponding value.  

### Deserialization
To start, let's consider this subset of the JSON data from above:

```JSON
{
    "name":"columbus",
    "forecast":[40, 50, 65, 60, 70]
}
```

For our examples, we'll store the JSON data in a string and use the Gson
library to convert between JSON and Java data.

```Java
package com.myname.week_14;

import com.google.gson.JsonArray;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

import java.util.Map;

public class Main {
    public static void main(String[] args) {
        String jsonData = "{\"name\": \"columbus\", \"forecast\": " +
                "[40, 50, 65, 60, 70]}";

        JsonParser parser = new JsonParser();

        // parse the JSON data and return a JSON object for the top-level data
        JsonObject jsonObject = parser.parse(jsonData).getAsJsonObject();

        // Iterate through the JSON object
        // Each key/value in the object can be represented as a
        // String/JSON element pair
        for (Map.Entry<String, JsonElement> entry: jsonObject.entrySet() ) {

            // if the value is a JSON array, convert the JSON element to
            // JSON array and iterate
            if (entry.getValue().isJsonArray()) {
                System.out.println(entry.getKey() + ":");

                JsonArray jsonArray = entry.getValue().getAsJsonArray();
                for (JsonElement element : jsonArray) {
                    System.out.println("\t" + element);
                }
            }

            // if the value is not a JSON array, get it's value
            else {
                System.out.println(entry.getKey() + ": " + entry.getValue());
            }
        }

    }
}
```

In this example, we start by creating a string representing JSON data.  Note
that because both Java and JSON strings are enclosed in double quotes, we must
use a backslash character within the Java string to "escape" the quotes used
for JSON strings.  Next, we create an instance of *JsonParser* from the Gson
library and use it to begin processing the JSON data contained in the
*jsonData* string.  We know that that the outer-most data type in the JSON data
is an object, so we can ask the parser to return a *JsonObject* using the
*getAsJsonObject()* method.  Because JSON objects consist of key/value pairs,
we can iterate through the object, one key/value pair at a time.  Instance
of *JsonObject* have a method, *entrySet()* that returns a *Map.Entry<>*
object we can use for iteration.  *Map.Entry<>* requires that we specify the
key and value types; the keys are Java strings and the values are instances of
*JsonElement* which is used to represent JSON data of any JSON type.  In this
case, We know that the values are either JSON arrays or strings.  We can check
if the value is an array using the *isJsonArray()* array method.  If
it is an array, we use the *getAsJsonArray()* method to create a *JsonArray*
object that we can use to further iterate.  In this example, we know that the
inner array contains numbers.  If we want to display the numbers, we can rely
on the *JsonElement.toString()* implementation to display the appropriate
value.  Similarly, if the value in the key/vale pair was not an array, we can
rely on the *JsonElement.toString()* method to display the value.

While this example shows how we can iterate through JSON data, it doesn't show
us how to use the data to create a Java object based on the data.

To do that, we can create a class representing forecast data and use it's
methods to set it's properties using the JSON data.

```Java
package com.myname.week_14;

import com.google.gson.JsonArray;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

// a class representing a city's forecast
class Forecast {
    private String name;
    private List<Double> forecast;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<Double> getForecast() {
        return new ArrayList<>(forecast);
    }

    public void setForecast(List<Double> forecast) {
        this.forecast = forecast;
    }

    public String toString() {
        List<String> forecastStrings = new ArrayList<>();
        for (Double temp: forecast) {
            forecastStrings.add(temp.toString());
        }
        String forecastString = String.join(", ", forecastStrings);
        return String.format("The forecast for %s: is %s", name, forecastString);
    }
}

public class Main {
    public static void main(String[] args) {
        String jsonData = "{\"name\": \"columbus\", \"forecast\": " +
                "[40, 50, 65, 60, 70]}";

        JsonParser parser = new JsonParser();

        // a Forecast object
        Forecast forecast = new Forecast();

        JsonObject jsonObject = parser.parse(jsonData).getAsJsonObject();

        for (Map.Entry<String, JsonElement> entry: jsonObject.entrySet() ) {

            // if the value is a JSON array, create a list of values and
            // assign it to the forecast
            if (entry.getValue().isJsonArray()) {
                List<Double> forecastData = new ArrayList<>();
                JsonArray jsonArray = entry.getValue().getAsJsonArray();
                for (JsonElement element : jsonArray) {
                    forecastData.add(element.getAsDouble());
                }
                forecast.setForecast(forecastData);
            }

            // if the value is not a JSON array, it's the forecast's name
            else {
                forecast.setName(entry.getValue().getAsString());
            }
        }

        //display forecast
        System.out.println(forecast);
    }
}
```

Here, we've created a class *Forecast* to represent a city's forecast by
storing the city's name and a list of temperatures corresponding to the
forecast.  Now, while iterating through the JSON data, we can use the data to
assign values to the *Forecast* instance's fields rather than simply displaying
the values at the console.

While these two examples, help us understand how to process JSON data
layer-by-layer, this approach can be tedious and we are likely to accidentally
introduce errors.  Fortunately, the Gson library provides us with another
way of creating instances of objects using JSON data.

```Java
package com.myname.week_14;


import com.google.gson.Gson;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

// a class representing a city's forecast
class Forecast {
    private String name;
    private List<Double> forecast;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<Double> getForecast() {
        return new ArrayList<>(forecast);
    }

    public void setForecast(List<Double> forecast) {
        this.forecast = forecast;
    }

    public String toString() {
        List<String> forecastStrings = new ArrayList<>();
        for (Double temp: forecast) {
            forecastStrings.add(temp.toString());
        }
        String forecastString = String.join(", ", forecastStrings);
        return String.format("The forecast for %s: is %s", name, forecastString);
    }
}

public class Main {
    public static void main(String[] args) {
        String jsonData = "{\"name\": \"columbus\", \"forecast\": " +
                "[40, 50, 65, 60, 70]}";

        Gson gson = new Gson();
        Forecast forecast = gson.fromJson(jsonData, Forecast.class);
        System.out.println(forecast);
    }
}
```

Using the *Gson* class, we can easily create instances of other classes from
JSON data.  In this example, the *Gson.fromJson()* method takes two parameters:
a string containing JSON data and an instance of the generic class *Class<T>*
used to determine the type of object to create from the JSON data.  

The Gson deserializer relies on field names and types to be able to match JSON
data with the appropriate fields in the class. While we don't have to provide
getters and setters for the Gson serializer to work properly, we do have to
use the JSON object's keys' names as field names in order for the deserializer
to work properly.  

Many forecasts:

```Java
package com.myname.week_14;

import com.google.gson.Gson;

public class Main {
    public static void main(String[] args) {
        String jsonData = "[{'name': 'columbus', 'forecast': [40, 50, 65, 60, 70]}," +
                "{'name': 'cleveland', 'forecast': [35, 40, 55, 60, 65]}," +
                "{'name': 'cincinnati', 'forecast': [45, 50, 70, 65, 65]}]";

        Gson gson = new Gson();

        Forecast[] forecasts = gson.fromJson(jsonData, Forecast[].class);
        for (Forecast cityForecast: forecasts) {
            System.out.println(cityForecast);
        }
    }
}
```

Alternative:
```Java
package com.myname.week_14;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

import java.lang.reflect.Type;
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        String jsonData = "[{'name': 'columbus', 'forecast': [40, 50, 65, 60, 70]}," +
                "{'name': 'cleveland', 'forecast': [35, 40, 55, 60, 65]}," +
                "{'name': 'cincinnati', 'forecast': [45, 50, 70, 65, 65]}]";

        Gson gson = new Gson();

        Type forecastListType = new TypeToken<ArrayList<Forecast>>(){}.getType();
        List<Forecast> forecasts = gson.fromJson(jsonData, forecastListType);

        for (Forecast cityForecast: forecasts) {
            System.out.println(cityForecast);
        }

    }
}
```

Another alternative using custom class:

ForecastCollection.class:

```Java
package com.myname.week_14;

import java.util.ArrayList;

public class ForecastCollection extends ArrayList<Forecast> {
}
```

Main.class:

```Java
package com.myname.week_14;

import com.google.gson.Gson;

public class Main {
    public static void main(String[] args) {
        String jsonData = "[{'name': 'columbus', 'forecast': [40, 50, 65, 60, 70]}," +
                "{'name': 'cleveland', 'forecast': [35, 40, 55, 60, 65]}," +
                "{'name': 'cincinnati', 'forecast': [45, 50, 70, 65, 65]}]";

        Gson gson = new Gson();

        ForecastCollection forecasts = gson.fromJson(jsonData, ForecastCollection.class);

        for (Forecast cityForecast: forecasts) {
            System.out.println(cityForecast);
        }

    }
}
```

### Serialziation

## Exercise
Task problem

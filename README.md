JSON in Java [package org.json]
===============================

[![Maven Central](https://img.shields.io/maven-central/v/org.json/json.svg)](https://mvnrepository.com/artifact/org.json/json)

**[Click here if you just want the latest release jar file.](https://repo1.maven.org/maven2/org/json/json/20200518/json-20200518.jar)**

JSON is a light-weight, language independent, data interchange format.
See http://www.JSON.org/

The files in this package implement JSON encoders/decoders in Java.
It also includes the capability to convert between JSON and XML, HTTP
headers, Cookies, and CDL.

This is a reference implementation. There is a large number of JSON packages
in Java. Perhaps someday the Java community will standardize on one. Until
then, choose carefully.

The license includes this restriction: "The software shall be used for good,
not evil." If your conscience cannot live with that, then choose a different
package.

The package compiles on Java 1.6-1.8.

# With commit [#515 Merge tests and pom and code](https://github.com/stleary/JSON-java/pull/515), the structure of the project has changed from a flat directory containing all of the Java files to a directory structure that includes unit tests. If you have difficulty using the new structure, please open an issue so we can work through it.



**JSONObject.java**: The `JSONObject` can parse text from a `String` or a `JSONTokener`
to produce a map-like object. The object provides methods for manipulating its
contents, and for producing a JSON compliant object serialization.

**JSONArray.java**: The `JSONArray` can parse text from a String or a `JSONTokener`
to produce a vector-like object. The object provides methods for manipulating
its contents, and for producing a JSON compliant array serialization.

**JSONTokener.java**: The `JSONTokener` breaks a text into a sequence of individual
tokens. It can be constructed from a `String`, `Reader`, or `InputStream`. It also can 
parse text from a `String`, `Number`, `Boolean` or `null` like `"hello"`, `42`, `true`, 
`null` to produce a simple json object.

**JSONException.java**: The `JSONException` is the standard exception type thrown
by this package.

**JSONPointer.java**: Implementation of
[JSON Pointer (RFC 6901)](https://tools.ietf.org/html/rfc6901). Supports
JSON Pointers both in the form of string representation and URI fragment
representation.

**JSONPropertyIgnore.java**: Annotation class that can be used on Java Bean getter methods.
When used on a bean method that would normally be serialized into a `JSONObject`, it
overrides the getter-to-key-name logic and forces the property to be excluded from the
resulting `JSONObject`.

**JSONPropertyName.java**: Annotation class that can be used on Java Bean getter methods.
When used on a bean method that would normally be serialized into a `JSONObject`, it
overrides the getter-to-key-name logic and uses the value of the annotation. The Bean
processor will look through the class hierarchy. This means you can use the annotation on
a base class or interface and the value of the annotation will be used even if the getter
is overridden in a child class.   

**JSONString.java**: The `JSONString` interface requires a `toJSONString` method,
allowing an object to provide its own serialization.

**JSONStringer.java**: The `JSONStringer` provides a convenient facility for
building JSON strings.

**JSONWriter.java**: The `JSONWriter` provides a convenient facility for building
JSON text through a writer.


**CDL.java**: `CDL` provides support for converting between JSON and comma
delimited lists.

**Cookie.java**: `Cookie` provides support for converting between JSON and cookies.

**CookieList.java**: `CookieList` provides support for converting between JSON and
cookie lists.

**HTTP.java**: `HTTP` provides support for converting between JSON and HTTP headers.

**HTTPTokener.java**: `HTTPTokener` extends `JSONTokener` for parsing HTTP headers.

**XML.java**: `XML` provides support for converting between JSON and XML.

**JSONML.java**: `JSONML` provides support for converting between JSONML and XML.

**XMLTokener.java**: `XMLTokener` extends `JSONTokener` for parsing XML text.

Unit tests are now included in the project, but require Java 1.8 at the present time. This will be fixed in a forthcoming commit.

Numeric types in this package comply with
[ECMA-404: The JSON Data Interchange Format](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) and
[RFC 8259: The JavaScript Object Notation (JSON) Data Interchange Format](https://tools.ietf.org/html/rfc8259#section-6).
This package fully supports `Integer`, `Long`, and `Double` Java types. Partial support
for `BigInteger` and `BigDecimal` values in `JSONObject` and `JSONArray` objects is provided
in the form of `get()`, `opt()`, and `put()` API methods.

Although 1.6 compatibility is currently supported, it is not a project goal and may be
removed in some future release.

In compliance with RFC8259 page 10 section 9, the parser is more lax with what is valid
JSON than the Generator. For Example, the tab character (U+0009) is allowed when reading
JSON Text strings, but when output by the Generator, tab is properly converted to \t in
the string. Other instances may occur where reading invalid JSON text does not cause an
error to be generated. Malformed JSON Texts such as missing end " (quote) on strings or
invalid number formats (1.2e6.3) will cause errors as such documents can not be read
reliably.

Some notible exceptions that the JSON Parser in this library accepts are:
* Unquoted keys `{ key: "value" }`
* Unquoted values `{ "key": value }`
* Unescaped literals like "tab" in string values `{ "key": "value   with an unescaped tab" }`
* Numbers out of range for `Double` or `Long` are parsed as strings

Release history:

~~~
20200518    Recent commits and snapshot before project structure change

20190722    Recent commits

20180813    POM change to include Automatic-Module-Name (#431)

20180130    Recent commits

20171018    Checkpoint for recent commits.

20170516    Roll up recent commits.

20160810    Revert code that was breaking opt*() methods.

20160807    This release contains a bug in the JSONObject.opt*() and JSONArray.opt*() methods,
it is not recommended for use.
Java 1.6 compatability fixed, JSONArray.toList() and JSONObject.toMap(),
RFC4180 compatibility, JSONPointer, some exception fixes, optional XML type conversion.
Contains the latest code as of 7 Aug, 2016

20160212    Java 1.6 compatibility, OSGi bundle. Contains the latest code as of 12 Feb, 2016.

20151123    JSONObject and JSONArray initialization with generics. Contains the
latest code as of 23 Nov, 2015.

20150729    Checkpoint for Maven central repository release. Contains the latest code
as of 29 July, 2015.
~~~


JSON-java releases can be found by searching the Maven repository for groupId "org.json"
and artifactId "json". For example:
https://search.maven.org/search?q=g:org.json%20AND%20a:json&core=gav

# Unit tests
The test suite can be executed with Maven by running:
```
mvn test
```
The test suite can be executed with Gradle (6.4 or greater) by running:
```
gradle clean build test
```



## Conventions
Test filenames should consist of the name of the module being tested, with the suffix "Test". 
For example, <b>Cookie.java</b> is tested by <b>CookieTest.java</b>.

<b>The fundamental issues with JSON-Java testing are:</b><br>
* <b>JSONObjects</b> are unordered, making simple string comparison ineffective. 
* Comparisons via **equals()** is not currently supported. Neither <b>JSONArray</b> nor <b>JSONObject</b> override <b>hashCode()</b> or <b>equals()</b>, so comparison defaults to the <b>Object</b> equals(), which is not useful.
* Access to the <b>JSONArray</b> and <b>JSONObject</b> internal containers for comparison is not currently available.

<b>General issues with unit testing are:</b><br>
* Just writing tests to make coverage goals tends to result in poor tests. 
* Unit tests are a form of documentation - how a given method actually works is demonstrated by the test. So for a code reviewer or future developer looking at code a good test helps explain how a function is supposed to work according to the original author. This can be difficult if you are not the original developer.
*	It is difficult to evaluate unit tests in a vacuum. You also need to see the code being tested to understand if a test is good. 
* Without unit tests it is hard to feel confident about the quality of the code, especially when fixing bugs or refactoring. Good tests prevents regressions and keeps the intent of the code correct.
* If you have unit test results along with pull requests, the reviewer has an easier time understanding your code and determining if the it works as intended.


**Caveats:**
JSON-Java is Java 1.6-compatible, but JSON-Java-unit-tests requires Java 1.8. If you see this error when building JSON-Java-unit-test, make sure you have 1.8 installed, on your path, and set in JAVA_HOME:
```
Execution failed for task ':compileJava'.
> invalid flag: -parameters
```


| Resource files used in test |
| ------------- |  
| EnumTest.java |
| MyBean.java |
| MyBigNumberBean.java |
| MyEnum.java |
| MyEnumClass.java |
| MyEnumField.java |
| MyJsonString.java |
| MyPublicClass.java |
| PropertyTest.java |
| JunitTestSuite.java | 
| StringsResourceBundle.java | 
| TestRunner.java | 
| Util.java | 




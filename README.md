# RJ
RJ(Readable JSON), is a config file format. It borrows name and value specs mostly from [JSON](https://json.org), and makes the file readable. Human readability makes it straightforward for developers to read and write, and "JSON" makes it easy to be parsed into data structures of different languages. 

## Example

```
# This is a RJ(Readable JSON) doc

name: "Example"
versionCode: 1
versionName: "0.0.1"
# Line breaks and indents are allowed inside an array
targetedPlatform: ["amd","amd64",
                    "arm","arm64"] 

# Node can be mapped to a JSON object
[publisher]
name: "FIPress"
email: "dev@fipress.org"
    [publisher.]

# Node list can be mapped to a JSON array of objects
[dependencies]
- name: "a"
  version: 1  # The indentation here is optional
- name: "b"   
  version: 2
  
   
```

## Brief

* RJ is UTF-8 encoded.
* RJ is a set of JSON name/value pairs. The difference is, they are separated by line breaks instead of comma `,`.
* Filename extension `.rj` is recommended.

## Spec

### Name/value pairs

Mostly, the name/value pairs followed JSON specs, except for object and object list values. In RJ, each pair takes a line, except for multiple-line-strings and arrays. 

#### Name
Since all names are strings, we do NOT surround them with quotes `"` in RJ.

### Value
RJ follows the same rule of strings, numbers, booleans, "null", objects and arrays as JSON. Besides, RJ supports two more kinds of values. 

**Raw string**
Raw strings are surrounded by backticks `\``. 

**Datetime** 

### Comment

A comment starts from a hash symbol `#`. It can take a whole line, or at the end of a line.

### Node

A node is equals to a name/value pair with an object value. The node name is the pair's name, and it should be surround with square brackets, `[NodeName]`. The node name will take a full line, the value take the next lines. And, a blank line ends a node.

```
[student]
name: "Jason"
age: 14

# Above blank line is required if there is other data below that does not belong to `student`   
```

It equals to following name/value pair in RJ.

```
student: {name: "Json", 
    age: 14}  # line breaks inside braces are allowed 
``` 

And it maps to following JSON.

```
{"sdudent": {"name": "Json", "age": 14}}
```

**Child node**
Child nodes are marked by their full names. A full name is `parentName.childName`. For example, if node `b` is a child node of node `a`, its name should be `a.b` and if b has a child node `c`, its name should be `a.b.c`, and so on.

```
[student]
name: "Jason"
age: 14

[student.address]
city: "Round Rock"
zipCode: "123456"
```

It equals to:
```
[student]
name: "Jason"
age: 14
address: {city: "Round Rock",zipCode: "123456"}
```
  
That represents following JSON.

```
{"sdudent": {"name": "Json", "age": 14, "address": {"city": "Round Rock", "zipCode": "123456"}}}
``` 

### Node list
A node list is actually a name/value pair with value of a object list. The name of a node list follows the same rule as a node. Each item starts with a `-`.

```
[player]
- name: "Amy"
  age: 22
- name: "Clare"
  age: 21
```

That is equals to following name/value pair.

```
player: [{name: "Amy", age: 22},
        {name: "Clare", age: 21}]  # line breaks inside square brackets are allowed 
```

Above RJ maps to following JSON.

```
{"player": [{"name": "Amy", "age": 22},
        {"name": "Clare", "age": 21}] 
}        
``` 
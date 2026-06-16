---
layout: single_content
title: "9 JSON"
permalink: /foundations/json/
toc: true
toc_sticky: true
author_profile: true
date: 2026-06-14 12:00:00
description: "An introduction to JSON."
categories:
  - JSON
  - data formats
read_time: true
sidebar:
  - title: "JSON"
    image: /assets/images/json.png
    image_alt: 
header:
  teaser: /assets/images/json_teaser.png
---

**Context**: This was written as accompanying material for the [MDG Library Carpentry workshop 2026 in Birmingham](/workshops/cilip_mdg_lc_bham_2026_05/). Check out the workshop page for a list of all covered topics.
{: .notice--primary}

JSON[^21] stands for **J**ava**S**cript **O**bject **N**otation (it was extended from JavaScript). It was developed 2000/2001 and first standardised in 2013 as ECMA-404[^18] and then again in 2017 as RFC 8259[^19] and ISO-IEC 21778:2017[^20]. 

Just like XML its **purpose** is **data serialisation** (i.e. data storage, transmission and reconstruction). 

But, it's a lot easier! No namespaces for starters😋

# JSON Syntax

The basic premise is **key-value pairs**. You've seen this in action as attributes in XML: the attribute name is the key and the value, well, the value. Key and value are separated by `:`. 

**Keys must be strings** in JSON, but **values** can have a number of different **data types**:

- **string** values are enclosed in `""` (`"` occurring in a string value must be escaped as `\"`),
- **number** (there is no distinction between integer and float),
- **Boolean** (`true` or `false`),
- **array**s are enclosed in `[ ]` and can consist of values of all data types in this list, 
- **object**s are enclosed in `{ }` and consist of key-value pairs whose keys should be unique within the object, and
- `null`, which means no value (slightly different from the key not existing at all)

The values in arrays and objects are separated by `,`.

The **root** element must either be an **object** or an **array**.

There is **no provision for comments** in JSON.

That's it. That's all the rules.[^23]

# JSON example

Using our cats example, let's start by describing an individual cat. We'll need keys for id, name, age, breed and colour; they are already strings, so let's just use them. And we need to wrap them in an object (`{ }`) as we need a root object or array.

```json
{
    "id": 1,
    "name": "Duncan",
    "age": 10,
    "breed": "moggy",
    "colour": ["orange", "white"]
}
```

That's it. Cat described. 

To add another cat, let's wrap our existing cat object in an array and add a second object describing a cat.

```json
[
    {
        "id": 1,
        "name": "Duncan",
        "age": 10,
        "breed": "moggy",
        "colour": ["orange", "white"]
    },
    {
        "id": 2,
        "name": "Izzy",
        "age": 5,
        "breed": "moggy",
        "colour": ["black"]
    }
]
```

The only two data types that out example doesn't cover yet are Boolean and `null`. 

To demonstrate Boolean, let's add the key "hungry" to each cat. Duncan is hungry and Izzy is not:

```json
[
    {
        "id": 1,
        "name": "Duncan",
        "age": 10,
        "breed": "moggy",
        "colour": ["orange", "white"],
        "hungry": true
    },
    {
        "id": 2,
        "name": "Izzy",
        "age": 5,
        "breed": "moggy",
        "colour": ["black"],
        "hungry": false
    }
]
```

Finally, `null`: Say we don't know the age of our cats. There are two options, we simply omit the key or we set its value to `null`.

```json
[
    {
        "id": 1,
        "name": "Duncan",
        "breed": "moggy",
        "colour": ["orange", "white"],
        "hungry": true
    },
    {
        "id": 2,
        "name": "Izzy",
        "age": null,
        "breed": "moggy",
        "colour": ["black"],
        "hungry": false
    }
]
```

What's the difference? You'll mostly notice it when working with JSON data and extracting values from it. If a key doesn't exist you might get an error, where as if it does and the value is `null`, you get the `null` value. 

# Validating JSON

The validation vocabulary for JSON is called **JSON Schema**[^22]. JSON Schema documents are JSON documents. I have yet to encounter a situation where I had to work with this beyond experimentation, hence we'll not dwell on it. I'll just show you what it looks like and talk you through. 

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id": "https://www.example.com/cats.json",
    "title": "Cats",
    "description": "Describe some cats",
    "type": "array",
    "properties": {
        "id": {
            "description": "The cat's ID",
            "type": "integer",
            "minimum": 1
        },
        "name": {
            "description": "The cat's name",
            "type": "string"
        },
        "age": {
            "description": "The cat's age",
            "type": "integer",
            "minimum": 0
        },
        "breed": {
            "description": "The cat's breed",
            "type": ["string", "null"]
        },
        "colour": {
            "description": "The cat's colour(s)",
            "type": "array",
            "items": {
                "enum":[
                    "black",
                    "white",
                    "orange",
                    "blue",
                    "cream",
                    "lilac",
                    "chocolate",
                    "cinnamon",
                    "fawn"
                ]
            }
        }
    },
    "required": [
        "id",
        "name"
    ],
    "additionalProperties": false
}
```

- `"$schema": "https://json-schema.org/draft/2020-12/schema"` defines the version of JSON Schema this document uses.
- `"$id": "https://www.example.com"` sets a unique identifier for the schema
- `"title"` and `"description"` is metadata about the schema, i.e. giving it a title and saying something about its purpose etc.
- `"type": "array"` means it expects an array as the root element
- `"properties": {...}` describes the keys and which data format their values are and any other restrictions placed on them. The keys described are:
  - `"id"`, whose value should be an integer with a minimum value of 1,
  - `"name"`, whose value should be a string,
  - `"age"`, whose value should be an integer with the minimum allowed value being 0,
  - `"breed"`, whose value should be a string or `null`,
  - `"colour"`, whose value should be an array whose value(s) should be `black`, `white`, `orange`, `blue`, `cream`, `lilac`, `chocolate`, `cinnamon` or `fawn`.
- `"required": ["id", "name" ]` means that the keys `id` and `name` must be present, for each cat, all others are optional.
- `additionalProperties": false"` means no keys other than the ones given are allowed.

# XML vs JSON

If your use case doesn't dictate which format to use, how do you decide? There are some significant differences between XML and JSON that usually make JSON the better and easier to work with choice, especially if you also work with the data programmatically.

|                        | XML                                                       | JSON                                                         |
| ---------------------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| Syntax and readability | Verbose and can be hard to read due to tag structure      | Compact and easier to read due to key-value pair syntax      |
| File size              | Larger, due to verbosity                                  | Smaller                                                      |
| Parsing                | Needs a complex parser to read and write programmatically | Is simpler and quicker to parse programmatically and maps easily into languages such as JavaScript and Python |
| Data type support      | More flexible and supports complex data types             | Limited range of data types                                  |
| Ease of use            | More complex and less flexible                            | Simple and flexible                                          |
| Validation             | Can be validated using XML Schema                         | Can be validated using JSON Schema                           |
| Security               | Has some security concerns re DTDs, don't use them.       | Is safer than XML                                            |

This might make it sound like JSON is always the better choice, but that's not true. If you need namespaces, data types JSON cannot support or very complex data structures, XML does the better job.[^24]

# Annotated bibliography and further reading


- <a name= "json-w3s"></a>'JavaScript JSON' (no date) *W3Schools*. Available at: [https://www.w3schools.com/js/js_json.asp](https://www.w3schools.com/js/js_json.asp) [Accessed: 24 May 2026] -- The W3Schools JSON tutorial is part of the JavaScript one and thus maybe not the best unless you are also interested in JavaScript.
- <a name= "json-wikipedia"></a>'JSON' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/JSON](https://en.wikipedia.org/wiki/JSON) [Accessed: 24 May 2026]
- <a name= "json-tp"></a>'JSON Cheatsheet' (no date) in *JSON Tutorial*. Available at: [https://www.tutorialspoint.com/json/index.htm](https://www.tutorialspoint.com/json/index.htm) [Accessed: 24 May 2026] -- The syntax summary is useful and has plenty of examples.

**The technical standards:**

- <a name= "bray-2017"></a>Bray, Tim (2017) *The JavaScript Object Notation (JSON) Data Interchange Format*. RFC 8259, STD 90. Available at: [https://www.rfc-editor.org/info/rfc8259/](https://www.rfc-editor.org/info/rfc8259/)[Accessed: 24 May 2026]

  *The current JSON standard.*

- <a name= "crockford-2006"></a>Crockford, Douglas (2006) *The application/json Media Type for JavaScript Object Notation (JSON)*. RFC 4627. Available at: [https://www.rfc-editor.org/info/rfc4627/](https://www.rfc-editor.org/info/rfc4627/) [Accessed: 24 May 2026] 

  *The first definition, an informational RFC, now obsolete, current standard is [Bray, T., 2017](#bray-2017).*

- <a name= "ecma404-1"></a>ECMA International (2013) *ECMA-404: The JSON data interchange syntax*. 1st ed. Available at: [https://ecma-international.org/wp-content/uploads/ECMA-404_1st_edition_october_2013.pdf](https://ecma-international.org/wp-content/uploads/ECMA-404_1st_edition_october_2013.pdf) [Accessed: 24 May 2026]

  *Now obsolete, the current standard is [Bray, T., 2017](#bray-2017).* 

- <a name= "ecma404-2"></a>ECMA International (2017) *ECMA-404: The JSON data interchange syntax*. 2nd ed. Available at: [https://ecma-international.org/wp-content/uploads/ECMA-404_2nd_edition_december_2017.pdf](https://ecma-international.org/wp-content/uploads/ECMA-404_2nd_edition_december_2017.pdf) [Accessed: 24 May 2026]

  *Describes only the allowed syntax, the more complete current standard is [Bray, T., 2017](#bray-2017).*

- <a name= "json-iso"></a>International Organization for Standardization (2017) *ISO/IEC 21778:2017Information technology: the JSON data interchange syntax*. Available at: [https://www.iso.org/standard/71616.html](https://www.iso.org/standard/71616.html) [Accessed: 24 May 2026]

  *Describes only the allowed syntax, the more complete current standard is [Bray, T., 2017](#bray-2017).*

**JSON Schema:**

- *<a name="json-schema"></a>JSON Schema* (no date) Available at: [https://json-schema.org/](https://json-schema.org/) [Accessed: 24 May 2026]

The **most recent version** of JSON Schema is 2020-12. JSON Schema is not a recognised standard (yet?), hence the version thing.

- *Draft 2020-12* (no date) Available at: [https://json-schema.org/draft/2020-12](https://json-schema.org/draft/2020-12) [Accessed: 24 May 2026]

  *The landing page of the version on the JSON Schema website*

- Wright, Austin, Andrews, Henry and Hutton, Ben (2022): *JSON Schema Validation: A Vocabulary for Structural Validation of JSON*. Available at: [https://json-schema.org/draft/2020-12/draft-bhutton-json-schema-validation-01](https://json-schema.org/draft/2020-12/draft-bhutton-json-schema-validation-01) [Accessed: 24 May 2026]

  *The actual specification document*

To **get started** with JSON Schema I found these pages useful:

- Creating your first schema (no date) Available at: [https://json-schema.org/learn/getting-started-step-by-step](https://json-schema.org/learn/getting-started-step-by-step) [Accessed: 24 May 2026]

- *JSON Schema reference* (no date) Available at: [https://json-schema.org/understanding-json-schema/reference](https://json-schema.org/understanding-json-schema/reference) [Accessed: 24 May 2026]

- 'JSON schemas and settings' (2026) in *Editing JSON with Visual Studio Code*. Available at: [https://code.visualstudio.com/docs/languages/json#_json-schemas-and-settings](https://code.visualstudio.com/docs/languages/json#_json-schemas-and-settings) [Accessed: 24 May 2026]

  *Has information in how to configure VS Code to validate a JSON file against a specific JSON Schema file.*

Other sources I used when putting the workshop together:

- <a name="aws"></a>Amazon Web Services (no date) *What’s the Difference Between JSON and XML?* Available at: [https://aws.amazon.com/compare/the-difference-between-json-xml/](https://aws.amazon.com/compare/the-difference-between-json-xml/) [Accessed: 24 May 2026]
- <a name="liquidtech"></a>Liquid Technologies (2025) *Understanding JSON: A Beginner’s Guide – Part 1 of 4*. Available at: [https://blog.liquid-technologies.com/understanding-json-a-beginners-guide-part-1-of-4](https://blog.liquid-technologies.com/understanding-json-a-beginners-guide-part-1-of-4) [Accessed: 24 May 2026]

[^18]: [ECMA International, 2013](#ecma404-1) and the second edition [ECMA International, 2017](#ecma404-2)
[^19]: [Bray, Tim, 2017](#bray-2017)
[^20]: [International Organization for Standardization, 2017](#json-iso)
[^21]: The information in this section has been, unless stated otherwise, compiled using the following sources: [Bray, Tim, 2017](#bray-2017), ['JavaScript JSON', no date](#json-w3s), ['JSON', 2026](#json-wikipedia)
[^22]: [*JSON Schema*, no date](#json-schema)
[^23]: There are some intricacies of which characters can be used for numbers and encoding bits for strings. None of which are terribly relevant for bibliographic use cases. See [Bray, Tim, 2017](#bray-2017). 
[^24]: The information in this section has been, unless stated otherwise, compiled using the following sources: [Amazon Web Services, no date](#aws), [Liquid Technologies, 2025](#liquidtech)

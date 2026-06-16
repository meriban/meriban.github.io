---
layout: single_content
title: "8 XML Schema (XSD)"
permalink: /foundations/xsd/
toc: true
toc_sticky: true
author_profile: true
date: 2026-06-14 12:00:00
description: "An introduction to XML Schema."
categories:
  - XML
  - data formats
read_time: true
sidebar:
  - title: "XML Schema"
    image: /assets/images/xsd.png
    image_alt: 
header:
  teaser: /assets/images/xsd_teaser.png
---

**Context**: This was written as accompanying material for the [MDG Library Carpentry workshop 2026 in Birmingham](/workshops/cilip_mdg_lc_bham_2026_05/) and is meant to be read in conjunction with [XML](/foundations/xml/) as the exerises build on each other.
{: .notice--primary}

XSD[^13] stands for **X**ML **S**chema **D**efinition and is the current standard to define the structure of an XML document. It defines:

- the **allowed** **elements** and attributes,
- the **order** and **number** of child elements,
- the **data types** of elements and attributes, and
- **default** and **fixed values** of elements and attributes.

**XSDs are themselves XML documents.** The schema defining their structure is the XML Schema schema. So what rules is this based on? Well, itself. Yes, that's somewhat confusing: a definition uses itself as the rules. But really it's no different that defining e.g. the English grammar rules using the English language which uses the English grammar rules, i.e. the very thing it is defining.

As mentioned before, XML Schema has **advantages** over its predecessor DTD:

- no need to learn another syntax, XML Schemas are XML documents,
- it supports data types,
- it supports namespaces (we'll talk about these in a sec),
- it is extensible as it is XML based,
- it allows for more fine-grained definitions of elements and attributes.

At this point you probably want to switch to [Namespaces](/foundations/namespaces/) unless you are already familiar and comfortable with that concept. [Namespaces](/foundations/namespaces/) also discusses how to link an XSD file to an XML file for validation.
{: .notice--warning}

# XML Schema Definition syntax

## `xs:schema`

Every XML Schema Definition's **root element is `<xs:schema>`**, with attributes defining 

- the `xs` namespace (so we can use the elements and attributes defined there) `xmlns:xs="http://www.w3.org/2001/XMLSchema"`, 
- the schema's default namespace `xmlns="https://www.example.com"`, 
- the namespace the elements, attributes etc. in the schema belong to `targetNamespace="https://www.example.com"` (this is the namespace that should be used to reference this schema in XML documents using it),
- some default values that guide how to XSD validation behaves `elementFormDefault="qualified"` and `attributeFormDefault="unqualified"`.[^17]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
</xs:schema>
```

## Defining elements `xs:element`

Elements are defines using the `xs:element` element. 

To define our first element `cats` we add an `xs:element` element with the `name` attribute containing the name of the defined element:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
    </xs:element>
</xs:schema>
```

There are two types of elements in XML Schema:

- **simple elements**,
  - **can only contain a value**. They cannot contain other elements or have attributes.
  - the value can be defined as being of a **specific data type**. XML Schema has built-in data types than can be used or one can define one's own data type.
  - the value can be **restricted** to match a pattern. One can specify that the value must e.g. be an integer between 0 and 10, or a string exactly 5 characters long.
- **complex elements**
  - **contain** other **elements** and/or **have attributes**.
  - can be:
    - **empty elements**
    - elements only containing other elements
    - elements only containing a value
    - elements containing both other elements and value(s)

As our root element `cats` contains other elements (`cat`) it must be a complex element.

```xml  
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Now we need to indicate in which fashion the child elements can appear:

- `<xs:all>` means that the children can appear in any order, but each only once. That's no good for our example as we have several cats, so `<cats>` must be repeatable.
- `<xs:choice>` means that either one child or another can occur. Again not good for our example.
- `<xs:sequence>` means that the children must occur in the order given, but there is no limitation on occurrences. That'll work for our example, so let's go with this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

To define the `cat` element we use the same tags and attribute at for `cats`, just swap the attribute value to 'cat'. `cat` also needs to be a complex type as it contains other elements (`name`, `age`, `breed`, and `colour`) and the `<xs:sequence>` indicator fits best again too.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat">
                    <xs:complexType>
                        <xs:sequence>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

The child elements of `cat` only contain values, so they can be simple elements. Unless there need to be restrictions defined for a simple element the entire element definition can be expressed in one line giving the name of the element and its data type:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer"/>
                            <xs:element name="breed" type="xs:string"/>
                            <xs:element name="colour" type="xs:string"/>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## XML Schema data types

As mentioned before XML Schema has built-in data types. Common ones are:

- `xs:string`
- `xs:normalizedString` (remove line feeds, carriage returns, and tab characters)
- `xs:integer`
- `xs:boolean`
- `xs:decimal`
- `xs:date` (2026-05-17, 2026-05-17Z -> UTC, 2026-05-07+06:00 -> UTC + 6hrs)
- `xs:time` (10:30:00, 10:30:00Z, 10:30:00-05:00 -> UTC - 5hrs)
- `xs:dateTime` (2026-05-17T10:30:00)

## Defining attributes `xs:attribute`

Attribute definitions follow the element definitions (they are also not part of the `xs:sequence`, `xs:all` or `xs:choice` block) and use the `xs:attribute` element for their definition. Attribute name and data type use the same syntax as elements.

Our example has the cat IDs as an attribute, so let's add that:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer"/>
                            <xs:element name="breed" type="xs:string"/>
                            <xs:element name="colour" type="xs:string"/>
                        </xs:sequence>
                            <xs:attribute name="id", type="xs:integer"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## Occurrence

So far so good, but if we look back at the meaning of `xs:sequence` it means that `cat` can only occur once. That's no good as we have several cats. To fix this we can use the attributes `minOccurs` and `maxOccurs` with element definitions. 

By **default** the value of  `minOccurs`  and `maxOccurs` is `1`. So if neither are defined the element must occur **exactly once**.

If you set `minOccurs` to `0`, the element becomes **optional**, as it's now ok for it to not be there at all. 

For our example, we can't tell how many `cat` elements might be added, so we can use `unbounded` as the value for `maxOccurs`, which means there is **no upper limit** to how often the element can occur. We leave `minOccurs` at `1` by not mentioning it at all. 

For `name` exactly one occurrence seems fine too. Every cat should have a name, but we may not know their age or breed. So let's make those two optional by setting `minOccur`s to `0`.

`colour` should also always be present, but it can also have multiple occurrences. `unbounded` seems a bit over the top here though, there are limited fur colour categories. Let's limit this to 4. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer" minOccurs="0"/>
                            <xs:element name="breed" type="xs:string" minOccurs="0"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                            <xs:attribute name="id", type="xs:integer"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

A similar construct exists for **attributes**, though here the choice is only between **optional** and **required** and the attribute is `use`. The default, i.e. if `use` is not specified, is optional. In our example it would be sensible to require an id, so let's implement that:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer" minOccurs="0"/>
                            <xs:element name="breed" type="xs:string" minOccurs="0"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                            <xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## Default and fixed values

Both **elements** and **attributes** can be assigned **default values** and **fixed values**. Default values are automatically assigned unless another value is specified. Fixed values mean the respective element or attribute value must be this. 

Let's say most cats we encounter and add to our example are moggies. It would make sense to set the default value of `breed` to `moggy` so we don't have to enter it all the time. The syntax is the same for elements and attributes.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer" minOccurs="0"/>
                            <xs:element name="breed" type="xs:string" minOccurs="0" default="moggy"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                            <xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

The syntax to declare a fixed value is `fixed="value"` for both elements and attributes.

## Restrictions

There is a range of restrictions that can be applied to **simple elements**. Which are possible and make sense depends on the data type of the element. The two we'll cover here are **numerical** restrictions and **enumerations**.

We already set the value of `age` to be in integer, but it would also make sense to restrict the range this can be as well. According to Wikipedia the oldest cat ever got to 38 years and 3 days[^25]. Giving a bit of leeway, let's set an upper limit of 40 and lower limit of 0 for the `age` element.  To do this we need to expand the simple element declaration into several lines:

- the element name and occurrence stays with the `<xs:element>` element, but it now needs a closing tag  (`</xs:element>`) as we'll add child elements to it containing the restriction,
- add a child element to it with the tag `xs:simpleType`,
- add a child to `xs:simpleType` with the tag `xs:restriction`. This is where the data type will live now, but the attribute is now called `base`. The data type is the basis of the restriction. 
- add a child with the tag `xs:minInclusive` and another with the tag `xs:maxInclusive` to the `<xs:restriction>` tag and set the `value` attribute for `xs:minInclusive` to `0` and for `xs:maxInclusive` to `40`. (Yes, `minExclusive` and `maxExclusive` are also options.)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" minOccurs="0">
                                <xs:simpleType>
                                    <xs:restriction base="xs:integer">
                                        <xs:minInclusive value="0"/>
                                        <xs:maxInclusive value="40"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                            <xs:element name="breed" type="xs:string" minOccurs="0" default="moggy"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                            <xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

For the **enumeration** example we'll restrict the `colour` element to only accept a list of values. Values outside this list will fail validation.

As before we need to expand the simple element into several lines. The `base` attribute value this time is `xs:string` and the child elements of `<xs:restriction>` are all `<xs:enumeration>` elements:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" minOccurs="0">
                                <xs:simpleType>
                                    <xs:restriction base="xs:integer">
                                        <xs:minInclusive value="0"/>
                                        <xs:maxInclusive value="40"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                            <xs:element name="breed" type="xs:string" minOccurs="0" default="moggy"/>
                            <xs:element name="colour" maxOccurs="4">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:enumeration value="black"/>
                                        <xs:enumeration value="white"/>
                                        <xs:enumeration value="orange"/> <!-- Technically this is called red, but I like orange better :P -->
                                        <xs:enumeration value="blue"/>
                                        <xs:enumeration value="cream"/>
                                        <xs:enumeration value="lilac"/>
                                        <xs:enumeration value="chocolate"/>
                                        <xs:enumeration value="cinnamon"/>
                                        <xs:enumeration value="fawn"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                        </xs:sequence>
                            <xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

# Annotated bibliography and further reading

My default XML, DTD and XML Schema references are the **W3Schools' XML tutorials**:

- <a name= "xsd-w3s"></a>'XML Schema Tutorial' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/schema_intro.asp](https://www.w3schools.com/xml/schema_intro.asp) [Accessed: 24 May 2026]

There are a lot of other tutorials out there though. Ones I found useful before are the TutorialsPoint ones:

- 'XSD Tutorial' (no date) *TutorialsPoint*. Available at: [https://www.tutorialspoint.com/xsd/index.htm](https://www.tutorialspoint.com/xsd/index.htm) [Accessed: 24 May 2026]

[GeeksforGeeks](https://www.geeksforgeeks.org/) is sometimes useful as well and, of course the awesomeness that is [StackOverflow](https://stackoverflow.com/) (though the usual way of ending up there is googling something). 

There are loads of resources on YouTube as well, but I don't learn well that way so I don't have any recommendations. Sorry!

The **technical XML and XSD specifications**. These are on the barely readable end unless you are very techy (and in this case: what are you doing here?)

**XML:**

- <a name= "xml10"></a>*Extensible Markup Language (XML) 1.0* (2008) 5th ed. Available at: [https://www.w3.org/TR/2008/REC-xml-20081126/](https://www.w3.org/TR/2008/REC-xml-20081126/) [Accessed: 24 May 2026]
- <a name= "xml11"></a>*Extensible Markup Language (XML) 1.1* (2006) 2nd ed. Available at: [https://www.w3.org/TR/2006/REC-xml11-20060816](https://www.w3.org/TR/2006/REC-xml11-20060816) [Accessed: 24 May 2026]

**XML Schema:**

- *W3C XML Schema Definition Language (XSD) 1.1 Part 1: Structures* (2012) Available at: [https://www.w3.org/TR/2012/REC-xmlschema11-1-20120405/](https://www.w3.org/TR/2012/REC-xmlschema11-1-20120405/) [Accessed: 24 May 2026]
- *W3C XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes* (2012) Available at: [https://www.w3.org/TR/2012/REC-xmlschema11-2-20120405/](https://www.w3.org/TR/2012/REC-xmlschema11-2-20120405/) [Accessed: 24 May 2026]

Other sources related to XML, DTDs and XML Schema I used for putting the workshop together:

- <a name= "kjhughes-2020"></a>kjhughes (2020) *Response to: What does elementFormDefault do in XSD?* Available at: [https://stackoverflow.com/a/46758277](https://stackoverflow.com/a/46758277) [Accessed: 24 May 2026]
- <a name= "markup"></a>'Markup language' (2026) *Wikipedia*. Available at: https://en.wikipedia.org/wiki/Markup_language [Accessed: 24 May 2026]
- <a name= "xml-wikidpedia"></a>'XML' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XML](https://en.wikipedia.org/wiki/XML) [Accessed: 24 May 2026]
- <a name= "xml-core-wg"></a>*XML Core Working Group Public Page* (2017) Available at: [https://www.w3.org/XML/Core](https://www.w3.org/XML/Core) [Accessed: 24 May 2026]
- <a name= "dtd-w3s-2"></a>'XML DTD' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_dtd.asp](https://www.w3schools.com/xml/xml_dtd.asp) [Accessed: 24 May 2026]
- <a name= "xml-in-10-points"></a>*XML in 10 points* (2014) Available at [https://www.w3.org/XML/1999/XML-in-10-points.html](https://www.w3.org/XML/1999/XML-in-10-points.html) [Accessed: 24 May 2026]
- <a name= "xsd-w3s-2"></a>'XML Schema' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_schema.asp](https://www.w3schools.com/xml/xml_schema.asp) [Accessed: 24 May 2026]
- <a name= "xsd-wikipedia"></a>'XML Schema (W3C)' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XML_Schema_(W3C)](https://en.wikipedia.org/wiki/XML_Schema_(W3C)) [Accessed: 24 May 2026]

[^13]: The information in this section has been, unless stated otherwise, compiled using the following sources: ['XML Schema Tutorial', no date](#xsd-w3s), ['XML Schema', no date](#xsd-w3s-2), ['XML Schema (W3C)', 2026](#xsd-wikipedia)
[^17]: What these two do goes much deeper than the scope of this lesson. If you want to know more see e.g. this [thread on StackOverflow](#kjhughes-2020).
---
layout: single_content
title: "6 DTD"
permalink: /foundations/dtd/
toc: true
toc_sticky: true
author_profile: true
date: 2026-06-14 12:00:00
description: "An introduction to DTD."
categories:
  - XML
  - data formats
read_time: true
sidebar:
  - title: "DTD"
    image: "/assets/images/dtd.png"
    image_alt: 
header:
  teaser: "/assets/images/dtd_teaser.png"
---

**Context**: This was written as accompanying material for the [MDG Library Carpentry workshop 2026 in Birmingham](/workshops/cilip_mdg_lc_bham_2026_05/) and is meant to be read in conjunction with [XML](/foundations/xml/) and [XML Schema](/foundations/xsd/) as the exerises build on each other.
{: .notice--primary}

DTD[^12] is a language to define rules for [XML](/foundations/XML/) documents such as the tag names that can be used, in which order elements are used, how often they can occur, which kind of value they contain and vice versa for attributes. It defines the structure of an XML document.

DTD is less flexible and has less features than [XML Schema](/foundations/xsd/), another way of writing rules for XML documents and validating them. There are also security concerns around the use of DTD. If you have a choice XML Schema is preferred.

DTD stands for **D**ocument **T**ype **D**efinition. Unlike XML Schema it can live inside the XML document itself rather than be a separate file (though that is also possible); XML Schemas are always separate from the XML documents they define.

# DTD syntax and use

Let's say the following is the DTD file `cats.dtd`:

```dtd
<!DOCTYPE cats
[
<!ELEMENT cat (name, age, colour+)>
<!ATTLIST cat id ID #REQUIRED>
<!ATTLIST cat colour (black|white|orange|blue) "black">
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT breed (#PCDATA)>
]
>
```

To reference it and validate our XML document with it we put the following into the XML:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE cats SYSTEM "cats.dtd">
<cats>
    <cat id="1">
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    ...
</cats>
```

In this case the XML document and the `cats.dtd` file live in the same place, so it's ok to give just the name. If the DTD file is elsewhere you'll need to give the path or web address it lives at.

## Doctype declaration

`!DOCTYPE elementName` declare the root element name followed by a list `[ ]` of its elements and attributes. It is opened by `<` and closed by `>`.

## Element declarations

`!ELEMENT elementName elementCategory` or `!ELEMENT elementName (elementContent)` declares an element. 

`<!ELEMENT cat (name, age, colour+)>` declares the element `cat` which has the child elements `name`, `age` and `colour`. The `+` after `colour` is a quantifiers that means that each `cat` must have at least one `colour` child element. As there is no symbol behind `name` and `age` this means they must occur exactly once in each `cat` element. The child elements must also occur within the `cat` element in the order given. There are other quantifiers apart from `+`:

| Quantifier | Meaning                  |
| ---------- | ------------------------ |
| +          | at least one occurrence  |
| *          | zero or more occurrences |
| ?          | zero or one occurrence   |


We'll meet these quantifiers again when talking about regular expressions. They are pretty universal and worth remembering.{: .notice--info}

One can also declare that one or more of a selection of child elements must occur. `<!ELEMENT note (from, to (message|greeting))>` means that the element `note` must have exactly one child element `from`, exactly one child element `to` and either a child element `message` or `greeting`, but not both. 

`|` (pipe) meaning "or" is also a good one to remember and one we'll meet again in regular expressions.{: .notice--info}

`<!ELEMENT break EMPTY>` declares that the element `break` is an empty element that cannot have a value or child elements.

`<!ELEMENT name (#PCDATA)>` declares that the element `name` has content, but no child elements. `PCDATA` stands for Parsed Character Data. 

You might see constructs like `<[!CDATA[some text here]]>` in XML files. `CDATA` is the opposite of `PCDATA`: it is data that could be interpreted as markup, but should not be. The parser doesn't check if there are any characters in there that would signal the start of a new element etc., it just takes the data as literal.{: .notice--info}

`<!ELEMENT cat ANY>` declared that the element `cat` can have a value, or child elements or both; basically anything that is parseable is possible.

`<!ELEMENT name (#PCDATA|firstname|lastname|title)*>` declared that the element `name` must have at least one of the following: a value (`PCDATA`), a child element `firstname`, a child element `lastname` or a child element `title`.

## Attribute declarations

The pattern for attribute declarations is `<!ATTLIST elementName attributeName attributeType attributeValue>`

`<!ATTLIST cat id ID #REQUIRED>` declares that the element `cat` has the attribute `id` which is of data type `ID` and must be given.

`<!ATTLIST cat breed (moggy|Korat|Munchkin) "moggy">` declares that the element `cat` has the attribute `breed` which must have one of the values `moggy`, `Korat` or `Munchkin` and the default value is `moggy`.

`<!ATTLIST cat breed CDATA #IMPLIED>` declares that the element `cat` has the attribute `breed`, which is optional (``#IMPLIED``). 

`<!ATTLIST cat hungry CDATA "yes">` declares that the element `cat` has the attribute `hungry` with the default value "yes". 

`<!ATTLIST cat hungry CDATA #FIXED "yes">` declares that the element `cat` has the attribute `hungry` which must always be (is fixed) "yes". 

## Embedding the DTD in the XML document

The example above is for a DTD in a separate file. DTDs can be embedded into the XML file directly like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE cats [
<!ELEMENT cat (name, age, colour+)>
<!ATTLIST cat id ID #REQUIRED>
<!ATTLIST cat colour (black|white|orange|blue) "black">
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT breed (#PCDATA)>
]>
<cats>
    <cat id="1">
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    ...
</cats>
```

## DTD disadvantages and further reading

DTD is an old way of doing things that has some distinct disadvantages over XML Schema:

- it does not support namespaces (we'll get to those in a minute),
- is a bit of a security risk (DOS attacks via exponentially expanding nested entities or providing the parser with an external resource that doesn't return),
- it knows only one data type: string,
- it lack expressiveness and readability,
- it uses regular expression syntax (remember `+`, `*`, `?` and `|`?), which makes it hard to parse.

There is more to it then what's covered above, if you want to know more, W3Schools has a pretty [accessible tutorial](#dtd-w3s) and the [Wikipedia entry](#dtd-wikipedia) is pretty comprehensive too. The actual specification is part of the [XML 1.0 specification](#xml10). Be warned though! It's not easy to tease out and barely comprehensible, stick with the tutorials, seriously!

# Annotated bibliography and further reading

My default XML, DTD and XML Schema references are the **W3Schools' XML tutorials**:

- <a name= "dtd-w3s"></a>'DTD Tutorial' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_dtd_intro.asp](https://www.w3schools.com/xml/xml_dtd_intro.asp) [Accessed: 24 May 2026]

There are a lot of other tutorials out there though. Ones I found useful before are the TutorialsPoint ones:

- 'DTD Tutorial' (no date) *TutorialsPoint*. Available at: [https://www.tutorialspoint.com/dtd/index.htm](https://www.tutorialspoint.com/dtd/index.htm) [Accessed: 24 May 2026]

[GeeksforGeeks](https://www.geeksforgeeks.org/) is sometimes useful as well and, of course the awesomeness that is [StackOverflow](https://stackoverflow.com/) (though the usual way of ending up there is googling something). 

Specifically for **DTD**, the Wikipedia page is basically a tutorial too:

- <a name= "dtd-wikipedia"></a>'Document type definition' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Document_type_definition](https://en.wikipedia.org/wiki/Document_type_definition) [Accessed: 24 May 2026]

There are loads of resources on YouTube as well, but I don't learn well that way so I don't have any recommendations. Sorry!

The **technical XML and XSD specifications**. These are on the barely readable end unless you are very techy (and in this case: what are you doing here?)

- <a name= "xml10"></a>*Extensible Markup Language (XML) 1.0* (2008) 5th ed. Available at: [https://www.w3.org/TR/2008/REC-xml-20081126/](https://www.w3.org/TR/2008/REC-xml-20081126/) [Accessed: 24 May 2026]
- <a name= "xml11"></a>*Extensible Markup Language (XML) 1.1* (2006) 2nd ed. Available at: [https://www.w3.org/TR/2006/REC-xml11-20060816](https://www.w3.org/TR/2006/REC-xml11-20060816) [Accessed: 24 May 2026]

Other sources related to DTDs I used for putting the workshop together:

- <a name= "dtd-w3s-2"></a>'XML DTD' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_dtd.asp](https://www.w3schools.com/xml/xml_dtd.asp) 

[^12]: The information in this section has been, unless stated otherwise, compiled using the following sources: ['DTD Tutorial', no date](#dtd-w3s), ['Document type definition', 2026](#dtd-wikipedia), [*Extensible Markup Language (XML) 1.0*, 2008](#xml10), ['XML DTD', no date](#dtd-w3s-2)
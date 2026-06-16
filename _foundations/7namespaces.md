---
layout: single_content
title: "7 XML Namespaces"
permalink: /foundations/namespaces/
toc: true
toc_sticky: true
author_profile: true
date: 2026-06-14 12:00:00
description: "An introduction to XML namespaces."
categories:
  - XML
  - data formats
read_time: true
sidebar:
  - title: "Namespaces"
    image: "/assets/images/namespace.png"
    image_alt: 
header:
  teaser: "/assets/images/namespace_teaser.png"
---

**Context**: This was written as accompanying material for the [MDG Library Carpentry workshop 2026 in Birmingham](/workshops/cilip_mdg_lc_bham_2026_05/). It is meant to be read in conjunction with [XML Schema](/foundations/xsd/) and [XML](/foundations/xml/).
{: .notice--primary}

# What is a namespace?

Namespaces[^14] are **prefixes** that keep **identifiers** from different schemas **apart** and thus **prevent name conflicts** and **ensure identifiers are unique**. 

These **prefixes are URIs** (**U**niform **R**esource **I**dentifier) and commonly of the URL (**U**niform **R**esource **L**ocator) subset, i.e. a **web address**. Web addresses are unique: every URL points to a single location, it cannot point to more than one location. Thus using them as a prefix means the identifiers (tags in this case) they are prefixing become unique. 

The **URI** used for this **does not need to be real**, its sole purpose is to give the namespace a unique name. It can be entirely fictional. However, for many schemas the URI is used and functional and contains information about the schema.

If you're using an XML Schema someone else has put together, the URI to use will be stated in the XML Schema Definition file in its root tag `<xs:schema>`'s `targetNamespace` attribute. 

For example, if one wanted to use elements defined in [MARCXML](https://www.loc.gov/standards/marcxml/) to record the favourite book of each cat, the namespace to use is [http://www.loc.gov/MARC21/slim](http://www.loc.gov/MARC21/slim). It's pretty cumbersome to prepend this every time and there is still no pointer to the schema that defines MARCXML and would allow validation:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats>
    <cat>
        ...
        <favouriteBook>
            <http://www.loc.gov/MARC21/slim/record>
                <http://www.loc.gov/MARC21/slim/datafield ind1="1" ind2="4" tag="245">
                    <http://www.loc.gov/MARC21/slim/subfield code="a">The hobbit, or, There and back again /</http://www.loc.gov/MARC21/slim/subfield>
                    <http://www.loc.gov/MARC21/slim/subfield code="c">by J.R.R. Tolkien ; illustrations by the author.</http://www.loc.gov/MARC21/slim/subfield>
                </http://www.loc.gov/MARC21/slim/datafield>
            </http://www.loc.gov/MARC21/slim/record>
        </favouriteBook>
    </cat>
</cats>

```

# Prefixed namespaces, qualified names

Instead of writing it out every time, one defines a **shorthand for the namespace** that is used instead of the URI itself. When declaring an element this shorthand and `:` are prepended to the element name. E.g. if we define that the shorthand for `http://www.loc.gov/MARC21/slim` is `marc` we can now declare a record element as `<marc:record></marc:record>`.

**Both** the **shorthand** and the **actual namespace**, i.e. the URI, are **referred to as "namespace"**. 

An element that uses a so **prefixed** tag name is a **qualified element**. And the prefixed **tag name** is referred to as a **qualified name** or **QName**. 
{: .notice--success}

# The default namespace, unqualified names

Besides these **prefixed namespaces** there can also be a **default namespace**. The default namespace can be bound to a URI, but the tag names in it have **no shorthand prefix**.[^15]

They are thus referred to as **unqualified names**. 
{: .notice--success}

# Defining namespaces and linking XSD files for validation

The namespace and shorthand definition is usually done in the root element or the highest parent element whose children need to use the respective namespace. 

To **define the default namespace** add `xmlns="URI"` as an attribute to the root element or respective parent. E.g. to declare that the default namespace is `http://www.example.com` you would write `xmlns="http://www.example.com"`.

To **define a prefixed namespace** add `xmlns:prefix="URI"` to the root element or respective parent. E.g. to declare the namespace `http://www.loc.gov/MARC21/slim` with the prefix `marc` you would write `xmlns:marc="http://www.loc.gov/MARC21/slim"`.

At the same time one also sets the **pointer to the XML Schema file** defining the respective element(s) in the namespace. To do this we need an attribute that is defined in the XML Schema Instance schema, namely `schemaLocation`. Thus we need to define a prefixed namespace for it before we can use it: `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`.

`xsi` is the prefix conventionally used to reference the XML Schema Instance. Technically one can use whatever prefix one wants, but over time conventions have been established for often used schemas/vocabularies.
{: .notice--info}

Now we can point to both the XSD file for our default namespace and the `marc` namespace using the `xsi:schemaLocation` attribute. Each "pointer" has two components:

- the respective namespace **URI**, and
- the **location** of the XSD file. This can be a URL as well, or a path to the respective file.

They are **separated** by a **space** from **each other** as well as the **next "pointer"** (if more than one XSD needs to be referenced).

`xsi:schemaLocation="https://www.example.com cats.xsd http://www.loc.gov/MARC21/slim https://www.loc.gov/standards/marcxml/schema/MARC21slim.xsd"`

Putting this all together for our example we end up with:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats xmlns="http://www.example.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:marc="http://www.loc.gov/MARC21/slim" xsi:schemaLocation="https://www.example.com cats.xsd http://www.loc.gov/MARC21/slim https://www.loc.gov/standards/marcxml/schema/MARC21slim.xsd">
<cat>
        ...
        <favouriteBook>
            <marc:record>
                <marc:datafield ind1="1" ind2="4" tag="245">
                    <marc:subfield code="a">The hobbit, or, There and back again /</marc:subfield>
                    <marc:subfield code="c">by J.R.R. Tolkien ; illustrations by the author.</marc:subfield>
                </marc:datafield>
            </marc:record>
        </favouriteBook>
    </cat>
</cats>
```

You might be wondering why we pointed to XSD files for the default namespace and `marc` but not `xsi`. The reason is that the pointer to the XSD for `xsi` is built-in. As long as you use `xsi` as the prefix, there is no need to point to the schema file for it.

# Annotated bibliography and further reading

- <a name= "mckamey-2019"></a>mckamey (2019) *XML Default namespaces for unqualified attribute names?* Available at: [https://stackoverflow.com/q/3312390](https://stackoverflow.com/q/3312390) [Accessed: 24 May 2026]

- <a name= "ns-wikipedia"></a>'Namespace' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Namespace](https://en.wikipedia.org/wiki/Namespace) [Accessed: 24 May 2026] 

  *This is reasonable accessible, but not specific to XML.*

- <a name= "ns10"></a>*Namespaces in XML 1.0* (2009) 3rd ed. Available at: https://www.w3.org/TR/2009/REC-xml-names-20091208/[](https://www.w3.org/TR/2009/REC-xml-names-20091208/) [Accessed: 24 May 2026]

  *This is the W3C technical specification, so pretty hard reading.*

- <a name= "ns11"></a>*Namespaces in XML 1.1* (2006) 2nd ed. Available at: [https://www.w3.org/TR/2006/REC-xml-names11-20060816](https://www.w3.org/TR/2006/REC-xml-names11-20060816) [Accessed: 24 May 2026]

  *This is the W3C technical specification for namespaces in XML 1.1, so pretty hard reading.*

- <a name= "xmlns-wikipedia"></a>'XML Namespace' (2025) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XML_namespace](https://en.wikipedia.org/wiki/XML_namespace) [Accessed: 24 May 2026]

  *This is reasonably accessible.*

- <a name= "ns-w3s"></a>'XML Namespaces' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_namespaces.asp](https://www.w3schools.com/xml/xml_namespaces.asp) [Accessed: 24 May 2026]

  *This is reasonably accessible.*

[^14]: The information in this section has been, unless stated otherwise, compiled using the following sources: ['Namespace', 2026](#ns-wikipedia), [*Namespaces in XML 1.0*, 2009](#ns10), ['XML Namespace', 2025](#xmlns-wikipedia), ['XML Namespaces', no date](#ns-w3s)
[^15]: The default namespace only works for elements. Namespaces for attributes work slightly differently. If you use an attribute that belong as per the schema to the respective element there is no need to prefix it, it is in this element's context. If you want to use an attribute from a different namespace though (as we e.g. do with `xsi:schemaLocation`) you need to prefix it. This [thread on StackOverflow](#mckamey-2019) has a good explanation of this.
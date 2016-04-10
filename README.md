# XML DTD XSD XSL in 10 minutes

## Table of contents

- [Introduction](#introduction)
- [XML](#xml-1)
- [DTD](#dtd-1)
- [XSD](#xsd-1)
- [XSL](#xsl-1)

## Introduction

### XML

- XML stands for eXtensible Markup Language
- XML is a markup language like HTML
- XML is designed to carry data
- XML simplifies data sharing
- XML tags are not predefined
- XML is designed to be self-descriptive
- XML is a W3C recommendation
- XML is plain text

### XML and HTML
- XML is not a replacement for HTML
- XML and HTML were designed with different goals (XML to transport and store data, HTML to display data)

### DTD

Document Type Definition (DTD) are a set of rules which a specific XML document has to conform to.

### XSD

XML Schema Definition (XSD) is used to define the elements of an XML document, just like DTD.

Defines the following:
- Elements that can appear in a document
- Attributes that can appear in a document
- Child elements
- Order of child elements
- Number of child elements
- Empty elements or not
- Data type for elements and attributes
- Default and fixed values for elements

XSD is the successor of DTD:

- Extensible future additions
- Richer
- Written in XML
- Support data types
- Support namespaces

### XSL

EXtensible Stylesheet Language (XSL) is a style sheet language for XML documents.

Describes how to XML document should be displayed.

Consists of:

- XSLT (a language that transforms XML documents)
- XPath (a language for defining parts of an XML document)
- XSL-FO (XSL Formatting Objects)

It can:
- Transform XML in XHTML
- Filter and sort XML data
- Define parts of an XML documents
- Format XML data based on its values
- Output XML data

## XML

### Example

Content example:

```XML
<message>
    <from>Ben</from>
    <to>Lara</to>
    <subject>Hello</subject>
    <body>Hey there Lara!</body>
</message>
```

### Declaration

```
<?xml version="1.0" encoding="UTF-16" standalone="yes"?>
```

or simply

```
<?xml version="1.0"?>
```

**Encodings:** UTF-8, UTF-16, ISO-8859-1, ...

### Root element

XML documents always have to start with a root element, that can have as many other elements inside it.

```XML
<?xml version="1.0"?>
<journeys>
    <journey>
        ...
    </journey>
    <journey>
        ...
    </journey>
</journeys>
```

### Attributes

XML elements can have attributes which can contain extra data, normally to [convey METADATA](http://stackoverflow.com/questions/1096797/should-i-use-elements-or-attributes-in-xml).

```XML
<?xml version="1.0"?>
<books>
  <book id="100">
    <title>Book name</title>
  </book>
</books>
```

### Special characters

Some characters have a special meaning in XML. Thus, if you want them to be displayed as plain text you will need to escape them by using entity references.

- ```&lt;``` less than
- ```&gt;``` greater than
- ```&amp;``` ampersand
- ```&apos;``` apostrophe
- ```&quot;``` quotation mark

### Comments
Same as HTML.

```HTML
<!-- This is a comment. -->
```

### Spaces

XML, unlike HTML, preserves multiple spaces.

### Other syntax

- XML tags are case sensitive
- XML elements have to be properly nested (parent always starting before children and ending after children)
- Attributes always have to be quoted

## DTD

### Declaration

#### DTD in an external file (recommended):

On XML file (books.xml):

```
<?xml version="1.0"?>
<!DOCTYPE books SYSTEM "books.dtd">
```

On DTD file (books.dtd):

_DTD file has no declaration._

#### DTD in the same XML file:

```
<?xml version="1.0"?>
<!DOCTYPE books [
 ... (dtd) ...
]>
... (xml) ...
```

### Example

books.xml

```XML
<?xml version="1.0"?>
<books>
  <book id="100">
    <title>Book name</title>
    <publish_date>2015-01-01</publish_date>
    <author>
      <name>Ben</name>
      <age>40</age>
    </author>
  </book>
</books>
```

books.dtd

```DTD
<!ELEMENT books (book*)>

<!ELEMENT book (name, publish_date, author)>
  <!ATTLIST book id CDATA #REQUIRED>
  <!ELEMENT name (#PCDATA)>
  <!ELEMENT publish_data (#PCDATA)>
  <!ELEMENT author (name, age)>
    <!ELEMENT name (#PCDATA)>
    <!ELEMENT age (#PCDATA)>
```

### Terminology

**!DOCTYPE books** defines that the root element is books.

**!ELEMENT** defines an ELEMENT.<br>
First param: indicates the element name<br>
Second param: what it contains (which can be a value or another element).
- **(book*)** 0 or more books
- **(book+)** 1 or more books
- **(#PCDATA)** text that will be parsed by a parser*
- **(#CDATA)** text that will not be parsed by a parser*

**!ATTLIST** defines an attribute.<br>
First param: element name<br>
Second param: attribute name<br>
Third param: type of attribute value
- **CDATA** text that will not be parse by a parser*

Fourth param: attribute value
- **value** default value of the attribute
- **#REQUIRED** attribute is required
- **#IMPLIED** attribute is optional
- **#FIXED value** attribute value is fixed

\* [Difference between PCDATA and CDATA](http://stackoverflow.com/questions/918450/difference-between-pcdata-and-cdata-in-dtd)

### Validation

XML documents can be validated with **xmllint** command-line tool.

```
xmllint --valid books.xml --noout
```

## XSD

### Declaration

On XML file (books.xml):

```
<?xml version="1.0"?>
<books xmlns="http://bookstore.example.com"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://bookstore.example.com books.xsd">
```

**xmlns**: namespace<br>
**xmlns:xsi**: where elements and data type come from<br>
**xsi:schemaLocation**: schema location. Can also be a relative URL.

On XSD file (books.xsd):

```
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://bookstore.example.com"
           xmlns="http://bookstore.example.com"
           elementFormDefault="qualified">
```

No namespace is possible as well:

```
xsi:noNamespaceSchemaLocation="books.xsd"
```

### Example

books.xml

```XML
<?xml version="1.0"?>
<books xmlns="http://bookstore.example.com"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://bookstore.example.com books.xsd">
  <book id="100">
    <title>Book name</title>
    <publish_date>2015-01-01</publish_date>
    <author>
      <name>Ben</name>
      <age>40</age>
    </author>
  </book>
</books>
```

books.xsd

```XML
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://bookstore.example.com"
           xmlns="http://bookstore.example.com"
           elementFormDefault="qualified">
<xs:element name="books">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="title" type="xs:string" />
      <xs:element name="publish_date" type="xs:date" />
      <xs:element name="author">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="name" type="xs:string" />
            <xs:element name="age" type="xs:integer" />
        </xs:complexType>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

### Terminology

**xs:element**: xml element
**xs:complexType**: contains other elements
**xs:sequence**: elements must appear in a sequence

### Data types

Most common data types:
- string
- decimal
- integer
- boolean
- date
- time

### Validate

XML documents can be validated with **xmllint** command-line tool.

```
xmllint --schema books.xsd books.xml --noout
```

## XSL

### Declaration

On XML file (books.xml):

```
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="books.xsl"?>
```

On XSL file (books.xsl):

```
<?xml version="1.0"?>
<xsl:stylesheet version="1.0"
                xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:template match="/">
  ...
</xsl:template>
</xsl:stylesheet>
```

### Example

books.xml

```XML
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="books.xsl"?>
<books>
  <book id="100">
    <title>Book name</title>
    <publish_date>2015-01-01</publish_date>
    <author>
      <name>Ben</name>
      <age>40</age>
    </author>
  </book>
</books>
```

books.xsl

```XML
<?xml version="1.0"?>
<xsl:stylesheet version="1.0"
                xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:template match="/">
  <html>
  <body>
    <h1>Books</h1>
    <table>
      <tr>
        <td>Title</td>
        <td>Publish date</td>
        <td>Author</td>
      </tr>
      <xsl:for-each select="books/book">
        <tr>
          <td width="20%"><xsl:value-of select="title"/></td>
          <td width="20%"><xsl:value-of select="publish_date"/></td>
          <td width="20%"><xsl:value-of select="author/name" />
            (age <xsl:value-of select="author/age" />)
          </td>
        </tr>
      </xsl:for-each>
    </table>
  </body>
  </html>
</xsl:template>
</xsl:stylesheet>
```

![XSL output on browser](https://raw.githubusercontent.com/zurfyx/XML-in-10-minutes/master/assets/books_xsl.png)

### Filter

XML output can be filter by adding a select attribute, such as:

```
<xsl:for-each select="books/book[title='Book title']">
```

#### Operators
- ```=``` (equal)
- ```!=``` (not equal)
- ```&lt;``` (less than)
- ```&gt;``` (greater than)

### Sort

```
<xsl:for-each select="books/book">
<xsl:sort data-type="text" order="descending" select="title"/>
```

### Conditional

```
<xsl:if test="age &gt; 10 and age &lt; 20">
  Teenager
</xsl:if>
```

#### Operators

- or
- and

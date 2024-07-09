---
layout: post
title:  "ASN1 and parsing ber"
date:   2024-07-09 19:50:02 +0530
tags: asn1 ber der
categories: code
published: true
---

Let's start with a problem. You want to build a communication protocol for your new and interesting application. What is the most basic thing you need to get started?

A description of the said protocol. What's the data you will send, how will it look what's the sequence etc. Now you can do this in plain english using maybe bullet lists. You can even use use class diagrams. What if I told you there exists a description language which is perfect for your usecase. A language to describe data, without specifying how it will be reprecented.

The language in question is - ASN.1 - a.k.a - Abstract Syntax Notation ver 1.

ASN.1 is at it's core a very simple to read and write description language that look very familiar if you know c-style structs. A trivial example - 
```asn.1
Student ::= SEQUENCE {
    id INTEGER,
    name UTF8String,
    age INTEGER,
    courses SEQUENCE OF Courses
}
```
That's all. This describes a sequence of data, where first item would be the id, the next name and so on. There is no mention of encoding, how is it represented. Is it a human readable JSON, or less readable but tedious XML, or something more exotic and binary - BER?

That's the beauty of it. The following JSON is a valid object based on the asn.1 description above - 
```json
{
    "id": 12345,
    "name": "John Doe",
    "age": 20,
    "courses": []
}
```
Or the XML - 
```xml
<Student>
    <id>12345</id>
    <name>John Doe</name>
    <age>20</age>
    <courses>
    </courses>
</Student>
```

There is a whole lot of rules, types, construction of type etc for asn.1 which you can read from the original spec - https://www.rfc-editor.org/rfc/rfc6025 or any other references like - https://www.oss.com/asn1/resources/asn1-made-simple/asn1-quick-reference.html which is definitely worth a visit if you are interested.

## BER - Basic Encoding Rules
The next problem for your communication protocol is sending your data accross clients. This process of converting data to transferable form and to usable form is called Serialization and Deserialization.

BER is a type of binary serialization format typically used heaviliy in networking. You can use BER to encode data described by ASN.1. The best way to think about it, think of a JSON and it's stringified version e.g. `{'name':'Jack'}`. This is not the object itself, but rather a simple string which represents the object. You can then `parse` it to get an objet out of it.

BER is very similar except it's serialized format is not human readable. The BER serialization would look something like this - 
```
0x14 0x56 0x67 0x56 0x19 (TODO, put actual value)
```

While this looks random and unreadble, there are some very reasonable rules and logic at play here. BER follows Tag-Length-Value pattern throughout the payload. While means the starting byte(s) will be a 'header tag' which talks about what the element is, whether it's an `INTEGER` a `UTF8STRING` or a composite type like `SEQUENCE`. This is followed by some bytes representing the length of the element. And finally the actual content of the element. The element might be raw data if it's a primitive type like integer. Or it could be another BER element that can further be divided into TAG-LENGTH-ELEMENT.

The nitty-gritty details of BER and it's grammer is rather complex for a single blog post. Also you most likely won't ever need to know that either unless you are planning on builing a parser of your own. However in the interest of completion you can find out more about it here in this awesome let's encrypt article - https://letsencrypt.org/docs/a-warm-welcome-to-asn1-and-der.

Or if you are brave enough here is the official specification - https://www.itu.int/rec/T-REC-X.690-202102-I/en

### Distinguished Encoding Rules - DER
DER is a spin off of BER. It's a more strict subset of DER, which makes it both easier to parse and does away with the biggest drawback of BER. There is only one way to encoding data in DER. 

Unlike DER, when using BER, same data can be represented in different bits, making it harder to handle and maintain. Essentially it's impossible to write a generic parser for BER because of it. You will always need to schema to relibly parser BER.

## Why should you care about BER and ASN.1?
Chances are you always interact with it in your digital life, the SSL/ TLS certificates? They are DER (Distinguished Encoding Rules) encoded. Their spec is defined using ASN.1. 

The E2E encryption every application uses? Yep, protocol described in ASN.1. PGP, Digital Signatures, Encryption, all of it in BER and described using ASN.1.

## PER and OER
Packed Encoding Rules and 

## Parsing ASN.1 based binary formats
If you want to build an application that wants to use binary communication vs JSON. ASN.1 is a very good candidate. You can have efficient fast easy to parse protocols using ASN.1. 

THe problem is tooling. Every language in Earth supports JSON. You have a standard library and many 3rd party JSON parsers for almost all toolchain. Which is where ASN.1 becomes hard to justify.

However still if you want your microservices to talk to each other in fast binary, parsing friendly protocols without redundant schema attached to the data, then you can always use this great OSS library to build parsers for your data - https://github.com/vlm/asn1c

## ver1
- https://github.com/bpmbpm/mdld-test/blob/main/ver2/doc/easy/alice3d.md

# Пользовательская онтология v1

**Префикс:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#`
**Разделитель:** `#` (hash namespace)

**Назначение:** Онтология для описания персон, их адресов и увлечений. Используется в примерах семантической разметки.

## Классы

### Person

Человек или вымышленный персонаж.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#Person`
- **Эквивалент:** `schema:Person`, `foaf:Person`, `vcard:Individual`, `dbo:Person`, `prov:Person`
- **Комментарий:** Основной класс для описания людей. Может иметь имя, адрес, увлечения.

### PostalAddress

Почтовый адрес.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#PostalAddress`
- **Эквивалент:** `schema:PostalAddress`
- **Комментарий:** Класс для структурированного описания адреса: улица, город, индекс, страна.

### Hobby

Увлечение или хобби.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#Hobby`
- **Эквивалент:** `schema:Thing`
- **Комментарий:** Класс для описания интересов и увлечений персоны.

## Свойства

### name

Полное имя персоны.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#name`
- **Эквивалент:** `schema:name`, `foaf:name`
- **Domain:** `Person`
- **Range:** `xsd:string`
- **Комментарий:** Используется для указания человекочитаемого имени.

### hasAddress

Связывает персону с её почтовым адресом.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#hasAddress`
- **Эквивалент:** `schema:address`
- **Domain:** `Person`
- **Range:** `PostalAddress`
- **Комментарий:** Объектное свойство, указывающее на узел адреса.

### streetAddress

Название улицы и номер дома.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#streetAddress`
- **Эквивалент:** `schema:streetAddress`
- **Domain:** `PostalAddress`
- **Range:** `xsd:string`
- **Комментарий:** Конкретная часть адреса — улица и дом.

### addressLocality

Название города или населённого пункта.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#addressLocality`
- **Эквивалент:** `schema:addressLocality`
- **Domain:** `PostalAddress`
- **Range:** `xsd:string`
- **Комментарий:** Город, посёлок или другой населённый пункт.

### postalCode

Почтовый индекс.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#postalCode`
- **Эквивалент:** `schema:postalCode`
- **Domain:** `PostalAddress`
- **Range:** `xsd:string`
- **Комментарий:** Почтовый индекс в любой системе.

### addressCountry

Название страны.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#addressCountry`
- **Эквивалент:** `schema:addressCountry`
- **Domain:** `PostalAddress`
- **Range:** `xsd:string`
- **Комментарий:** Страна (или код страны) в адресе.

### hasHobby

Связывает персону с её увлечением.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#hasHobby`
- **Эквивалент:** `schema:knowsAbout`
- **Domain:** `Person`
- **Range:** `Hobby`
- **Комментарий:** Объектное свойство, указывающее на узел увлечения.

## Исходный код Turtle

```turtle
@prefix onto: <https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

onto:Person a owl:Class ;
    rdfs:label "Person"@en ;
    rdfs:comment "Человек или вымышленный персонаж."@ru ;
    owl:equivalentClass <http://schema.org/Person> ,
                        <http://xmlns.com/foaf/0.1/Person> ,
                        <http://www.w3.org/2006/vcard/ns#Individual> ,
                        <http://dbpedia.org/ontology/Person> ,
                        <http://www.w3.org/ns/prov#Person> .

onto:PostalAddress a owl:Class ;
    rdfs:label "PostalAddress"@en ;
    rdfs:comment "Почтовый адрес."@ru ;
    owl:equivalentClass <http://schema.org/PostalAddress> .

onto:Hobby a owl:Class ;
    rdfs:label "Hobby"@en ;
    rdfs:comment "Увлечение или хобби."@ru ;
    owl:equivalentClass <http://schema.org/Thing> .

onto:name a owl:DatatypeProperty ;
    rdfs:label "name"@en ;
    rdfs:comment "Полное имя персоны."@ru ;
    rdfs:domain onto:Person ;
    rdfs:range xsd:string ;
    owl:equivalentProperty <http://schema.org/name> ,
                           <http://xmlns.com/foaf/0.1/name> .

onto:hasAddress a owl:ObjectProperty ;
    rdfs:label "hasAddress"@en ;
    rdfs:comment "Связывает персону с её почтовым адресом."@ru ;
    rdfs:domain onto:Person ;
    rdfs:range onto:PostalAddress ;
    owl:equivalentProperty <http://schema.org/address> .

onto:streetAddress a owl:DatatypeProperty ;
    rdfs:label "streetAddress"@en ;
    rdfs:comment "Название улицы и номер дома."@ru ;
    rdfs:domain onto:PostalAddress ;
    rdfs:range xsd:string ;
    owl:equivalentProperty <http://schema.org/streetAddress> .

onto:addressLocality a owl:DatatypeProperty ;
    rdfs:label "addressLocality"@en ;
    rdfs:comment "Название города или населённого пункта."@ru ;
    rdfs:domain onto:PostalAddress ;
    rdfs:range xsd:string ;
    owl:equivalentProperty <http://schema.org/addressLocality> .

onto:postalCode a owl:DatatypeProperty ;
    rdfs:label "postalCode"@en ;
    rdfs:comment "Почтовый индекс."@ru ;
    rdfs:domain onto:PostalAddress ;
    rdfs:range xsd:string ;
    owl:equivalentProperty <http://schema.org/postalCode> .

onto:addressCountry a owl:DatatypeProperty ;
    rdfs:label "addressCountry"@en ;
    rdfs:comment "Название страны."@ru ;
    rdfs:domain onto:PostalAddress ;
    rdfs:range xsd:string ;
    owl:equivalentProperty <http://schema.org/addressCountry> .

onto:hasHobby a owl:ObjectProperty ;
    rdfs:label "hasHobby"@en ;
    rdfs:comment "Связывает персону с её увлечением."@ru ;
    rdfs:domain onto:Person ;
    rdfs:range onto:Hobby ;
    owl:equivalentProperty <http://schema.org/knowsAbout> .  

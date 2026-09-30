# Custom Ontology v1  

@prefix onto: <https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#> .  
@prefix rdf2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdf2.md#> .  
@prefix rdfs2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2.md#> .  
@prefix owl: <http://www.w3.org/2002/07/owl#> .  
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .  

# Classes  

## Person  

onto:Person rdf2:type owl:Class ;  
    rdfs2:label "Person"@en ;  
    rdfs2:label "Персона"@ru ;  
    rdfs2:comment "Человек или вымышленный персонаж."@ru ;  
    owl:equivalentClass <http://schema.org/Person> ,  
                        <http://xmlns.com/foaf/0.1/Person> ,  
                        <http://www.w3.org/2006/vcard/ns#Individual> ,  
                        <http://dbpedia.org/ontology/Person> ,  
                        <http://www.w3.org/ns/prov#Person> .  

## PostalAddress  

onto:PostalAddress rdf2:type owl:Class ;  
    rdfs2:label "PostalAddress"@en ;  
    rdfs2:label "Почтовый адрес"@ru ;  
    rdfs2:comment "Почтовый адрес."@ru ;  
    owl:equivalentClass <http://schema.org/PostalAddress> .  

## Hobby  

onto:Hobby rdf2:type owl:Class ;  
    rdfs2:label "Hobby"@en ;  
    rdfs2:label "Увлечение"@ru ;  
    rdfs2:comment "Увлечение или хобби."@ru ;  
    owl:equivalentClass <http://schema.org/Thing> .  

# Properties  

## hasAddress  

onto:hasAddress rdf2:type owl:ObjectProperty ;  
    rdfs2:label "hasAddress"@en ;  
    rdfs2:label "имеет адрес"@ru ;  
    rdfs2:comment "Связывает персону с её почтовым адресом."@ru ;  
    rdfs2:domain onto:Person ;  
    rdfs2:range onto:PostalAddress ;  
    owl:equivalentProperty <http://schema.org/address> .  

## streetAddress  

onto:streetAddress rdf2:type owl:DatatypeProperty ;  
    rdfs2:label "streetAddress"@en ;  
    rdfs2:label "улица"@ru ;  
    rdfs2:comment "Название улицы и номер дома."@ru ;  
    rdfs2:domain onto:PostalAddress ;  
    rdfs2:range xsd:string ;  
    owl:equivalentProperty <http://schema.org/streetAddress> .  

## addressLocality  

onto:addressLocality rdf2:type owl:DatatypeProperty ;  
    rdfs2:label "addressLocality"@en ;  
    rdfs2:label "город"@ru ;  
    rdfs2:comment "Название города или населённого пункта."@ru ;  
    rdfs2:domain onto:PostalAddress ;  
    rdfs2:range xsd:string ;  
    owl:equivalentProperty <http://schema.org/addressLocality> .  

## postalCode  

onto:postalCode rdf2:type owl:DatatypeProperty ;  
    rdfs2:label "postalCode"@en ;  
    rdfs2:label "почтовый индекс"@ru ;  
    rdfs2:comment "Почтовый индекс."@ru ;  
    rdfs2:domain onto:PostalAddress ;  
    rdfs2:range xsd:string ;  
    owl:equivalentProperty <http://schema.org/postalCode> .  

## addressCountry  

onto:addressCountry rdf2:type owl:DatatypeProperty ;  
    rdfs2:label "addressCountry"@en ;  
    rdfs2:label "страна"@ru ;  
    rdfs2:comment "Название страны."@ru ;  
    rdfs2:domain onto:PostalAddress ;  
    rdfs2:range xsd:string ;  
    owl:equivalentProperty <http://schema.org/addressCountry> .  

## hasHobby  

onto:hasHobby rdf2:type owl:ObjectProperty ;  
    rdfs2:label "hasHobby"@en ;  
    rdfs2:label "увлекается"@ru ;  
    rdfs2:comment "Связывает персону с её увлечением."@ru ;  
    rdfs2:domain onto:Person ;  
    rdfs2:range onto:Hobby ;  
    owl:equivalentProperty <http://schema.org/knowsAbout> .  

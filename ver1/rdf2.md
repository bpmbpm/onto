
# RDF Vocabulary (документация)  

**Префикс:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2.md#`  
**Разделитель:** `#` (hash namespace)  
**Назначение:** Английские имена для стандартного пространства `rdf:` с русскими описаниями.  

@prefix rdf2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdf2.md#> .  
@prefix rdfs2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2.md#> .  
@prefix owl: <http://www.w3.org/2002/07/owl#> .  
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .  

# Classes  

## Property  

rdf2:Property rdf2:type owl:Class ;  
    rdfs2:label "Property"@en ;  
    rdfs2:label "Свойство"@ru ;  
    rdfs2:comment "Класс, представляющий RDF-свойство (предикат)."@ru ;  
    owl:equivalentClass rdf:Property .  

## Statement  

rdf2:Statement rdf2:type owl:Class ;  
    rdfs2:label "Statement"@en ;  
    rdfs2:label "Утверждение"@ru ;  
    rdfs2:comment "Класс, представляющий RDF-утверждение (реифицированный триплет)."@ru ;  
    owl:equivalentClass rdf:Statement .  

## List  

rdf2:List rdf2:type owl:Class ;  
    rdfs2:label "List"@en ;  
    rdfs2:label "Список"@ru ;  
    rdfs2:comment "Класс RDF-списка."@ru ;  
    owl:equivalentClass rdf:List .  

# Properties  

## type  

rdf2:type rdf2:type owl:ObjectProperty ;  
    rdfs2:label "type"@en ;  
    rdfs2:label "тип"@ru ;  
    rdfs2:comment "Указывает, что ресурс является экземпляром класса."@ru ;  
    owl:equivalentProperty rdf:type .  

## subject  

rdf2:subject rdf2:type owl:ObjectProperty ;  
    rdfs2:label "subject"@en ;  
    rdfs2:label "субъект"@ru ;  
    rdfs2:comment "Субъект RDF-утверждения."@ru ;  
    owl:equivalentProperty rdf:subject .  

## predicate  

rdf2:predicate rdf2:type owl:ObjectProperty ;  
    rdfs2:label "predicate"@en ;  
    rdfs2:label "предикат"@ru ;  
    rdfs2:comment "Предикат RDF-утверждения."@ru ;  
    owl:equivalentProperty rdf:predicate .  

## object  

rdf2:object rdf2:type owl:ObjectProperty ;  
    rdfs2:label "object"@en ;  
    rdfs2:label "объект"@ru ;  
    rdfs2:comment "Объект RDF-утверждения."@ru ;  
    owl:equivalentProperty rdf:object .  

## first  

rdf2:first rdf2:type owl:ObjectProperty ;  
    rdfs2:label "first"@en ;  
    rdfs2:label "первый"@ru ;  
    rdfs2:comment "Первый элемент RDF-списка."@ru ;  
    owl:equivalentProperty rdf:first .  

## rest  

rdf2:rest rdf2:type owl:ObjectProperty ;  
    rdfs2:label "rest"@en ;  
    rdfs2:label "остаток"@ru ;  
    rdfs2:comment "Остаток RDF-списка."@ru ;  
    owl:equivalentProperty rdf:rest .  

## value  

rdf2:value rdf2:type owl:DatatypeProperty ;  
    rdfs2:label "value"@en ;  
    rdfs2:label "значение"@ru ;  
    rdfs2:comment "Значение свойства."@ru ;  
    owl:equivalentProperty rdf:value .  

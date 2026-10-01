# RDFS Vocabulary (documentation)  

## prefix 
### `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2.md#`  
## separator 
### `#` (hash namespace)  
## use
### Английские имена для стандартного пространства `rdfs:` с русскими описаниями.  

@prefix rdfs2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2.md#> .  
@prefix rdf2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdf2.md#> .  
@prefix owl: <http://www.w3.org/2002/07/owl#> .  
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .  

# Classes  

## Resource  

rdfs2:Resource rdf2:type owl:Class ;  
    rdfs2:label "Resource"@en ;  
    rdfs2:label "Ресурс"@ru ;  
    rdfs2:comment "Всё, что может быть описано в RDF."@ru ;  
    owl:equivalentClass rdfs:Resource .  

## Class  

rdfs2:Class rdf2:type owl:Class ;  
    rdfs2:label "Class"@en ;  
    rdfs2:label "Класс"@ru ;  
    rdfs2:comment "Класс RDFS, определяющий группы ресурсов."@ru ;  
    owl:equivalentClass rdfs:Class .  

## Literal  

rdfs2:Literal rdf2:type owl:Class ;  
    rdfs2:label "Literal"@en ;  
    rdfs2:label "Литерал"@ru ;  
    rdfs2:comment "Класс литералов."@ru ;  
    owl:equivalentClass rdfs:Literal .  

## Datatype  

rdfs2:Datatype rdf2:type owl:Class ;  
    rdfs2:label "Datatype"@en ;  
    rdfs2:label "Тип данных"@ru ;  
    rdfs2:comment "Класс типов данных."@ru ;  
    owl:equivalentClass rdfs:Datatype .  

# Properties  

## subClassOf  

rdfs2:subClassOf rdf2:type owl:ObjectProperty ;  
    rdfs2:label "subClassOf"@en ;  
    rdfs2:label "подкласс"@ru ;  
    rdfs2:comment "Отношение между классом и его подклассом."@ru ;  
    owl:equivalentProperty rdfs:subClassOf .  

## subPropertyOf  

rdfs2:subPropertyOf rdf2:type owl:ObjectProperty ;  
    rdfs2:label "subPropertyOf"@en ;  
    rdfs2:label "подсвойство"@ru ;  
    rdfs2:comment "Отношение между свойством и его подсвойством."@ru ;  
    owl:equivalentProperty rdfs:subPropertyOf .  

## domain  

rdfs2:domain rdf2:type owl:ObjectProperty ;  
    rdfs2:label "domain"@en ;  
    rdfs2:label "область определения"@ru ;  
    rdfs2:comment "Область определения свойства."@ru ;  
    owl:equivalentProperty rdfs:domain .  

## range  

rdfs2:range rdf2:type owl:ObjectProperty ;  
    rdfs2:label "range"@en ;  
    rdfs2:label "область значений"@ru ;  
    rdfs2:comment "Область значений свойства."@ru ;  
    owl:equivalentProperty rdfs:range .  

## label  

rdfs2:label rdf2:type owl:DatatypeProperty ;  
    rdfs2:label "label"@en ;  
    rdfs2:label "метка"@ru ;  
    rdfs2:comment "Человекочитаемая метка ресурса."@ru ;  
    owl:equivalentProperty rdfs:label .  

## comment  

rdfs2:comment rdf2:type owl:DatatypeProperty ;  
    rdfs2:label "comment"@en ;  
    rdfs2:label "комментарий"@ru ;  
    rdfs2:comment "Комментарий к ресурсу."@ru ;  
    owl:equivalentProperty rdfs:comment .  

## seeAlso  

rdfs2:seeAlso rdf2:type owl:ObjectProperty ;  
    rdfs2:label "seeAlso"@en ;  
    rdfs2:label "см. также"@ru ;  
    rdfs2:comment "Ссылка на дополнительную информацию."@ru ;  
    owl:equivalentProperty rdfs:seeAlso .  

## isDefinedBy  

rdfs2:isDefinedBy rdf2:type owl:ObjectProperty ;  
    rdfs2:label "isDefinedBy"@en ;  
    rdfs2:label "определён в"@ru ;  
    rdfs2:comment "Указывает, где определён ресурс."@ru ;  
    owl:equivalentProperty rdfs:isDefinedBy .  

## member  

rdfs2:member rdf2:type owl:ObjectProperty ;  
    rdfs2:label "member"@en ;  
    rdfs2:label "член"@ru ;  
    rdfs2:comment "Член контейнера."@ru ;  
    owl:equivalentProperty rdfs:member .  

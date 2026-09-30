# Правила именования и выбора префиксов  

## Основное правило  

При описании данных и онтологии действует следующее правило выбора префикса:  

**Если у термина есть аналог в RDF или RDFS — используется `rdf2:` или `rdfs2:`.**  
**Если аналога нет — используется `onto:`.**  

Это правило распространяется на все файлы, кроме самих определений `rdf2.md` и `rdfs2.md` (они описывают соответствие между `rdf2:`/`rdfs2:` и стандартными `rdf:`/`rdfs:`).  

## Таблица соответствия  

| Стандартный термин | Замена | Где используется |
|---|---|---|
| `rdf:type` | `rdf2:type` | Все данные и онтологии |
| `rdf:Property` | `rdf2:Property` | Определения онтологий |
| `rdf:Statement` | `rdf2:Statement` | Определения онтологий |
| `rdf:List` | `rdf2:List` | Определения онтологий |
| `rdf:first` | `rdf2:first` | Определения онтологий |
| `rdf:rest` | `rdf2:rest` | Определения онтологий |
| `rdf:value` | `rdf2:value` | Определения онтологий |
| `rdfs:label` | `rdfs2:label` | Метки ресурсов |
| `rdfs:comment` | `rdfs2:comment` | Комментарии к ресурсам |
| `rdfs:domain` | `rdfs2:domain` | Область определения свойства |
| `rdfs:range` | `rdfs2:range` | Область значений свойства |
| `rdfs:subClassOf` | `rdfs2:subClassOf` | Иерархия классов |
| `rdfs:subPropertyOf` | `rdfs2:subPropertyOf` | Иерархия свойств |
| `rdfs:seeAlso` | `rdfs2:seeAlso` | Ссылки |
| `rdfs:isDefinedBy` | `rdfs2:isDefinedBy` | Определение ресурса |
| `rdfs:member` | `rdfs2:member` | Членство в контейнере |
| `rdfs:Resource` | `rdfs2:Resource` | Определения онтологий |
| `rdfs:Class` | `rdfs2:Class` | Определения онтологий |
| `rdfs:Literal` | `rdfs2:Literal` | Определения онтологий |
| `rdfs:Datatype` | `rdfs2:Datatype` | Определения онтологий |

## Термины без аналогов в RDF/RDFS  

Следующие термины **не имеют** аналогов в RDF/RDFS и всегда используются с префиксом `onto:` (или другим пространством имён, например `owl:`, `xsd:`):  

| Термин | Пространство имён | Комментарий |
|---|---|---|
| `owl:Class` | OWL | Класс OWL |
| `owl:ObjectProperty` | OWL | Объектное свойство |
| `owl:DatatypeProperty` | OWL | Свойство-литерал |
| `owl:equivalentClass` | OWL | Эквивалентность классов |
| `owl:equivalentProperty` | OWL | Эквивалентность свойств |
| `xsd:string` | XSD | Строковый тип данных |
| `xsd:integer` | XSD | Целочисленный тип |
| `onto:Person` | onto | Уникальный класс |
| `onto:hasAddress` | onto | Уникальное свойство |
| `onto:streetAddress` | onto | Уникальное свойство |
| ... | ... | ... |

## Правило для `rdf2:type` вместо `a`  

В стандартном Turtle `a` — это сокращение для `rdf:type`. Поскольку мы используем `rdf2:type`, в документах **не следует** использовать `a`. Всегда пишите:  

ex:alice rdf2:type onto:Person .


Это делает соответствие явным и не зависит от «магического» сокращения.

## Пример применения  

### Данные (test1.md)  

```
ex:alice rdf2:type onto:Person ;
rdfs2:label "Алиса" ;
onto:hasAddress ex:aliceAddress .
```

- `rdf2:type` — аналог `rdf:type`.
- `rdfs2:label` — аналог `rdfs:label`.
- `onto:hasAddress` — уникальное свойство, аналога в RDF/RDFS нет.

### Онтология (ontology1.md)  

```
onto:Person rdf2:type owl:Class ;
rdfs2:label "Person"@en ;
rdfs2:comment "Человек или вымышленный персонаж."@ru ;
owl:equivalentClass http://schema.org/Person .
```


- `rdf2:type` — аналог `rdf:type`.
- `rdfs2:label`, `rdfs2:comment` — аналоги RDFS.
- `owl:Class`, `owl:equivalentClass` — термины OWL, аналогов в RDF/RDFS нет.

## Формат файлов  

Все `.md` файлы содержат «сырой» Turtle:  

- Никаких code fences.  
- В конце каждой строки — два пробела (для отображения в Markdown).  
- `#` в начале строки — заголовок Markdown и комментарий Turtle одновременно.  
- URIs в `<...>` отображаются GitHub как автолинки, но парсер читает их корректно.  

## Ссылки  

- [RDF 1.1 Primer (W3C)](https://www.w3.org/TR/rdf11-primer/)  
- [RDF Schema 1.1 (W3C)](https://www.w3.org/TR/rdf-schema/)  
- [OWL 2 Web Ontology Language (W3C)](https://www.w3.org/TR/owl2-overview/)  
- [Turtle (W3C)](https://www.w3.org/TR/turtle/)  


---
## link2
- https://github.com/bpmbpm/mdld-test/blob/main/ver2/doc/easy/alice3d3.md
- https://www.w3.org/TR/turtle/ ; https://en.wikipedia.org/wiki/Turtle_(syntax) ; https://www.w3.org/TR/rdf-schema/
- https://habr.com/ru/companies/vktech/articles/948492/
- https://web.archive.org/web/20090401192412/http://shcherbak.net/translations/ru_sparql_shcherbak_net.html
  

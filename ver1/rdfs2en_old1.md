## ver1
- https://github.com/bpmbpm/mdld-test/blob/main/ver2/doc/easy/alice3d.md
  
# RDFS English Vocabulary (документация)

**Префикс:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#`
**Разделитель:** `#` (hash namespace)
**Назначение:** Русскоязычная документация для английских имён стандартного пространства `rdfs:`.

## Классы

### Resource

Всё, что может быть описано в RDF.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#Resource`
- **Эквивалент:** `rdfs:Resource`
- **Описание:** Базовый класс для всех RDF-ресурсов. Всё, что имеет URI, является экземпляром этого класса.

### Class

Класс RDFS, определяющий группы ресурсов.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#Class`
- **Эквивалент:** `rdfs:Class`
- **Описание:** Используется для определения классов. Экземпляры класса — это ресурсы, принадлежащие к данной группе.

### Literal

Класс литералов.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#Literal`
- **Эквивалент:** `rdfs:Literal`
- **Описание:** Класс всех литеральных значений (строк, чисел, дат и т.д.).

### Datatype

Класс типов данных.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#Datatype`
- **Эквивалент:** `rdfs:Datatype`
- **Описание:** Класс типов данных, используемых для литералов (например, `xsd:string`, `xsd:integer`).

## Свойства

### subClassOf

Отношение между классом и его подклассом.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#subClassOf`
- **Эквивалент:** `rdfs:subClassOf`
- **Описание:** Указывает, что один класс является более специфичным, чем другой. Транзитивно.

### subPropertyOf

Отношение между свойством и его подсвойством.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#subPropertyOf`
- **Эквивалент:** `rdfs:subPropertyOf`
- **Описание:** Указывает, что одно свойство является частным случаем другого.

### domain

Область определения свойства.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#domain`
- **Эквивалент:** `rdfs:domain`
- **Описание:** Указывает класс, к которому должны принадлежать субъекты данного свойства.

### range

Область значений свойства.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#range`
- **Эквивалент:** `rdfs:range`
- **Описание:** Указывает класс, к которому должны принадлежать объекты данного свойства.

### label

Человекочитаемая метка ресурса.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#label`
- **Эквивалент:** `rdfs:label`
- **Описание:** Используется для отображения имени ресурса человеку. Может иметь языковую метку (`@ru`, `@en`).

### comment

Комментарий к ресурсу.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#comment`
- **Эквивалент:** `rdfs:comment`
- **Описание:** Предназначен для документации ресурса. Может иметь языковую метку.

### seeAlso

Ссылка на дополнительную информацию.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#seeAlso`
- **Эквивалент:** `rdfs:seeAlso`
- **Описание:** Указывает на другой ресурс, содержащий дополнительную информацию о данном ресурсе.

### isDefinedBy

Указывает, где определён ресурс.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#isDefinedBy`
- **Эквивалент:** `rdfs:isDefinedBy`
- **Описание:** Связывает ресурс с документом или онтологией, где он определён.

### member

Член контейнера.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2en.md#member`
- **Эквивалент:** `rdfs:member`
- **Описание:** Используется для указания членства в контейнере (например, в `rdf:Bag`, `rdf:Seq`, `rdf:Alt`).

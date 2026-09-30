## ver1
- https://github.com/bpmbpm/mdld-test/blob/main/ver2/doc/easy/alice3d.md

# RDF English Vocabulary (документация)

**Префикс:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#`
**Разделитель:** `#` (hash namespace)
**Назначение:** Русскоязычная документация для английских имён стандартного пространства `rdf:`.

## Классы

### Property

Класс, представляющий RDF-свойство (предикат).

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#Property`
- **Эквивалент:** `rdf:Property`
- **Описание:** Свойство определяет бинарное отношение между субъектом и объектом в RDF-утверждении.

### Statement

Класс, представляющий RDF-утверждение (реифицированный триплет).

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#Statement`
- **Эквивалент:** `rdf:Statement`
- **Описание:** Используется для описания самого триплета как ресурса (субъект, предикат, объект).

### List

Класс RDF-списка.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#List`
- **Эквивалент:** `rdf:List`
- **Описание:** Упорядоченная коллекция элементов, реализованная через `rdf:first` и `rdf:rest`.

## Свойства

### type

Указывает, что ресурс является экземпляром класса.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#type`
- **Эквивалент:** `rdf:type`
- **Описание:** Основной предикат для указания класса ресурса.

### subject

Субъект RDF-утверждения.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#subject`
- **Эквивалент:** `rdf:subject`
- **Описание:** Используется при реификации для указания субъекта исходного триплета.

### predicate

Предикат RDF-утверждения.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#predicate`
- **Эквивалент:** `rdf:predicate`
- **Описание:** Используется при реификации для указания предиката исходного триплета.

### object

Объект RDF-утверждения.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#object`
- **Эквивалент:** `rdf:object`
- **Описание:** Используется при реификации для указания объекта исходного триплета.

### first

Первый элемент RDF-списка.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#first`
- **Эквивалент:** `rdf:first`
- **Описание:** Связывает узел списка с его первым элементом.

### rest

Остаток RDF-списка.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#rest`
- **Эквивалент:** `rdf:rest`
- **Описание:** Связывает узел списка с остальной частью списка (или с `rdf:nil` для конца).

### value

Значение свойства.

- **URI:** `https://github.com/bpmbpm/onto/blob/main/ver1/rdf2en.md#value`
- **Эквивалент:** `rdf:value`
- **Описание:** Используется для указания значения в контексте реификации или структурированных значений.  

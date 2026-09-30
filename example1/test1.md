## ver1
- https://github.com/bpmbpm/mdld-test/blob/main/ver2/doc/easy/alice3d.md

```turtle
@prefix ex:   <https://github.com/bpmbpm/onto/blob/main/example1/test1.md#> .
@prefix onto: <https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

# alice

ex:alice rdf:type onto:Person ;
    onto:name "Алиса" ;
    onto:hasAddress ex:aliceAddress ;
    onto:hasHobby ex:aliceHobbyPhoto , ex:aliceHobbyChess .

ex:aliceAddress rdf:type onto:PostalAddress ;
    onto:name "Адрес Алисы" ;
    onto:streetAddress "ул. Ленина, д. 10" ;
    onto:addressLocality "Москва" ;
    onto:postalCode "101000" ;
    onto:addressCountry "Россия" .

ex:aliceHobbyPhoto rdf:type onto:Hobby ;
    onto:name "Фотография" .

ex:aliceHobbyChess rdf:type onto:Hobby ;
    onto:name "Шахматы" .

# bob

ex:bob rdf:type onto:Person ;
    onto:name "Боб" ;
    onto:hasAddress ex:bobAddress ;
    onto:hasHobby ex:bobHobbyBike , ex:bobHobbyCode .

ex:bobAddress rdf:type onto:PostalAddress ;
    onto:name "Адрес Боба" ;
    onto:streetAddress "ул. Пушкина, д. 25" ;
    onto:addressLocality "Химки" ;
    onto:postalCode "141400" ;
    onto:addressCountry "Россия" .

ex:bobHobbyBike rdf:type onto:Hobby ;
    onto:name "Велоспорт" .

ex:bobHobbyCode rdf:type onto:Hobby ;
    onto:name "Программирование" .
```

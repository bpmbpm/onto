# Test Data: Alice and Bob  

@prefix ex: <https://github.com/bpmbpm/onto/blob/main/example1/test1.md#> .  
@prefix onto: <https://github.com/bpmbpm/onto/blob/main/ver1/ontology1.md#> .  
@prefix rdf2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdf2.md#> .  
@prefix rdfs2: <https://github.com/bpmbpm/onto/blob/main/ver1/rdfs2.md#> .  

# Alice  

ex:alice rdf2:type onto:Person ;  
    rdfs2:label "Алиса" ;  
    onto:hasAddress ex:aliceAddress ;  
    onto:hasHobby ex:aliceHobbyPhoto , ex:aliceHobbyChess .  

ex:aliceAddress rdf2:type onto:PostalAddress ;  
    rdfs2:label "Адрес Алисы" ;  
    onto:streetAddress "ул. Ленина, д. 10" ;  
    onto:addressLocality "Москва" ;  
    onto:postalCode "101000" ;  
    onto:addressCountry "Россия" .  

ex:aliceHobbyPhoto rdf2:type onto:Hobby ;  
    rdfs2:label "Фотография" .  

ex:aliceHobbyChess rdf2:type onto:Hobby ;  
    rdfs2:label "Шахматы" .  

# Bob  

ex:bob rdf2:type onto:Person ;  
    rdfs2:label "Боб" ;  
    onto:hasAddress ex:bobAddress ;  
    onto:hasHobby ex:bobHobbyBike , ex:bobHobbyCode .  

ex:bobAddress rdf2:type onto:PostalAddress ;  
    rdfs2:label "Адрес Боба" ;  
    onto:streetAddress "ул. Пушкина, д. 25" ;  
    onto:addressLocality "Химки" ;  
    onto:postalCode "141400" ;  
    onto:addressCountry "Россия" .  

ex:bobHobbyBike rdf2:type onto:Hobby ;  
    rdfs2:label "Велоспорт" .  

ex:bobHobbyCode rdf2:type onto:Hobby ;  
    rdfs2:label "Программирование" .  

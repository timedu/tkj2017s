---
layout: collection_index
permalink: /:collection/index.html
---


Sarakeperhetietokannoista[^1] tiettävästi [eniten käytetty][ranking] on [Apache Cassandra][cassandra], joka toimii tämän osan esimerkkijärjestelmänä. Cassandran sivustolla tuotteesta mainitaan mm. seuraavaa:

[^1]: Ks. esim. [JYU/ITKA204/sarakeperhetietokannat]( https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#sarakeperhetietokannat)

[ranking]: http://db-engines.com/en/ranking/wide+column+store
[cassandra]: http://cassandra.apache.org


> The Apache Cassandra database is the right choice when you need scalability and high availability without compromising performance. Linear scalability and proven fault-tolerance on commodity hardware or cloud infrastructure make it the perfect platform for mission-critical data.

Järjestemän käyttö muistuttaa huomattavasti relaatiotietokannan käsittelyä, koska käytettävän *CQL*-kielen komennot ovat usein täysin samoja kuin vastaavat *SQL*-komennot. Tietokannan muodostamisperiaate kuitenkin poikkeaa huomattavasti perinteisesti relaatiotietokantojen yhteydessä noudatetusta käytännöstä.

Relaatiotietokantojen taulut tyypillisesti normalisoidaan siten, että tietojen ylläpito kohdistuu yleensä ainoastaan yhteen tietokokonaisuuteen. Tällöin kyselytarpeet toteutetaan monen taulun liitoksilla. *Cassandran* yhteydessä tilanne on päinvastainen. Järjestelmä ei sisällä lainkaan liitos-operaatiota. Tietokanta de-normalisoidaan siten, että kulloinkin tarvittavat tiedot voidaan hakea ainoastaan yhdestä taulusta.

Relaatiotietokannassa avain yksilöi taulun rivin. Avain on sellainen, että avaimesta ei voi poistaa yhtään osaa ilman, että samalla menetetään yksilöivyys. Relaatiotietokannan avaimen valinnalla ei myöskään suoraan oteta kantaa tiedon talletusrakenteeseen. Nämäkin asiat ovat *Cassandran* yhteydessä hieman eri tavalla. Avain yksilöi tiedon, mutta useinkaan se ei ole minimalistinen[^2]. Avain myös osaltaan määrittelee tietojen sijoitusta ja järjestystä tietovälineellä. 

[^2]: Avaimessa on yksilöivyyden näkökulmasta turhia osia. 

### Tehtävät


Kurssin tämä osa sisältää kaksi ohjelmointitehtävää ja kaksi sarjaa eri aineistoihin perustuvia monivalintatehtäviä.


{% include exercises_list.md %}


[Tehtävän 5.1](tehtava51) lähtökohtana on joukko Cassandra:a esitteleviä lyhyitä videoita, jotka ovat osa Datastaxin [Data Modeling][data-modeling] -verkkokurssia[^3]. Aineistoon liittyy 12 monivalintatehtävää. Tehtävät [5.2](tehtava52) ja [5.3](tehtava53) ovat ohjelmointitehtäviä - sovellukseen laaditaan ensin kyselyt ja sitten sitä täydennetään  ylläpito-operaatioilla. [Tehtävä 5.4](tehtava54) sisältää kurssilukemiston ([NoSQL Distilled][nosql-distilled]), lukuihin 9 ja 10 perustuvan kysymyssarjan.

[^3]: Verkkokurssia voi opiskella veloituksetta. [Datastax](http://www.datastax.com/company) on organisaatio, joka tarjoaa kaupallisia Cassandraan perustuvia tuotteita ja palveluja. 

[data-modeling]: https://academy.datastax.com/resources/ds220-data-modeling

[nosql-distilled]: /tkj2017s/viitteet/#nosql-distilled



<br/>


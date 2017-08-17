---
layout: site_index
# kesken: 1
---

Tämä on [TTY Porin](http://www.poridi.fi) lukuvuoden 2017/18 Tietokantajärjestelmät -opintojakson materiaali, joka rakentuu kurssitoteutuksen edetessä melko pitkälle [edellisen lukuvuoden materiaalin](https://timedu.github.io/tkj2017k/) pohjalta. 


~~~
sivusto täydentyy ja päivittyy kurssitoteutuksen edetessä
~~~

*Tietokantajärjestelmät* on erityyppisiin tietokantoihin tutustuttava kurssi, joka tarjoaa aihepiiristään sekä ison kuvan että käytännön osaamista järjestelmien rakentamiseen. Esillä on perinteisen relaatiotietokannan ohella joukko ns. NoSQL-tietokantoja, joiden soveltamisella tavoitellaan toisaalta sovelluskehitystyön parempaa tuottavuutta ja toisaalta suurempaa tehokkuuutta käsitellä entistä suurempia tietomassoja.

Kurssin lukemistona on *Pramod J. Sadalagen* ja *Martin Fowlerin* tiivis kirja [NoSQL Distilled](https://martinfowler.com/books/nosql.html). Teknisessä osuudessä on esillä eri tietokantatyyppien yleisimpiä edustajia. Kurssi kurssi on jakaantuu seitsemään osaan seuraavasti:

{% include collections_list.md %}

Kurssiin liittyvät teknisen tehtävät laaditaan pääosin JavaScript-ympäristössä suoritusalustana [Node.js](https://nodejs.org/en/), johon tutustutaan *ensimmäisessä osassa* ennen tietokanta-aiheisiin siirtymistä. *Toisessa osassa* esillä on relaatiotietokanta ja sen käyttö sovellusohjelmissa perinteisellä SQL-rajapinnalla ja siten, että tietokannan tiedot näkyvät tyypillisinä ohjelmointikielen objekteina ([ORM](https://en.wikipedia.org/wiki/Object-relational_mapping)). Esimerkkinä relaatiotietokannoista on [SQLite][sqlite], jonka käyttö ei edellytä erillistä palvelinohjelmistoa.

Kurssin osissa 3-6 on esillä neljä NoSQL-tietokantojen  perustyyppiä: *avain-arvopari*, *dokumentti*, *sarakeperhe* ja *graafi*. Näistä kolme ensimmäistä ovat ns. [aggregaattitietokantoja](https://martinfowler.com/bliki/AggregateOrientedDatabase.html), joista esimerkkeinä toimivat [Riak][riak], [MongoDB][mongodb] ja [Cassandra][cassandra]. Graafitietokannoista tarkastelun kohteena on [Neo4j][neo4j]. *Viimeisessä osassa* esillä on hybriditietokanta, [OrientDB][orientdb], jossa on sekä dokumentti- että graafitietokannan ominaisuudet. Näiden lisäksi esimerkkinä olevalla järjestelmällä on vielä oliopiirteitä.


[sqlite]: https://www.sqlite.org
[riak]: http://basho.com/products/riak-kv/
[mongodb]: https://www.mongodb.com
[neo4j]: https://neo4j.com
[cassandra]: http://cassandra.apache.org
[orientdb]: http://orientdb.com




{% comment %}

Kopio edellisestä versiosta


*Tietokantajärjestelmät* on erityyppisiin tietokantoihin tutustuttava ohjelmointi-orientutunut kurssi, jonka painopiste on NoSQL-tietokannoissa. Keskeisiä pohjatietoja tälle kurssille antavat TTY Porissa [Olio-ohjelmointi][olio], [Web-ohjelmointi][jwo] sekä [Tiedonhallinta ja tietokannat][tiha]. Opintojakson toteutusperiaatteena on *ongelmakeskeinen itseopiskelu ohjatusti (OIO)* siten, että varsinasta luento-opetusta ei ole. Kurssi on jaettu seitsemään osaan, joista kunkin ytimen muodostaa tiettyyn teemaan keskittyvä tehtäväsarja.

[olio]: http://www.tut.fi/opinto-opas/wwwoppaat/opas2016-2017/pori/laitokset/Pori/PLA-32100.html
[jwo]: http://www.tut.fi/opinto-opas/wwwoppaat/opas2016-2017/pori/laitokset/Pori/PLA-32811.html
[tiha]: http://www.tut.fi/opinto-opas/wwwoppaat/opas2016-2017/pori/laitokset/Pori/PLA-32602.html

Kurssi *ensimmäinen osa* tutustuttaa kurssilla käyttettavään kehitystyön peruskalustoon. *Toisessa osassa* siirrytään tietokantakäsittelyyn. Esillä on relaatiotietokanta ja sen käyttö sovellusohjelmissa siten, että tietokannan tiedot näkyvät tyypillisinä ohjelmointikielen objekteina. Kurssin osissa 3-6 on esillä neljä NoSQL-tietokantojen  perustyyppiä: *avain-arvopari*, *dokumentti*, *graafi* ja *sarakeperhe*. *Viimeisessä osassa* tarkastelun kohteena on hybriditietokanta, jossa on sekä dokumentti- että graafitietokannan ominaisuudet. Näiden lisäksi esimerkkinä olevalla järjestelmällä on vielä oliopiirteitä.

#### Lisätietoja

[Kurssin konteksti](konteksti)   
[Suorituksen arvostelu](arvostelu)   
[Koodin katselmointipyynnöt](https://moodle2.tut.fi/mod/forum/discuss.php?d=74758)   


{% endcomment %}

#### Lisätietoja

[Suorituksen arvostelu](arvostelu)   

---
layout: collection_index
permalink: /:collection/index.html
---


Kurssin osassa 3 käsiteltiin avain-arvoparitietokantoja, joiden yhteydessä fyysistä tietokantaa käsittelemä järjestelmä ei tunne arvojen rakennetta vaan arvojen tulkinnan tekee järjestelmän kautta tietokantaa käsittelevä sovellus. Osan tehtävissä tietokantaan  talletettiin [JSON][JSON]-muodossa olevia merkkijonoja, jotka esimerkkinä käytetty järjestelmä tosin kykeni muuntamaan sovelluksessa käytettäviksi JavaScript-objekteiksi.

[JSON]: http://www.json.org

Dokumenttitietokannoissa[^1] tiedot talletetaan juuri JSON-tyyppisiksi rakenteiksi, joita näiden tietokantojen yhteydessä kutsutaan dokumenteiksi. Erona avain-arvoparitietokantoihin on mm. se, että järjestelmä tunnistaa nämä rakenteet so. järjestelmällä on tiedossaan mitä attribuutteja jokin dokumentti sisältää, mikä mahdollistaa järjestelmälle toteuttaa esim. kyselyissa attribuuttien arvoihin perustuvan valinnan ja lajittelun. 

[^1]: Ks. esim. [ITKA204/Dokumenttitietokannat](https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#dokumenttitietokannat)

Tämän osan esimerkkijärjestelmä, [MongoDB][MongoDB], on tiettävästi [eniten käytetty dokumenttitietokanta][ranking], joka lienee samalla myös suosituin NoSQL-tietokanta. [Mongon sivustolla][what-is-mongodb] tuotteesta kerrotaan seuraavaa:

>MongoDB stores data in flexible, JSON-like documents, meaning fields can vary from document to document and data structure can be changed over time.
>The document model maps to the objects in your application code, making data easy to work with.
> Ad hoc queries, indexing, and real time aggregation provide powerful ways to access and analyze your data.
> MongoDB is a distributed database at its core, so high availability, horizontal scaling, and geographic distribution are built in and easy to use.
> MongoDB is free and open-source, published under the GNU Affero General Public License.


[MongoDB]: https://www.mongodb.com
[ranking]: http://db-engines.com/en/ranking/document+store
[what-is-mongodb]: https://www.mongodb.com/what-is-mongodb

{% comment %}

Mongon ohella tämän osan tehtävissä on esillä kaksi muuta järjestelmää, joita käytettäessä (Mongosta poiketen) tietokanta voidaan upottaa sovellukseen:  

> [TingoDB][TingoDB] is embedded JavaScript NoSql database for Node.js and node-webkit. Its API and features designed to be upward compatible with MongoDB and its driver for Node.js. 
>
> [NeDB][NeDB]: Embedded persistent or in memory database for Node.js, nw.js, Electron and browsers, 100% JavaScript, no binary dependency. API is a subset of MongoDB's and it's plenty fast.

[TingoDB]: http://www.tingodb.com
[NeDB]: https://github.com/louischatriot/nedb/blob/master/README.md

{% endcomment %}



### Tehtävät

Osan ohjelmointiehtävissä jatketaan edelleen *kurssien ja opettajien* käsittelyä. Tehtävissä on esillä kahdella eri tavalla jäsennetty dokumenttitietokanta, joka sijaitsee nyt verkon yli käytettävässä pilvipalvelussa.


{% include exercises_list.md %}


[Tehtävässä 4.1](tehtava41) tietokanta on jäsennetty kahdeksi dokumenttikoelmaksi, jotka vastaavat relaatiotietokannan tauluja. Tietokannan rakenne on muutenkin "relaatiomainen": *Kurssi*-dokumentintissa on attribuutti, johon on talletettu kurssin opettajaa vastaavan *Opettaja*-dokumentin tietokantatunniste. Tehtävässä toteutetaan ainoastaan tietokantaan liittyvät kyselyt.

[Tehtävän 4.2](tehtava42) tietokanta muodostuu yhdestä dokumenttikokoelmasta siten, että *Opettaja* esiintyy varsinaisena kokoelman dokumenttina, jonka attribuutin arvona on opettajan pitämien *kurssien* tiedot. Tehtävässä toteutetaan edellisen tehtävän tapaan ainoastaan tietokantaan kohdistyvat kyselyt.

[Tehtävässä 4.3](tehtava43) tehtävän 4.1 ratkaisua täydennetään tietokannan ylläpito-operaatioilla. Sovellusta varten myös perustetaan tietokanta [mLab][mLab]-palvelun kautta [Amazon][Amazon]:in pilveen. [mLab][mLab] tarjoaa veloituksetta (ja ilman luottokorttitietojen antamista) tutustumiskäyttöön 0,5GB ns. hiekkalaatikon.

[mLab]: https://mlab.com
[Amazon]: https://aws.amazon.com

[Tehtävä 4.4](tehtava44) sisältää kurssilukemiston ([NoSQL Distilled][nosql-distilled]), lukuihin 7 ja 8 perustuvan kysymyssarjan.

[nosql-distilled]: /tkj2017s/viitteet/#nosql-distilled


{% comment %}

[Tehtävässä 4.1](tehtava41) tietokanta on jäsennetty kahdeksi dokumenttikoelmaksi, jotka vastaavat relaatiotietokannan tauluja. Tietokannan rakenne on muutenkin "relaatiomainen": *Kurssi*-dokumentintissa on attribuutti, johon on talletettu kurssin opettajaa vastaavan *Opettaja*-dokumentin tietokantatunniste. Tehtävässä toteutetaan tietokantaan liittyvät kyselyt sekä opettajatietojen osalta ylläpito-operaatiot. Tietokanta on upotettu sovellukseen ja sitä käsitellään [TingoDB][TingoDB]-kirjastolla.

[Tehtävän 4.2](tehtava42) tietokanta muodostuu yhdestä dokumenttikokoelmasta siten, että *Opettaja* esiintyy varsinaisena kokoelman dokumenttina, jonka attribuutin arvona on opettajan pitämien *kurssien* tiedot. Tehtävässä toteutetaan ainoastaan tietokantaan kohdistyvat kyselyt. Edellisen tehtävän tapaan tietokanta on upotettu sovellukseen, mutta sitä käsitellään Tingon sijaan [NeDB][NeDB]-kirjastolla.

[Tehtävässä 4.3](tehtava43) tietokanta siirtyy verkon yli hyödynnettävään pilvipalveluun. Käytettävä [MongoDB][MongoDB]-tietokanta on perustettu [mLab][mLab]:in kautta [Amazon][Amazon]:in pilveen. Tietokannan rakenne vastaa tehtävää 4.1. Tässä toteutetaan dokumenttikokoelmiin kohdistuvat kyselyt. Tehtävän ratkaisu on kyselyjen osalta likimain sama kuin Tehtävässä 4.1. Tietokantayhteys kuitenkin muodostetaan uudelleen kunkin sovellukseen tulevan pyynnön yhteydessä. Valmiin tietokannan sijaan tehtävässä voi käyttää pilvipalveluun itse perustamaansa tietokantaa.

[mLab]: https://mlab.com
[Amazon]: https://aws.amazon.com

{% endcomment %}


<br/>
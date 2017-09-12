---
layout: collection_index
permalink: /:collection/index.html
---


Kurssin edellisessä osassa on esillä relaatiotietokanta, joissa tiedot jäsentyvät matemaattisten relaatioiden pohjalta sarakkeiksia ja taulukoiksi. Rivien välisiä yhteyksiä muodostetaan perus- ja vierasavainsarakkeiden avulla. Ennen kuin tietokantaa voidaan käyttää, muodostetaan sen skeema so. määritellään ennalta, minkälaista tietoa tietokanta voi sisältää.

Tassä osassa siirrytään tarkastelemaan ns. NoSQL -tietokantoja[^1], joissa tietojen rakenne poikkeaa merkittävästi perinteisestä relaatiomallista. Tyypillisesti tietokannan skeemaakaan ei tarvitse määritellä ennalta ainakaan kokonaisuudessaan. Monet NoSQL -järjestälmät tukevat luonnollisella tavalla myös tietojen hajautusta.


[^1]: Jyväskylän yliopiston *Tietokannat ja tiedonhallinnan perusteet* -kurssin materiaalissa on luku, [Tietokantaparadigmat][itka204-8], joka esittelee lyhyesti eri tyyppisiä NoSQL -tietokantoja. Youtubesta löytyy esim. Martin Fowlerin pitämä esitys [Introduction to NoSQL][youtube-fowler].

[itka204-8]: https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#tietokantaparadigmat
[youtube-fowler]: https://www.youtube.com/watch?v=qI_g07C_Q5I


Nyt NoSQL -tietokannoista esillä on *avain-arvoparitietokanta*[^2], jossa nimensä mukaisesti tiedot jäsentyvät siten kuin tietokantatyypin nimitys antaa olettaa. Yksittäistä avain-arvoparia relaatiotietokannassa voi vastata esim. taulukon yksi rivi tai vaikka kokonainen taulukko. Tietokannan hallintajärjestelmä ei tunne arvojen rakennetta vaan tulkinnan tekee tietoja käyttävä sovellus. Tässä avain-arvoparitietokannoista esimerkkinä on [LevelDB][LevelDB][^3], josta kerrotaan seuraavaa:


[^2]: Ks. esim. [ITKA204/Avain-arvoparitietokannat](https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#avain-arvoparitietokannat)

[^3]: *LevelDB* korvaa tähän kurssitoteutukseen aiotun *Riak KV*:n, koska Riakin taustalla olevalla organisaatiolla ([Basho](http://basho.com)) lienee käynnissä jotakin liiketoimintaan liittyviä järjestelyjä. Esim. sivuilla olevat keskeiset linkit kuten *Academy*, *Download Riak* ja *Docs* eivät ole käytettävissä. Järjestelmän [Docker image](https://hub.docker.com/r/basho/riak-kv/) löytyy vielä *Docker Hub*ista, [dokumentaatio](https://www.tiot.jp/riak-docs/riak/kv/2.2.3/) eräältä japanilaiselta sivustolta ja ohjelmointirajapintoja ([riak-nodejs-client](https://github.com/basho/riak-nodejs-client/blob/master/README.md), [riak-js](http://riak-js.org)) Nodelle *npm*:n kautta, mutta virallinen tausta vaikuttaisi tällä hetkellä olevan kadoksissa.


> [LevelDB][LevelDB] is a simple key/value data store built by Google, inspired by [BigTable][BigTable]. It's used in Google Chrome and many other products. LevelDB supports arbitrary byte arrays as both keys and values, singular get, put and delete operations, batched put and delete, bi-directional iterators and simple compression using the very fast Snappy algorithm.
> 
> [LevelUP][LevelUP] aims to expose the features of LevelDB in a Node.js-friendly way. All standard Buffer encoding types are supported, as is a special JSON encoding. LevelDB's iterators are exposed as a Node.js-style readable stream.

[LevelDB]: http://leveldb.org 
[LevelUP]: https://github.com/Level/levelup/blob/master/README.md
[BigTable]: https://research.google.com/archive/bigtable.html


Edellisessä osassa esillä olleen *SQLite:n* tapaan *LevelDB*-tietokanta voidaan upottaa sovellukseen siten, että erillistä tietokannan hallintajärjestelmää ei tarvita. 


### Tehtävät


Tehtävistä kolmen ensimmäisen taustalla on [edellisen osan](../osa2) tehtävistä tuttu *Kurssit ja opettajat* -tietokanta, mikä nyt on jäsennetty kahdella eri tavalla avain-arvoparitietokannaksi. 


{% include exercises_list.md %}


Avain-arvoparitietokanta on eräs tyyppi aggregaattitietokannoista, joiden rakenteen suunnittelun lähtökohtana on tyypillisesti sovelluksen käsittelemät tietokokonaisuudet. [Tehtävässä 3.1](tehtava31) tiedot jäsentyvät juuri tällä tavalla. Tietokantaan talletetut arvot vastaavat rakenteeltaan näkymien esittämiä tietorakenteita.  

[Tehtävässä 3.2](tehtava32) toteutetaan edelisen kanssa käyttäjän näkökulmasta samanlainen kurssi- ja opettajatietoja esittävä sovellus. Taustalla olevan tietokannan rakenne tässä on kuitenkin erilainen. Rakenne vastaa normalisoitua relaatiotietokantaa, jolloin tiedon tuottaminen näkymiä varten edellyttää edelliseen verrattuna laajempaa koodia.

[Tehtävässä 3.3](tehtava33) laajennetaan tehtävän 3.2 sovellusta ylläpitotoiminnoilla, jotka osin huomioivat samanaikaisuuteen liittyvät ongelmat aikaleimojen avulla.


[Tehtävä 3.4](tehtava34) sisältää kurssilukemiston ([NoSQL Distilled][nosql-distilled]), lukuihin 5 ja 6 perustuvan kysymyssarjan.

[nosql-distilled]: /tkj2017s/viitteet/#nosql-distilled

<br/>
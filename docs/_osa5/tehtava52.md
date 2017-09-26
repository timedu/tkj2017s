---
layout: exercise_page
title: "Tehtävä 5.2: Kurssit ja opettajat, Cassandra (3p)"
exercise_template_name: W5E02.KurssitJaOpettajatCassandra
exercise_discussion_id: 85227
exercise_upload_id: 342881
---

Laadi ulkoisilta ominaisuksiltaan edellisten tehtävien ratkaisujen kaltainen sovellus, jolla voidaan tarkastella *kurssi- ja opettajatietoja* sekä *yhteenveto-* että *erittelymuotoisten* näkymien kautta. Taustalla oleva tietokantaratkaisu on nyt [Cassandra][Cassandra] -sarakeperhetietokanta, jota ajetaan [Docker-kontissa](#docker-konteista) tai [virtuaalikoneessa](#virtuaalikoneesta), jos konttiin perustuva installaatio ei kehitysympäristössä ole mahdollista.


[Cassandra]: http://cassandra.apache.org


Sovelluksen lähdekoodi jäsentyy edellisten tehtävien tapaan.  Moduuleja, `opettajaController.js` ja `kurssiController.js`, lukuunottamatta sovellus on rakennettu valmiiksi. Kontrollerit ottavat moduulin `config/db_connection.js` käyttöönsä siten, että tietokantarajapinta näkyy tunnisteessa `db`.


Tehtäväpohjan moduulissa `config/db_create.js` on koodi, joka muodostaa tietokannan taulut datoineen sovelluksen jokaisen käynnistyskerran yhteydessä. Jos tietokanta on jo muodostettu, em. moduulissa voi vaihtaa tietyt kommenttimerkit toiseen paikkaan[^2], jolloin moduuli ei luo tietokantaa uudelleen.

[^2]: Moduuliin on kirjattu tästä ohje

Cassandra:n on oltava käynnissä (kontissa tai virtuaalikoneessa) sovellusta käytettäessä.


{% comment %}

Bitnamista ladattavissa oleva [linux-virtuaalikone][cassandra-vm] käynnistää automaatisesti Cassandran, johon liittyvät tunnistetiedot ovat näkyvissä koneen *login*-ikkunassa. Moduuliin `configs/db_connection.js` kirjattujen tunnistetietojen tulee olla ne, jotka näkyvät virtuaalikoneessa. Koneeseen ei tarvitse kirjautua.

{% endcomment %}



**Palauta** tehtävän ratkaisuna tiedostot `opettajaCOntroller.js` ja `kurssiController.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla sekä käyttäen oheistettuja Selenium-testejä.



#### Tietokannnan rakenne

Tietokanta rakentuu siten, että kutakin kyselyä varten on oma tietokantataulunsa,  *opettaja_list*, *opettajat*, *kurssi_list* ja *kurssit*. Seuraavissa *Listauksissa 1-6* on esitetty luontikomennot *tauluille* ja niiden viittaamille *tyypeille*[^1].

[^1]: Listaukset on poimittu moduulista `config/db_create.js`.


{% highlight javascript %}

const CREATE_TABLE_opettaja_list = '        \
CREATE TABLE IF NOT EXISTS opettaja_list (  \
  block_id  int,                            \
  key       uuid,                           \
  sukunimi  text,                           \
  etunimi   text,                           \
PRIMARY KEY ( (block_id), sukunimi, etunimi, key) )';

{% endhighlight %}

<small>Listaus 1. *opettaja_list*-taulu</small>


Taulussa `opettaja_list` olevan partitio-avaimen `block_id` arvo on kaikkien taulun rivien osalta sama so. rivit talletetaan yhteen partitioon (*Listaus 1*).


{% highlight javascript %}

const CREATE_TABLE_opettajat = '        \
CREATE TABLE IF NOT EXISTS opettajat (  \
  key       uuid,                       \
  sukunimi  text STATIC,                \
  etunimi   text STATIC,                \
  kurssi    frozen<kurssi>,             \
PRIMARY KEY ((key), kurssi) )';

{% endhighlight %}

<small>Listaus 2. *opettajat*-taulu</small>



{% highlight javascript %}

const CREATE_TYPE_kurssi = '        \
CREATE TYPE IF NOT EXISTS kurssi (  \
  nimi  text,                       \
  key   uuid                        \
)';

{% endhighlight %}

<small>Listaus 3. *kurssi*-tyyppi</small>


*Listauksen 2* esittämässä `opettajat` -taulun perustamiskomennossa  viitataan tyyppiin `kurssi`, jonka määrittely on *Listauksessa 3*.


{% highlight javascript %}

const CREATE_TABLE_kurssi_list = '        \
CREATE TABLE IF NOT EXISTS kurssi_list (  \
  block_id  int,                          \
  key       uuid,                         \
  nimi      text,                         \
PRIMARY KEY ( (block_id), nimi, key) )';

{% endhighlight %}

<small>Listaus 4. *kurssi_list*-taulu</small>


*Listauksen 4* esittämän `kurssi_list` -taulun rivit talletetaan yhteen partitioon taulun `opettaja_list` (*Listaus 1*) tapaan.


{% highlight javascript %}

const CREATE_TABLE_kurssit = '        \
CREATE TABLE IF NOT EXISTS kurssit (  \
  key     uuid,                       \
  tunnus  text,                       \
  nimi    text,                       \
  laajuus text,                       \
  opettaja frozen<opettaja>,          \
PRIMARY KEY (key) )';

{% endhighlight %}

<small>Listaus 5. *kurssit*-taulu</small>


{% highlight javascript %}

const CREATE_TYPE_opettaja = '        \
CREATE TYPE IF NOT EXISTS opettaja (  \
  key       uuid,                     \
  sukunimi  text,                     \
  etunimi   text                      \
)';

{% endhighlight %}

<small>Listaus 6. *opettaja*-tyyppi</small>


*Listauksen 5* esittämässä `kurssit` -taulun perustamiskomennossa  viitataan tyyppiin `opettaja`, jonka määrittely on *Listauksessa 6*.


### Vihjeitä ja lisätietoja

#### Sovellusrajapinnasta

Cassandra-tietokannan sovellusrajapinta on kuvattu [rajapinnan käsikirjassa][Driver]. Tässä rajapinnasta käytetään sen [client][class.Client]-oliota, jonka sovelluksen kontrollerit ottavat käyttöönsä tunnuksella `db`. Kaikki tarvittavat kyselyt voidaan toteuttaa olion [`execute`][execute] -metodilla[^4], mutta yhdessä kyselyssä toteutus saattaa muodostua yksinkertaisemmaksi [`eachRow`][each-row]-metodia käyttämällä. Metodeille annetaan perametrina [CQL][CQL]-kielisiä komentoja, jotka tässä eivät mitenkään eroa SQL:stä.

Kysely ilman parametreja:

{% highlight js %}

db.execute(CQL_komento, function (err, result) {      
    // kyselyn palauttamat rivit löytyvät 
    // taulukosta result.rows          
});

{% endhighlight %}


Kysely, jossa on parametreja (parametrien paikat merkitään CQL-komentoon `?`-merkeillä):

{% highlight js %}

db.execute(CQL_komento, parametritaulukko, {prepare: true}, function(err, result) {
  // ...
});

{% endhighlight %}

`eachRow`-kysely:

{% highlight js %}

db.eachRow(CQL_komento, parametritaulukko, {prepare: true}, 
function(n, row){
    // funktio suoritetaan koskien jokaista tietokannasta palautettavaa riviä;
    // tiedot löytyvät objektina muuttujassa row
}, function(err) {
    // funktio suoritetaan käsittelyn lopussa
});

{% endhighlight %}


[Driver]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/
[class.Client]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/
[execute]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#execute
[each-row]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#each-row
[CQL]: http://docs.datastax.com/en/cql/3.1/index.html


[^4]: Callback-based API; node.js ei välttämättä vielä tue toista (Promise-based API, using async/await)


#### Cassandra kehitysympäristöön

[Cassandran Download-sivulla](http://cassandra.apache.org/download/) on kuvattu, miten järjestelmä asennetaan Linux-ympäristöön. Muissa ympäristöissä (esim. atk-luokissa) voidaan käyttää virtuaalikoneita, ja soveltuvin osin [Docker](https://www.docker.com)-kontteja (ei tosin luokkakoneissa). Vanhempien Cassandra-versioiden asennuspaketteja mm. Windows-ympäristöön löytyy [DataStaxin  Download-sivulta](https://academy.datastax.com/planet-cassandra/cassandra).


##### Virtuaalikoneesta

Virtulaalikone on sanansa mukaisesti virtuaalinen tietokone, joka toimii toisen tietokoneen sisällä. Tässä kone on sellainen, johon on asennettu Linux-käyttöjärjestelmä ja Cassandra tietokannan hallintajärjestelmä. Valmiin virtuaalikoneen voi ladata [Bitnamin sivustolta](https://bitnami.com/stack/cassandra/virtual-machine). Kone on kuitenkin sellainen,  jonka tietokannan käyttö koneen ulkopuolelta edellyttänee koneen [porttiasetuksiin pieniä säätöjä](https://docs.bitnami.com/virtual-machine/infrastructure/cassandra/#how-to-connect-to-the-cassandra-server-from-a-different-machine). Koneen hieman vanhemmassa versiossa näitä säätöjä ei tarvitse tehdä. Tämän virtuaalikoneen latausosoite löytyy [Moodlesta](https://moodle2.tut.fi/mod/page/view.php?id=343244).

Luokkakoneisiin on asennettu  *VMWare Workstation* - ja *VMWare Player* -ohjelmistot, joilla voi ajaa virtuaalikoneita. Virtualisointiohjelmiston voi ladata halutessaan myös omaan kehitysympäristöön. [VMWare Workstation Player][player]:ia voi käyttää vapaasti, kunhan käyttö on ei-kaupallista. Binamin konetta voi ajaa myös [VirtualBox][VirtualBox]:lla, jonka voi niin ikään ladata veloituksetta. 

[VirtualBox]: https://www.virtualbox.org
[player]: http://www.vmware.com/products/player/playerpro-evaluation.html

Bitnamin koneet ovat [ova][ova]-formaatissa, jonka virtualisointiohjelmisto (esim. VMWare Workstation Player) osaa purkaa käyttämäänsä muotoon. Lataa  ja pura luokkakoneissa virtuaalikone käyttöä varten koneen `C:\Temp`-hakemistoon, jonne on kirjoitusoikeus.

[ova]: https://en.wikipedia.org/wiki/Open_Virtualization_Format

Virtuaalikoneen Network Adapter asetuksen tulisi olla `NAT`. Jos asetus on `Bridged` (saattaa olla oletus), kone ei toimine toivotulla tavalla esim. silloin, kun verkkoyhteyttä ei ole käytettävissä. Esim. VMWare Fusion -ohjelmassa (Mac) tämä asetus löytyy valikosta *Virtual Machine / Network Adapter*. Windows / Linux -ympäristöjen VMWare Workstation / Player -ohjelmistoista löytyy vastaava asetus.

Kun Bitnamin virtuaalikoneen käynnistää, esiin ilmestyvässä Linux-ikkunasta pitäisi löytyä ip-osoite ja portti sekä tietokannan käyttäjätunnus ja salasana. Nämä tiedot tulisi kirjata ao.kohtiin sovelluksen moduulissa `config/db_connection.js`.


![login-ikkuna](../img/bitnami-wm.png){: style="display: block; margin: auto; margin-top: 10px; width: 600px;"}


Esim. edellisestä ikkunasta on poimittavissa seuraavat tiedot, joiden avulla voidaan ottaa virtuaalikoneessa toimivaan tietokantaan:


{% highlight js %}

const IP = '172.16.156.155';
const PORT = '9042';
const USER = 'cassandra';
const PWD = 'bitnami';

{% endhighlight %} 


Kun tehtävän pohjakoodin käynnistää, niin seuraavanlaista tekstiä pitäisi ilmestyä NetBeansin *Output*-ikkunaan, mikä ilmentää tietokannan olevan käytettävissä:

~~~
** db_create **
** db_connection **
** db_data **
server running at http://127.0.0.1:3000/
logged to cassandra
rows inserted
~~~

Virtuaalikoneeseen ei tarvitse kirjautua, koska tietokannan hallintajärjestelmä käynnistyy automaatisesti koneen käynnistyessä.  Kun konetta ei enää käytetä, se kannattaa ajaa alas hallitusti. Virtualisointiohejelmiston valikosta tai vastaavasta painikkeesta löytyy tätä varten toiminto *Shut Down*. 


##### Docker-konteista

Kontititus[^docker-fn1] on virtuaalikoneisiin verrattuna kevyempi virtualisointimenettely. Cassandran saa myös valmiiksi kontitettuna. Konttien ajamista varten tarvitaan [Docker](https://www.docker.com)-ohjelmisto, jonka voi ladata tuotteen sivustolta mm. [Windows-ympäristöön](https://store.docker.com/editions/community/docker-ce-desktop-windows)[^docker-fn2]. 


[^docker-fn1]: Ks. esim. [Koodin kontitus kannattaa – "Ei liikutella binääreitä vaan kokonaisuuksia"](http://www.tivi.fi/Kaikki_uutiset/koodin-kontitus-kannattaa-ei-liikutella-binaareita-vaan-kokonaisuuksia-6559362) (tivi 14.6.2016) ja [Koodin kontitus räjähtää kasvuun](http://www.tivi.fi/Kaikki_uutiset/koodin-kontitus-rajahtaa-kasvuun-6615431) (16.1.2017)


[^docker-fn2]: Docker:ia Windows-ymäristössä käsittelee dokumentti [Get started with Docker for Windows](https://docs.docker.com/docker-for-windows/). Saattaa olla, että virtuaalikoneen käyttö on tässä konttia suoraviivaisempi ratkaisu.

Docker:in avulla Cassandran saa kehitysympäristöön esim. kirjaamalla tietostoon `docker-compose.yml` seuraavan sisällön:

~~~
version: '2'

services:
  cassandra:
    image: 'bitnami/cassandra:latest'
    labels:
      kompose.service.type: nodeport
    ports:
      - '7000:7000'
      - '7001:7001'
      - '9042:9042'
      - '9160:9160'
    volumes:
      - 'cassandra_data:/bitnami'
volumes:
  cassandra_data:
    driver: local
~~~

Tämän jälkeen mennään käyttöjärjestelmän konsoli-ikkunassa (Windows: "Command Prompt") em. tiedoston sisältävään hakemistoon ja annetaan komento `docker-compose up -d`. Konmento lataa tarvittaessa kehityslaitteeseen cassandra-imagen, muodostaa siitä ajettavan kontin ja käynnistää sen, minkä jälkeen Cassandra on käytössä[^docker-fn3] kehitysympäristössä seuraavilla tunnuksilla:


[^docker-fn3]: Tilanteen voi tarkistaa komennolla `docker ps`. Jos Cassandraa ei näy luettelossa, komennon  `docker-compose up -d` voi ajaa uudelleen.


{% highlight js %}

const IP = 'localhost';
const PORT = '9042';
const USER = 'cassandra';
const PWD = 'cassandra';

{% endhighlight %}

Edellä oleva erimerkki liittyy [Bitnami](https://bitnami.com):n tarjoamaan [Cassandra-image](https://hub.docker.com/r/bitnami/cassandra/):en.

Dockeria ohjaavat komennot on dokumentoitu sen [käsikirjaan](https://docs.docker.com/reference/), josta löytyvät mm. komennot [`docker-compose`](https://docs.docker.com/compose/reference/overview/) ja [`docker`](https://docs.docker.com/engine/reference/commandline/cli/).


#### Tietokannan rakenteesta

[DataStax][dataModeling]:n sivustolla Cassandran tietomallista kerrotaan mm. seuraavaa:

> * A table stores data based on a primary key, which consists of a partition key and optional clustering columns.
* A partition key defines the node on which the data is stored.
* A clustering column defines the order of data stored in a row.
* A primary key is used to access the data in the table.

[dataModeling]: http://docs.datastax.com/en/landing_page/doc/landing_page/dataModeling.html

Edellisen lainauksen perusteella taulun riviksi (*row*) voidaan kutsua sitä, minkä taulussa yksilöi partitioavain (*partition key*). Parempi termi tälle saattaisi olla *partitio*, koska tietokantaan kohdistuva kysely saattaa palauttaa useita rivejä *rivejä* yhdestä partitiosta. Seuraavassa on vielä uudelleen *Listauksessa 1* esitetty komento.

{% highlight sql %}

CREATE TABLE IF NOT EXISTS opettaja_list (
  block_id  int,
  key       uuid,
  sukunimi  text,
  etunimi   text,
PRIMARY KEY ((block_id), sukunimi, etunimi, key) )

{% endhighlight %}

<small>Listaus 7. *opettaja_list*-taulu</small>

Cassandra-tietokannan suunnittelun lähtökohtana on tyypilisesti se, että kyselyn edellyttämä data toimitetaan mahdollisimman nopeasti. Tällöin tiedot ovat yleestä yhdessä taulussa ja juuri kyselyn edellyttämässä järjestyksessä. *Listauksen 7* taulu on muodostettu tämän periaatteen mukaan - taulu tuottaan datan näkymään, joka esittää aakkosjärjestyksessä olevan opettajaluettelon. 

Taulun data on talletettu yhteen partitioavaimen *block_id* (arvo: 1) määrittelemään partitioon so. tarvittava data kokonaisuudessaan pystytään paikantamaan *block_id:n* perusteella. Komennossa oleva perusavain muodostuu myös muista osista, *sukunimi*, *etunimi* ja *key*, joka määrittelevät rivien järjestyksen partitiossa ja toisaalta varmistavat sen, että perusavain yksilöi rivin. Täten Cassandran perusavaimella on huomattavasti moninaisempi merkitys kuin perusavaimella relaatiotietokannassa.

Seuraavassa on jo *Listauksessa 2* esitetty taulu, josta tuotetaan data yksittäisen opettajan tiedot esittävään näkymään:


{% highlight sql %}

CREATE TABLE IF NOT EXISTS opettajat (
  key       uuid,
  sukunimi  text STATIC,
  etunimi   text STATIC,
  kurssi    frozen<kurssi>,
PRIMARY KEY ((key), kurssi) )

{% endhighlight %}

<small>Listaus 8. *opettajat*-taulu</small>


*Listausten 7 ja 8* perusteella voidaan todeta, että sama data on talletettu yhtä useampaan tauluun - taulujen rakenne perustuu kyselyjen määräämään tarpeeseen: opettajan erittely-näkymä esittää yksittäisen opettajan tiedot mukaan lukien opettajan kurssit nimen mukaisessa aakkosjäjestyksessä.

Kunkin opettajan tiedot kursseineen on talletettu omaan, *key*-attribuutin määräämään, partitioon. Partition sisältämät opettajan pitämät kurssit on järjestetty kurssin nimen mukaiseen aakkosjärjestykseen[^3], koska *kurssi* on perusavaimessa partitioavaimen jälkeen. *Listauksessa 8* esiityvä `STATIC`-määre tarkoittaa sitä, että attribuutti talletetaan partitioon ainoastaan yhteen kertaan (esim. tässä *sukunimeä* ei toisteta jokaisen *kurssin* rinnalla).
 
[^3]: *kurssi* on *nimi*- ja *key*-attribuutit sisältävä tyyppi, jossa *nimi* on ensimmäisenä (*Listaus 3*)



{% comment %}

Tietokantataulut ja mallien metodikokonaisuus vastaavat sivukarttaa (*Kuva 2*) - neljä taulua, neljä metodia, neljä sivua. Kaikki sivuilla esitetyt luettelot ovat nousevassa aakkosjärjestyksessä. 

Kussakin metodissa on ainoastaan yksi *CQL*:n *SELECT*-komento. Mikään komennoista ei sisällä *ORDER BY* -lausetta. 


{% endcomment %}





<br/>



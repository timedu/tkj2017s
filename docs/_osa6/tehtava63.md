---
layout: exercise_page
title: "Tehtävä 6.3: Kurssit ja opettajat, Neo4j"
exercise_template_name: # W6E03.KurssitJaOpettajatNeo4j
exercise_discussion_id: 
exercise_upload_id: 
no_review: 1
kesken: 1
julkaisu: 3.10.2017
---


\- draft -
{: style="color:gray; font-size: 80%; text-align: center;"}



![Kurssi-sivu](../img/w5e03-kurssi.png){: style="max-width: 350px; height: auto; float: right;"}k

Laadi sovellus, jolla voidaan tarkastella *kurssi- ja opettajatietoja* sekä *yhteenveto-* että *erittelymuotoisten* näkymien kautta. Edellisten tehtävien ratkaisuista sovelluksen ulkoinen käyttäytyminen poikkeaa siten, että *Kurssi*-sivu esittää kurssien välisiä riippuvuuksia. Tietokantaratkaisuna tässä on [pilvipalvelussa][graphenedb] toimiva [Neo4j][Neo4j]-graafitietokanta.

[graphenedb]: http://www.graphenedb.com
[Neo4j]: https://neo4j.com

*Kurssi*-sivulla kurssien välisiä riippuvuuksia esitetään kolmessa ryhmässä. *Välittömät esitiedot* ovat vastaavia, joita on esitetty opinto-oppaassa. *p*-merkintä esitietona olevan kurssin jäljessä tarkoittaa *pakollista* esitietoa ja *s* suositeltavaa esitietoa.  Luettelo on kurssin nimen mukaisessa aakkosjärjestyksessä kuitenkin niin, että pakolliset esitiedot ovat ennen suositeltavia esitietoja.

*Muut esitiedot* -ryhmässä on kursseja, jotka ovat *välittömien esitietojen* esitietoja. Esim. oheisessa kuvassa *Ohjelmointitekniikka* on *Välittömät esitiedot* -ryhmässä olevan kurssin *Olio-ohjelmointi* esitieto. Siten *Ohjelmointitekniikka* on sivulla kahdessa ryhmässä. *Muut esitiedot* -ryhmässä olevien kurssien osalta ei esitetä *p/s* -merkintöjä. Luettelo on nimen mukaisessa aakkosjärjestyksessä.

*Esitieto kursseille* -otsikon alla on luettelo kursseista, joille tämä kurssi on *välitön* esitieto. Tämän ryhmän kurssien  jäljessä on *p/s* -merkintä. Luettelo on nimen mukaisessa aakkosjärjestuksessä.

### Tietokanta

Tietokanta muodostuu *Opettaja*- ja *Kurssi* -solmuista (*node*) sekä *OPETTAA*- ja *ON_ESITIETO*-kaarista (*arc*). Seuraavissa *Listauksissa 1-4* on esimerkkejä sovellukseen upotetuista[^1] [Cypher][Cypher]-komennoista, joilla tietokanta on perustettu.

[Cypher]: https://neo4j.com/developer/cypher/
[^1]: `configs/db_seed.js`


{% highlight javascript %}

  tx.run("CREATE (:Opettaja {sukunimi: {sn}, etunimi: {en}})", {
    sn: 'Simpsonius',
    en: 'Bartholomeus'
  });

{% endhighlight %}

<small>Listaus 1. *Opettaja* -solmun luonti.</small>



{% highlight javascript %}

  tx.run("CREATE (:Kurssi {tunnus: {t}, nimi: {n}, laajuus: {l}})", {
    t: 'PLA-33300',
    n: 'Johdatus ohjelmistotuotantoon',
    l: '5'
  });

{% endhighlight %}

<small>Listaus 2. *Kurssi* -solmun luonti.</small>



{% highlight javascript %}

  tx.run(" MATCH (o:Opettaja), (k:Kurssi)               \
           WHERE o.sukunimi = {os} AND k.tunnus = {kt}  \
           CREATE (o)-[:OPETTAA]->(k)", {
              kt: 'PLA-32811',
              os: 'Käkilä'
  });

{% endhighlight %}

<small>Listaus 3. *OPETTAA* -kaaren luonti.</small>



{% highlight javascript %}

  tx.run("  MATCH (k1:Kurssi {nimi: {kn1} } ),  \
                  (k2:Kurssi {nimi: {kn2} } )   \
            CREATE (k1)-[:ON_ESITIETO {tyyppi: {ps} }]->(k2)", {
               kn1: 'Ohjelmointitekniikka',
               kn2: 'Olio-ohjelmointi',
               ps:  'p'
  });

{% endhighlight %}

<small>Listaus 4. *ON_ESITIETO* -kaaren luonti.</small>


Tässä käytetään tietokantaa, joka on perustettu [GrapheneDB][graphenedb]-palvelun kautta Amazonin pilveen. Palvelu tarjoaa veloituksella "harrastehiekkalaatikon", johon voi perustaa kooltaan rajoitettuja tietokantoja. Käytettävän tietokannan tunnukset löytyvät [Moodlesta][tunnukset], jotka tulisi kopioida sovelluksen moduuliin `configs/db_connection.js`. Tunnusten ohessa on myös linkki työkaluun, jolla voi *Cypher*-komentojen avulla tutkia tietokannan sisältöä.

[tunnukset]: https://moodle2.tut.fi/mod/page/view.php?id=310616

Tietokantaan on ainoastaan lukuoikeus. Halutessasi voit perustaa palveluun myös oman tietokantasi, jolloin siihen liittyvät tunnisteet tulee kopioda sovelluksen yhteysmoduuliin (`db_connection`). Sovellus tallettaa tiedot tietokantaan käynnistyksen yhteydessä (`db_seed`), jos käytettävässä tietokannassa ei ole solmuja.

Tietokanta näkyy sovelluksen malleissa tunnuksella `db`.

### Toiminnot

Sovelluksen tietokantakäsittely on edellisten tehtävien tapaan keskitetty moduuleihin `models/Opettaja.js` ja `models/Kurssi.js`, joista ensin mainittu toimii esimerkkeinä. Laadittavana on jäkimmäisen moduulin muodostamaan runkoon kaksi metodia, `findAll` ja `findByKey`. 

### Palautettava aineisto

Palauta tehtävän ratkaisuna tiedosto `models/Kurssi.js`. Pohjakoodi ei sisällä testejä, joten varmista sovelluksen toiminta sitä ajamalla. Yhtenä testitapauksena voi toimia tämän sivun alussa oleva kuva. 

{% comment %}

Tietokantaan on lisätty (vrt. esim. tehtävä 5.1) kurssi "Johdatus ohjelmistotuotantoon" ja opettaja "Simpsonius, Bartholomeus"; näiltä osin ao. testikoodia on muokattava, jos sitä hyödynnetään tämän tehtävän yhteydessä

{% endcomment %}


### Vinkkejä ja lisätietoja

[Neo4j][Neo4j]-tietokantaa käsitellään hieman SQL:ää muistuttavalla [Cypher][Cypher] -kielellä, jonka dokumentaatiosta löytyy mm. opas [From SQL to Cypher – A hands-on Guide][sql-cypher]. Tiivis yhteenveto kielestä on sen [referenssikortissa][ref-card].

[sql-cypher]: https://neo4j.com/developer/guide-sql-to-cypher/
[ref-card]: https://neo4j.com/docs/cypher-refcard/current/

Sovelluohjelmassa Cypher-komennot välitetään Neo4j:lle käyttäen ajuria, joka tarjoaa rajapinnan tietokantaan. Tässä käytetään "virallista" Neo4j-tietokannan [node-ajuria][node-ajuri].  

[node-ajuri]: http://neo4j.com/docs/api/javascript-driver/current/

#### Pohjakoodista poimittuja esimerkkejä

Sovelluksen moduuli `configs/db_connection.js` muodostaa ja julkistaa [`Driver`][Driver]-olion:  

[Driver]: http://neo4j.com/docs/api/javascript-driver/current/class/src/v1/driver.js~Driver.html


{% highlight javascript %}

// ...
const driver = neo4j.driver(URL, neo4j.auth.basic(USER, PWD));
module.exports = driver;

{% endhighlight %}

<small>Listaus 5. </small>


Mallit ottavat moduulin käyttöönsä siten, että *driver* näkyy tunnisteessa `db`.


{% highlight javascript %}

// ...
const db = require('../configs/db_connection');
// ...

{% endhighlight %}

<small>Listaus 6. </small>


*Driver*-olion avulla muodostetaan [`Session`][Session]-olio, jonka `run`-metodilla voidaan toimittaa *Cypher* -komento tietokannalle:

[Session]: http://neo4j.com/docs/api/javascript-driver/current/class/src/v1/session.js~Session.html

{% highlight javascript %}

   const session = db.session();
   session.run("MATCH (o:Opettaja) RETURN o ORDER BY o.sukunimi").then((result) => {
      // ...
      session.close();
   }).catch((error) => {
      console.log(error);
      // ..
   });

{% endhighlight %}

<small>Listaus 7. </small>


Edellisen listauksen moduulista `models/Opettaja.js` poimittu komento tuottaa aakkosjärjestyksessä oleva opettajaluettelon. *MATCH* määrittelee, mihin solmuihin komento kohdistuu; tässä solmut, jotka on otsikoitu tunnisteella *Opettaja* (vrt. *Listaus 1*). Komennon muuttuja *o* viittaa näihin solmuihin, jotka sitten *RETURN* palauttaa sovellukselle. *ORDER BY* täsmentää, että palautettavan tulostaulukon solmut ovat *sukunimi* -ominaisuuden mukaisessa järjestyksessä.

Seuraavan *listauksen  8* *Cypher*-komento on edellistä hieman monimutkaisempi. Myöskin tämä on poimittu moduulista `models/Opettaja.js`.  Komento tuottaa tietyn opettajan tiedot ja sen lisäksi kaikki opettajan pitämät kurssit siten, että kurssit ovat nimen mukaisessa aakkosjärjestyksessä. 

{% highlight javascript %}

   const session = db.session();
   session.run("  MATCH (o:Opettaja)                           \
                  WHERE ID(o) = toInt({id})                    \
                  OPTIONAL MATCH (o)-[:OPETTAA]-> (k:Kurssi)   \
                  RETURN o, k                                  \
                  ORDER BY k.nimi", {
      id: opettaja_key
   }).then((result) => {
      // ...
      session.close();
   }).catch((error) => {
      console.log(error);
      // ...
   });

{% endhighlight %}

<small>Listaus 8. </small>

*Listauksessa 8* `run`-metodilla on kaksi argumettia, joista jälkimmäinen on objekti sisältäen parametreja *Cypher* -komennolle (tässä tosin parametreja on vain yksi). Komennon *MATCH* määrittelee edellisen *listauksen 7* tapaan, että käsittelyn lähtökohtana on *Opettaja* -otsikolla varustetut solmut. *WHERE* kuitenkin täsmentää, että käsittelyyn otetaan ainoastaan yksi solmu, jonka tietokantatunniste annetaan komennolle parametrina. 


*WHERE*-osan  jälkeen komennossa on jo valitun solmun (opettaja) kannalta valinnainen, *OPTIONAL MATCH*, joka poimii tulostaulukkoon opettajien rinnalle kaikki opettajan kurssit. Tässä käytetään hyväksi *Opettaja*- ja *Kurssi*-solmujen välille määriteltyjä *OPETTAA*-kaaria (vrt. *Listaus 3*). Valinnaisuus *MATCH*:in osalta tarkoittaa sitä, että opettaja saadaan tulostaulukkoon, vaikka opettajalla ei olisi yhtään kurssia[^2].

[^2]: vastaa SQL:n ulkoliitosta

*Session* -olion `run` -metodi palauttaa [`Promise`][Promise] -olion, jonka `then` -metodin parametriksi annettu funktio suoritetaan, kun tietokannasta pyydetty data on käytettävissä. Funktion parametrina saadaan [`Result`][Result] -olio, jonka `records` -ominaisuus on taulukko alkioinaan tietokannasta haetut tiedot sisältävät [`Record`][Record] -oliot. 

[Promise]:  https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

[Result]: http://neo4j.com/docs/api/javascript-driver/current/class/src/v1/result.js~Result.html

[Record]: http://neo4j.com/docs/api/javascript-driver/current/class/src/v1/record.js~Record.html

*Listauksessa 9* on koodi, jolla *Listauksen 7* `result` -olion perusteella voidaan muodostaa halutun muotoinen taulukko opettajista. Taulukon alkio on seuraavanlaista muotoa: `{key: '99', sukunimi: 'Simpsonius', etunimi: 'Bartholomeus'}`.


{% highlight javascript %}

  const opettajat = [];

  result.records.forEach((record) => {
    var opettaja = Object.assign({}, record.get(0).properties);
    opettaja.key = record.get(0).identity.toString();
    opettajat.push(opettaja);
  });

{% endhighlight %}

<small>Listaus 9. </small>

*Cypher*-komennon *RETURN* -lauseen palauttamaa tietoa voidaan lukea *Record* -olion `get`-metodilla, jonka parametrilla viitataan luettavan tiedon sijaintiin *RETURN*-lauseessa[^3]. *Listauksen 7* *RETURN* palauttaa *Opettaja*-solmuja, joita sovelluksessa vastaa [`Node`][Node]-olio. Sen `properties` -attribuutti sisältää solmuun talletetut avain-arvoparit (vrt. *Listaus 1*) ja `identity` -ominaisuus talletuksen yhteydessä automaattisesti muodostuvan solmun tietokantatunnisteen. 

[^3]: sijainnin ohella voidaan viitata myös tiedon tunisteeseen

[Node]: http://neo4j.com/docs/api/javascript-driver/current/class/src/v1/graph-types.js~Node.html


Tehtävän pohjakoodissa on apufunktio `arrify`, jota käytäen *Listauksen 9* koodi voidaan korvata seuraavalla:


{% highlight javascript %}

  const opettajat = arrify(result.records);

{% endhighlight %}

<small>Listaus 10. </small>

*Listauksen 8* `result`-oliosta opettaja kursseineen voidaan poimia sopivaan muotoon `arrify`-funktiota käyttäen seuraavasti:


{% highlight javascript %}

  const opettaja = arrify(result.records)[0];
  opettaja.kurssit = arrify(result.records, 1);

{% endhighlight %}

<small>Listaus 11. </small>

#### Tehtävän toteutuksesta

Tehtävässä laaditaan kurssi-tietohin kohdituvat kyselyt moduulin `models/Kurssi.js` funktiorunkoihin, `findAll` ja `findByKey`, joista jälkimmäisen toteutus edellyttää kaarien (*OPETTAA*, *ON_ESITIETO*) hyödyntämistä kyselyn toteutuksessa. Kyselyn rakentaminen onnistunee yhdellä *Cypher*-komennolla, mutta toteutus linee selkeämpi[^4], jos ongelman ratkaisun perustaa useampaan *Cypher*-komentoon. Komentoja voi olla esim. neljä: 1) kurssi ja sen opettaja, 2) kurssin välittömät esitiedot, 3) kurssin muut esitiedot, 4) kurssit, joille tämä kurssi on esitietona. Tällöin komennoista muodostuu seuraavanlainen ketju:

[^4]: tässä ei oteta kantaa suorituskykykysymyksiin


{% highlight javascript %}

session.run("hae kurssi ja opettaja", {/* par */}).then((result1) => {
  // ...
  session.run("hae välittömät esitiedot", {/* par */}).then((result2) => {
    // ...
    session.run("hae muut esitiedot", {/* par */}).then((result3) => {
      // ...
      session.run("hae kurssit, joille tämä esitietona", {/* par */}).then((result4) => {
        // ...
      }).catch( error => {/*...*/});        
      // ...
    }).catch( error => {/*...*/});    
    // ...
  }).catch( error => {/*...*/});
  // ...
}).catch((error) => {/*...*/});

{% endhighlight %}

<small>Listaus 12. </small>

Kaikki *listauksen 12* komennot voidaan myös suorittaa rinnakkain, koska minkään komennon valintaehto ei perustu edellisen komennon tulokseen. Tosin, jos käy niin, että komento *hae kurssi ja opettaja* ei palautta kurssia, jälkimmäisiä komentoja ei tarvitse suorittaa. Täten *listauksen 13* komennot voisi järjestää niin, että *hae kurssi ja opettaja* suoritetaan ensin ja muut rinnakkain sen jälkeen:

{% highlight javascript %}

session.run("hae kurssi ja opettaja", {/* par */}).then((result1) => {
  // ...
  Promise.all([
    session.run("hae välittömät esitiedot", {/* par */}),
    session.run("hae muut esitiedot", {/* par */}),
    session.run("hae kurssit, joille tämä esitietona", {/* par */})
  ]).then( resultArr => {
    const result2 = resultArr[0],
    const result3 = resultArr[1],
    const result4 = resultArr[2],
    // ...
  }).catch( error => {/*...*/});      
  // ...
}).catch((error) => {/*...*/});

{% endhighlight %}

<small>Listaus 13. </small>

Tehtävän pohjakoodin moduulissa `configs/db_seed.js` on CSV-tiedostojen lukeminen toteutettu *Listauksen 13* tapaan hyödyntäen [`Promise.all`][Promise.all] -metodia.

[Promise.all]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all

*Listauksen 13* komennot "*hae välittömät esitiedot*" ja "*hae kurssit, joille tämä esitietona*" palauttavat dataa, joka on talletettuna *solmujen* ohella *kaariin* (ks. *Listaus 4*). Tältä osin *kaaria* käsitellään *solmujen* tapaan:


{% highlight javascript %}

  session.run("MATCH (k1)-[e:ON_ESITIETO]->(k2) RETURN e, k1, k2");

{% endhighlight %}

<small>Listaus 14. </small>

Edellisen listauksen komento palautaa tietokannan kaikki *ON_ESITIETO* -riippuvuudet (*kaaret*) kursseineen. Listauksessa *kaaren* otsikon edessä on tunniste *e*, joka viittaa tietokannan kaariin. Komennon *RETURN* -lause palauttaa siten *solmujen* ohella myös *kaaren*.

Ohjelmassa *kaarta* edustaa [`Relationship`][Relationship]-olio, jolla on ominaisuudet `identity` ja `properties` kuten edellä esillä olleella [`Node`][Node]-oliollakin.
   
[Relationship]: http://neo4j.com/docs/api/javascript-driver/current/class/src/v1/graph-types.js~Relationship.html

*Listauksissa 12 ja 13* esiintyvän komennon "*hae muut esitiedot*" toteutus edellyttää, että esitieto-polkua seurataan yhtä *ON_ESITIETO*-kaarta etäämmälle. Tällaisen komennon rakentamiseen antanee tukea esim. *Cypherin* [referenssikortin][ref-card] kohta *Patterns*, josta löytyy mm. seuraava esimerkki: `(n)-[*1..5]->(m)` ("*Variable length path of between 1 and 5 relationships from n to m.*").

#### Muutoksia

* 170210: *Vinkkejä ja lisätietoja* -kohtaan lisätty alakohta *Tehtävän toteutuksesta*


<br/>


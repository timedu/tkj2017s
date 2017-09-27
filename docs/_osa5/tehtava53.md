---
layout: exercise_page
title: "Tehtävä 5.3: Kurssit ja opettajat, Cassandra CRUD (3p)"
exercise_template_name: W5E03.KurssitJaOpettajatCassandraCRUD
exercise_discussion_id: 85228
exercise_upload_id: 342882
---

Täydennä [edellisen tehtävän](../tehtava52) ratkaisuasi opettajatietoihin liittyvillä ylläpitotoiminnolla, *lisäys*, *muutos* ja *poisto*. Tietokanta tässä on aivan sama kuin [Tehtävässä 5.2](../tehtava52). Käyttäjän näkökulmasta ylläpito tapahtuu [Tehtävän 2.3](../../osa2/tehtava23) sivukartan (*kuva 2*) mukaan.

Sovelluksen lähdekoodi jäsentyy edellisten tehtävien tapaan.  Kyselyt toteuttavat moduulit `opettajaController.js` ja `kurssiController.js` voi kopioida edellisen tehtävän ratkaisusta. Laadittavaksi tässä jää moduuli  `opettajaControllerCUD.js`, joka tosin sisältää jo ylläpidossa tarvittavat CQL-komennot.

Kontrollerit ottavat moduulin `config/db_connection.js` käyttöönsä siten, että tietokantarajapinta näkyy tunnisteessa `db`.

Tietokannassa on neljä taulua `opettajat`, `opettaja_list`, `kurssit` ja `kurssi_list` (ks. [edellinen tehtävä](../tehtava52#tietokannnan-rakenne)), joista opettajatietojen ylläpito kohdistuu kolmeen ensin mainittuun tauluun. 

Tietokannan toisteisuudesta johtuen kukin ylläpito-toiminto kohdistuu yhtä useampaan tauluun. Opettajan *lisäys* tapahtuu sekä `opettajat`- että `opettaja_list`-tauluun. 
*Muutos* ja *poisto* kohdistuvat edellisten lisäksi myös `kurssit`-tauluun.
Opettajan muutoksen yhteydessä on huomioitava, että `opettaja_list`-tauluun kohdistuu *UPDATE*-operaation sijaan *DELETE*- ja *INSERT* -operaatiot, koska muutettava tieto on osana perusavainta.


**Palauta** tehtävän ratkaisuna tiedosto `opettajaControllerCUD.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan, ajaen sovellusta sekä käyttäen oheistettuja Selenium-testejä[^4]. 

[^4]: Nämä testaavat sovelluksen ulospäin näkyvää toiminnallisuutta eivätkä siten paljasta sisäisiä vikoja.


### Vihjeitä ja lisätietoja

Tietokantaan kohdistuvat ylläpito-operaatiot voi suorittaa kyselyjen tapaan rajapinnan [client][class.Client] -olion [execute][execute] -metodilla, jolle annetaan parametrina [CQL][CQL]:n *INSERT*-, *UPDATE*- tai *DELETE* -komentoja. Tässä kuitenkin jokainen ylläpitotoiminto muodostuu useasta operaatiosta, jolloin operaatioiden suorittamisessa tulisi käyttää [batch][batch] -metodia, jonka avulla operaatioita voidaan niputtaa yhteen.

Tehtävän pohjakoodin moduulissa `configs/db_create.js` tiedot tietokantaan talletetaan *batch*-metodilla. Seuraavassa on koodista poimintoja, joissa on esillä myös yksilöllisen avaimen generointi lisättävälle tiedolle.


[class.Client]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/
[execute]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#execute
[batch]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#batch
[CQL]: http://docs.datastax.com/en/cql/3.1/index.html


{% highlight javascript %}

// ...
const Uuid = require('cassandra-driver').types.Uuid;
const db = require('./db_connection');
// ...

// --
// komento, joka suoritetaan osana nippua (merkkijono);
// '?'-merkit korvataan parametreilla
// 
const INSERT_opettaja_list = "\
INSERT INTO opettaja_list (block_id, sukunimi, etunimi, key) \
VALUES (1, ?, ?, ?)";
// ...

// --
// taulukko, johon talletetaan nipussa suoritettavat operaatiot
// 
const InsertRows = [];
// ...

// --
// yksilöivän avaimen generointi 
//
const opettaja_key = Uuid.random();

// --
// alkion lisääminen taulukkoon, joka annetaan parametrina 
// batch-metodille; alkio on objekti, jonka query-attribuutissa on
// suoritettava komento ja params-attribuutissa komennolle annettavat
// parametrit
// 
InsertRows.push({
   query: INSERT_opettaja_list,
   params: [
      opettaja.sukunimi,
      opettaja.etunimi,
      opettaja_key
   ]
});
// ...

// --
// batch-metodin kutsu; parametrina komennot sisältävä taulukko 
//
db.batch(InsertRows).then( () => {
   log('rows inserted');
}).catch( err => {
   log(err);
});
// ...

{% endhighlight %}

<small>Listaus 1. Poimintoja moduulista *config/db_create.js*</small>


<br/>




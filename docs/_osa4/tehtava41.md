---
layout: exercise_page
title: "Tehtävä 4.1: Kurssit ja opettajat, MongoDB 1 (3p)"
exercise_template_name: W4E01.KurssitJaOpettajatMongo1
exercise_discussion_id: 84710
exercise_upload_id: 341905
---

Laadi [tehtävän 3.2](../../osa3/tehtava32) ratkaisua vastaava sovellus kuitenkin niin, että avain-arvoparitietokannan korvaa vastaava, pilvipalvelussa toimiva, [MongoDB][MongoDB] -dokumenttitietokanta. 

[MongoDB]: https://www.mongodb.com


Sovelluksen lähdekoodi jäsentyy tässä edellisten tehtävien tapaan. Moduuleja `kurssiController.js` ja `opettajaController.js` lukuunottamatta sovellus on rakennettu valmiiksi. Näistä jälkimmmäinen sisältää jo koodin, joka tuottaa näkymää varten  yksittäisen opettaja tiedot (ml. opettajan kurssit).

Mongo-tietokanta muodostuu hieman relaatiotietokannan tauluja vastaavista dokumenttikokoelmista. Tietokanta rakentuu tässä seuraavasti:

~~~
  +----------------+     +----------------+
  |     kurssit    |     |   opettajat    |
  +----------------+     +----------------+
  | _id            |     | _id            |
  | tunnus         |     | etunimi        |
  | nimi           |     | sukunimi       |
  | laajuus        |     +----------------+
  | opettaja_id    |
  +----------------+
~~~
<small>Kuva 1. Tietokannan rakenne</small>

Tietokanta on jäsennetty samoin kuin se tyypillisesti tehtäisiin käytettäessä relaatiotietokantaa. Tietokannassa on kaksi dokumenttikokoelmaa, jotka tässä vastaavat relaatiotietokannan tauluja. Kokoelmien dokumenteilla on järjestelmän automaattisesti generoima yksilöllinen avain, `_id`.  *Kurssi*-kokoelman dokumenteilla on `opettaja_id`-attribuutti, jonka sisältönä on kurssin opettajan `_id`. Järjestelmä ei mitenkään ylläpidä tätä yhteyttä (vs. relaatiotietokannan *primary key* - *foreign key* -riippuvuudet).

Tietokantayhteys määritellään moduulissa `config/db_connection.js` olevilla tunnuksilla. Niiden arvot löytyvät [Moodlesta][tunnisteet], jotka tulee siis kopioida ao. kohtaan em. mooduulissa. Tunnisteiden avulla voidaan ottaa yhteys [mLab][mLab]:in palveluun perustettuun tietokantaan. Palvelu tarjoaa veloituksetta kokeiluja varten 500 MB "hiekkalaatikon".

[tunnisteet]: https://moodle2.tut.fi/mod/page/view.php?id=329046
[mLab]: https://mlab.com

Pohjakoodi on rakennettu siten, että yhteys tietokantaan muodostetaan ennen kutakin tietokantaan kohdistuvaa kyselyä, jonka suorittamisen jälkeen yhteys  tulisi sitten sulkea. Kontrollereissa tietokantakäsittely voidaan suorittaa seuraavan rungon mukaan:


{% highlight javascript %}


// mongo on funktio, joka ottaa yhteyden tietokantaan ja
// kutsuu sille parametrina annettua funktiota
const mongo = require('../config/db_connection');

// mongo-funktion kutsu; 
// parametrina funktio, joka toteuttaa tietokantakäsittelyn;
// parametri db on ohjelmointirajapinnan objekti 
mongo(function (db) {
    // tietokantaan liittyvä rajapinnan metodikutsu (esim. kysely)
          // palautetun datan käsittely callback-funktiossa (esim. render)
          // tietokantayhteyden sulkeminen
          db.close();     
 });


{% endhighlight %}

<small>Listaus 1.</small>

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.


### Vihjeitä ja lisätietoja

Edellä olevassa *Listauksessa 1* esiintyvä `db` on Mongon [ohjelmointirajapinnan][mongo-api] [`Db`][Db]-objekti, jonka `collection`-metodi palauttaa tietokannan kokoelmaa vastaavan [`Collection`][Collection]-objektin. Tässä tarvitaan sen `findOne`- ja `find`-metodeja. Seuraavassa listauksessa on tehtäväpohjassa esimerkkinä oleva yksittäisen opettajan tietojen luku tietokannasta:

[mongo-api]: http://mongodb.github.io/node-mongodb-native
[Db]: http://mongodb.github.io/node-mongodb-native/2.2/api/Db.html
[Collection]: http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html


{% highlight javascript linenos %}

app.get('/opettajat/:id', function (req, res) {
    // suoritetaan tietokannan käsittelyyn liittyvä funktio
    mongo(function (db) {
        // kokoelmia vastaavat objektit
        const opettajat = db.collection('opettajat');
        const kurssit = db.collection('kurssit');
        // parametrina saatu hakuehtona käytettävä id on merkkijono;
        // muunnetaan merkkijono mongon edellyttämäksi objektiksi
        const opettajaId = ObjectID(req.params.id);
        // haetaan opettaja valintaehtona _id
        opettajat.findOne({'_id': opettajaId}, function (err, opettaja) {
            // haetaan opettajan kurssit
            // ... nimen mukaan lajiteltuna
            // ... taulukkomuodossa
            kurssit.find({'opettaja_id': opettajaId})
                    .sort({'nimi': 1})
                    .toArray(function (err, kurssit) {
                        // paketoidaan kurssit opettajan oheen
                        opettaja.kurssit = kurssit;
                        // hahmonnetaan näkymä edellä luetuilla tiedoilla
                        res.render('opettaja_detail', {
                            opettaja: opettaja
                        });
                        // suljetaan tietokantayhteys
                        db.close();
                    });
        });
    });
});
{% endhighlight %}

<small>Listaus 2.  Yksittäisen opettajan tietojen luku tietokannasta.</small>

Pelkistetty avain-arvoparitietokanta ei tunne tietokantaan talletettujen arvojen sisäistä rakennetta, mutta tässä käytetty dokumenttitietokanta tuntee, mitä ilmentää esim. *Lisauksessa 2* *Rivillä 15* oleva valintaehto sekä *Rivillä 16* esiintyvä lajittelu.


<br/>

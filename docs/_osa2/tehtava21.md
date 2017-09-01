---
layout: exercise_page
title: "Tehtävä 2.1: Kurssit ja opettajat, SQL (3p)"
exercise_template_name: W2E01.KurssitJaOpettajatSQL
exercise_discussion_id: 
exercise_upload_id: 
kesken: 1
julkaisu: 4.9.2017
no_review: 1
---

Tässä tehtävässä käsitellään relaatiotietokantaa SQL-kielellä ohjelmointirajapinnan kautta. Tietokantana on [SQLite][SQLite] ja ohjelmointitajapintana [node-sqlite3].  

[SQLite]: https://www.sqlite.org
[node-sqlite3]: https://github.com/mapbox/node-sqlite3/blob/master/README.md

Pohjakoodissa oleva tietokanta sisältää kaksi tietokantataulua: *Kurssi* ja *Opettaja*. Opettajalla voi olla useita kursseja, mutta kurssilla on enintään yksi opettaja. Laadi sovellus, jolla voidaan tarkastella tietokannan sisältämää tietoa. Sovellus käyttäytyy seuraavan sivukartan (Kuva 1) mukaan.


![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 1. Sivukartta</small>

Tehtäessä pyyntö polkuun `/kurssit`, tulee esiin *Kurssit* -sivu, joka esittää luettelon tietokannan sisältämistä kursseista nimen mukaisessa aakkosjärjestyksessä. Kurssin nimi toimii samalla linkkinä, jonka klikkaus tuo esiin *Kurssi* -sivun sisältäen yksittäisen kurssin tiedot. *Kurssi*-sivulle voidaan siirtyä myös suoraan tekemällä pyyntö seuraavanlaiseen polkuun: `/kurssit/PLA-32820` (polun loppuosa on kurssin tunnus). *Kurssi*-sivu esittää myös kurssin opettajan nimen, joka toimii samalla linkkinä *Opettaja* -sivulle.

Tehtäessä pyyntö polkuun `/opettajat`, tulee esiin *Opettaja* -sivu, joka esittää luettelon tietokannan sisältämistä opettajista sukunimen mukaisessa aakkosjärjestyksessä. Opettajan nimi toimii samalla linkkinä, jonka klikkaus tuo esiin *Opettaja* -sivun sisältäen yksittäisen opettajan tiedot. *Opettaja*-sivulle voidaan siirtyä myös suoraan tekemällä pyyntö seuraavanlaiseen polkuun: `/opettajat/5` (polun loppuosa on opettajan id). *Opettaja*-sivu esittää myös aakkosjärjestyksessä olevan luettelon opettajan kursseista, jotka toimivat samalla linkkeinä *Kurssi* -sivuille.

Sovelluksen lähdekoodi jäsentyy seuraavasti:

~~~
Sources
 ├── main.js
 ├── config
 │     ├── db_connection.js 
 │     ├── db_create.js 
 │     ├── db_data.js 
 ├── controllers
 │     ├── kurssiController.js 
 │     └── opettajaController.js 
 └── views
     ├── kurssi_detail.hbs
     ├── kurssi_list.hbs
     ├── opettaja_detal.hbs
     ├── opettaja_list.hbs
     └── layouts
          └── main.hbs                 
~~~

<small>Kuva 2. Sovelluksen lähdekoodi</small>

Kontrollereita lukuunottamatta sovellus on jo rakennettu valmiiksi. Kontrollereihin sisällytetään tietokantakäsittely, jota pohjassa on jo aloitettu: 1) *Kurssi*-sivu esittää yksittäisen kurssin tiedot - esim. pyynnön polkuun `/kurssit/PLA-32820` pitäisi esittää *Kurssit*-sivulla  *Mobiiliohjelmointi* -kurssin tiedot; 2) *Opettajat*-sivu esittää luettelon kaikista opettajista tehtäessä pyyntö osoitteeseen `/opettajat`. 

Pohjakoodi on rakennettu niin, että sovelluksen käynnistyksen yhteydessä muodostuu muistinvarainen tietokanta, johon otetaan yhteys (`db_connection.js`). Moduuli `db_create.js` perustaa tietokantataulut ja tallettaa niihin esimerkkidatan, joka on määritelty moduulissa `db_data.js`.

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.


### Vihjeitä ja lisätietoja

#### opettajaController

Seuraavassa *listauksessa 1* on ote tehtäväpohjassa olevan tiedoston `opettajaController.js` koodista.


{% highlight javascript %}
...

module.exports = function (app) {

    app.get('/opettajat', function (req, res) {
        db.all(FindAllOpettajat, function (err, rows) {
            res.render('opettaja_list', {
                opettajat: rows
            });
        });
    });
    
    app.get('/opettajat/:id', function (req, res) {
        res.render('opettaja_detail');
    });
};

{% endhighlight %}

<small>Listaus 1.</small>


Tiedosto on Node-moduuli, joka siten julkaisee ulospäin sen, joka sijoitetaan moduulin koodissa muuttujan `module.exports` arvoksi. Tässä arvoksi sijoitetaan funktio. Sovelluksen päämoduuli `main.js` ottaa moduulin käyttöön seuraavasti:


{% highlight javascript %}
...
require('./controllers/opettajaController')(app);
...

{% endhighlight %}

<small>Listaus 2.</small>


*Listauksessa 2* `require('./controllers/opettajaController')` edustaa siis funktiota, joka *Listauksessa 1* sijoitetaan muuttujan `module.exports` arvoksi. Siten `require('./controllers/opettajaController')(app)` on tämän funktion kutsu. Parametrina oleva `app` on *Express*-sovelluskehyksen tarjoama [Application][Application]-objekti. Sen avulla voidaan mm. määritellä eri polkuihin tuleviin pyyntöihin liittyvä käsittely, joka tässä tehtävässä tapahtuu moduuleissa `opettajaControlle.js` ja `kurssiController.js`.

[Application]: http://expressjs.com/en/4x/api.html#app


Seuraavassa *listauksessa 3* moduulista `opettajaController.js` on ote, joka liittyy opettajaluettelon tuottamiseen.


{% highlight javascript linenos %}

const db = require('../config/db_connection');

const FindAllOpettajat = '\
    SELECT id, sukunimi, etunimi    \
    FROM opettaja                   \
    ORDER BY sukunimi';

module.exports = function (app) {

    app.get('/opettajat', function (req, res) {

        db.all(FindAllOpettajat, function (err, rows) {

            res.render('opettaja_list', {
                opettajat: rows
            });
        });
    });
};


{% endhighlight %}

<small>Listaus 3.</small>

Opettajaluettelo tuotetaan vasteena polkuun `/opettajat` tulevaan pyyntöön, mikä on määritelty listauksen rivillä 10 olevalla metodikutsulla: `app.get('/opettajat', function (req, res) {..})`. Pyyntöön vastataan välittämällä tietokannalle kysely ohjelmointirajapinnan kautta (Listaus 3 / rivi 12) :  

{% highlight javascript %}

db.all(FindAllOpettajat, function (err, rows) {
  ...
});

{% endhighlight %}

<small>Listaus 4.</small>


*Listauksen 3 rivillä 1* otetaan käyttöön moduulissa `db_connection.js` määritelty tietokantayhteys. Sen myötä moduulissa on objekti db, jonka kautta voidaan käsitellä tietokantaa. Käytettävissä olevat metodit löytyvät SQLite-tietokannan Node-sovelluksille tarkoitetun ohjelmointirajapinnan (API) [dokumentaatiosta][api-doc]. 

Tässä käytetään [`all`][all]-metodia, jolle annetaan kaksi parametria: SQL-komento ja funktio. SQL-komento on muuttujassa `FindAllOpettajat`, johon asetetaan arvo *Listauksen 3 rivillä 3*. Rajapinta kutsuu funktiota SQL-komennon suorituksen tuloksen palauttamiseksi. Funktion ensimmäinen parametri (tässä: `err`) sisältää tietoa mahdollisesta virheestä[^fn1] ja toinen parametri (tässä: `rows`) kyselyn tuloksen taulukkona, joka välitetään näkymälle (*Listaus 3: Rivi 14*). 

[api-doc]: https://github.com/mapbox/node-sqlite3/wiki/API
[all]: https://github.com/mapbox/node-sqlite3/wiki/API#databaseallsql-param--callback

[^fn1]: Tässä ei tarvitse toteuttaa virheenkäsittelyä

#### kurssiController

...

{% comment %}

{% highlight javascript %}


{% endhighlight %}

<small>Listaus 4.</small>


Moduulista näkyy ulospäin se, joka sijoitetaan muuttujan `module.exports` arvoksi eli tässä funktio, joka määrittelee kahteen polkuun liittyvät funktiot. Nämä siis suoritetaan kun ao. polkuun tulee pyyntö. Koodissa `(app) => { ... }` voisi olla myös `function(app) { ... }`. Sovelluksessa sen käynnistyksen yhteydessä `config.js` kutsuu tämän moduulin tarjoamaa funktiota `app`-parametrilla. 


Polkuun `/kurssit/:tunnus` liittyvä funktio (*Listaus 2*) on koodissa laadittu valmiiksi. Polun osana on muuttuja, `:tunnus`, joka saadaan funktion käyttöön viittauksella  `req.params.tunnus`. 


{% highlight javascript %}

    app.get('/kurssit/:tunnus', function (req, res) {
    
        global.db.get(SelectOneKurssi, req.params.tunnus, (err, row) => {
        
            res.render('kurssi', {
                kurssi: row
            });
            
        });
        
    });


{% endhighlight %}

<small>Listaus 2.</small>


Funktio toteuttaa tietokantakyselyn (`db.get`) ja sitten, kun kyselyn palauttama tulos on käytettävissä, hahmontaa (`render`) tietokannasta luetut tiedot sivulle. Viite tietokantayhteyteen (`db`) on sovelluksessa asetettu[^1] sovelluksen globaalin objektin 
(`global`) ominaisuudeksi. 

[^1]: Tämän tekee `db_config.js`.


[Tietokantarajapinnan][node-sqlite3] [`get`][get] -metodilla on tässä kolme parametria: 1) sql-komento, 2) sql-komennon parametri, ja 3) funktio, joka suoritetaan, kun data tietokannasta on saatu. Data saadaan funktioon sen toisen parametrin kautta objektimuodossa.

[node-sqlite3]: https://github.com/mapbox/node-sqlite3
[get]: https://github.com/mapbox/node-sqlite3/wiki/API#databasegetsql-param--callback

Sql-komento on moduulissa asetettu `SelectOneKurssi`-vakion arvoksi:


{% highlight javascript %}

const SelectOneKurssi = '                       \
SELECT                                          \
    k.*,                                        \
    o.etunimi AS opettaja_etunimi,              \
    o.sukunimi AS opettaja_sukunimi             \
FROM kurssi AS k LEFT OUTER JOIN opettaja AS o  \
    ON k.opettaja_id = o.id                     \
WHERE k.tunnus = ?';

{% endhighlight %}

<small>Listaus 3.</small>


*Listauksessa 3* olevassa sql-komennossa[^2] `?` on paikka parametrille, jonka arvoksi siis asettuu tässä `req.params.tunnus`.

[^2]: SQLite:n  sql-komennot: <https://www.sqlite.org/lang.html>

Tässä käytetään rajapinnan [`get`][get] -metodia, koska kysely palauttaa taulusta ainoastaan yhden rivin. Jos kysely palautta monta riviä, on käytettävä [`all`][all]-metodia.

[all]: https://github.com/mapbox/node-sqlite3/wiki/API#databaseallsql-param--callback

{% endcomment %}


<br/>



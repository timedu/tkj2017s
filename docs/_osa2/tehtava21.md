---
layout: exercise_page
title: "Tehtävä 2.1: Kurssit ja opettajat, SQL (3p)"
exercise_template_name: W2E01.KurssitJaOpettajatSQL
exercise_discussion_id: 83184
exercise_upload_id: 338810
---

Tässä käsitellään relaatiotietokantaa SQL-kielellä ohjelmointirajapinnan kautta. Tehtävänä on täydentää hieman keskeneräiseksi jäänyt tietokantasovellus. Tietokantana on [SQLite][SQLite] ja ohjelmointitajapintana [node-sqlite3].  

[SQLite]: https://www.sqlite.org
[node-sqlite3]: https://github.com/mapbox/node-sqlite3/blob/master/README.md

Pohjakoodissa oleva tietokanta sisältää kaksi tietokantataulua: *Kurssi* ja *Opettaja*. Opettajalla voi olla useita kursseja, mutta kurssilla on enintään yksi opettaja. Laadi sovellus, jolla voidaan tarkastella tietokannan sisältämää tietoa. Sovellus käyttäytyy seuraavan sivukartan (Kuva 1) mukaan.

{% comment %}

![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

{% endcomment %}


![sivukartta](../img/w2e01.png){: style="display: block; margin: auto; margin-top: 10px; width: 350px;"}

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
 │     └── db_data.js 
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

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi[^0a]. Sovelluksen on oltava käynnissä testejä ajettaessa.

[^0a]: Jos sovellus vaikuttaa toimivan kuvauksen mukaan, mutta testit tuottavat runsaasti virheilmoituksia, kannattaa tarkistaa ainakin, onko luetteloiden lajittelujärjestys odotusten mukainen (huom. myös opettajakohtainen kurssiluettelo).


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

<small>Listaus 1. Ote moduulista *opettajaController.js*</small>


Tiedosto on Node-moduuli, joka siten julkaisee ulospäin sen, joka sijoitetaan moduulin koodissa muuttujan `module.exports` arvoksi. Tässä arvoksi sijoitetaan funktio. Sovelluksen päämoduuli `main.js` ottaa moduulin käyttöön seuraavasti:


{% highlight javascript %}
...
require('./controllers/opettajaController')(app);
...

{% endhighlight %}

<small>Listaus 2. Moduulin *opettajaController.js* käyttöönotto moduulissa *main.js*</small>


*Listauksessa 2* `require('./controllers/opettajaController')` edustaa siis funktiota, joka *Listauksessa 1* sijoitetaan muuttujan `module.exports` arvoksi. Siten `require('./controllers/opettajaController')(app)` on tämän funktion kutsu. Parametrina oleva `app` on *Express*-sovelluskehyksen tarjoama [Application][Application]-objekti. Sen avulla voidaan mm. määritellä eri polkuihin tuleviin pyyntöihin liittyvä käsittely, joka tässä tehtävässä tapahtuu moduuleissa `opettajaControlle.js` ja `kurssiController.js`.

[Application]: http://expressjs.com/en/4x/api.html#app

*Opettajaluettelo*

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

<small>Listaus 3. Ote moduulista *opettajaController.js*</small>

Opettajaluettelo tuotetaan vasteena polkuun `/opettajat` tulevaan pyyntöön, mikä on määritelty listauksen rivillä 10 olevalla metodikutsulla: `app.get('/opettajat', function (req, res) {..})`. Pyyntöön vastataan välittämällä tietokannalle kysely ohjelmointirajapinnan kautta (Listaus 3 / rivi 12) :  

{% highlight javascript %}

db.all(FindAllOpettajat, function (err, rows) {
  ...
});

{% endhighlight %}

<small>Listaus 4. Tietokannan ohjelmointirajapinnan *all*-metodin kutsu</small>


*Listauksen 3 rivillä 1* otetaan käyttöön moduulissa `db_connection.js` määritelty tietokantayhteys. Sen myötä moduulissa on objekti db, jonka kautta voidaan käsitellä tietokantaa. Käytettävissä olevat metodit löytyvät SQLite-tietokannan Node-sovelluksille tarkoitetun ohjelmointirajapinnan (API) [dokumentaatiosta][api-doc]. 

Tässä käytetään [`all`][all]-metodia, jolle annetaan kaksi parametria: SQL-komento ja funktio. SQL-komento on muuttujassa `FindAllOpettajat`, johon asetetaan arvo *Listauksen 3 rivillä 3*. Rajapinta kutsuu funktiota SQL-komennon suorituksen tuloksen palauttamiseksi. Funktion ensimmäinen parametri (tässä: `err`) sisältää tietoa mahdollisesta virheestä[^fn1] ja toinen parametri (tässä: `rows`) kyselyn tuloksen taulukkona, joka välitetään näkymälle (*Listaus 3: Rivi 14*). 

[api-doc]: https://github.com/mapbox/node-sqlite3/wiki/API
[all]: https://github.com/mapbox/node-sqlite3/wiki/API#databaseallsql-param--callback

[^fn1]: Tässä ei tarvitse toteuttaa virheenkäsittelyä


*Listauksen 3 rivillä 14* muodostetaan pyyntöön vaste hahmontamalla *Handelbars*-näkymä, jolle välitetään tietokannasta luettu data. Tietojen välitys näkymälle tapahtuu `render`-metodin toisena parametrina olevassa objektissa, jolla tässä on `opettajat`-ominaisuus (*rivi 15*). Näkymän (`opettaja_list.hbs`) listauksessa (5) viitataan ominaisuuteen *rivillä 3*.


{% highlight html linenos %}
{% raw %}

<h2>Opettajat</h2>
<ul>
    {{#each opettajat }}
    <li>
        <a href="/opettajat/{{id}}">{{sukunimi}}, {{etunimi}}</a>
    </li>
    {{/each}}
</ul>

{% endraw %}
{% endhighlight %}

<small>Listaus 5. Näkymä *opettaja_list.hbs* (Opettajaluettelo)</small>

*Opettaja kursseineen*

Yksittäisen opettajan tietojen hakuun liittyen tehtäväpohja sisältään rungon (*Listaus 6*):


{% highlight javascript %}

const FindOpettajaById = '';
const FindOpettajanKurssit = '';

module.exports = function (app) {

    app.get('/opettajat/:id', function (req, res) {

        res.render('opettaja_detail');
    });

};

{% endhighlight %}

<small>Listaus 6. Ote moduulista *opettajaController.js*</small>

Pohja ehdottaa, että tietojen haku tietokannasta toteutetaan kahdella kyselyllä, mutta yhdelläkin kyselyllä ratkaisu onnistunee tässä yhtä hyvin. Silmäilemällä näkymän `opettaja_detail.hbs` koodia voidaan todeta, että tiedot tulisi välittää näkymällä ominaisuuksissa `opettaja` ja `kurssit`.

Opettaja kursseineen esitetään, kun pyynnön polku on muotoa `/opettajat/:id`. Polun muuttuva osa (`:id`) löytyy [`Request`][request]-objektista: `req.params.id`.

[request]: http://expressjs.com/en/4x/api.html#req

#### kurssiController

Moduuli `kurssiController.js` (*Listaus 7*) vastaa rakenteeltaan edellä esitettyä. Kurssiluettelon muodostamista lukuunottamatta moduuli on pohjassa valmiina. 

{% highlight javascript  linenos %}

const db = require('../config/db_connection');

const FindAllKurssit = '';
const FindOneKurssi = '\
    SELECT \
        k.*,                            \
        o.etunimi AS opettaja_etunimi,  \
        o.sukunimi AS opettaja_sukunimi \
    FROM kurssi AS k LEFT OUTER JOIN opettaja AS o \
        ON k.opettaja_id = o.id         \
    WHERE k.tunnus = ?';

module.exports = function (app) {

    app.get('/', function (req, res) {
        res.redirect('/kurssit');
    });

    app.get('/kurssit', function (req, res) {
        res.render('kurssi_list');
    });

    app.get('/kurssit/:tunnus', function (req, res) {
        db.get(FindOneKurssi, req.params.tunnus, function (err, row) {
            res.render('kurssi_detail', {
                kurssi: row
            });
        });
    });
};


{% endhighlight %}

<small>Listaus 7. Ote moduulista *kurssiController.js*</small>

*Rivillä 15* on määritelty polkuun `/` liittyvä käsittely: uudelleenohjaus polkuun `/kurssit`  so. kurssiluettelon muodostaminen pyynnön vasteeksi.


*Rivill 23* käytetään tietokannan ohjelmointirajapinnan metodia [`get`][get], joka palauttaa kyselyn ensimmäisen rivin. Kysely sisältää `?`-merkillä osoitetun paikan parametrille (*rivi 11*), jolle annetaan arvo `get`-metodin kutsussa (`req.params.tunnus`; *rivi 23*). 

[get]: https://github.com/mapbox/node-sqlite3/wiki/API#databasegetsql-param--callback

SQL-komento *Listauksessa 7* on hieman monimutkaisempi verratuna  komentoon, joka esiintyy *Listauksessa 3*. [SQL-käsikirja][sql-manuaali] löytyy esim. [SQLite][SQLite]-sivustolta.


[sql-manuaali]: https://www.sqlite.org/lang.html

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

Tässä tehtävässä SQL:n avulla  kuvattiin tietokantakyselyt, jotka sitten välitettiin suoritettavaksi ohjelmointirajapinnan kautta. [Seuraavassa tehtävässä](../tehtava22) sama sovellus toteutetaan ilman SQL:n käyttöä.

<br/>



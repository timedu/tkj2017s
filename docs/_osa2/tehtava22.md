---
layout: exercise_page
title: "Tehtävä 2.2: Kurssit ja opettajat, ORM (3p)"
exercise_template_name:  W2E02.KurssitJaOpettajatORM
exercise_discussion_id: 83185
exercise_upload_id: 339414
---

[Edellisessä tehtävässä](../tehtava21) sovelluksen SQLite-tietokannnan käsittely kuvattiin SQL:n avulla. Tässä tehtävässä käsittely tapahtuu ilman SQL:ää ORM-rajapinnan kautta, jolloin tietokannan tiedot näkyvät sovelluksessa ohjelmointikielen objekteina.


Laadi relaatiotietokantaa käsittelevä sovellus, joka käyttäytyy kuten [Tehtävän 2.1](../tehtava21) ratkaisu, mutta toteuttaa tietokantakäsittelyn [Sequelize][sequelize]-ORM:n avulla.

[sequelize]: http://www.sequelizejs.com


Sovelluksen lähdekoodi jäsentyy edellisen tehtävän kanssa samalla tavalla (*Kuva 1*). Uusina moduuleina kuitenkin tässä ovat`models`-kansiossa sijaitsevat tiedostot, `Kurssi.js` ja `Opettaja.js`, jotka määrittelevät, mitä tietokannan tietoja sovelluksessa käsitellään. Näiden määrittelyjen myötä sovelluksessa on käytettävissä `Kurssi`- ja `Opettaja`-objektit, joiden kautta tietokannan käsittely tapahtuu.

~~~
Sources
 ├── main.js
 ├── config
 │     ├── db_connection.js       // tietokantayhteys
 │     ├── db_create.js           // oletusdatan talletus tietokantaan
 │     └── db_data.js             // oletusdata
 ├── controllers
 │     ├── kurssiController.js 
 │     └── opettajaController.js 
 ├── models                       // "mapping" tietokantatauluihin
 │     ├── Kurssi.js 
 │     └── Opettaja.js 
 └── views
~~~

<small>Kuva 1. Sovelluksen lähdekoodi</small>

Kontrollereita, `kurssiController.js` ja `opettajaKontrolle.js`,  lukuunottamatta sovellus on jo rakennettu valmiiksi.  Kontrollereihin sisällytetään ORM-rajapinnan kautta tapahtuva tietokantakäsittely. Moduuliin `opettajaKontrolle.js` on pohjassa valmiiksi ladittu koodi, joka esittää yksittäisen opettajan tiedot sisältäen opettajan vastuulla olevat kurssit (pyyntö esim. osoitteeseen `localhost:3000/opettaja/1`). 

Tietokanta sijaitsee projektin `database`-kansiossa nimellä `koulu.sqlite`[^1]. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimistä tiedostoa. Tietokanta näkyy kontrollereissa viittauksilla `Kurssi` ja `Opettaja`, jotka ovat Sequelizen [Model][model]-objekteja. 

[^1]: `databases`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

[model]:http://docs.sequelizejs.com/class/lib/model.js~Model.html


**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.


### Vihjeitä ja lisätietoja

#### Tietokannan ja sovelluksen välinen yhteys


Yhteys tietokantaan on määritelty moduulissa `db_connection.js`:


{% highlight javascript  linenos %}

const Sequelize = require('sequelize');

const db = new Sequelize('koulu', '', '', {
   dialect: 'sqlite',
   storage: 'database/koulu.sqlite',
   define: {
      timestamps: false
   }
});

module.exports = db;

{% endhighlight %}

<small>Listaus 1. *db_connection.js*</small>


*Rivillä 5* on määritelty tiedosto, jossa *SQLite*-tietokanta sijaitsee. Tiedosto muodostuu automaattisesti, jos sitä ei ole olemassa [`Sequelize`][Sequelize-object]-objektia muodostettaessa .

[Sequelize-object]: http://docs.sequelizejs.com/class/lib/sequelize.js~Sequelize.html

Tehtäväpohjan tiedostoissa `Kurssi.js` (*Listaus 2*) ja `Opettaja.js` (*Listaus 3*)  on kuvattu sovelluksessa käytettävien objektien liittyminen tietokantaan. Tiedostoissa määritellään tietokannan tauluja vastaavat [`Model`][Model-object]-objektit. Taulut muodostuvat tietokantaan *Sequelize*-objektin `define`-metodin (esim. *Listaus 2/Rivi 6*) kutsun myötä, jos niitä ei ole ennalta olemassa. 


[Model-object]: http://docs.sequelizejs.com/class/lib/model.js~Model.html


{% highlight javascript linenos %}

const db = require('../config/db_connection');
const Opettaja = require('./Opettaja');

const DataTypes = db.Sequelize.DataTypes;

const Kurssi = db.define('kurssi', {
   id: {
      type: DataTypes.INTEGER,
      autoIncrement: true,
      primaryKey: true
   },
   tunnus: DataTypes.STRING,
   nimi: DataTypes.STRING,
   laajuus: DataTypes.STRING
});

Kurssi.belongsTo(Opettaja);
Opettaja.hasMany(Kurssi);

db.sync();

module.exports = Kurssi;

{% endhighlight %}


<small>Listaus 2. *Kurssi.js*</small>


Tietokantaan  muodostuvat taulut nimetään oletusarvoisesi käyttäen englanninkielen monikkoa, ja siten *Listausten 2 ja 3* määrittelyjen myötä tietokannassa on taulut `kurssis` ja `opettajas`. Tässä moduuliin `Kurssi.js` (*Listaus 2*) on sisällytetty taulujen välisten yhteyksien määrittely (*Rivit 17 ja 18*), joiden avulla *kurssi*-objektin  kautta löytyy kurssin opettaja (*opettaja*-ominaisuus) ja *opettaja*-objektin kautta opettajan pitämät kurssit (*kurssis*-ominaisuus). 



{% highlight javascript  linenos %}

const db = require('../config/db_connection');
const DataTypes = db.Sequelize.DataTypes;

const Opettaja = db.define('opettaja', {
   id: {
      type: DataTypes.INTEGER,
      autoIncrement: true,
      primaryKey: true
   },
   etunimi: DataTypes.STRING,
   sukunimi: DataTypes.STRING
});

module.exports = Opettaja;

{% endhighlight %}

<small>Listaus 3. *Kurssi.js*</small>


#### opettajaController.js


Moduulissa `opettajaController.js` kuvataan polkuihin `/opettajat` ja `/opettajat/:id`liittyvät käsittelyt, joista jäkimäiseen polkuu liittyvä koodi on pohjassa valmiina (*Listaus 4*).   


{% highlight javascript  linenos %}

const Opettaja = require('../models/Opettaja');
const Kurssi = require('../models/Kurssi');

module.exports = function (app) {

    app.get('/opettajat', function (req, res) {
        res.render('opettaja_list');
    });

    app.get('/opettajat/:id', function (req, res) {
        Opettaja.findById(req.params.id, {
            include: [Kurssi], order: [[Kurssi, 'nimi']]
        }).then(function (opettaja) {
            res.render('opettaja_detail', {
                opettaja: opettaja
            });
        });
    });
};

{% endhighlight %}


<small>Listaus 4. Pohjan *opettajaController.js*</small>


Polkuihin tuleviin pyyntöihin vastataan tässä aivan samalla tavalla kuin [edellisessäkin tehtävässä](../tehtava21): 


{% highlight javascript %}

    app.get('/opettajat/:id', function (req, res) {
        // ...
    });
};

{% endhighlight %}


<small>Listaus 5. Ote moduulista *opettajaController.js*</small>

Opettaja-tietojen hakuun käytetään tietokannan `opettajas`-taulua vastaavaa `Opettaja`-objektia. Jos samalla ei haeta opettaja kursseja, haku id:n perusteella voidaan toteuttaa seuraavaan tapaan: 


{% highlight javascript %}

        Opettaja.findById(req.params.id).then(function (opettaja) {
            res.render('opettaja_detail', {
                opettaja: opettaja
            });
        });

{% endhighlight %}


<small>Listaus 6. Opettajan tietojen haku ilman kursseja</small>


Jos halutaan, että tuloksessa on mukana opettajan kurssit, koodia on hieman täydennettävä:


{% highlight javascript %}

        Opettaja.findById(req.params.id, {include: [Kurssi]}).then(function (opettaja) {
            res.render('opettaja_detail', {
                opettaja: opettaja
            });
        });
    });

{% endhighlight %}

<small>Listaus 7. Opettajan tietojen haku kurssien kera</small>

*Listauksen 7* koodi ei vielä lajittele opettajan kursseja. Lajittelun edellyttämä täydennys on esitetty *listauksessa 4*.

Tämän kontrollerin osalta tehtäväksi jääneen metodin  koodi (aakkosjärjestyksessa olevan opettajaluettelon tuottaminen) on edellä kuvattua huomattavasti yksinkertaisempi. [`Model`][Model-object]-objektin tarjoamista metodeista ongelman ratkaisee `findAll`, jonka käytöstä löytyy esimerkkejä esim. [Sequelize-tutoriaalista][tutorial-findAll].  

[tutorial-findAll]: http://docs.sequelizejs.com/manual/tutorial/querying.html


#### kurssiController.js


{% highlight javascript  linenos %}

const Kurssi = require('../models/Kurssi');
const Opettaja = require('../models/Opettaja');

module.exports = function (app) {

    app.get('/', function (req, res) {
        res.redirect('/kurssit');
    });

    app.get('/kurssit', function (req, res) {
            res.render('kurssi_list');
    });

    app.get('/kurssit/:tunnus', function (req, res) {
            res.render('kurssi_detail');
    });
};

{% endhighlight %}


<small>Listaus 5. Pohjan *kurssiController.js*</small>





{% comment %}



Tietokannan käsittely tapahtuu Model-objektin metodien avulla. Tämän tehtävän ratkaisussa käyttökelpoisia metodeja lienevät [findAll][findAll], [findById][findById] ja [findOne][findOne]. Sequelize-dokumentaation kohdissa [Models/Usage][models-usage] ja [Querying][querying] on näiden metodien käyttöön liittyviä esimerkkejä, joista muutama on poimittu alla olevaan kohtaan *Esimerkkejä*.

[findAll]: http://docs.sequelizejs.com/en/v3/api/model/#findalloptions-promisearrayinstance
[findById]: http://docs.sequelizejs.com/en/v3/api/model/#findbyidid-options-promiseinstance
[findOne]: http://docs.sequelizejs.com/en/v3/api/model/#findoneoptions-promiseinstance

[models-usage]: http://docs.sequelizejs.com/en/v3/docs/models-usage/
[querying]: http://docs.sequelizejs.com/en/v3/docs/querying/


Model-objektien väliset yhteydet kuvataan ao. metodien avulla, tässä käyttäen [belongsTo][belongsTo]- ja [hasMany][hasMany] -metodeja (*Listaus 4*).

[belongsTo]: http://docs.sequelizejs.com/en/v3/api/associations/#belongstotarget-options
[hasMany]: http://docs.sequelizejs.com/en/v3/api/associations/#hasmanytarget-options


{% highlight javascript %}

    Kurssi.belongsTo(Opettaja, {foreignKey: 'opettaja_id', as: 'Opettaja'});
    Opettaja.hasMany(Kurssi, {foreignKey: 'opettaja_id', as: 'Kurssit'});

{% endhighlight %}

<small>Listaus 4. Model-obejektien väliset assosiaatiot.</small>

*Listauksen 4* määrittely aikaansaa mm. sen, että `Kurssi`-objektin tietokantahaun tuloksena palauttama objekti sisältää metodin `getOpettaja`, ja vastaavasti `Opettaja`-objektin palauttama objekti metodin `getKurssit`.

#### Esimerkkejä

Seuraavat esimerkit on lainattu suoraan Sequelize-dokumentaation kohdasta [Models/Usage][models-usage].

{% highlight javascript %}

// search for known ids
Project.findById(123).then(function(project) {
  // project will be an instance of Project and stores the content of the table entry
  // with id 123. if such an entry is not defined you will get null
})

{% endhighlight %}


{% highlight javascript %}

// search for attributes
Project.findOne({ where: {title: 'aProject'} }).then(function(project) {
  // project will be the first entry of the Projects table with the title 'aProject' || null
})

{% endhighlight %}


{% highlight javascript %}

// find multiple entries
Project.findAll().then(function(projects) {
  // projects will be an array of all Project instances
})

{% endhighlight %}


{% highlight javascript %}

// search for specific attributes - hash usage
Project.findAll({ where: { name: 'A Project' } }).then(function(projects) {
  // projects will be an array of Project instances with the specified name
})

{% endhighlight %}


{% highlight javascript %}

Project.findAll({order: 'title DESC'})
// yields ORDER BY title DESC

{% endhighlight %}

{% endcomment %}


<br/>




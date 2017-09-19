---
layout: exercise_page
title: "Tehtävä 4.2: Kurssit ja opettajat, MongoDB 2 (3p)"
exercise_template_name: W4E02.KurssitJaOpettajatMongo2
exercise_discussion_id: 84711
exercise_upload_id: 341908
---

Laadi [edellisen tehtävän](../tehtava41) ratkaisua vastaava sovellus kuitenkin niin, että käytössä olevassa [MongoDB][MongoDB] -dokumenttitietokannassa on ainoastaan yksi kokoelma. 

[MongoDB]: https://www.mongodb.com


Sovelluksen lähdekoodi jäsentyy tässä edellisten tehtävien tapaan. Moduuleja `kurssiController.js` ja `opettajaController.js` lukuunottamatta sovellus on rakennettu valmiiksi. 

Tietokanta rakentuu tässä seuraavasti:

~~~
  +----------------+
  |   opettajat    |
  +----------------+
  | _id            |
  | etunimi        |
  | sukunimi       |
  | kurssis        | 
  |    id          |
  |    tunnus      |
  |    nimi        |
  |    laajuus     |
  +----------------+
~~~
<small>Kuva 1. Tietokannan rakenne</small>

Tietokannassa on ainoastaan yksi dokumenttikokoelma, `opettajat`, jonka  dokumentit sisältävät `kurssis`-attribuutissa opettajan kurssin tiedot.

Tietokantayhteys määritellään moduulissa `config/db_connection.js` olevilla tunnuksilla. Niiden arvot löytyvät [Moodlesta][tunnisteet], jotka tulee siis kopioida ao. kohtaan em. mooduulissa. Edellisen tehtävän tapaan tietokanta sijaitsee [mLab][mLab]-palvelussa. 

[tunnisteet]: https://moodle2.tut.fi/mod/page/view.php?id=341961
[mLab]: https://mlab.com


**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.


### Vihjeitä ja lisätietoja

Tehtävä voidaan ratkaista hakemalla tarvittava data [Collection][Collection]-objektin `find`- ja `findOne`-metodien avulla ja muokaamalla se sitten sovelluksessa näkymien edellyttämään muotoon. Tehtäväpohjassa on mukana [Osan 3](../../osa3) tehtävissä hyödynnetty lajittelufunktio, jota voi käyttää ratkaistaessa tehtävä tällä periaatteella.

Luontevampi (ja usein tehokkaampi) tapa on kuitenkin saada data tietokannasta suoraan siinä kokoonpanossa ja muodossa kuin näkymä sitä odottaa. Sen aikaasaamiseen voidaan tässä käyttää [Collection][Collection]-objektin `aggregate`-metodia, jonka paremetriksi  annetaan ns. putkilinja (*pipeline*) so. joukko suodattimien tavoin toimivia [operaatioita][aggregation]:


[Collection]: http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html
[aggregation]: https://docs.mongodb.com/manual/reference/operator/aggregation/


{% highlight js %}

db.collection('opettajat').aggregate([
    // suodattimet
], function (err, docs) {
    // näkymän hahmonnus
    // tietokantayhteyden sulkeminen
    db.close();
});

{% endhighlight %}

<small>Listaus 1.</small>


*Listauksessa 2* on putkilinjasta esimerkki, joka toteuttaa kurssin haun tietokannasta ja palauttaa datan näkymän odottamassa muodossa (*Kuva 2*).

~~~
  id
  tunnus
  nimi
  laajuus
  opettaja
      _id
      etunimi
      sukunimi  
~~~
<small>Kuva 2. </small>

Haluttu data on tässä niin, että ylätasolla on kurssin tiedot, ja niiden "alapuolelle" kurssin opettajan tiedot, kun taas tietokannassa (*Kuva 1*) tilanne on päinvastainen. Muokkaus haluttuun muotoon voidaan toteuttaa esim. seuraavanlaisella putkilinjalla[^1]:

[^1]: Tämän voi kopioida osaksi omaa ratkaisua, mutta oletettavasti on löydettävissä huomattavasti viirtaviivaisempia vaihtoehtoja.


{% highlight js linenos%}

db.collection('opettajat').aggregate([
    {$match: {'kurssis.id': req.params.id}},
    {$unwind: "$kurssis"},
    {$match: {'kurssis.id': req.params.id}},
    {$addFields: {
            opettaja: {
                _id: "$_id",
                etunimi: "$etunimi",
                sukunimi: "$sukunimi"
            },
            id: "$kurssis.id",
            tunnus: "$kurssis.tunnus",
            nimi: "$kurssis.nimi",
            laajuus: "$kurssis.laajuus"
        }
    },
    {$project: {
            _id: 0, etunimi: 0, sukunimi: 0,
            kurssis: 0
        }}
], function (err, kurssit) {
    !!err ? console.error(err) : 1;
    res.render('kurssi_detail', {
        kurssi: kurssit[0]
    });
    db.close();
});

{% endhighlight %}

<small>Listaus 2.</small>

Edellisen listauksen *riveillä 2-20* viisi pipeline-operaatiota (`match`, `unwind`, `match`, `addFields`, `project`), jotka ottavat lähtökohdakseen aina edellisen operaation tuloksen. *Rivin 2* `match` poimii tietokannasta yhden dokumentin (opettajan), jonka jollakin kurssilla on hakuehdossa annettu id. `unwind` purkaa dokumentin siten, opettajan tiedot toistuvat jokaisen opettan kurssin tietojen rinnalla. Jos opettajalla on esim. neljä kurssia, tämän operaation jälkeen käsittelyssä on neljä dokumenttia. *Rivin 4* `match` poimii käsittelyllä olevista kursseita sen (opettajineen), joka toteuttaa valintaehdon. `addFields` lisää dokumenttiin kenttiä niille tasoille kuin näkymä edellyttää ja `project` poistaa näkymän näkökulmasta ylimääräiset kentät pois dokumentista.


Kurssiluettelon tuottamisen ei tarvittane `match`-operaatiota, mutta `sort` lienee hyödyllinen. Seuraavassa listauksella on *sort*-operaatio, jolla joukko  dokumentteja saadaan kurssin nimen mukaiseen järjestykseen:   

{% highlight js %}

db.collection('opettajat').aggregate([
    // muut suodattimet
    {$sort: {"kurssis.nimi": 1}},
    // muut suodattimet    
], function (err, docs) {
    // näkymän hahmonnus
    // tietokantayhteyden sulkeminen
    db.close();
});

{% endhighlight %}

<small>Listaus 3.</small>


Yksittäisen opettajan tietoja esittävässä näkymässä opettajan kurssit ovat nimen mukaisessa aakkosjärjestyksessä. Kurssien lajittelu `sort`-operaatiolla edellyttää, että kurssit ovat erillisissä dokumenteissa, mikä aikaansaadaan `unwind`-operaatiolla. Dokumentti voidaan purun jälkeen kasata uudelleen `group`-operaatiolla, esim.

{% highlight js %}

db.collection('opettajat').aggregate([
    // muut suodattimet
    {$group: {
          _id: '$_id',
          etunimi: { "$first": "$etunimi" },
          sukunimi: { "$first": "$sukunimi" },
          kurssis: {$push: '$kurssis'}
       }
    }
    // muut mahdolliset suodattimet    
], function (err, docs) {
    // näkymän hahmonnus
    // tietokantayhteyden sulkeminen
    db.close();
});

{% endhighlight %}

<small>Listaus 4.</small>

Lisää lisätietoja:

* [The MongoDB Manual](https://docs.mongodb.com/manual/)
* [MongoDB Node.js Driver Documentation](http://mongodb.github.io/node-mongodb-native/2.2/)
* [Node.js MongoDB Driver API](http://mongodb.github.io/node-mongodb-native/2.2/api/)



<br/>
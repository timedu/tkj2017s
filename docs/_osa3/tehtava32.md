---
layout: exercise_page
title: "Tehtävä 3.2: Kurssit ja opettajat, LevelDB II (3p)"
exercise_template_name: W3E01.KurssitJaOpettajatLevel2
exercise_discussion_id: 83918
exercise_upload_id: 340397
---



Laadi [edellistä tehtävää](../tehtava31) vastaava [LevelDB][LevelDB] -avain-arvoparitietokantaa käsittelevä sovellus, jonka  taustalla on kuitenkin edelliseen verattuna rakenteeltaan hieman erilainen tietokanta.

[LevelDB]: http://leveldb.org

Sovelluksen lähdekoodi jäsentyy edellisen tehtävän tapaan. Moduuleja `kurssiController.js` ja `opettajaController.js` lukuunottamatta sovellus on rakennettu valmiiksi. Näistä ensimmäinen sisältää jo kurssiluettelon tuottamiseen liittyvän koodin.

Tietokannat tässä rakentuvat kuten normalisointu relaatiotietokanta ilman toisteisuutta. Tässä ei esim. ole edellisen tehtävän tietokannan sisältämiä kurssi- ja opettajaluetteloja. Tietokanta sisältää *Listausten 1 ja 2* mukaisia avain-arvopareja. 



{% highlight json %}

{ 
  "key": "cj7dbwrzi000a6n1knacrlvo0",
  "value": { 
      "id": "cj7dbwrzi000a6n1knacrlvo0",
      "tunnus": "PLA-33110",
      "nimi": "Käyttäjäkeskeinen suunnittelu",
      "laajuus": "4",
      "opettajaId": "cj7dbwrzi00096n1kawfncd6y"
  }
}

{% endhighlight %}

<small>Listaus 1. Avain-arvopari tietokannassa *kurssi.level*</small>



{% highlight json %}

{ 
  "key": "cj7dbwrzi00096n1kawfncd6y",
  "value": { 
      "id": "cj7dbwrzi00096n1kawfncd6y",
      "sukunimi": "Ahtola",
      "etunimi": "Pertti" 
  }
}

{% endhighlight %}

<small>Listaus 2. Avain-arvopari tietokannassa *opettaja.level*</small>


Tietokannan `kurssi.level` arvot (value) sisältävät tietokannan `opettaja.level` avaimia (key), jotka kuvaavat yhteydet tietokantojen välillä samoin kuin vierasavaimet relaatiotietokannassa. 

Edellisen tehtävän tapaan moduulin `db_connection.js` määrittelelemä tietokantayhteys näkyy kontrollereissa tunnisteissa `db.kurssi` ja `db.opettaja`, joiden kautta voi käyttää tietokannan [ohjelmointirajapinnan][api] metodeja.

[api]: https://github.com/Level/levelup/blob/master/README.md#api


**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.


### Vihjeitä ja lisätietoja

Edellisessä tehtävässä tietokantakäsittelyn toteuttamiseen riitti [ohjelmointirajapinnan][api] [`get`][get] -metodi: 

[get]: https://github.com/Level/levelup/blob/master/README.md#get


{% highlight javascript %}

db.get(key, function (err, value) {
  // ... 
});

{% endhighlight %}

<small>Listaus 3. Ohjelmointirajapinnan *get*-metodi</small>


Tässä tehtävässä on tilanteita, joissa tietokannasta on luetteva useita avain-arvopareja, mikä voidaan toteuttaa esim. käyttäen [`createValueStream`][createValueStream] -metodia:


[createValueStream]: https://github.com/Level/levelup/blob/master/README.md#createValueStream


{% highlight javascript %}

db.createValueStream()
  .on('data', function (data) {
    // ...
  })
 .on('end', function () {
    // ...
  });

{% endhighlight %}

<small>Listaus 4. Ohjelmointirajapinnan *createValueStream*-metodi</small>

*createValueStream*-metodia käytettäessä tietokannan arvot (value) saadaan tietokannasta yksi kerrallaan funktiolla, joka liitetään `data`-tapahtumaan.  Se laukeaa aina, kun tietokannasta on luettu arvo. `end`-tapahtuma laukeaa silloin, kun kaikki arvot on luettu. *Listauksessa 5* on esimerkki `createValueStream`-metodin käytöstä tässä tehtävässä.


{% highlight javascript %}

    app.get('/kurssit', function (req, res) {

        var kurssit = [];
        db.kurssi.createValueStream().on('data', function (kurssi) {
            kurssit.push(kurssi);
        }).on('end', function () {
            res.render('kurssi_list', {
                kurssit: sortBy('nimi', kurssit)
            });
        });
    });

{% endhighlight %}

<small>Listaus 5. Esimerkki *createValueStream*-metodin käytöstä rakennettavassa sovelluksessa</small>

*Listauksen 5* koodi toteuttaa kurssiluettelon. Muuttuja `db.kurssi` toimii rajapintana `kurssi.level` -tietokantaan. `createValueStream`-metodi lukee kaikki tietokannan sisältämät kurssit (arvot). `data`-tapahtumaan reagoidaan asettamalla luettu kurssi `kurssit`-taulukkoon. `end`-tapahtumaan reagoidaan hahmontamalla näkymä `kurssi_list` välittäen sille tietokannasta luetut kurssit. 

Näkymä esittää kurssit aakkosjärjestyksessä. Lajittelun toteuttaa moduulissa `config/sort.js` määritelty funktio, johon *listauksessa 5* viittaa tunniste `sortBy`.



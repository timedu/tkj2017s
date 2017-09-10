---
layout: exercise_page
title: "Tehtävä 3.1: Kurssit ja opettajat, LevelDB I (3p)"
exercise_template_name: W3E01.KurssitJaOpettajatLevel1
exercise_discussion_id: 83917
exercise_upload_id: 340392
---


Laadi [LevelDB][LevelDB] -avain-arvoparitietokantaa käsittelevä sovellus, joka käyttäytyy ulospäin kuten [Tehtävän 2.1]({{site.baseurl}}/osa2/tehtava21) ratkaisu[^1a].

[LevelDB]: http://leveldb.org

[^1a]: Tässä yksittäisen kurssin tiedot poimitaan tietokannasta id:n perusteella eikä käyttäen kurssin tunnusta kuten tehtävässä 2.1


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
       └── ...  
~~~

<small>Kuva 1. Sovelluksen lähdekoodi</small>


Moduuleja `kurssiController.js` ja `opettajaController.js` lukuunottamatta sovellus on rakennettu valmiiksi. Näissä moduuleissa määriteltävät funktiot lukevat *LevelDB* -tietokannasta tarvittavan datan ja välittävät sen edelleen näkymille.


Sovelluksella on kaksi erillistä[^1b] *LevelDB* -avain-arvoparitietokantaa,  `kurssi.level` ja `opettaja.level`, jotka sijaitsevat projektin `database`-kansiossa[^2]. Tietokannat datoineen muodostuvat[^1c] sovelluksen käynnistyksen yhteydessä, jos tietokanta ei sisällä kursseja eikä opettajia. Tietokantojen *rakenne vastaa näkymien esittämää tiedon rakennetta* siten, että tietojen muokkausta ei juuri tarvita näkymiä varten. 


[^1b]: Tässä käytetyssä perusmuotoisessa *LeveDB*-tietokannassa sen sisältämiä avain-arvoapareja ei voi ryhmitellä esim. *Riak KV* -tietokannan tapaisiin, jotenkin relaatiotietokannan tauluja vastaaviin, *bucket*'eihin. *LevelDB*:n *Sub-levels* -laajennuksen avulla ryhmittelyn voisi kuitenkin tehdä.

[^1c]: Moduuli `db_data.js` sisältää tietokantaan talletettavan datan.  Moduuli `db_create.js` suorittaa talletuksen, kun on ensin hieman muokannut lähtödataa tassä tarvittavaan muotoon.

[^2]: `database`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa


Seuraavissa *listauksissa 1-4* on esittettu tietokantojen avain-arvopareja. 


{% highlight json %}

{ 
  "key": "list",
  "value": [ 
    { "id": "cj7czgm19000az61kg84894kq", "nimi": "Käyttäjäkeskeinen suunnittelu" },
    { "id": "cj7czgm1a000fz61khj1jnmno", "nimi": "Mobiiliohjelmointi" },
    { "id": "cj7czgm180004z61krs768sma", "nimi": "Ohjelmistoprojekti" },
    ...
  ]
}

{% endhighlight %}

<small>Listaus 1. Kurssiluettelo tietokannassa *kurssi.level*</small>


{% highlight json %}

{ 
  "key": "cj7czgm19000az61kg84894kq",
  "value": { 
      "id": "cj7czgm19000az61kg84894kq",
      "tunnus": "PLA-33110",
      "nimi": "Käyttäjäkeskeinen suunnittelu",
      "laajuus": "4",
      "opettaja": { 
          "id": "cj7czgm190009z61kovgnbu47",
          "sukunimi": "Ahtola",
          "etunimi": "Pertti" 
      } 
  }
}

{% endhighlight %}

<small>Listaus 2. Yksittäinen kurssi tietokannassa *kurssi.level*</small>



{% highlight json %}

{ 
  "key": "list",
  "value": [ 
    { "id": "cj7czgm190009z61kovgnbu47", "sukunimi": "Ahtola", "etunimi": "Pertti" },
    { "id": "cj7czgm190005z61kbpuhou0w", "sukunimi": "Jukola", "etunimi": "Leevi" },
    { "id": "cj7czgm1a000gz61kbx9xooal", "sukunimi": "Käkilä", "etunimi": "Simo" },
    ...
  ]
}

{% endhighlight %}

<small>Listaus 3. Opettajaluettelo tietokannassa *opettaja.level*</small>


{% highlight json %}

{ 
  "key": "cj7czgm190009z61kovgnbu47",
  "value": { 
      "id": "cj7czgm190009z61kovgnbu47",
      "sukunimi": "Ahtola",
      "etunimi": "Pertti",
      "kurssis": [ 
          {"id": "cj7czgm19000az61kg84894kq", "nimi": "Käyttäjäkeskeinen suunnittelu"} 
      ] 
  }
}

{% endhighlight %}

<small>Listaus 4. Yksittäinen opettaja tietokannassa *opettaja.level*</small>


Tietokanta `kurssi.level` sisältää yksittäisten kurssien lisäksi avaimella (key) `list` luettelon kaikista kursseista (*Listaus 1*) ja vastaavasti tietokanta `opettaja.level` yksittäisten opettajien lisäksi opettaluettelon  avaimella `list` (*Listaus 3*). Yksittäisen opettajan/kurssin avain-arvoparin avaimena (key) toimii  opettajan/kurssin `id`-attribuutin[^3] arvo. 


[^3]: Moduuli `db_create.js` muodostaa `id`-attribuuteille yksilölliset arvot `cuid`-paketilla


Moduulin `db_connection.js` määrittelelemä tietokantayhteys näkyy tunnisteissa `db.kurssi` ja `db.opettaja`. Nämä ovat objekteja, joiden kautta voi käyttää tietokannan  [ohjelmointirajapinnan][api] metodeja. 


[api]: https://github.com/Level/levelup/blob/master/README.md#api


**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.


### Vihjeitä ja lisätietoja

Noden LevelDB-rajapinta, [LevelUp][levelup], tarjoaa joukon metodeja, joilla voidaan *lisätä*, *poistaa*, *muuttaa* ja *kysellä* tietokannan avain-arvopareja. Tämän tehtävän ratkaisussa tietokantaan kohdistetaan ainoastaan yksinkertaisia *kyselyjä*.  

Yksittäisen arvon (value) avaimen (key) perusteella voi hakea tietokannasta rajapinnan [`get`][get] -metodilla: 

[levelup]: https://github.com/Level/levelup/blob/master/README.md
[get]: https://github.com/Level/levelup/blob/master/README.md#get


{% highlight javascript %}

db.get(key, function (err, value) {
  // ... 
});

{% endhighlight %}

<small>Listaus 5. *get*-metodi</small>


Pelkistetyssä muodossa `get`-metodin kutsussa on kaksi parametria  - ensimmäisenä *avain* ja toisena funktio, joka suoritetaan, kun *arvo* tietokannasta on saatu. 

Jos *arvo* on tietokannassa [JSON][JSON]-muodossa oleva merkkijono, sen saa JavaScript -objektina ilmaisemalla koodaus metodikutsun tai tietokantayhteyden määrittelyn yhteydessä[^4]. 


[^4]: Moduulissa `db_connection.js` on ao. yhteydessä ilmaistu, että arvot tässä käytetyissä tietokannoissa ovat *JSON*-muotoisia.


[JSON]: http://www.json.org



<br/>

##### Alaviitteet
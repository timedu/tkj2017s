---
layout: exercise_page
title: "Tehtävä 4.2: Kurssit ja opettajat, MongoDB 2 (3p)"
exercise_template_name: W4E02.KurssitJaOpettajatMongo2
exercise_discussion_id: 84711
exercise_upload_id: 341908
kesken: 1
julkaisu: 19.9.2017
---

Laadi [edellisen tehtävän](../tehtava41) ratkaisua vastaava sovellus kuitenkin niin, että käytössä olevessa [MongoDB][MongoDB] -dokumenttitietokannassa on ainoastaan yksi kokoelma. 

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

* [Mongo-API][mongo-api]
* [Db-objekti][Db]
* [Collection-objekti][Collection]

[mongo-api]: http://mongodb.github.io/node-mongodb-native
[Db]: http://mongodb.github.io/node-mongodb-native/2.2/api/Db.html
[Collection]: http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html

...


<br/>
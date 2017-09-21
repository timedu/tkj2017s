---
layout: exercise_page
title: "Tehtävä 4.3: Kurssit ja opettajat, MongoDB CRUD (4p)"
exercise_template_name: W4E03.KurssitJaOpettajatMongoCRUD
exercise_discussion_id: 84712
exercise_upload_id: # 341907
kesken: 1
julkaisu: 21.9.2017
no_review: 1
---

Täydennä [tehtävän 4.1](../tehtava41) ratkaisua siten, että sovelluksella voi kyselyjen lisäksi myös ylläpitää kurssi- ja opettajatietoja. Perusta itsellesi [mLab][mLab]-palveluun [MongoDB][MongoDB] -dokumenttitietokanta ja käytä sitä sovelluksen taustalla.  Käyttäjän kannalta sovellus käyttäytyy kuten [tehtävän 2.3](../../osa2/tehtava23) ratkaisu.

[MongoDB]: https://www.mongodb.com
[mLab]: https://mlab.com

Sovelluksen lähdekoodi jäsentyy tässä edellisten tehtävien tapaan. Tietojen kyselyyn liittyvät moduulit, `kurssiController.js` ja `opettajaController.js`, voi kopioida  sellaisenaan [tehtävän 4.1](../tehtava41)  ratkaisusta.  Tietojen ylläpidon toteuttavista moduuleista, `kurssiControllerCUD.js` ja `opettajaControllerCUD.js`, puuttuu ainoastaan varsinainen ylläpito so. tietokantaan ei tapahdu muutoksia ao. painikkeiden klikkauksista huolimatta.


...


**Palauta** tehtävän ratkaisuna tiedostot `kurssiControllerCUD.js` ja `opettajaControllerCUD.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä ja tietokannan alkutilassaan[^0] testejä ajettaessa. 


[^0]: Tietokannan saa datan osalta alkutilaansa, kun ennen sovelluksen käynnistämistä poistaa tietokannan sisältämät dokumenttikokoelmat [mLab][mLab]-palvelussa (*Database*-sivu/*Collections*-välilehti).


### Vihjeitä ja lisätietoja


#### Tietokannan perustaminen mLab-palveluun

Mene [mLab][mLab]-sivustolle ja suorita rekisteröityminen (*Sign Up*)[^1]. Tutustu palvelun käyttöehtoihin ja syötä pyydetyt tiedot.[^2]

Kun olet rekisteröitynyt ja kirjautunut palveluun, sen [pääsivulla](https://mlab.com/home) on kohdassa *MongoDB Deployments*  luettelo perustamistasi tietokannoista. Alussa lista on tyhjä, mutta kohdasta löytyy painike *Create new*, jolla tietokannan voi perustaa. Klikkaa sitä ja vastaa annettuihin kysymyksiin esim. seuraavasti:

* Cloud Provider: Amazon
* Plan Type: SandBox (Free)
* AWS Region: Europe (Ireland)
* Database name: tkj

Kun on antanut em. vastauksia, päätyy sivulle *Order Confirmation*. Tässä kannattaa tarkistaa vielä, että parissakin kohtaa lukee *Free*.

Tietokannan perustamisen jälkeen, tietokanta näkyy [pääsivulla](https://mlab.com/home)  kohdassa *MongoDB Deployments*.

Tietokannan käyttö edellyttää vielä käyttäjän perustamista. Sen voi tehdä *Database*-sivulla, joka tulee esiin klikattaessa tietokantaa pääsivulla. *Database*-sivulla on viisi välilehteä: *Collections*, *Users*, *Stats*, *Backups* ja
*Tools*. *Add database user* -painike löytyy *Users*-välilehdeltä.


*Database*-sivulta löytyy seuraavaa muotoa oleva merkkijono, joka tarvitaan otettaessa sovelluksesta yhteys tietokantaan: `mongodb://<dbuser>:<dbpassword>@ds99999.mlab.com:99999/tkj`. Merkkijonossa kohtien `<dbuser>` ja `<dbpassword>` paikalle tulee perustamasi käyttäjän tunnisteet.


[^1]: Rekisteröityessäsi teet oman sopimuksen *mLab*:in kanssa. Jos koet tämän ongelmallisena, voit saada tälle vaihtoehtoisen tehtävän. Luottokorttitietoja ei tarvitse/pidä missään vaiheessa antaa ainakaan tätä tarkoitusta varten.

[^2]: Tällaisiin palveluihin rekisteröitymiseen voisi käyttää jotakin toissijaista sähköpostiosoitetta. Tosin kokemusten perusteella *mLab* ei ole osoittautunut kovin agressiiviseksi maksullisten palvelujen markkinoijaksi.


<br/>
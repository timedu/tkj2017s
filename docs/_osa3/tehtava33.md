---
layout: exercise_page
title: "Tehtävä 3.3: Kurssit ja opettajat, LevelDB CRUD (4p)"
exercise_template_name: W3E03.KurssitJaOpettajatLevelCRUD
exercise_discussion_id: 83919
exercise_upload_id: 340753
---

Täydennä [tehtävän 3.2](../tehtava32) ratkaisua siten, että sovelluksella voi opettaja- ja kurssitietojen tarkastelun ohella ylläpitää (*lisäys*, *muutos*, *poisto*) opettajatietoja. Käyttäjän kannalta sovellus käyttäytyy pieniä lisäyksiä lukuunottamatta kuten [tehtävän 2.3](../../osa2/tehtava23) ratkaisu.

Sovelluksen lähdekoodi jäsentyy edellisen tehtävän tapaan. Tietojen kyselyyn liittyvät moduulit, `kurssiController.js` ja `opettajaController.js`, voi kopioida tähän sellaisenaan edellisen tehtävän ratkaisusta.  Moduuli `kurssiControllerCUD.js`, joka sisältää kurssitietojen ylläpitoon liittyvät toiminnot[^1], on tehtäväpohjassa valmiina. Laadittavaksi jää opettajan ylläpitoon (*lisäys*, *muutos*, *poisto*) liittyvät toiminnot moduuliin `opettajaControllerCUD.js`.

Tietokanta on tässä samanlainen kuin [edellisessä tehtävässä](../tehtava32) kuitenkin niin, että opettajan tietojen yhteyteen on talletettu aika, jolloin tietoa on viimeksi muutettu. Tätä käytetään tietojen samanaikaisesta käsittelystä johtuvien ongelmien ratkaisemiseen. Seuraavassa *listauksessa  1* on esimerkki tässä opettajan tietoja sisältävästä arvosta. Listauksessa esiintyvä `timestamp` muodostetaan funktiolla [`Date.now`][Date.now]. Yksilöivä `id` voidaan generoida funktiolla [`cuid`][cuid].


[Date.now]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/now
[cuid]: https://www.npmjs.com/package/cuid

[^1]: Ei sisällä aikaleimoihin liittyvää käsittelyä.


{% highlight json %}

{ 
  "id": "cj7hi3dxc000978n1mcsyx4cb",
  "sukunimi": "Ahtola",
  "etunimi": "Pertti",
  "timestamp": 1505214910272 
}

{% endhighlight %}

<small>Listaus 1. Arvo tietokannassa *opettaja.level*</small>


Tehtäväpohjan moduuliin `opettajaControllerCUD.js` on kommentoitu ylläpitotoimintoihin (*lisäys, muutos, poisto*) liittyvä, tässä laadittava, käsittely. Moduuli välittää  viestejä[^2]  ylläpidon onnistumisesta näkymille, joka esittää ne käyttäjälle:

[^2]: Viestien välittämiseen liittyvä koodi on moduulissa kommentoituna.


1. Opettaja lisätty.
2. Opettaja päivitetty.
3. Opettaja N.N. poistettu tietokannasta.
4. Päivitys ei onnistunut: Opettajan tietoja muutettu toisaalta. Yritä uudelleen.
5. Poisto ei onnistunut: Opettajan tietoja muutettu toisaalta. Yritä uudelleen.

Viesteistä kaksi viimeisintä koskevat tilanteita, joissa päivitykseen littyvällä lomakkeella oleva aikaleima ei täsmää tietokannassa olevaan aikaleimaan. 

Tukena voi käyttää  moduulia `kurssiControllerCUD.js`, joka tosin ei sisällä aikaleimakäsittelyä eikä viestien välitystä näkymille.


**Palauta** tehtävän ratkaisuna tiedosto `opettajaControllerCUD.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä ja tietokannan alkutilassaan testejä ajettaessa.

Selenium-testit eivät testaa aikaleimakäsittelyä, mutta sen onnistumisen voi todeta sovellusta ajamalla käyttämällä kahta selainikkunaa. Esim. edellä olevan luettelon neljännen viestin tulisi tulla nakyviin seuraavasti:

* Ikkuna 1 ottaa esiin opettajan *Päivitä opettaja* -sivulla
* Ikkuna 2 ottaa esiin saman opettajan *Päivitä opettaja* -sivulla
* Ikkunassa 2 klikataan *Talleta* -painiketta
* Ikkunassa 1 klikataan *Talleta* -painiketta

Ikkunaan 1 pitäisi ilmestyä punaisella teksti "*Poisto ei onnistunut: Opettajan tietoja muutettu toisaalta. Yritä uudelleen.*"


<br/>
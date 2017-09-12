---
layout: exercise_page
title: "Tehtävä 3.3: Kurssit ja opettajat, LevelDB CRUD (4p)"
exercise_template_name: W3E03.KurssitJaOpettajatLevelCRUD
exercise_discussion_id: 83919
exercise_upload_id: 340753
no_review: 1
kesken: 1
julkaisu: 12.9.2017
---

Täydennä [tehtävän 3.2](../tehtava32) ratkaisua ominaisuuksiltaan siten, että sovelluksella voi opettaja- ja kurssitietojen tarkastelun ohella ylläpitää (*lisäys*, *muutos*, *poisto*) opettajatietoja. Käyttäjän kannalta sovellus käyttäytyy pieniä lisäyksiä lukuunottamatta kuten [tehtävän 2.3](../../osa2/tehtava23) ratkaisu.

Sovelluksen lähdekoodi jäsentyy edellisen tehtävän tapaan. Tietojen kyselyyn liittyvät moduulit, `kurssiController.js` ja `opettajaController.js`, voi kopioida tähän sellaisenaan edellisen tehtävän ratkaisusta.  Moduuli `kurssiControllerCUD.js`, joka sisältää kurssitietojen ylläpitoon liittyvät toiminnot, on tehtäväpohjassa valmiina. Laadittavaksi jää opettajan ylläpitoon (*lisäys*, *muutos*, *poisto*) liittyvät toiminnot moduuliin `opettajaControllerCUD.js`.

Tietokanta tässä on tässä samanlainen kuin [edellisessä tehtävässä](../tehtava32) kuitenkin niin, että opettajan tietojen yhteyteen on talletettu aika, jolloin tietoa on viimeksi muutettu. Tätä käytetään tietojen samanaikaisesta käsittelystä johtuvien ongelmien ratkaisemiseen.


   


**Palauta** tehtävän ratkaisuna tiedosto `opettajaControllerCUD.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä ja tietokannan alkutilassaan testejä ajettaessa.

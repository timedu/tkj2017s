---
layout: exercise_page
title: "Tehtävä 6.2: Kurssit ja opettajat, LevelGraph CRUD"
exercise_template_name: W6E02.KurssitJaOpettajatLevelGraphCRUD
exercise_discussion_id: 85904
exercise_upload_id: 344277
---


Täydennä [edellisen tehtävän](../tehtava61) ratkaisuasi opettajatietoihin liittyvillä ylläpitotoiminnolla, *lisäys*, *muutos* ja *poisto*. Tietokanta tässä on tehtävän 6.1 kanssa samarakenteinen. Sovelluksen tulee toimia [Tehtävän 2.3](../../osa2/tehtava23) kuvauksessa olevien sivukarttojen mukaan.

Sovelluksen lähdekoodi jäsentyy edellisten tehtävien tapaan.  Kyselyt toteuttavat moduulit `opettajaController.js` ja `kurssiController.js` voi kopioida edellisen tehtävän ratkaisusta. Laadittavaksi tässä jää moduuliin  `opettajaControllerCUD.js` tuleva tietokantakäsittelyyn liittyvä koodi. Kontrollerit ottavat moduulin `config/db_connection.js` käyttöönsä siten, että tietokantarajapinta näkyy tunnisteessa `db`.


[LevelUp]: https://github.com/Level/levelup/blob/master/README.md
[LevelGraph]: https://github.com/mcollina/levelgraph/blob/master/README.md


Opettajan *lisäys* tapahtuu tallettamalla tietokantaan *on_opettaja*-kolmikko (ks. [edellisen tehtävän](../tehtava61) *Listaus 1*). Talletuksen yhteydessä on huomioitava, että kolmikon `object`-attribuutin arvoksi tulee *JSON*-merkkijono.

*Muutos* edellyttää kahta operaatiota, koska kolmikkoihin ei liity erillistä muutos-metodia. Muutettava opettaja (*on_opettaja* -kolmikko) on poistettava ja sen tilalle on talletettava uusi. 

*Poistettaessa* opettajaa on tietokannasta poistettava ao. *on_opettaja* -kolmikon lisäksi myös kaikki opettajaan liittyvät *opettaa* -kolmikot (ks. [edellisen tehtävän](../tehtava61) *Listaus 3*).

**Palauta** tehtävän ratkaisuna tiedosto `opettajaControllerCUD.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sekä sovellusta ajamalla sekä käyttäen oheistettuja Selenium-testejä[^4]. 

[^4]: Nämä testaavat sovelluksen ulospäin näkyvää toiminnallisuutta eivätkä siten paljasta sisäisiä vikoja.

### Vihjeitä ja lisätietoja

#### Sovellusrajapinnasta

Kolmikon *lisäyksen* tietokantaan voidaan tehdä [LevelGraph][LevelGraph] -sovellusrajapinnan [`put`][put]-metodilla ja *poiston* [`del`][del]-metodilla. Monen kolmikon poisto sujunee kätevimmin "[striimeillä][streams]".


[put]: https://github.com/mcollina/levelgraph#get-and-put
[del]: https://github.com/mcollina/levelgraph#deleting
[streams]: https://github.com/levelgraph/levelgraph/blob/master/README.md#putting-and-deleting-through-streams

#### Muista apuneuvoista

JavaScript-objektin voi muuntaa JSON-merkkijonoksi funktiolla [`JSON.stringify`][JSON.stringify]. 

[JSON.stringify]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify


<br/>

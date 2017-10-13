---
layout: exercise_page
title: "Tehtävä 2.3: Kurssit ja opettajat, CRUD (4p)"
exercise_template_name: W2E03.KurssitJaOpettajatCRUD
exercise_discussion_id: 83186
exercise_upload_id: 339512
---


Täydennä [Tehtävän 2.2](../tehtava22) ratkaisua siten, että se sisältää kyselyjen lisäksi toiminnot tietokannan tietojen ylläpitoa varten: CRUD - Create, Retrieve, Update, Delete.

Edellisten tehtävien ([2.1](../tehtava21) ja [2.2](../tehtava22)) ratkaisujen sivukartta on seuraavanlainen:

![sivukartta](../img/w2e01.png){: style="display: block; margin: auto; margin-top: 10px; width: 300px;"}

<small>Kuva 1. Sivukartta: Kurssien ja opettajien *luettelot* ja *eritelyt*.</small>


Tässä sivukartta laajenee sekä kurssien että opettajien osalta siten, että kuhunkin tietojen ylläpito-operaatioon (lisäys, muutos, poisto) liittyy oma lomakesivunsa (*Kuva 2*).


![sivukartta](../img/w2e03.png){: style="display: block; margin: auto; margin-top: 10px; width: 500px;"}

<small>Kuva 2. Sivukartta: sisältää lomakkeet (*uusi*, *muuta* ja *poista*) tietojen ylläpitoa varten.</small>


*Luettelo*-sivulta voidaa siirtyä lomakkeelle (*uusi*), jolla voi syöttää uuden rivin tietokantaan. Muille lomakkeille (*muuta*, *poista*) voidaan siirtyä *erittely*-sivulta. Kullakin lomakkeella voi joko vahvistaa tai peruuttaa ylläpito-operaation ao. painikkeiden avulla. *Kuvassa 2* on esitetty, mille sivuille käsittely siirtyy lomakesivuita painikkeiden klikkauksen seurauksena. 

Sovelluksen lähdekoodi jäsentyy edellisen tehtävän kanssa samalla tavalla (*Kuva 3*). Näkymiä rakenteessa on kuitenkin aiempaa enemmän sekä ylläpitotoiminnoille (*create, update, delete*) on omat kontrollerinsa (`kurssiControllerCUD.js` ja `opettajaControllerCUD.js`).

~~~
Sources
 ├── main.js
 ├── config
 │     ├── db_connection.js     // tietokantayhteys
 │     ├── db_create.js         // oletusdatan talletus tietokantaan
 │     └── db_data.js           // oletusdata
 ├── controllers
 │     ├── kurssiController.js      // kyselyt (luettelo, erittely)
 │     ├── kurssiControllerCUD.js   // lisäys, muutos, poisto
 │     ├── opettajaController.js    // kyselyt (luettelo, erittely)
 │     └── opettajaControllerCUD.js // lisäys, muutos, poisto
 ├── models
 │     ├── Kurssi.js            // kurssin "mapping" tietokantaan
 │     └── Opettaja.js          // opettajan "mapping" tietokantaan
 └── views
     ├── kurssi_detail.hbs      // yksitäinen kurssi
     ├── kurssi_list.hbs        // kurssiluettelo
     ├── kurssi_create.hbs      // lomake uudelle kurssille
     ├── kurssi_update.hbs      // lomake kurssin muutokselle
     ├── kurssi_delete.hbs      // lomake kussin poistolle
     ├── opettaja_detail.hbs    // yksittäinen opettaja
     ├── opettaja_list.hbs      // opettajaluettelo
     ├── opettaja_create.hbs    // lomake uudelle opettajalle
     ├── opettaja_update.hbs    // lomake opettajan muutokselle
     ├── opettaja_delete.hbs    // lomake opettajan poistolle
     └── layouts
          └── main.hbs                 
~~~

<small>Kuva 3. Sovelluksen lähdekoodin struktuuri.</small>


Kontrollereita lukuunottamatta sovellus on jo rakennettu valmiiksi. Kontrollereihin sisällytetään ORM-rajapinnan kautta tapahtuva tietokantakäsittely, johon sisältyy  kyselyjen lisäksi nyt myös tietokannan tilaa muuttavat operaatiot. 


Kyselyt moduuleihin `kurssiController.js` ja `opettajaController.js` voi kopioida edellisen tehtävän ratkaisusta. Tietojen kyselyihin liittyen tässä on edellisiin tehtäviin verrattuna sellainen pieni muutos, että yksittäisen kurssin tiedot haetaan `id`-attribuutin perusteella `tunnus`-attribuutin sijaan. Moduulissa `kurssiController.js` on valmiina tähän liittyvä koodi, joka siis esittää yksittäisen kurssin tiedot (pyyntö esim. polkuun `/kurssit/1`). 


Moduulissa `kurssiControllerCUD.js` on valmiina kurssin muutokseen ("update") liittyvä koodi (pyyntö esim. polkuun `/kurssit/1/update`). Laadittavaksi jää  *kurssin lisäys* ja *kurssin poisto* ("create", "delete").

Moduulissa `opettajaControllerCUD.js` on valmiina *opettajan lisäykseen ja poistoon*  ("create", "delete") liittyvä koodi. Laadittavaksi jää  *opettajan muutos* ("update").

Tietokanta sijaitsee projektin `database`-kansiossa nimellä `koulu.sqlite`[^3]. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimistä tiedostoa. Tietokanta näkyy kontrollereissa viittauksilla `Kurssi` ja `Opettaja`, jotka ovat *Sequelizen* [Model][model]-objekteja. 

[^3]: `database`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

[model]:http://docs.sequelizejs.com/class/lib/model.js~Model.html

**Palauta** tehtävän ratkaisuna tiedostot `kurssiControllerCUD.js` ja `opettajaControllerCUD.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. 

Sovelluksen on oltava käynnissä testejä ajettaessa ja tietokannan tulee olla alkutilassaan. Tarvittaessa tietokannan (`database`-kansiossa oleva tiedoston `koulu.sqlite`) voi poistaa ja käynnistää sovelluksen uudelleen, jolloin tietokanta muodostuu uudelleen alkutilaansa.


### Vihjeitä ja lisätietoja

Kuhinkin ylläpito-operaatioon liittyy kaksi polkua, joista toiseen tulee pyyntö `GET`-metodilla ja toiseen `POST`-metodilla. *GET*-pyyntöön tuotetaan vasteeksi  ylläpito-operaatioon liittyvä lomake-sivu. *POST*-pyyntöön reagoidaan toteuttamalla  tietokantaoperaatio ja ohjaamalla käsittely sitten seuraavalle sivulle (ks. *Kuvan 2* sivukartta).

Moduuleihin `kurssiControllerCUD.js` ja `opettajaControllerCUD.js` on kirjattu kommentteja, jotka kuvaavat yläpito-operaatioden etenemistä.

Alla olevassa *Listauksessa 1* on esitetty moduulista `kurssiControllerCUD.js` metodi, joka tuottaa vasteeksi sivun (*Listaus 3*) sisältäen lomakkeen, jonka avulla voidaan muuttaa kurssin tietoja[^1]. Lomakkeelle välitetään muutoksen kohteena oleva *kurssi* sekä *opettajat* siltä varalta, että kurssille halutaan muutoksen yhteydessä valita toinen opettaja. 

[^1]: Mm. tämä koodi löytyy valmiina tehtäväpohjassa.

{% highlight javascript %}

   app.get('/kurssit/:id/update', function (req, res) {

      Kurssi.findById(req.params.id).then(function(kurssi) {

         if (!kurssi) {
            res.render('kurssi_detail');
            return;
         }

         Opettaja.findAll({order: ['sukunimi']}).then(function(opettajat) {

            // ao. option-elementin merkintä valituksi
            opettajat.forEach(function(opettaja) {
               opettaja.selected = opettaja.id === kurssi.opettajaId ? 'selected' : '';
            });

            res.render('kurssi_update', {
               kurssi: kurssi,
               opettajat: opettajat
            });
            
         });
      });
   });
   
{% endhighlight %}

<small>Listaus 1. Ote tiedostosta *kurssiKontrollerCUD.js*.</small>


Polussa on mukana pyyntöön liittyvä parametri. Polku voi olla esim. `/kurssit/10/update`, jossa `10` on muutoksen kohteena olevan kurssin `id`-attribuutin arvo tietokannassa. Polussa olevaan `id`-parametriin päästään käsiksi viittauksella `req.params.id`, joka perusteella tietokannasta haetaa rivi käyttäen `findById` -metodia. Jos riviä ei jostakin syystä löydy, hahmonnetaan näkymä `kurssi.hbs` ilman dataa, ja päätetään funktion suoritus tähän.

Jos tietokannasta saadaan muutoksen kohteena oleva kurssi, luettaan tietokannasta `findAll`-metodilla kaikki opettajat lomakkeella olevaa `select`-elementtiä varten (ks. *Listaus 2*). Jotta lomakkeella olisi oikea opettaja valittuna, kullekin opettajalle määritellään tässä `selected`-ominaisuus, jonka arvo määrää aktiivisen valinnan (*Listaus 2*). Tämän jälkeen hahmonnetaan näkymä `kurssi_update.hbs` (*Listaus 2*) so. tuotetaan pyynnön vasteeksi muutoslomakkeen sisältävä sivu.

{% highlight html %}
{% raw %}

<h2>Päivitä kurssi</h2>

<form method="POST" action="/kurssit/update">
    <div>
        Tunnus: 
        <input name="tunnus" value="{{kurssi.tunnus}}"/>
    </div>
    <div>
        Nimi: 
        <input name="nimi" value="{{kurssi.nimi}}"/>
    </div>
    <div>
        Laajuus: 
        <input name="laajuus" value="{{kurssi.laajuus}}"/>
    </div>
    <div>
        Opettaja: 
        <select name="opettajaId">
            <option></option>
            {{#each opettajat}}
            <option value="{{id}}" {{selected}}>{{sukunimi}}, {{etunimi}}</option>
            {{/each}}
        </select>
    </div>
    <br/>
    <input type="hidden" name="id" value="{{kurssi.id}}"/>    
    <input type="submit" name="_update" value="Talleta"/>
    <input type="submit" name="_cancel" value="Peruuta"/>
</form>


{% endraw %}
{% endhighlight %}

<small>Listaus 2. Näkymä *kurssi_update.hbs*.</small>

Lomakeen (*Listaus 2*) *submit*-napin klikkaus aikaansaa `POST`-metodilla polkuun `/kurssit/update` pyynnön, joka käsitellään *Listauksen 3*  koodilla.

{% highlight javascript %}

   app.post('/kurssit/update', function (req, res){

      if (req.body._cancel) {
         res.redirect('/kurssit/' + req.body.id);
         return;
      }

      Kurssi.findById(req.body.id).then(function(kurssi) {

         if (!req.body.opettajaId.length)
            req.body.opettajaId = null;

         kurssi.update(req.body).then(function(kurssi) {
            res.redirect('/kurssit/' + kurssi.id);
         });
      });
   });

{% endhighlight %}

<small>Listaus 3. Ote tiedostosta *kurssiControllerCUD.js*.</small>

Lomakkeen lähettämä data löytyy `req.body`-olion[^5] attribuuteista. Käsittelyn alussa selvitetään, onko käyttäjä lähettänyt lomakkeen klikkaamalla *Peruuta*-painiketta. Jos näin on, palautetaan käsittely kurssitietojen erittely -sivulle (ks. *Kuva 2*). Muussa tapauksessa luetaan muutettava kurssi tietokannasta.

[^5]: jäsennyksen toteuttaa `body-parser`-moduuli, jonka käyttöönotto on määritelty  tiedostossa `configs/config.js`

Lomakeelta saatavan `opettaja_id`-tiedon osalta on tässä tehtävä pieni "säätö" ennen tietojen muutoksen toteuttamista: mahdollisen tyhjän merkkijonon vaihtaminen arvoon `null`. 

Muutos tietokantaan voidaan toteuttaa [`update`][update]-metodilla, jonka suorituksen jälkeen käsittely ohjataan kurssitietojen erittely -sivulle.


[`update`][update]-metodin lisäksi tehtävän ratkaisussa tarvittaneen [`create`][create]- ja [`destroy`][destroy] -metodeja. Näiden käytöstä löytyy esimerkkejä *Sequelize*-dokumentaation kohdasta [Instances][instances].

[update]: http://docs.sequelizejs.com/class/lib/model.js~Model.html#instance-method-update
[create]: http://docs.sequelizejs.com/class/lib/model.js~Model.html#static-method-create
[destroy]: http://docs.sequelizejs.com/class/lib/model.js~Model.html#instance-method-destroy

[instances]: http://docs.sequelizejs.com/manual/tutorial/instances.html


<br/>


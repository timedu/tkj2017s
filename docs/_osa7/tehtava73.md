---
layout: exercise_page
title: "Tehtävä 7.3: Kurssit ja opettajat, OrientDB - Graph (3p)"
exercise_template_name: W7E03.KurssitJaOpettajatOrientDBGraph
exercise_discussion_id: 86114
exercise_upload_id: 329062
---

Laadi sovellus, jolla voidaan tarkastella *kurssi- ja opettajatietoja* sekä *yhteenveto-* että *erittelymuotoisten* näkymien kautta. [Tähtävän 6.3](../../osa6/tehtava63) tapaan  *Kurssi*-sivu esittää kurssien välisiä esitieto-riippuvuuksia. Kuten [edellisessä tehtävässä](../tehtava72) tietokantaratkaisuna on tässä [OrientDB][OrientDB], josta nyt hyödynnetään sen graafiominaisuuksia.

[OrientDB]: http://orientdb.com

Sovelluksen lähdekoodi jäsentyy edellisten tehtävien tapaan. Moduuleja `kurssiController.js` ja `opettajaController.js`lukuunottamatta sovellus on pohjakoodissa valmiina. Tehtävä ratkaistaan täydentämällä nämä moduulit. Kontrollerit ottavat moduulin `config/db_connection.js` käyttöönsä siten, että tietokantarajapinta näkyy tunnisteessa `db`. Tietokantapalvelimen on oltava [edellisen tehtävän](../tehtava72/#tietokantapalvelimen-käynnistäminen) tapaan käynnissä sovellusta ajettaessa.

Tietokanta muodostuu *Kurssi*- ja *Opettaja* -solmuista (`Vertex`) sekä *Opettaa*- ja *OnEsitieto* -kaarista (`Edge`). Nämä kaikki ovat tietokannan luokkia (`Class`), joita voidaan käsitellä SQL:n avulla vastaavasti kuin relaatiotietokannassa käsitellään tauluja SQL-komennoilla. *Kurssi* ja *Opettaja* on periytetty *V* -luokasta ("vertex"), ja *Opettaa* ja *OnEsitieto* luokasta *E* ("edge").

Tietueiden väliset yhteydet muodostuvat kaarien avulla. Kaari-luokka on verrattavissa relaatiotietokannan ns. välitauluun, jolla kuvataan tietojen välisiä n:m -yheyksiä. Kaarella on *in*- ja *out*-ominaisuudet, jotka ovat linkkejä kaareen liittyviin solmuihin. Esim. tilanteessa "*opettaja opettaa kurssia*", *opettaa* -kaaren *in*-linkki osoittaa *kurssiin* ja *out* -linkki *opettajaan*:

~~~~
  [ Opettaja ] -(out)----( Opettaa )----(in)-> [ Kurssi ]
~~~~
<small>Kuva 1. </small>


Pohjakoodin moduuli `config/db_create.js` luo tietokantaan luokat ja tietueet sovelluksen käynnistyksen yhteydessä, jos tietokanta ei sisällä jotakin em. luokista. Tietokanta kuitenkin tulee perustaa [edellisen tehtävän](../tehtava72/#tietokannan-perustaminen) tapaan tietokannan hallintatyökalulla. Tietokannan tulee olla nimellä `KouluGraph`. 

Tietojen talletuksen yhteydessä *Opettaja* -tietueille muodostuu ominaisuudet *sukunimi* ja *etunimi*, ja *Kurssi* -tietueille ominaisuudet *tunnus*, *nimi* ja *laajuus*. Tietueilla ei ole [edellisen tehtävän](../tehtava72) tietokannassa esiintyneitä *Link* / *Linklist* -tyyppisiä *opettaja* / *kurssit* -ominaisuuksia, koska tässä yhteydet on toteutettu kaarien avulla. 


{% comment %}
???
*Opettaa* -tietueilla on *tyyppi* -ominaisuus, jolla esitetään onko esitieto pakollinen (p) vai suositeltava (s).
{% endcomment %}


*OnEsitieto* -kaarella on *tyyppi* -ominaisuus, jolla esitetään onko esitieto pakollinen (p) vai suositeltava (s). [Tehtävän 6.3](../../osa6/tehtava63) kuvauksessa on esitetty kurssien esitieto-riippuvuuksiin liittyvä sovelluksen ulkoinen käyttäytyminen [^1].


[^1]: [Tehtävän 6.3](../../osa6/tehtava63) kuvauksen alussa on esitietoriippuvuuksia esittävä kuva, jossa sama kurssi esiintyy sekä *Välittömät esitiedot* - että *Muut esitiedot* -luettelossa. Tässä tehtävässä *Välittömät esitiedot* -luettelossa oleva kurssi ei ole mukana enää *Muut esitiedot* -luettelossa.


**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla. Pohjakoodi ei sisällä kurssien esitietoriippuvuuksia koskevia testejä.


### Vihjeitä ja lisätietoja

{% comment %}

Tehtävässä on täydennettävänä neljä metodia, joista `findAll` -metodit voi kopioida suoraan [edellisen tehtävän](../tehtava71) ratkaisusta. Ehdotukset `models/Opettaja.js` -moduulin `findByKey`- metodiin liittyviksi SQL-komennoiksi on osana pohjakoodia, mutta näitä käyttävä koodi tulee laatia metodin sisällöksi. Moduulissa `models/Kurssi.js` tilanne on päinvastoin: `findByKey` -metodi on kutakuinkin valmis, mutta sen viittaamat SQL-komennot on jätetty laadittavaksi.

{% endcomment %}

#### opettajaController

Pohjan moduuli `opettajaController.js` sisältää kaikki ratkaisuun tarvittavat *SQL* -komennot. Laadittavana on ainoastaan metodi, joka *SQL*-komentoja hyödyntäen tuottaa näkymään yksittäisen opettajan tiedot kursseineen.

Pohjakoodi ehdottaa, että tiedot opettajan erittelysivulle tuotetaan kahdella SQL-komennolla (*Listaukset 1 ja 2*): opettajan perustiedot haetaan *Opettaja* -luokan (solmu) kautta ja opettajan kurssit *Opettaa* -luokkaan (kaari) kohdistuvalla kyselyllä.


{% highlight javascript %}

const FindOpettajaByKey = '\
SELECT \
   @RID.substring(1) AS key, \
   sukunimi, \
   etunimi \
FROM Opettaja \
WHERE @RID = :rid';

{% endhighlight %}

<small>Listaus 1. </small>


{% highlight javascript %}

const FindKurssitUsingEdge = '\
SELECT \
  in.nimi AS nimi,    \
  in.@RID.substring(1) AS key \
FROM Opettaa \
WHERE out = :opettaja_rid \
ORDER BY nimi ';

{% endhighlight %}

<small>Listaus 2. </small>

*Listauksessa 2* ominaisuus *out* on linkki, joka osoittaa *Opettaja* -tietueeseen ja *in* -ominaisuus *Kurssi* -tietueeseen osoittava linkki. 


#### kurssiController

Pohjan moduuli `kurssiController.js` sisältää kurssiluettelon tuottamiseen liittyvän *SQL* -komennon sekä ehdotukset yksittäisen kurssin tietojen haussa käytettävien *SQL*-komentojen rakenteesta. Pohjassa on myös valmis metodi, jolla voidaan hakea tiedot yksittäisen kurssin tiedot esitttävään näkymään, jos ratkaisu toteutetaan em. ehdotusten mukaisena. Laadittavana on metodi, joka tuotaa näkymään kurssiluettelon, sekä joukko yksittäisen kurssien tietojen hakuun liittyviä *SQL*-komentoja.


*Listauksessa 3* on moduulista `kurssiController.js` löytyvä metodi, joka tuottaa tiedot kurssin erittelysivulle. Myös tässä ratkaisu perustuu yhtä useamman kyselyn käyttöön. Metodin toteutuksen rakenneperiaatetta on kuvattu [tehtävässä 6.3](../../osa6/tehtava63/#tehtävän-toteutuksesta). 


{% highlight javascript %}

app.get('/kurssit/:key', (req, res) => {

   db.record.get('#' + req.params.key).then(kurssi => {

      Promise.all([

         db.query(FindOpettajaUsingEdge, {
            params: {kurssi_rid: kurssi['@rid']}
         }),
         db.query(FindValittomatEsitiedot, {
            params: {kurssi_rid: kurssi['@rid']}
         }),
         db.query(FindEsitietoKursseille, {
            params: {kurssi_rid: kurssi['@rid']}
         }),
         db.query(TraverseMuutEsitiedot, {
            params: {kurssi_rid: kurssi['@rid']}
         })

      ]).then(resArr => {

         kurssi.opettaja = resArr[0][0];
         kurssi.valittomat_esitiedot = resArr[1];
         kurssi.esitieto_kursseille = resArr[2];
         kurssi.muut_esitiedot = resArr[3];

         res.render('kurssi_detail', {
            kurssi: kurssi
         });

      }).catch(err => {
         res.send('error occurred - see console');
         console.error('kurssiController:', err);
      });
   }).catch(err => {
      res.send('error occurred - see console');
      console.error('kurssiController:', err);
   });

});



{% endhighlight %}

<small>Listaus 3. </small>


*Listaus 3* viittaa neljään merkkijonojen sisältönä olevaan SQL-komentoon: *FindOpettajaUsingEdge*, *FindValittomatEsitiedot*, *FindEsitietoKursseille* ja *TraverseMuutEsitiedot*. Näistä kolme ensimmäistä voidaan toteuttaa samalla periaatteella kaareen kohdistuvalla kyselyllä kuin *Listauksessa 2* oleva komento. 

*TraverseMuutEsitiedot* tuottaa kurssin epäsuorat esitietovaatumukset. (Esim. *Kuvassa 2* kurssi *Ohjelmiointitekniikka* on kurssin *Tietorakenteet* epäsuora esitieto.) Tämänkaltaiset ongelmat voidaan voidaan ratkaista [`Traverse`][Traverse] -komennolla.

[Traverse]: http://orientdb.com/docs/last/SQL-Traverse.html

~~~~
[Ohjelmiointitekniikka]-->[Olio-ohjelmointi]-->[Tietorakenteet]
~~~~
 
<small>Kuva 2. </small>


Seuraavassa on eräs `TRAVERSE`-komentoa soveltava kysely, joka poimii kavereita kaveri-polun varrelta tiettyltä etäisyydeltä. `WHERE` poistaa tuloksesta henkilön (`#10:1234`), josta polun seuraaminen lähti liikkeelle.

~~~
SELECT 
FROM (TRAVERSE out("Friend") FROM #10:1234 MAXDEPTH 4) 
WHERE $depth >= 1
~~~

<small>Listaus 4. </small>



{% comment %}

\- draft - draft - draft - draft - draft - draft - 
{: style="color:gray; font-size: 80%; text-align: center;"}

{% endcomment %}


<br/>






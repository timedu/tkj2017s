---
layout: exercise_page
title: "Tehtävä 1.1: Hello Node (2p)"
exercise_template_name: "W1E01.HelloNode"
exercise_discussion_id: 82687
exercise_upload_id: 329025
---

[Edellisen tehtävän](../tehtava10) ratkaisu tuottaa selaimen ikkunaan *Hello World* -tekstin, kun selaimella tehdään pyyntö osoitteeseen `localhost:3000` seuraavasti:

~~~
Hello World
~~~

Tuloste ei ole riippuvainen osoitteessa olevasta polusta. Esim. osoitteeseen `localhost:3000/polku` tehty pyyntö tuo selaimelle aivan saman tekstin.


Muokkaa sovellusta siten, että se tuottaa selaimen ikkunaan seuraavanlaisen tekstin:


~~~
Hello 3 from Node (/abc)
~~~

Edellä oleva tuloste liittyy tilanteeseen, jossa sovelluksen käynnistämisen jälkeen osoiteeseen `localhost:3000/abc` on tehty kolmas pyyntö. Jos tämän jälkeen tehdään ensimmäinen pyyntö osoitteeseen `localhost:3000/def`, tuloste selaimessa on seuraava:

~~~
Hello 1 from Node (/def)
~~~

Palattaessa aiempaan pyyntöön selaimeen saatu teksti on seuraava:

~~~
Hello 4 from Node (/abc)
~~~

Sovellus siis laskee kuinka monta pyyntöä kuhunkin polkuun kohdistuu sovelluksen käynnissäoloaikana.

#### Tehtäväpohja

Lataa tehtäväpohja tehtävän otsikon yläpuolella olevasta ao. linkistä. Tehtäväpohja on zip-arkistoitu NetBeans-projekti. Sijoita se (esim.) kansioon `NetBeansProjects/tkj2017s`. Projekti sisältää [edellisen tehtävän](../tehtava10) ratkaisun tiedostossaan `main.js`.

Oletettavasti NetBeans esittää projektin punaisella ilmoittaen siten, että projekti ei ole aivan käyttövalmis. Kehitettävä sovellus ei ole riippuvainen ulkopuolisista kirjastoista, mutta projektissa on mukana testikoodia, jolla näitä riippuvuuksia on. Projekti saadaan myös testien osalta toimintakuntoiseksi projektivalikon valinnalla *npm Install* (tai *Resolve Project Problems ...*)[^fn1].

[^fn1]: Projektissa olevaan tiedostoon `package.json` on kuvattu nämä riippuvuudet. Kirjastot latautuvat osaksi projektia. `package.json` näkyy *Projects*-ikkunan tiedostopuun `Important Files`-kansiossa.

 
 
#### Ratkaisun laatiminen

Pohjassa oleva koodin tulos selaimen ikkunaan on `Hello World`. Ratkaisun voisi esim. kehittää seuraavin vaihein: 1) `Hello from Node`, 2) `Hello from Node (/abc)` ja 3) `Hello 3 from Node (/abc)`. 

Tehtävä edellyttää pieniä muutoksia `createServer`-metodin kutsun parametrina olevaan funktioon, joka suoritetaan vasteena pyyntöön:

{% highlight javascript  linenos %}

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

{% endhighlight %}

Funktiolla on parametreinaan kaksi objektia, `req` ja `res`, joista ensimmäinen edustaa pyyntöä ("request") ja jälkimmäinen pyyntöön annettavaa vastetta ("response"):

~~~
request --> [funktio] --> response
~~~

Tehtävän ensimmäisen vaiheen (1) ratkaisuksi riittää pieni vakiotekstin muutos riville, jonka myötä muodostuu selaimen ikkunassa näkyvä tuloste, mutta toisessa vaiheessa (2) tarvitaan tietoa siitä, mihin polkuun pyyntö on kohdistunut. Vastaus löytyy selaamalla Noden [ohjelmointirajapinnan dokumentaatiota][node-api]: `req`-objekti on tyyppiä [`IncomingMessage`][IncomingMessage] ja objektin `url`-ominaisuudesta löytyy juuri tässä tarvittava tieto. 

[node-api]: https://nodejs.org/api/
[IncomingMessage]: https://nodejs.org/api/http.html#http_class_http_incomingmessage


Kolmas vaihe (3) edellyttää tiedon ylläpitämistä eri polkuihin tulleiden pyyntöjen määrästä. Tähän tarvitaan esim. sopiva, funktion ulkopuolinen, muuttuja sekä funktion sisällä suoritettava laskenta ja muuttujan arvon ylläpito. Asia hoituu kätevästi objektilla, jota JavaScriptilla voidaan käsitellä kuten merkkijonoilla indeksoitua taulukkoa, esim.

{% highlight javascript  linenos %}

var obj = {};     // "tyhjä" objekti
obj['abc'] = 1;   // objektin abc-ominaisuuden arvona on 1 ts.
                  // indeksin abc osoittamassa paikassa on 1

{% endhighlight %}



#### Sovelluksen testaus

Tehtäväpohjissa voi olla mukana kahdenlaisia testejä: yksikkötestejä ja kokonaisuutta testaavia Selenium -testejä. Testeihin liittyvät valinnat tulevat näkyviin IDE:ssä klikattaessa hiiren oikealla napilla projektia *Projects* -ikkunassa: *Test* (yksikkötestien ajo) ja *Run Selenium Tests*. 

Selenium -testit edellyttävät, että testattava sovellus on käynnissä, mutta yksikkötestaus onnistuu ilman sovelluksen ajoa. Tässä tehtävässä on mukana sekä yksikkötesti että Selenium -testejä[^fn1a]. Tosin yksikkötesti onnistuu aina ilmoittaen, että "ei sisällä yksikkötestejä". Testien ajon tulokset ilmestyvät NetBeansin *Test Results* -ikkunaan, joka tulee esiin automaattisesti, jos testit epäonnistuvat.

[^fn1a]: Testi edellyttää, että tulosteessa olevien eri merkkijonojen välissä on täsmälleen yksi välilyönti.


Jotta testien ajo Netbeansista käsin onnistuu, kehitettävällä projektilla on oltava tieto, minkä tuella projektin testit ajetaan. Tässä sekä yksikkö- että Selenium -testien *Test Provider* on *Mocha*. Jos testin käynnistyksen yhteydessä asetus puuttuu, NetBeans kysyy tätä ennen testin ajoa.  Vastauksen jälkeen NetBeans saattaa vielä varmistaa *Mochan* sijainnin, joka oletettavasti on NetBeansin arvauksen mukainen.[^fn2] [^fn3]

[^fn2]: Määrityksen voi myös tehdä ao. kohdassa ikkunassa, joka tulee esiin projektivalikon *Properties*-valinnalla.


[^fn3]: Testaus käyttää paketteja [Mocha][mocha], [Selenium Web-Driver][web-driver] ja [PhantomJS][phantom]. Yksikkötestien ajamiseen riittää *Mocha*, mutta Selenium-testit edellyttävät myös kahta jälkimmäistä, jolloin *Selenium* ohjaa *Phantom*-selainta, joka on yhteydessä kehitettävään sovellukseen. Projektissa olevaan tiedostoon `package.json` on kuvattu nämä riippuvuudet siten, että paketit voidaan asetaan projektivalikon valinnalla *npm Install* (tai *Resolve Project Problems ...*).

[mocha]: https://mochajs.org
[web-driver]: http://www.seleniumhq.org/docs/03_webdriver.jsp
[phantom]: http://phantomjs.org

{% comment %} näihin ei viitata: {% endcomment %}

[npm-mocha]: https://www.npmjs.com/package/mocha
[npm-selenium]: https://www.npmjs.com/package/selenium-webdriver
[npm-phantom]: https://www.npmjs.com/package/phantomjs-prebuilt


#### Ratkaisun palautus 

Kun tehtäväpohjassa olevat testit onnistuvat, palauta projektista tiedosto `main.js` käyttäen Moodlessa olevaa työkalua, joka tulee esiin klikattaessa tehtävän otsikon yläpuolella olevaa ao. linkkiä.


#### Ongelmia?

Jos tehtävän ratkaisun yhteydessä tulee ongelmia, voit esittää ongelmasi keskusteltavaksi tehtävän otsikon yläpuolella olevan ao. linkin kautta. Linkki johtaa Moodlessa olevaan tehtävälle varattuun keskusteluun.

Jos ongelmaa ei ole luonteva käsitellä keskustelussa, voit lähettää koodiin liittyvän katselmointipyynnön: 1) Palauta ratkaisu (tässä siis tiedosto `main.js`) ja 2) lähetä sähköpostiviesti, jossa ilmoitat jättäneesi koodisi katselmoitavaksi. Sähköpostin voi lähettää tehtävän otsikon yläpuolella olevan *Katselmointipyyntö* -linkin kautta [^fn4]. Kirjaa ongelma mahdollisimman tarkasti koodin oheen ja/tai sähköpostiviestiin.


[^fn4]: Linkki osoittaa Moodle-työkaluun, jonka pitäisi avata uusi viesti-ikkuna työasemassa olevaan sähköpostityökaluun siten, että viestin vastaanottaja ja osin myös otsikko on valmiiksi täytetty.  Saattaa olla, että joissakin ympäristöissä tämä ei onnistu, jolloin voit lähettää viestin ilman työkalun apua.

<br/>


Tässä tehtävässä rakennettiin yksinkertainen Nodeen perustuva web -sovellus. [Seraavassa tehtävässä](../tehtava12) laaditaan vastaava sovellus kuitenkin niin, että otetaan käyttöön web-sovellusten rakentamista Node-ympäristössä yksinkertaistava [Express][express] -sovelluskehys. Samalla sovelluksen koodi jaetaan kahteen erilliseen moduuliin.

[express]: http://expressjs.com

<br/>

##### Alaviitteet

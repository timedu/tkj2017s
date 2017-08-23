---
layout: exercise_page
title: "Tehtävä 1.0: Hello World (0p)"
exercise_discussion_id: 82685
no_review: 1
---

Varmista, että ohjelmointitehtävissä käytettävä kalusto on asennettu kehitysympäristöösi, ja laadi sitten Node-pohjainen `Hello World` -sovellus.

#### NetBeans IDE

Ohjelmakoodin laatimisessa käytetään *Netbeans IDE:ä*, josta tarvitaan paketti `All` tai `HTML5/JavaScript`. Version tulisi olla vähintään `8.2`, jolloin editori osaa korostaa oikein JavaScriptin [tuoreen syntaksin][es6]. NetBeansin voi ladata tarvittaessa osoitteesta <https://netbeans.org/downloads/>.

[es6]: http://es6-features.org/


#### Node.js

Ohjelmointikielenä käytetään JavaScriptia, joka tulkitaan [Nodella][node]. Sen voi ladata osoitteesta <https://nodejs.org/en/download/>.

[node]: https://nodejs.org 



#### Sovelluksen kehittäminen

Kun kalusto on asennettu, perusta NetBeansilla uusi `HTML5/JavaScript` -kategorian `Node.js Application` -projekti antaen projektille nimen (esim.) `W1E00.HelloWorld` tallettaen sen (esim.) kansioon `NetbeansProjects/tkj2017s`. 

Projektin perustamisen myötä muodostuu oletusarvoisesti kaksi tiedostopohjaa, `main.js` ja `package.json`, joista ensimmäiseen laaditaan pääohjelman koodi. Tiedostoon [package.json](https://docs.npmjs.com/files/package.json) määritellään mm. mahdolliset laadittavan sovelluksen riippuvuudet ulkopuolisista kirjastoista. 

Hello World -sovelluksella ei ole kirjastoriippuvuuksia, ja tiedostoon `main.js` tarvittavan JavaScript-koodinkin voi kopioida suoraan sivulta  <https://nodejs.org/en/about/>.

NetBeans IDE saatta osoittaa jonkin olevan projektissa vialla esittämällä sen `Projects` -ikkunassa punaisella. IDE osaa tyypillisesti antaa osuvan vihjeen asian saattamiseksi kuntoon, jos klikkaa hiiren oikealla näppäimellä projektia ja valitsee esiin tulevasta valikosta valinnan *Resolve Project Problems*.



#### Sovelluksen ajo

Sovellus voidaan käynnistää *Run* -valikosta löytyvällä valinnalla tai vastaavalla pikanäppäimellä. Käynnistyksen myötä NetBeansin `Output`-ikkunaan pitäisi ilmestyä sovelluksen käynnistykomento sekä sovelluksessa olevan `console.log` -funktion kutsun tuottama ilmoitus, `Server running at http://127.0.0.1:3000/`.  Käynnistyskomento on pelkistetysti seuraavanlainen:

~~~
node main.js
~~~

Komennossa käynnistetään `node` -ohjelma, jolle annetaan parametriksi JavaScript -tiedosto `main.js`. Tosin NetBeansin ikkunassa komento on hieman monimutkaisempi, koska mukana on esim. tiedostoihin liittyvät hakemistopolut.

Kun sovellus on käynnissä `Output`-ikkunan vasemmassa reunassa on punainen neliö, joka toimii *stop* -painikkeena. Ikkunan alapuolella on `Running` -teksti, jonka vieressä olevaa pientä `x`-painiketta klikkaamalla sovellus saadaan myös pysäytettyä.  

Tässä kehitetty ohjelma on palvelinpään web-sovellus[^fn1]. Kun selaimella tehdään pyyntö osoitteeseen `localhost:3000`, selaimen ikkunaan pitäisi ilmestyä teksti `Hello World`.


[^fn1]: Sovellus on samalla web-palvelin eikä sen ajamiseen tarvita erillistä palvelinohjelmistoa.



#### Ohjelmakoodista 

Sivulta <https://nodejs.org/en/about/> löytyvä koodi sisältää viisi lausetta:

~~~
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
~~~

Lauseissa esiintyy JavaScript -standardin, [ECMAScript 6](http://www.ecma-international.org/ecma-262/6.0/), kuvaamia ominaisuuksia. Esim. lauseista neljässä ensimmäisessä määritellään vakio (`const`), joka on kuten muuttuja (`var`), mutta sen arvo ei ole vaihdettavissa. 



Koodin ensimmäinen lause ottaa käyttöön (`require`) Node [http][http] -moduulin: 

[http]: https://nodejs.org/dist/latest-v6.x/docs/api/http.html

{% highlight javascript %}

const http = require('http');

{% endhighlight %}


Lauseen myötä ohjelmassa on käytössä `http` -objekti, Tämän jälkeen seuraavilla riveillä määritellään kaksi jäljempänä viitattavaa vakiota:

{% highlight javascript %}

const hostname = '127.0.0.1';
const port = 3000;

{% endhighlight %}



Koodin neljäs lause muodostaa web-palvelimen kutsumalla `http`-objektin `createServer`:

{% highlight javascript %}

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

{% endhighlight %}

Metodin parametrina on funktio, jonka palvelin suorittaa vasteena sille tulevaan pyyntöön. Koodissa funktio esitetään *ECMAScript6* -syntaksilla, mutta funktion määrittelyssä voisi käyttää myös perinteisempää syntaksia:

{% highlight javascript %}

const server = http.createServer( function(req, res) {
    ...
});

{% endhighlight %}



Pyyntöön vastaavalla funktiolla on kaksi parametria: `http.IncomingMessage`-tyyppinen `req` ("request") ja `http.ServerResponse`-tyyppinen `res` ("response"). Tässä `res`-olion avulla muodostetaan pyyntöön annetava vaste: paluukoodi, palautettavan sisällön tyyppi sekä paluuviestin varsinainen sisältö:

{% highlight javascript %}

  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');

{% endhighlight %}



Paluuviestin sisällön tyypin määrittelyssä voisi olla mukana myös merkistökoodaus[^fn2]: 


{% highlight javascript %}

  res.setHeader('Content-Type', 'text/plain; charset=utf-8');

{% endhighlight %}


[^fn2]: Selaimen konsolille saattaa ilmestyä varoitus, jos tieto merkistökoodauksesta puuttuu.


Koodin viimeinen lause käynnistää edellisen lauseen muodostaman web-palvelimen sen `listen`-metodilla, jonka parametrina esiintyvät edellä määritellyt vakiot `port` ja `hostname` sekä funktio, joka suoritetaan käynnistyksen yhteydessä:

~~~
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
~~~



Lauseen voisi kirjoittaa myös seuraavasti erinteisempää JavaScript -syntaksia käyttäen:


{% highlight javascript %}

server.listen(port, hostname, function() {
  console.log('Server running at http://' + hostname + ':' + port + '/');
});

{% endhighlight %}



<br/>

##### Alaviitteet


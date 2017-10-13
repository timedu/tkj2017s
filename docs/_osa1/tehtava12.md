---
layout: exercise_page
title: "Tehtävä 1.2: Hello Express (2p)"
exercise_template_name: "W1E02.HelloExpress"
exercise_discussion_id: 82688
exercise_upload_id: 335622
---


Tehtäväpohjassa on *Express*-sovelluskehykseen  tukeutuva *Node*-sovellus [^1] , joka tuottaa selaimen ikkunaan tekstin `Hello World!`, kun selain tekee pyynnön osoitteeseen `localhost:3000/`. 

[^1]: Pohjakoodi on kopioitu osoitteesta <http://expressjs.com/en/starter/hello-world.html>. 

Muokkaa sovellusta siten, että se käyttäytyy ulospäin kuten [tehtävän 1.1](../tehtava11) ratkaisu kuitenkin niin, että teksti "Node" on tulosteessa korvattu tekstillä "Express", esim:

~~~
Hello 3 from Express (/abc)
~~~

Toteuta ratkaisu siten, että se tukeutuu *express*-moduulin ohella itse laatimaasi *counter*-moduuliin, joka ylläpitää tietoja eri polkuihin kohdistuneiden pyyntöjen lukumääristä. Moduuli otetaan käyttöön seuraavasti:


{% highlight javascript %}

var count = require('./counter');

{% endhighlight %}

Käyttöönoton jälkeen tunniste `count` viittaa funktioon, joka käyttäytyy seuraavaan tapaan:


{% highlight javascript %}

x = count('/abc'); // 1
x = count('/abc'); // 2
x = count('/abc'); // 3
x = count('/def'); // 1
x = count('/abc'); // 4

{% endhighlight %}


`counter`-moduulin runko löytyy tehtäväpohjasta siten, että sen tarjoama funktio palauttaa aina arvon 9.

**Palauta** tehtävän ratkaisuna tiedostot `main.js` ja `counter.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat testit menevät läpi. Selenium-testit testaavat sovellusta kokonaisuutena. Yksikkötestauksen kohteena on *counter*-moduuli. Huomioi, että Selenium-testejä ajettaessa sovelluksen on oltava käynnissä.


### Vihjeitä ja lisätietoja

#### main.js

Tehtäväpohjan tiedosto `main.js` sisältää seuraavan koodin[^1]:

{% highlight javascript  linenos%}

var express = require('express');
var app = express();

app.get('/', function (req, res) {
    res.send('Hello World!');
});

app.listen(3000, function () {
    console.log('Example app listening on port 3000!');
});

{% endhighlight %}


Rivillä 1 otetaan käyttöön *Express* -sovelluskehys. `require`-funktio palauttaa funktion, joka sijoitetaan muuttujaan `express`.  Tämän funktion kutsu taas palauttaa muuttujaan `app` sijoitettavan [Application][Application]-tyyppisen objektin, jonka metodeita hyödynnetään riveillä 4 ja 8.    

[Application]: http://expressjs.com/en/4x/api.html#app

[Express][express]-sivustolla esimerkkisovellusta kuvataan seuraavasti:

> The app starts a server and listens on port 3000 for connections. The app responds with “Hello World!” for requests to the root URL (/) or route. For every other path, it will respond with a 404 Not Found. The `req` (request) and `res` (response) are the exact same objects that Node provides. 

[express]: http://expressjs.com

Jos halutaan, että funktio suoritetaan riippumatta pyynnössä olevasta polusta, edellä olevan listaukssa `app.get`-metodin kutsun [^2]  ensimmäisen parametri tulisi korvata arvolla `'*'`: 


[^2]: Sekä `use`- että mm. `get`-metodien kutsuilla voidaan määritellä pyynnön käsittelijäfunktiota (ks. [Writing][writing] \| [Using][using] Middleware).

[writing]: http://expressjs.com/en/guide/writing-middleware.html
[using]: http://expressjs.com/en/guide/using-middleware.html



{% highlight javascript %}

app.get('*', function (req, res) {
    res.send('Hello World');
});

{% endhighlight %}

Edellä esitetyllä muutoksella sovellus toimii kutakuinkin samoin kuin [tehtävän 1.0](../tehtava10) ratkaisu.


#### counter.js

Tehtäväpohjaan tiedostossa `counter.js` on määritelty seuraava moduuli: 


{% highlight javascript %}

var requests = {};

module.exports = function (url) {    
    return 9;
};

{% endhighlight %}


Moduulista näkyy ulospäin se, mikä on sijoitettu `exports`-muuttujan arvoksi so. tässä funktio, joka palauttaa kokonaisluvun `9`. Jos moduuli otetaan käyttöön lauseella `var count = require('counter')`, tunnisteen `count` kautta on käytettävissä moduulin määrittelemä funktio.

<br/>

Tehtävässä otettiin käyttöön *Express*-sovelluskehys, joka yksinkertaistaa web-sovellusten rakentamista perus-Nodeen verrattuna. Esillä oli myös periaate, miten sovellus voidaan jakaa erillisiin moduuleihin. [Seuraavassa tehtävässä](../tehtava13) sovelluksen käyttöön otetaan sivupohjat, jotka helpottavat käyttöliittymän rakentamista. 

<br/>

##### Alaviitteet

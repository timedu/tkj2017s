---
layout: exercise_page
title: "Tehtävä 1.5: Hello MVC (2p)"
exercise_template_name: W1E05.HelloMVC
exercise_discussion_id: 82691
exercise_upload_id: 
---

Laadi sovellus, joka toimiin ulospäin samoin[^1] [edellisen tehtävän](../tehtava14) ratkaisun kanssa. Sovellusten lähdekoodien jäsennykset kuitenkin poikkeavat toisistaan. Tässä tehtävässä sovellus rakentuu erään tyyppisen MVC-mallin mukaan (Model-View-Controller):

[^1]: Erona on kuitenkin se, että tässä Hello-sivulle tulostuu teksti *MVC* tekstin *Routing* sijaan.

~~~
                (request)  
                    ▾
[ Model ] <-- [ Controller ] --> [ View ]
~~~

*Kontrolleri* ottaa vastaan pyynnön, käsittelee pyyntöön liittyvän datan *mallin* avulla ja muodostaa sitten vasteen pyyntöön *näkymän* tuella.

Tässä tehtävän lähdekoodi jäsentyy seuraavasti:

~~~
Sources
 ├── controller.js    // Controller
 ├── counter.js       // Model
 ├── main.js
 └── views            // View
     ├── hello.hbs
     ├── summary.hbs
     └── layouts
         └── main.hbs        
~~~

Edelliseen tehtävään verrattuna tähän on lisätty moduuli `controller.js`, johon on tarkoitus sijoittaa aiemmin moduulissa `main.js` sijainneet `app.get` -metodin kutsut. Näiden sijaan päämoduuli ottaa käyttöön kontrollerin seuraavasti: 


{% highlight javascript linenos %}

...
const counter = require('./counter');

const controller = require('./controller');
controller.init(app, counter);
...

{% endhighlight %}

<small>Listaus 1. </small>

`main.js` ja `counter.js` ovat tehtäväpohjassa valmiina. Näkymät `hello.hbs` ja `summary.hbs`voi kopioida sellaisenaan edellisen tehtävän ratkaisusta.


**Palauta** tehtävän ratkaisuna tiedosto `controller.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat testit menevät läpi. Selenium-testit testaavat sovellusta kokonaisuutena. Yksikkötestaus testaa *counter*-moduulia, vaikka se ei sisällykään palautettavaan aineistoon. Selenium-testien osalta on huomioitava, että **sovellus on käynnistettävä uudelleen** juuri ennen testien ajoa.


### Vihjeitä ja lisätietoja

Moduulista palautuu sen käyttöönottamaan moduuliin se, mikä moduulin koodissa sijoitetaan `module.exports`-ominaisuuden arvoksi. Esim. edellä olevassa listauksessä rivillä 2 olevan lauseen myötä `counter` on seuraavanlainen objekti[^2]:

[^2]: ks. `counter.js`

~~~
+------------------+
| counter          |
+------------------+
| addRequest(url)  |
| getRequests(url) |
| getRequestsAll() |
+------------------+
~~~

Rivillä 4 olevan lauseen tulisi tuottaa seuraavanlainen objekti:

~~~
+---------------------+
| controller          |
+---------------------+
| init(app, counter)  |
+---------------------+
~~~

Rivillä 5 kutsutaan objektin `init`-metodia, jonka tulisi sisältää tässä tarvitavien `app.get`-metodien kutsut[^3].

[^3]: vastaavat kuin [edellisessä tehtävässä](../tehtava14) tuli laatia moduuliin `main.js`.


<br/>

Tämän osan ohjelmointitehtävissä tutustuttiin kurssin tehtävissä jatkossakin esillä olevaan peruskalustoon, *Node*, *Express* ja *Handlebars*. [Seuraava tehtävä](../tehtava16) sisältää kysymysjoukon kurssilukemiston kahdesta ensimmäisestä luvusta.

<br/>

##### Alaviitteet



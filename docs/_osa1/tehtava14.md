---
layout: exercise_page
title: "Tehtävä 1.4: Hello Routing (2p)"
exercise_template_name: W1E04.HelloRouting
exercise_discussion_id: 82690
exercise_upload_id: 
---


Muokkaa [edellisen tehtävän](../tehtava13) ratkaisuasi siten, että tehtäessä pyyntö osoitteen juureen (`localhost:3000/`) sovellus esittää yhteenvedon eri polkuihin tehdyistä pyynnöistä. 

Kun pyyntö kohdistuu johonkin muuhun polkuun kuin juureen, vaste on samankaltainen kuin edellisissä tehtävissä. Esim. kolmas osoitteeseen `localhost:3000/abc` tehty pyyntö tuottaa seuraavan vasteen:

~~~
Hello 3 from Routing (/abc)
~~~

Osoitteesta `localhost:3000/` saatava pyyntöjen yhteenveto  näkyy selaimessa seuraavasti:

~~~
Request Summary

/abc: 3
/def: 2
~~~

Vaste on html-muodossa seuraavanlainen:

{% highlight  html %}

<p>Request Summary</p>
<div>
  <span>/abc: 3</span><br/>
  <span>/def: 2</span><br/>
</div>

{% endhighlight %}

Edellä esitetty liittyy siis tilanteeseen, jossa on tehty ennen yhteenvetopyyntoä kolme pyyntöä osoitteeseen `localhost:3000/abc` ja kaksi pyyntöä osoitteeseen `localhost:3000/def`. 

Sovelluksen lähdekoodi jäsentyy projektissa seuraavasti:

~~~
├── counter.js
├── main.js
└── views
    ├── hello.hbs
    ├── summary.hbs
    └── layouts
        └── main.hbs        
~~~

`counter.js` ja `views/layouts/main.hbs` ovat projektipohjassa valmiina. Tiedoston `views/hello.hbs` voi kopioda sellaisenaan [edellisen tehtävän](../tehtava13) ratkaisusta. Tiedostoista `main.js` ja `views/summary.hbs` pohja sisältää suhteellisen pitkälle viedyt rungot. 

Pohjassa oleva *counter* -moduuli tarjoaa seuraavat kolme metodia.

* `addRequest`: lisää polun pyyntöjen määrää yhdellä
* `getRequests`: palauttaa polun pyyntöjen määrän
* `getRequestsAll`: palauttaa objektin, joka sisältää kaikkien polkujen pyyntöjen määrät

**Palauta** tehtävän ratkaisuna tiedostot `main.js` ja `summary.hbs`. Varmista ennen palautusta, että tehtäväpohjassa olevat testit menevät läpi. Selenium-testit testaavat sovellusta kokonaisuutena. Yksikkötestaus testaa counter-moduulia, vaikka se ei sisällykään palautettavaan aineistoon. Selenium-testien osalta on huomioitava, että **sovellus on käynnistettävä uudelleen** ennen testien ajoa.

### Vihjeitä ja lisätietoja

#### Reitityksestä

Toteuta reititys kahdella [Express][express] -kehyksen `get`-metodin kutsulla. Sovellus toimii niin, että pyynnön yhteydessä ainoastaan yksi `get`-metodien kutsuissa määritelty funktio suoritetaan. `get`-metodien kutsujen järjestys sovelluksessa on merkityksellinen. Tässä `get`-metodien kutsuissa esiintyvät polut ovat `'*'` ja `'/'` - kumpaan liittyvä `get` pitäisi olla ensin? Tietoa Expressin reitityksestä on mm. sivuilla [Basic routing][basic-routing] ja  [Routing][routing].

[express]: http://expressjs.com
[basic-routing]: http://expressjs.com/en/starter/basic-routing.html
[routing]: http://expressjs.com/en/guide/routing.html

#### Sivupohjien ohjausrakenteista

[Handlebars][handlebars] -sivupohjat voivat sisältää html-koodin ja muuttujien lisäksi ns. helppereitä, joiden avulla on toteutettu paketissa valmiina olevat ohjausrakenteet. Tehtävän ratkaisua tässä tukenee [each][each-helper] -helpperi, johon liittyvä seuraava, Handelbars -sivustolta poimittu, esimerkki lienee suhteellisen osuva tähän tilanteeseen: 


{% highlight html %}
{% raw %}

{{#each object}}
  {{@key}}: {{this}}
{{/each}}

{% endraw %}
{% endhighlight %}


`object` on objekti, jonka kaikki ominaisuudet käydään läpi silmukassa. Silmukan sisällä {% raw %}`{{@key}}`{% endraw %} viittaa objektin ominaisuuden nimeen ja {% raw %}`{{this}}`{% endraw %} ominaisuuden arvoon. Tehtäväpohjassa olevan `counter`-objektin `getRequestsAll`-metodi palauttaa objektin, jonka sisältämät tiedot voidaan purkaa näkymään edellä olevaa esimerkkiä noudattamalla.
 
[handlebars]: http://handlebarsjs.com
[each-helper]: http://handlebarsjs.com/builtin_helpers.html#iteration

<br/>

Tässä sovellukseen rakennettiin reititys siten, että pyynnön yhteydessä suoritettava koodi oli riippuvainen pyynnössä olevasta polusta. Samalla sovellukseen lisättiin toinen näkymä. [Seuraavassa tehtävässä](../tehtava15) sovellus jäsennetään yleistesti käytetyn MVC -mallin[^mvc] mukaan.

[^mvc]: Model - View - Controller

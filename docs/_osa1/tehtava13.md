---
layout: exercise_page
title: "Tehtävä 1.3: Hello Handlebars (2p)"
exercise_template_name: "W1E03.Hellohandlebars"
exercise_discussion_id: 82689
exercise_upload_id: 
---


Tehtäväpohjassa on *Express*-sovelluskehykseen ja *Handlebars*-sivupohjiin (template) tukeutuva *Node*-sovellus, joka tuottaa selaimen ikkunaan tekstin `Hello Handlebars`, kun selain tekee pyynnön osoitteeseen `localhost:3000/`. 

Muokkaa sovellusta siten, että se käyttäytyy ulospäin kuten [tehtävän 1.1](../tehtava11) ratkaisu kuitenkin niin, että teksti "Node" on tulosteessa korvattu tekstillä "Handlebars", esim:

~~~
Hello 3 from Handlebars (/abc)
~~~

Toteuta ratkaisu siten, että se tukeutuu *Handlebars*-sivupohjiin, joiden rungot löytyvät pohjakoodista: `main.hbs` ja `hello.hbs`. [Edellisen tehtävän](../tehtava12) tapaan sovellus hyödyntää *Express*-sovelluskehystä sekä *counter*-moduulia, jonka voi kopioida sellaisenaan edellisen tehtävän ratkaisusta.

**Palauta** tehtävän ratkaisuna tiedostot `main.js` ja `hello.hbs`. Varmista ennen palautusta, että tehtäväpohjassa olevat testit menevät läpi. Selenium-testit testaavat sovellusta kokonaisuutena. Yksikkötestaus testaa on counter-moduulia, vaikka se ei sisällykään palautettavaan aineistoon. Selenium-testejä ajettaessa sovelluksen on oltava käynnissä.

### Vihjeitä ja lisätietoja

#### Handlebars-perusteita

Seuraavassa on muutama esimerkki, jotka on laadittu tässä käytettävän [Express Handlebars][express-handlebars] -paketin sivustolta löytyvien esimerkkien pohjalta.

[express-handlebars]: https://github.com/ericf/express-handlebars

Sivupohjat talletetaan tyypillisesti sovelluksen lähdekoodihakemiston `views`-alihakemistoon:

~~~
├── ...
└── views
    ├── home.hbs
    └── layouts
        └── main.hbs
~~~

<small>Kuva 1.<small>


Edellä olevassa esimerkissä on kaksi sivupohjaa, joista `main.hbs` on *layout* ja `home.hbs` on näkymä (*view*). Näkymä hahmonnetaan osaksi layoutia. Layout voi olla esim. html-sivu, johon on merkitty paikka näkymälle:


{% highlight  html %}
{% raw %}

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Example</title>
  </head>
  <body>
    {{{body}}}
  </body>
</html>

{% endraw %}
{% endhighlight %}

<small>Listaus 1.<small>


{% raw %}`{{{body}}}`{% endraw %} siis korvataan näkymällä, joka voi olla layoutiin sijoitettava html-viipale, esim.


{% highlight  html %}

<h1>Example: Home</h1>

{% endhighlight %}

<small>Listaus 2.<small>


Edellä esitetty näkymä voidaan hahmontaa sovelluksessa "response"-objektin `render`-metodin kutsulla seuraavasti:


{% highlight javascript %}

app.get('/', function (req, res) {
    res.render('home');
});

{% endhighlight %}

<small>Listaus 3.<small>


`render`-metodin parametrina annetaan hahmonnettava näkymä. Pyynnön vasteeksi palautuu seuraavanlainen html-sivu: 


{% highlight  html %}

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Example</title>
  </head>
  <body>
    <h1>Example: Home</h1>
  </body>
</html>

{% endhighlight %}

<small>Listaus 4.<small>


Näkymä voi sisältää muuttujia, esim:


{% highlight  html %}
{% raw %}

<h1>Example: {{pageName}}</h1>

{% endraw %}
{% endhighlight %}

<small>Listaus 5.<small>


Näkymän muuttujalle voidaan määritellä arvo hahmonnuksen yhteydessä:


{% highlight javascript %}

app.get('/', function (req, res) {
    res.render('home', {
        pageName: 'Home'
    });
});

{% endhighlight %}

<small>Listaus 6.<small>


`render`-metodin toisena parametrina annetaan objekti, jolla välitetään tietoja näkymälle. Hahmonnuksen yhteydessä *Listauksessa 5* esiintyvä {% raw %}`{{pageName}}`{% endraw %} korvautuu tässä merkkijonolla `'Home'`.

Tehtäväpohjan `main.js`-moduulissa on valmiina *Handelbars*-sivupohjien käyttöönotto ja käyttöön liittyvät asetukset:


{% highlight javascript %}

...
var handlebars = require('express-handlebars');
app.engine('.hbs', handlebars({
    defaultLayout: 'main',
    extname: '.hbs',
    layoutsDir: 'sources/views/layouts/'
}));
app.set('view engine', '.hbs');
app.set('views', __dirname + '/views'); // 
...

{% endhighlight %}

<small>Listaus 6.<small>

Lisätietoa asetuksista löytyy [Express Handlebars][express-handlebars] -sivustolta. Sivupohjien laatimista ohjaa tarkemmin varsinainen [Handlebars-sivusto][handlebars].

[handlebars]: http://handlebarsjs.com

#### Handlebars NetBeans:issa

NetBeans ei sisällä erityisisä *Handlebars* -koodipohjia. Handlebars -tiedoston voi muodostaa esim. tiedostopohjasta *Empty File* antamalla sille `.hbs` -loppuliitteen tai vaikka pohjasta *HTML File* , jonka jälkeen tiedoston loppuliitteen voi muuttaa tiedoston *Properties* -valinnan kautta. 

`.hbs` -loppuisille tiedostoille ei ole NetBeansissa oletusarvoisesti mitään syntaksikorostusta, mutta sen voi määritellä *Stack Overflow* -palstalta löytyvien [ohjeiden][hbs-syntax] mukaan. Luontevin MIME-tyyppi tiedostoille lienee *text/html*.

[hbs-syntax]: http://stackoverflow.com/questions/22533481

Handlebars -tiedostoille voi myös halutessaan laatia tiedostopohjia (*Save As Template ...*), joita voi sitten muokata *Tools* -valikon *Templates* -valinnan kautta. 

<br/>

Tässä tehtävässä sovellukselle muodostettiin html-käyttöliittymä hyödyntäen *Handelbars* -sivupohjia. [Seuraavassa tehtävässä](../tehtava14) sovellusta täydennetään toisella näkymällä siten, että näkymän valintaa ohjaa pyynnössä oleva polku.

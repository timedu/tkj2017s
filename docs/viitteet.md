---
layout: site_page
title: Viitteet
permalink: /viitteet/index.html 
site_menu: 1
---

### Yleistä tietokannoista

#### NoSQL Distilled

Sadalage, Pramod J. & Fowler, Martin (2013). [NoSQL Distilled: A Brief Guide to the Emerging World of Polyglot Persistence][NoSQL Distilled].
 
[NoSQL Distilled]: https://www.pearson.com/us/higher-education/program/Sadalage-No-SQL-Distilled-A-Brief-Guide-to-the-Emerging-World-of-Polyglot-Persistence/PGM75436.html

*NoSQL Distilled* toimii kurssilukemistona siten, että kuhunkin kurssin seitsemään osaan sisältyy kirjaan perustuva kysymyssarja. Tentti sisältää vastaavanlaisia kysymyksiä. Kirjassa on n. 150 sivua, jotka jakaantuvat 15 lukuun. Kysymyssarjoissa käsitellään kirjaa luvuittain siten, että osien 1-6 kysymysten pohjana on aina kirjan kaksi lukua, jolloin osaan 7 jää kysymyksiä kolmesta viimeisestä luvuista.

Googlen Play-kaupassa on kirjasta [ilmainen näyte][free-sample][^1], joka sisältää kirjan alkupään luvut. Verkosta löytynee myös muita ehkä laajempiakin näytekappaleita. 

[free-sample]: https://play.google.com/store/books/details?id=AyY1a6-k3PIC
[^1]: viitattu 31.8.2017

Kirjan ydinkohtia on tuotu esiin sivuilla [NoSQL Databases: An Overview][nosql-overview] ja [Key Points from NoSQL Distilled][nosql-distilled-key-points]. Painovirheet on listattu [Errata-sivulla][nosql-distilled-errata].

[nosql-overview]: https://www.thoughtworks.com/insights/blog/nosql-databases-overview
[nosql-distilled-key-points]: https://martinfowler.com/articles/nosqlKeyPoints.html
[nosql-distilled-errata]: https://martinfowler.com/nosqlErrata.html

Seuraavat suhteellisen samaisältöiset Martin Fowlerin Youtube-videot pohjautuvat *NoSQL Distilled* -kirjaan:

* [Introduction to NoSQL][NoSQL-youtube-1] (50 min.)   
[International Software Development Conference][goto2012] in Aarhus, Denmark 2012
* [NoSQL Distilled to an hour][NoSQL-youtube-2] (1 h)   
[NoSQL matters Conference][NoSQLmatters2013] in Cologne, Germany 2013

[NoSQL-youtube-1]: https://www.youtube.com/watch?v=qI_g07C_Q5I
[NoSQL-youtube-2]: https://www.youtube.com/watch?v=ASiU89Gl0F0
[goto2012]: http://gotocon.com/aarhus-2012/presentation/Introduction%20to%20NoSQL
[NoSQLmatters2013]: https://2013.nosql-matters.org/cgn/index.html%3Fp=1834.html

Yliopiston kirjastossa ei ole kirjasta e-versiota. Perinteisiä niteitä on niukahkosti, mutta täydennystä odotellaan parhaillaan.


#### Seven Databases

Redmond, Eric & Wilson, Jim R. (2012). [Seven Databases in Seven Weeks: A Guide to Modern Databases and the NoSQL Movement][seven-databases].  

[seven-databases]: https://pragprog.com/book/rwdata/seven-%20databases-in-seven-weeks

*Seven Databases* on yksi tämän kurssin inspiraation lähteistä. Tosin kurssilla on esillä ainoastaan kuusi eri tietokannan hallintajärjestelmää ja käsiteltävät esimerkit eivät ole aivan samoja kuin *Seven Databases* -kirjassa. *Seven Databases* on jonkin verran teknisempi kuin *NoSQL Distilled*.



#### Tietokantojen perusteita yliopistoissa

[Tietokannat ja tiedonhallinnan perusteet][ITKA204]. 
Kurssimoniste. Jyväskylän yliopisto.
(Viitattu 28.8.2017)   
[Tietokantojen perusteet][581328].
Kurssimateriaali. Helsingin yliopisto.
(Viitattu 28.8.2017)

[ITKA204]: https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste
[581328]: http://tietokantojen-perusteet.github.io

#### Muita viitteitä

<http://db-engines.com>   

<https://bitnami.com/stacks/database>


### Sovellusympäristö

[NetBeans IDE](http://netbeans.org)  


#### JavaScript

<https://developer.mozilla.org/en-US/docs/Web>

<http://es6-features.org/>

<http://www.ecma-international.org/ecma-262/6.0/>

<http://eloquentjavascript.net>


#### [Node][node] & [Express][express]   

* [Node Documentation][node-doc]: [HTTP][node-http]
* [Express API Reference][express-api]

* [body-parser][body-parser]
* [csvtojson](https://www.npmjs.com/package/csvtojson)

<https://docs.npmjs.com>

[node]: https://nodejs.org 
[node-http]: https://nodejs.org/dist/latest-v6.x/docs/api/http.html 
[node-doc]: https://nodejs.org/dist/latest-v6.x/docs/api/index.html
[express]: http://expressjs.com  
[express-api]: http://expressjs.com/en/4x/api.html
[body-parser]: https://www.npmjs.com/package/body-parser


#### [Handlebars][handlebars]
   
* [express-handlebars][express-handlebars]
   
   
[handlebars]: http://handlebarsjs.com
[express-handlebars]:https://github.com/ericf/express-handlebars



### Relaatiotietokannat ja ORM

[SQLite][sqlite]  

* [SQL As Understood By SQLite](https://www.sqlite.org/lang.html)
* [node-sqlite3](https://github.com/mapbox/node-sqlite3), [API](https://github.com/mapbox/node-sqlite3/wiki/API)
* [SQLite Manager (Firefox lisäosa)](https://addons.mozilla.org/fi/firefox/addon/sqlite-manager/)
* [DB Browser for SQLite](http://sqlitebrowser.org)

[Sequelize][sequelize]   
[Bookshelf](http://bookshelfjs.org)

### Avain-arvoparitietokannat

[LevelDB](http://leveldb.org)

* [Node.js Databases: An Embedded Database Using LevelDB](https://blog.yld.io/2016/10/24/node-js-databases-an-embedded-database-using-leveldb). Pedro Teixeira, 2016.
* [LevelUP](https://github.com/Level/levelup/blob/master/README.md)
* [level-sublevel](https://github.com/dominictarr/level-sublevel/blob/master/README.md#level-sublevel)
* [cuid](https://github.com/ericelliott/cuid/blob/master/README.markdown#cuid)
  
[Redis][redis] 
 
[redis]: https://redis.io
  
### Dokumenttitietokannat
  
[MongoDB][mongodb]  

* [MongoDB Manual](https://docs.mongodb.com/manual/)
* [MongoDB Node.JS Driver](http://mongodb.github.io/node-mongodb-native/)
* [node-mongodb-native](https://github.com/mongodb/node-mongodb-native/blob/2.2/README.md)
* <https://mlab.com>
* [MongoDB for Node.js Developers](https://university.mongodb.com/courses/M101JS/about) (Online course)

[NeDB](https://github.com/louischatriot/nedb/blob/master/README.md)   
[TingoDB](http://www.tingodb.com)


### Graafitietokannat

[LevelGraph](https://github.com/mcollina/levelgraph/blob/master/README.md)

* [Graph databases in the browser: using LevelGraph to explore New Delhi](http://www.vldb.org/pvldb/vol9/p1469-maccioni.pdf). Maccioni&Collina, 2016.

[Neo4j][neo4j]

* [Cypher Query Language](https://neo4j.com/developer/cypher/)
* [Neo4j Driver for Javascript](http://neo4j.com/docs/api/javascript-driver/current/)
* [GrapheneDB](http://www.graphenedb.com)

### Sarakeperhetietokannat

[Cassandra][cassandra]  

* [DataStax Node.js Driver for Apache Cassandra](http://docs.datastax.com/en/developer/nodejs-driver/3.2/)
* [Cassandra Virtual Machines ](https://bitnami.com/stack/cassandra/virtual-machine) (download)
* [Bitnami Cassandra Virtual Machine](https://docs.bitnami.com/virtual-machine/infrastructure/cassandra/) (doc)
* [DS101: Introduction to Apache Cassandra](https://academy.datastax.com/resources/ds101-introduction-cassandra). DataStax Academy.
* [DS220: Data Modeling](https://academy.datastax.com/resources/ds220-data-modeling).
DataStax Academy.
* [Datastax Docs](http://docs.datastax.com/en/landing_page/doc/landing_page/current.html)
* [NetCassandraBeans - plugin](http://plugins.netbeans.org/plugin/59444/netcassandrabeans)

### Hybriditietokannat

[OrientDB][orientdb]  

* [OrientDB Manual](http://orientdb.com/docs/last/)
* [OrientJS Driver](http://orientdb.com/docs/last/OrientJS.html) 
* [OrientDB - Getting Started](https://www.udemy.com/orientdb-getting-started/). Udemy.


[sqlite]: https://www.sqlite.org
[sequelize]: http://www.sequelizejs.com

[mongodb]: https://www.mongodb.com
[neo4j]: https://neo4j.com
[cassandra]: http://cassandra.apache.org
[orientdb]: http://orientdb.com


### Kurssin tehtävien testeihin liittyviä tekniikoita

[Mocha](https://mochajs.org)

* [mocha (npm)](https://www.npmjs.com/package/mocha)

[Node.js / Assert](https://nodejs.org/dist/latest-v6.x/docs/api/assert.html)

[PhantomJS](http://phantomjs.org)

* [phantomjs-prebuilt (npm)](https://www.npmjs.com/package/phantomjs-prebuilt)

[Selenium WebDriver](http://www.seleniumhq.org/docs/03_webdriver.jsp)

* [JavaScript API](http://seleniumhq.github.io/selenium/docs/api/javascript/)
* [selenium-webdriver (npm)](https://www.npmjs.com/package/selenium-webdriver)
* [selenium-webdriver/phantomjs note](https://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/phantomjs.html)

<http://chaijs.com>


### Tietokantoihin liittyviä opintojaksoja yliopistoissa


#### TTY, Hervanta

* [Johdatus tietokantoihin](http://www.tut.fi/opinto-opas/wwwoppaat/opas2017-2018/perus/aineryhmat/Ohjelmistotekniikka/TIE-22101.html)
* [Tietokantojen suunnittelu](http://www.tut.fi/opinto-opas/wwwoppaat/opas2017-2018/perus/aineryhmat/Ohjelmistotekniikka/TIE-22201.html)
* [Data-Intensive Programming](http://www.tut.fi/opinto-opas/wwwoppaat/opas2017-2018/perus/aineryhmat/Ohjelmistotekniikka/TIE-22307.html)

#### TTY, Pori

* [Tiedonhallinta ja tietokannat](http://www.tut.fi/opinto-opas/wwwoppaat/opas2017-2018/pori/aineryhmat/Ohjelmistotekniikka/PLA-32602.html)
* [Tietokantajärjestelmät](http://www.tut.fi/opinto-opas/wwwoppaat/opas2017-2018/pori/aineryhmat/Ohjelmistotekniikka/PLA-32611.html) (tämä kurssi)

#### Helsingin yliopisto, Tietojenkäsittelytiede


* [Tietokantojen perusteet](https://courses.helsinki.fi/fi/TKT10004/119284739) (perus)
* [Tietokannan suunnittelu](https://courses.helsinki.fi/fi/TKT21001/119284741) (aine)
* [Introduction to Big Data Management](https://courses.helsinki.fi/fi/DATA14002/119122647) (syventävä)


<br/>


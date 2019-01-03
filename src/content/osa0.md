---
title: osa 0
subTitle: Web-sovelluksen toimintaperiaatteita
path: /osa0/perusteet
mainImage: ../images/osa0.png
part: 0
letter: a
partColor: blue
---

<div class="content">

Ennen kuin aloitamme ohjelmoinnin, käymme läpi web-sovellusten toimintaperiaatteita tarkastelemalla osoitteessa <https://fullstack-exampleapp.herokuapp.com/> olevaa esimerkkisovellusta. Huom. sovelluksen toinen versio on osoitteessa <https://fullstack-example.now.sh>, voit käyttää kumpaa vaan.

Sovelluksen olemassaolon tarkoitus on ainoastaan havainnollistaa kurssin peruskäsitteistöä. Sovellus ei ole missään tapauksessa esimerkki siitä, _miten_ web-sovelluksia kannattaisi kehittää. Päinvastoin se demonstroi eräitä historiallisia web-sovellusten toteutukseen käytettyjä tapoja ja tekniikoita, joiden katsotaan nykyään olevan jopa _huonoja käytänteitä_.

Kurssin suosittelemaa tyyliä noudattavan koodin kirjoittaminen alkaa [osasta 1](/osa1).

Käytä nyt ja _koko ajan_ tämän kurssin aikana Chrome-selainta.

Avataan selaimella [esimerkkisovellus](https://fullstack-exampleapp.herokuapp.com/). Sivun ensimmäinen lataus kestää joskus hetken.

### Web-sovelluskehityksen sääntö numero yksi

#### Pidä selaimen developer-konsoli koko ajan auki

Konsoli avautuu macilla painamalla yhtä aikaa _alt_ _cmd_ ja _i_.

Ennen kun jatkat eteenpäin, selvitä miten saat koneellasi konsolin auki (googlaa tarvittaessa) ja muista pitää se auki _aina_ kun teet web-sovelluksia.

Konsoli näyttää seuraavalta:
![](./images/1/1.png)

Varmista, että välilehti _Network_ on avattuna ja aktivoi valinta _Disable cache_ kuten kuvassa on tehty. Myös _Preserve logs_ on joskus hyödyllinen, se säilyttää sovelluksen tulostamat logit sivujen uudelleenlatauksen yhteydessä.

**HUOM:** konsolin tärkein välilehti on _Console_. Käytämme nyt johdanto-osassa kuitenkin ensin melko paljon välilehteä _Network_.

### HTTP GET

Selain ja web-palvelin kommunikoivat keskenään [HTTP](https://developer.mozilla.org/fi/docs/Web/HTTP)-protokollaa käyttäen. Avoinna oleva konsolin Network-tabi kertoo miten selain ja palvelin kommunikoivat.

Kun nyt reloadaat sivun, kertoo konsoli, että tapahtuu kaksi asiaa

- selain hakee web-palvelimelta sivun _fullstack-exampleapp.herokuapp.com/_ sisällön
- ja lataa kuvan _kuva.png_

![](./images/1/2.png)

Jos ruutusi on pieni, saatat joutua suurentamaan konsoli-ikkunaa, jotta saat selaimen tekemät haut näkyviin.

Klikkaamalla näistä ensimmäistä, paljastuu tarkempaa tietoa siitä mistä on kyse:

![](./images/1/3.png)

Ylimmästä osasta _General_ selviää, että selain teki [GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)-metodilla pyynnön osoitteeseen _https://fullstack-exampleapp.herokuapp.com/_ ja että pyyntö oli onnistunut, sillä pyyntöön saatiin vastaus, jonka [Status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) on 200.

Pyyntöön ja palvelimen lähettämään vastaukseen liittyy erinäinen määrä otsakkeita eli [headereita](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields):

![](./images/1/4.png)

Ylempänä oleva _Response headers_ kertoo mm. vastauksen koon tavuina ja vastaushetken. Tärkeä headeri [Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) kertoo, että vastaus on [utf-8](https://en.wikipedia.org/wiki/UTF-8)-muodossa oleva tekstitiedosto, jonka sisältö on muotoiltu HTML:llä. Näin selain tietää, että kyseessä on normaali [HTML](https://en.wikipedia.org/wiki/HTML)-sivu, joka tulee renderöidä käyttäjän selaimeen "websivun tavoin".

Välilehti _Preview_ näyttää, miltä pyyntöön vastauksena lähetetty data näyttää. Kyseessä on siis normaali HTML-sivu, jonka _body_-osassa määritellään selaimessa näytettävän sivun rakenne:

![](./images/1/5.png)

Sivu sisältää [div](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div)-elementin, jonka sisällä on otsikko sekä tieto luotujen muistiinpanojen määrästä, linkki sivulle _muistiinpanot_ ja kuvaa vastaava [img](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)-tagi.

img-tagin ansiosta selain tekee toisenkin _HTTP-pyynnön_, jonka avulla se hakee kuvan _kuva.png_ palvelimelta. Pyynnön tiedot näyttävät seuraavalta:

![](./images/1/6.png)

eli pyyntö on tehty osoitteeseen _https://fullstack-exampleapp.herokuapp.com/kuva.png_ ja se on tyypiltään HTTP GET. Vastaukseen liittyvät headerit kertovat että vastauksen koko on 89350 tavua ja vastauksen [Content-type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) on _image/png_, eli kyseessä on png-tyyppinen kuva. Tämän tiedon ansiosta selain tietää, miten kuva on piirrettävä HTML-sivulle.

### Perinteinen web-sovellus

Esimerkkisovelluksen pääsivu toimii _perinteisen web-sovelluksen_ tapaan. Mentäessä sivulle, selain hakee palvelimelta sivun strukturoinnin ja tekstuaalisen sisällön määrittelevän HTML-dokumentin.

Palvelin on muodostanut dokumentin jollain tavalla. Dokumentti voi olla _staattista sisältöä_ eli palvelimen hakemistossa oleva tekstitiedosto. Dokumentti voi myös olla _dynaaminen_, eli palvelin voi muodostaa HTML-dokumentit ohjelmakoodin avulla hyödyntäen esim. tietokannassa olevaa dataa. Esimerkkisovelluksessa sivun HTML-koodi on muodostettu dynaamisesti, sillä se sisältää tiedon luotujen muistiinpanojen lukumäärästä.

Etusivun muodostava koodi näyttää seuraavalta:

```js
const getFrontPageHtml = noteCount => {
  return `
    <!DOCTYPE html>
    <html>
      <head>
      </head>
      <body>
        <div class="container">
          <h1>Full stack -esimerkkisovellus</h1>
          <p>muistiinpanoja luotu ${noteCount} kappaletta</p>
          <a href="/notes">muistiinpanot</a>
          <img src="kuva.png" width="200" />
        </div>
      </body>
    </html>
  `;
};

app.get('/', (req, res) => {
  const page = getFrontPageHtml(notes.length);
  res.send(page);
});
```

Koodia ei tarvitse vielä ymmärtää, mutta käytännössä HTML-sivun sisältö on talletettu ns. template stringinä, eli merkkijonona, jonka sekaan on mahdollisuus evaluoida esim. muuttujien arvoja. Etusivun dynaamisesti muuttuva osa, eli muistiinpanojen lukumäärä (koodissa _noteCount_) korvataan template stringissä sen hetkisellä konkreettisella lukuarvolla (koodissa _notes.length_).

HTML:n kirjoittaminen suoraan koodin sekaan ei tietenkään ole järkevää, mutta vanhan liiton PHP-ohjelmoijille se oli arkipäivää.

Perinteisissä websovelluksissa selain on "tyhmä", se ainoastaan pyytää palvelimelta HTML-muodossa olevia sisältöjä, kaikki sovelluslogiikka on palvelimessa. Palvelin voi olla tehty esim. kurssin [Web-palvelinohjelmointi](https://courses.helsinki.fi/fi/tkt21007/119558639) tapaan Java Springillä tai [tietokantasovelluksessa](http://tsoha.github.io/#/johdanto#top) PHP:llä tai [Ruby on Railsilla](http://rubyonrails.org/). Esimerkissä on käytetty Node.js:n [Express](https://expressjs.com/)-sovelluskehystä. Tulemme käyttämään kurssilla Node.js:ää ja Expressiä web-palvelimen toteuttamiseen.

### Selaimessa suoritettava sovelluslogiikka

Pidä konsoli edelleen auki. Tyhjennä konsolin näkymä painamalla vasemmalla olevaa &empty;-symbolia.

Kun menet nyt [muistiinpanojen](https://fullstack-exampleapp.herokuapp.com/notes) sivulle, selain tekee 4 HTTP-pyyntöä:

![](./assets/1/7.png)

Kaikki pyynnöt ovat _eri tyyppisiä_. Ensimmäinen pyyntö on tyypiltään _document_.
Kyseessä on sivun HTML-koodi, joka näyttää seuraavalta:

![](./assets/1/8.png)

Kun vertaamme, selaimen näyttämää sivua ja pyynnön palauttamaa HTML-koodia, huomaamme, että koodi ei sisällä ollenkaan muistiinpanoja sisältävää listaa.

HTML-koodin [head](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head)-osio sisältää [script](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)-tagin, jonka ansiosta selain lataa _main.js_-nimisen Javascript-tiedoston palvelimelta.

Ladattu Javascript-koodi näyttää seuraavalta:

```js
var xhttp = new XMLHttpRequest();

xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText);
    console.log(data);

    var ul = document.createElement('ul');
    ul.setAttribute('class', 'notes');

    data.forEach(function(note) {
      var li = document.createElement('li');

      ul.appendChild(li);
      li.appendChild(document.createTextNode(note.content));
    });

    document.getElementById('notes').appendChild(ul);
  }
};

xhttp.open('GET', '/data.json', true);
xhttp.send();
```

Koodin yksityiskohdat eivät ole tässä osassa oleellisia, koodia on kuitenkin liitetty mukaan tekstin ja kuvien mausteeksi. Pääsemme kunnolla koodin pariin vasta osassa 1. Tämän osan esimerkkisovelluksen koodi ei itseasiassa ole ollenkaan relevanttia kurssilla käytettävien ohjelmointitekniikoiden kannalta.

> Joku saattaa ihmetellä miksi käytössä on xhttp-olio eikä modernimpi fetch. Syynä on se, että tässä osassa ei haluta mennä ollenkaan promiseihin ja koodin rooli esimerkissä on muutenkin sekundäärinen. Palaamme osassa 2 uudenaikaisempiin tapoihin tehdä pyyntöjä palvelimelle.

Heti ladattuaan _script_-tagin sisältämän Javascriptin selain suorittaa koodin.

Kaksi viimeistä riviä määrittelevät, että selain tekee GET-tyyppisen HTTP-pyynnön palvelimen osoitteeseen _/data.json_:

```js
xhttp.open('GET', '/data.json', true);
xhttp.send();
```

Kyseessä on alin Network-välilehden näyttämistä selaimen tekemistä pyynnöistä.

Voimme kokeilla mennä osoitteeseen <https://fullstack-exampleapp.herokuapp.com/data.json> suoraan selaimella:

![](./assets/1/9.png)

Osoitteesta löytyvät muistiinpanot [JSON](https://en.wikipedia.org/wiki/JSON)-muotoisena "raakadatana". Oletusarvoisesti selain ei osaa näyttää JSON-dataa kovin hyvin, mutta on olemassa lukuisia plugineja, jotka hoitavat muotoilun. Asenna nyt Chromeen esim. [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc) ja lataa sivu uudelleen. Data on nyt miellyttävämmin muotoiltua:

![](./assets/1/10.png)

Ylläoleva muistiinpanojen sivun Javascript-koodi siis lataa muistiinpanot sisältävän JSON-muotoisen datan ja muodostaa datan avulla selaimeen "bulletlistan" muistiinpanojen sisällöstä:

Tämän saa aikaan seuraava koodi:

```js
const data = JSON.parse(this.responseText);
console.log(data);

var ul = document.createElement('ul');
ul.setAttribute('class', 'notes');

data.forEach(function(note) {
  var li = document.createElement('li');

  ul.appendChild(li);
  li.appendChild(document.createTextNode(note.content));
});

document.getElementById('notes').appendChild(ul);
```

Koodi muodostaa ensin järjestämätöntä listaa edustavan [ul](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul)-tagin:

```js
var ul = document.createElement('ul');
ul.setAttribute('class', 'notes');
```

ja lisää ul:n sisään yhden [li](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/li)-elementin kutakin muistiinpanoa kohti. Ainoastaan muistiinpanon _content_-kenttä tulee li-elementin sisällöksi, raakadatassa olevia aikaleimoja ei käytetä mihinkään.

```js
data.forEach(function(note) {
  var li = document.createElement('li');

  ul.appendChild(li);
  li.appendChild(document.createTextNode(note.content));
});
```

Avaa nyt konsolin _Console_-välilehti:

![](./assets/1/11.png)

Painamalla rivin alussa olevaa kolmiota saat laajennettua konsolissa olevan rivin:

![](./assets/1/12.png)

Konsoliin ilmestynyt tulostus johtuu siitä, että koodiin oli lisätty komento _console.log_:

```js
const data = JSON.parse(this.responseText);
console.log(data);
```

eli vastaanotettuaan datan palvelimelta, koodi tulostaa datan konsoliin.

Konsolin välilehti _Console_ sekä komento _console.log_ tulevat varmasti erittäin tutuiksi kurssin kuluessa.

### Tapahtumankäsittelijä ja takaisinkutsu

Koodin rakenne on hieman erikoinen:

```js
var xhttp = new XMLHttpRequest();

xhttp.onreadystatechange = function() {
  // koodi, joka käsittelee palvelimen vastauksen
};

xhttp.open('GET', '/data.json', true);
xhttp.send();
```

eli palvelimelle tehtävä pyyntö suoritetaan vasta viimeisellä rivillä. Palvelimen vastauksen käsittelyn määrittelevä koodi on kirjoitettu jo aiemmin. Mistä on kyse?

Rivillä

```js
xhttp.onreadystatechange = function () {
```

kyselyn tekevään <code>xhttp</code>-olioon määritellään _tapahtumankäsittelijä_ (event handler) tilanteelle _onreadystatechange_. Kun kyselyn tekevän olion tila muuttuu, kutsuu selain tapahtumankäsittelijänä olevaa funktiota. Funktion koodi tarkastaa, että [readyState](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/readyState):n arvo on 4 (joka kuvaa tilannetta _The operation is complete_) ja, että vastauksen HTTP-statuskoodi on onnistumisesta kertova 200.

```js
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    // koodi, joka käsittelee palvelimen vastauksen
  }
};
```

Tapahtumankäsittelijöihin liittyvä mekanismi koodin suorittamiseen on Javascriptissä erittäin yleistä. Tapahtumankäsittelijöinä olevia Javascript-funktioita kutsutaan [callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)- eli takaisinkutsufunktioiksi, sillä sovelluksen koodi ei kutsu niitä itse, vaan suoritusympäristö, eli web-selain suorittaa funktion kutsumisen sopivana ajankohtana, eli kyseisen _tapahtuman_ tapahduttua.

### Document Object Model eli DOM

Voimme ajatella, että HTML-sivut muodostavat implisiittisen puurakenteen

<pre>
html
  head
    link
    script
  body
    div
      h1
      div
        ul
          li
          li
          li
      form
        input
        input
</pre>

Sama puumaisuus on nähtävissä konsolin välilehdellä _Elements_

![](./assets/1/13.png)

Selainten toiminta perustuukin ideaan esittää HTML-elementit puurakenteena.

Document Object Model eli [DOM](https://en.wikipedia.org/wiki/Document_Object_Model) on ohjelmointirajapinta eli _API_, joka mahdollistaa selaimessa esitettävien web-sivuja vastaavien _elementtipuiden_ muokkaamisen ohjelmallisesti.

Edellisessä luvussa esittelemämme Javascript-koodi käytti nimenomaan DOM-apia lisätäkseen sivulle muistiinpanojen listan.

Allaoleva koodi luo muuttujaan _ul_ DOM-apin avulla uuden "solmun" ja lisää sille joukon lapsisolmuja:

```js
var ul = document.createElement('ul');

data.forEach(function(note) {
  var li = document.createElement('li');

  ul.appendChild(li);
  li.appendChild(document.createTextNode(note.content));
});
```

lopulta muuttujassa _ul_ oleva puun palanen yhdistetään sopivaan paikkaan koko sovelluksen HTML-koodia edustavassa puussa:

```js
document.getElementById('notes').appendChild(ul);
```

Toisena esimerkkinä HTML-dokumentin

![](./assets/1/13b.png)

DOM:a havainnollistava kuva Wikipedian sivulta:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/428px-DOM-model.svg.png)

### document-olio ja sivun manipulointi konsolista

HTML-dokumenttia esittävän DOM-puun ylimpänä solmuna on olio nimeltään _document_. Olioon pääsee käsiksi Console-välilehdeltä:

![](./assets/1/14.png)

Tiedot saa esiin komennolla:

```js
document;
```

Voimme suorittaa konsolista käsin DOM-apin avulla erilaisia operaatioita selaimessa näytettävälle web-sivulle hyödyntämällä _document_-olioa.

Lisätään nyt sivulle uusi muistiinpano suoraan konsolista.

Haetaan ensin sivulta muistiinpanojen lista, eli sivun ul-elementeistä ensimmäinen:

```js
lista = document.getElementsByTagName('ul')[0];
```

luodaan uusi li-elementti ja lisätään sille sopiva tekstisisältö:

```js
uusi = document.createElement('li');
uusi.textContent = 'Sivun manipulointi konsolista on helppoa';
```

liitetään li-elementti listalle:

```js
lista.appendChild(uusi);
```

![](./assets/1/15.png)

Vaikka selaimen näyttämä sivu päivittyy, ei muutos ole lopullinen. Jos sivu uudelleenladataan, katoaa uusi muistiinpano, sillä muutos ei mennyt palvelimelle asti. Selaimen lataama Javascript luo muistiinpanojen listan aina palvelimelta osoitteesta <https://fullstack-exampleapp.herokuapp.com/data.json> haettavan JSON-muotoisen raakadatan perusteella.

### CSS

Muistiinpanojen sivun HTML-koodin _head_-osio sisältää [link](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link)-tagin, joka määrittelee, että selaimen tulee ladata palvelimelta osoitteesta [main.css](https://fullstack-exampleapp.herokuapp.com/main.css) sivulla käytettävä [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)-tyylitiedosto.

Cascading Style Sheets eli CSS on kieli, jonka avulla web-sovellusten ulkoasu määritellään.

Ladattu css-tiedosto näyttää seuraavalta:

```css
.container {
  padding: 10px;
  border: 1px solid;
}

.notes {
  color: blue;
}
```

Tiedosto määrittelee kaksi [luokkaselektoria](https://developer.mozilla.org/en-US/docs/Web/CSS/Class_selectors), joiden avulla valitaan tietty sivun alue ja määritellään alueelle sovellettavat tyylisäännöt.

Luokkaselektori alkaa aina pisteellä ja sisältää luokan nimen.

Luokat ovat [attribuutteja](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) joita voidaan liittää HTML-elementeille.

Konsolin _Elements_-välilehti mahdollistaa class-attribuuttien tarkastelun:

![](./assets/1/16.png)

sovelluksen uloimmalle _div_-elementille on siis liitetty luokka _container_. Muistiinpanojen listan sisältävä _ul_-elementin sisällä oleva lista sisältää luokan _notes_.

CSS-säännön avulla on määritelty, että _container_-luokan sisältävä elementti ympäröidään yhden pikselin paksuisella [border](https://developer.mozilla.org/en-US/docs/Web/CSS/border):illa. Elementille asetetaan myös 10 pikselin [padding](https://developer.mozilla.org/en-US/docs/Web/CSS/padding), jonka ansiosta elementin sisällön ja elementin ulkorajan väliin jätetään hieman tilaa.

Toinen määritelty CSS-sääntö asettaa muistiinpanojen kirjainten värin siniseksi.

HTML-elementeillä on muitakin attribuutteja kuin luokkia. Muistiinpanot sisältävä _div_-elementti sisältää [id](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id)-attribuutin. Javascript-koodi hyödyntää attribuuttia elementin etsimiseen.

Konsolin _Elements_-välilehdellä on mahdollista manipuloida elementtien tyylejä:
![](./assets/1/17.png)

Tehdyt muutokset eivät luonnollisestikaan jää voimaan kun selaimen sivu uudelleenladataan, eli jos muutokset halutaan pysyviksi, tulee ne konsolissa tehtävien kokeilujen jälkeen tallettaa palvelimella olevaan tyylitiedostoon.

### Lomake ja HTTP POST

Tutkitaan seuraavaksi sitä, miten uusien muistiinpanojen luominen tapahtuu. Tätä varten muistiinpanojen sivu sisältää lomakkeen eli [form-elementin](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form).

![](./assets/1/18.png)

Kun lomakkeen painiketta painetaan, lähettää selain lomakkeelle syötetyn datan palvelimelle. Avataan _Network_-välilehti ja katsotaan miltä lomakkeen lähettäminen näyttää:

![](./assets/1/19.png)

Lomakkeen lähettäminen aiheuttaa yllättäen yhteensä _viisi_ HTTP-pyyntöä. Näistä ensimmäinen vastaa lomakkeen lähetystapahtumaa. Tarkennetaan siihen:

![](./assets/1/20.png)

Kyseessä on siis [HTTP POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) -pyyntö ja se on tehty palvelimen osoitteeseen <em>new*note</em>. Palvelin vastaa pyyntöön HTTP-statuskoodilla 302. Kyseessä on ns. [uudelleenohjauspyyntö](https://en.wikipedia.org/wiki/URL_redirection) eli redirectaus, minkä avulla palvelin kehottaa selainta tekemään automaattisesti uuden HTTP GET -pyynnön headerin \_Location* kertomaan paikkaan, eli osoitteeseen _notes_.

Selain siis lataa uudelleen muistiinpanojen sivun. Sivunlataus saa aikaan myös kolme muuta HTTP-pyyntöä: tyylitiedoston (main.css), Javascript-koodin (main.js) ja muistiinpanojen raakadatan (data.json) lataamisen.

Network-välilehti näyttää myös lomakkeen mukana lähetetyn datan:

![](./assets/1/21.png)

Jos käytät normaalia Chrome-selainta, ei konsoli ehkä näytä lähetettävää dataa. Kyseessä on eräissä Chromen versioissa oleva [bugi](https://bugs.chromium.org/p/chromium/issues/detail?id=766715). Bugi on korjattu Chromen uusimpaan versioon.

Lomakkeen lähettäminen tapahtuu HTTP POST -pyyntönä ja osoitteeseen _new_note_ form-tagiin määriteltyjen attribuuttien _action_ ja _method_ ansiosta:

<img src="/assets/1/22.png" height="150">

POST-pyynnöstä huolehtiva palvelimen koodi on yksinkertainen (huom: tämä koodi on siis palvelimella eikä näy selaimen lataamassa Javascript-tiedostossa):

```js
app.post('/new_note', (req, res) => {
  notes.push({
    content: req.body.note,
    date: new Date(),
  });

  return res.redirect('/notes');
});
```

POST-pyyntöihin liitettävä data lähetetään pyynnön mukana "runkona" eli [bodynä](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST). Palvelin saa POST-pyynnön datan pyytämällä sitä pyyntöä vastaavan olion _req_ kentästä _req.body_.

Tekstikenttään kirjoitettu data on avaimen _note_ alla, eli palvelin viittaa siihen _req.body.note_.

Palvelin luo uutta muistiinpanoa vastaavan olion ja laittaa sen muistiinpanot sisältävään taulukkoon nimeltään _notes_:

```js
notes.push({
  content: req.body.note,
  date: new Date(),
});
```

Muistiinpano-olioilla on siis kaksi kenttää, varsinaisen sisällön kuvaava _content_ ja luomishetken kertova _date_.

Palvelin ei talleta muistiinpanoja tietokantaan, joten uudet muistiinpanot katoavat aina Herokun uudelleenkäynnistäessä palvelun.

### AJAX

Sovelluksen muistiinpanojen sivu noudattaa vuosituhannen alun tyyliä ja se "käyttää AJAX:ia", eli on silloisen kehityksen aallonharjalla.

[AJAX](<https://en.wikipedia.org/wiki/Ajax_(programming)>) (Asynchronous Javascript and XML) on termi, joka lanseerattiin vuoden 2005 helmikuussa kuvaamaan selainten kehityksen mahdollistamaa vallankumouksellista tapaa, missä HTML-sivulle sisällytetyn Javascriptin avulla oli mahdollista ladata sivulle lisää sisältöä lataamatta itse sivua uudelleen.

Ennen AJAX:in aikakautta jokainen sivu toimi aiemmassa luvussa olevan [perinteisen web-sovelluksen](/osa0#perinteinen-web-sovellus) tapaan, eli oleellisesti ottaen kaikki sivuilla näytettävä data tuli palvelimen generoimassa HTML-koodissa.

Muistiinpanojen sivu siis lataa näytettävän datan AJAX:illa. Lomakkeen lähetys sen sijaan tapahtuu perinteisen web-lomakkeen lähetysmekanismin kautta.

Sovelluksen urlit heijastavat vanhaa huoletonta aikaa. JSON-muotoinen data haetaan urlista <https://fullstack-exampleapp.herokuapp.com/data.json> ja uuden muistiinpanon tiedot lähetetään urliin <https://fullstack-exampleapp.herokuapp.com/new_note>. Nykyään näin valittuja urleja ei pidettäisi ollenkaan hyvinä, ne eivät noudata ns. [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer#Applied_to_Web_services)-apien yleisesti hyväksyttyjä konventioita. Käsittelemme asiaa tarkemmin [osassa 3](/osa3).

AJAXiksi kutsuttu asia on arkipäiväistynyt, ja muuttunut itsestäänselvyydeksi. Koko termi on hiipunut unholaan ja nuori polvi ei ole sitä edes ikinä kuullut.

## Single page app

Esimerkkisovelluksemme pääsivu toimii perinteisten web-sivujen tapaan: kaikki sovelluslogiikka on palvelimella ja selain ainoastaan renderöi palvelimen lähettämää HTML-koodia.

Muistiinpanoista huolehtivassa sivussa osa sovelluslogiikasta, eli olemassaolevien muistiinpanojen HTML-koodin generointi on siirretty selaimen vastuulle. Selain hoitaa tehtävän suorittamalla palvelimelta lataamansa Javascript-koodin. Selaimella suoritettava koodi hakee ensin muistiinpanot palvelimelta JSON-muotoisena raakadatana ja lisää sivulle muistiinpanoja edustavat HTML-elementit [DOM-apia](/osa0#document-object-model-eli-dom) hyödyntäen.

Viimeisten vuosien aikana on noussut esiin tyyli tehdä web-sovellukset käyttäen [Single-page application](https://en.wikipedia.org/wiki/Single-page_application) (SPA) -tyyliä, missä sovelluksille ei enää tehdä esimerkkisovelluksemme tapaan erillisiä, palvelimen sille lähettämiä sivuja, vaan sovellus koostuu ainoastaan yhdestä palvelimen lähettämästä HTML-sivusta, jonka sisältöä manipuloidaan selaimessa suoritettavalla Javascriptillä.

Sovelluksemme muistiinpanosivu muistuttaa jo hiukan SPA-tyylistä sovellusta, sitä se ei kuitenkaan vielä ole, sillä vaikka muistiinpanojen renderöintilogiikka on toteutettu selaimessa, käyttää sivu vielä perinteistä mekanisimia uusien muistiinpanojen luomiseen, eli se lähettää uuden muistiinpanon tiedot lomakkeen avulla ja palvelin pyytää _uudelleenohjauksen_ avulla selainta lataamaan muistiinpanojen sivun uudelleen.

Osoitteesta <https://fullstack-exampleapp.herokuapp.com/spa> löytyy sovelluksen single page app -versio.

Sovellus näyttää ensivilkaisulta täsmälleen samalta kuin edellinen versio.

HTML-koodi on lähes samanlainen, erona on ladattava Javascript-tiedosto (_spa.js_) ja pieni muutos form-tagin määrittelyssä:

![](./assets/1/23.png)

Avaa nyt _Network_-välilehti ja tyhjennä se &empty;-symbolilla. Kun luot uuden muistiinpanon, huomaat, että selain lähettää ainoastaan yhden pyynnön palvelimelle:

![](./assets/1/24.png)

Pyyntö kohdistuu osoitteeseen _new_note_spa_, on tyypiltään POST ja se sisältää JSON-muodossa olevan uuden muistiinpanon, johon kuuluu sekä sisältö (_content_), että aikaleima (_date_):

```js
{
  content: "single page app ei tee turhia sivun latauksia",
  date: "2017-12-11T10:51:29.025Z"
}
```

Pyyntöön liitetty headeri _Content-Type_ kertoo palvelimelle, että pyynnön mukana tuleva data on JSON-muotoista:

![](./assets/1/25.png)

Ilman headeria palvelin ei osaisi parsia pyynnön mukana tulevaa dataa oiken.

Palvelin vastaa kyselyyn statuskoodilla [201 created](https://httpstatuses.com/201). Tällä kertaa palvelin ei pyydä uudelleenohjausta kuten aiemmassa versiossamme. Selain pysyy samalla sivulla ja muita HTTP-pyyntöjä ei suoriteta.

Ohjelman single page app -versiossa lomakkeen tietoja ei lähetetä selaimen "normaalin" lomakkeiden lähetysmekanismin avulla, lähettämisen hoitaa selaimen lataamassa Javascript-tiedostossa määritelty koodi. Katsotaan hieman koodia vaikka yksityiskohdista ei tarvitse nytkään välittää liikaa.

```js
var form = document.getElementById('notes_form');
form.onsubmit = function(e) {
  e.preventDefault();

  var note = {
    content: e.target.elements[0].value,
    date: new Date(),
  };

  notes.push(note);
  e.target.elements[0].value = '';
  redrawNotes();
  sendToServer(note);
};
```

Komennolla <code>document.getElementById('notes*form')</code> koodi hakee sivulta lomake-elementin ja rekisteröi sille \_tapahtumankäsittelijän* hoitamaan tilanteen, missä lomake "submitoidaan", eli lähetetään. Tapahtumankäsittelijä kutsuu heti metodia <code>e.preventDefault()</code> jolla se estää lomakkeen lähetyksen oletusarvoisen toiminnan. Oletusarvoinen toiminta aiheuttaisi lomakkeen lähettämisen ja sivun uudelleen lataamisen, sitä emme single page -sovelluksissa halua tapahtuvan.

Tämän jälkeen se luo muistiinpanon, lisää sen muistiinpanojen listalle komennolla <code>notes.push(note)</code>, piirtää ruudun sisällön eli muistiinpanojen listan uudelleen ja lähettää uuden muistiinpanon palvelimelle.

Palvelimelle muistiinpanon lähettävä koodi seuraavassa:

```js
var sendToServer = function(note) {
  var xhttpForPost = new XMLHttpRequest();
  // ...

  xhttpForPost.open('POST', '/new_note_spa', true);
  xhttpForPost.setRequestHeader('Content-type', 'application/json');
  xhttpForPost.send(JSON.stringify(note));
};
```

Koodissa siis määritellään, että kyse on HTTP POST -pyynnöstä, määritellään headerin _Content-type_ avulla lähetettävän datan tyypiksi JSON, ja lähetetään data JSON-merkkijonona.

Sovelluksen koodi on nähtävissä osoitteessa <https://github.com/mluukkai/example_app>. Kannattaa huomata, että sovellus on tarkoitettu ainoastaan kurssin käsitteistöä demonstroivaksi esimerkiksi, koodi on osin tyyliltään huonoa ja siitä ei tule ottaa mallia omia sovelluksia tehdessä. Sama koskee käytettyjä urleja, single page app -tyyliä noudattavan sivun käyttämä uusien muistiinpanojen kohdeosoite _new_note_spa_ ei noudata nykyisin suositeltavia käytäntöjä.

## Javascript-kirjastot

Kurssin esimerkkisovellus on tehty ns. [vanilla Javascriptillä](https://medium.freecodecamp.org/is-vanilla-javascript-worth-learning-absolutely-c2c67140ac34) eli käyttäen pelkkää DOM-apia ja Javascript-kieltä sivujen rakenteen manipulointiin.

Pelkän Javascriptin ja DOM-apin käytön sijaan Web-ohjelmoinnissa hyödynnetään yleensä kirjastoja, jotka sisältävät DOM-apia helpommin käytettäviä työkaluja sivujen muokkaamiseen. Eräs tälläinen kirjasto on edelleenkin hyvin suosittu [JQuery](https://jquery.com/).

JQuery on kehitetty aikana, jolloin web-sivut olivat vielä suurimmaksi osaksi perinteisiä, eli palvelin muodosti HTML-sivuja, joiden toiminnallisuutta rikastettiin selaimessa JQueryllä kirjoitetun Javascript-koodin avulla. Yksi syy JQueryn suosion taustalla oli niin sanottu cross-browser yhteensopivuus, eli kirjasto toimi selaimesta ja selainvalmistajasta riippumatta samalla tavalla, eikä sitä käyttäessä ollut enää tarvetta kirjoittaa selainversiospesifisiä ratkaisuja. Nykyisin perus JQueryn käyttö ei ole enää yhtä perusteltua kuin aikaisemmin, sillä vanillaJS on kehittynyt paljon ja käytetyimmät selaimet tukevat yleisesti ottaen hyvin perustoiminnallisuuksia.

Single page app -tyylin noustua suosioon on ilmestynyt useita JQueryä "modernimpia" tapoja sovellusten kehittämiseen. Ensimmäisen aallon suosikki oli [BackboneJS](http://backbonejs.org/). Googlen kehittämä [AngularJS](https://angularjs.org/) nousi 2012 tapahtuneen [julkaisun](https://github.com/angular/angular.js/blob/master/CHANGELOG.md#100-temporal-domination-2012-06-13) jälkeen erittäin nopeasti lähes de facto -standardin asemaan modernissa web-sovelluskehityksessä.

Angularin suosio kuitenkin romahti siinä vaiheessa kun Angular-tiimi [ilmoitti](https://jaxenter.com/angular-2-0-announcement-backfires-112127.html) lokakuussa 2014, että version 1 tuki lopetetaan ja Angular 2 ei tule olemaan taaksepäin yhteensopiva ykkösversion kanssa. Angular 2 ja uudemmat versiot eivät ole saaneet kovin innostunutta vastaanottoa.

Nykyisin suosituin tapa toteuttaa web-sovellusten selainpuolen logiikka on Facebookin kehittämä [React](https://reactjs.org/)-kirjasto. Tulemme tutustumaan kurssin aikana Reactiin ja sen kanssa yleisesti käytettyyn [Redux](https://github.com/reactjs/redux)-kirjastoon.

Reactin asema näyttää tällä hetkellä vahvalta, mutta Javascript-maailma ei lepää koskaan. Viime aikoina kiinnostusta on alkanut herättää mm. uudempi tulokas [VueJS](https://vuejs.org/).

## Full stack -websovelluskehitys

Mitä tarkoitetaan kurssin nimellä _Full stack -websovelluskehitys_? Full stack on hypenomainen termi; kaikki puhuvat siitä, mutta kukaan ei oikein tiedä, mitä se tarkoittaa tai ainakaan mitään yhteneväistä määritelmää termille ei ole.

Käytännössä kaikki websovellukset sisältävät (ainakin) kaksi "kerrosta", ylempänä, eli lähempänä loppukäyttäjää olevan selaimen ja alla olevan palvelimen. Palvelimen alapuolella on usein vielä tietokanta. Näin websovelluksen _arkkitehtuurin_ voi ajatella muodostavan pinon, englanniksi _stack_.

Websovelluskehityksen yhteydessä puhutaan usein myös "frontista" ([frontend](https://en.wikipedia.org/wiki/Front_and_back_ends)) ja "backistä" ([backend](https://en.wikipedia.org/wiki/Front_and_back_ends)). Selain on frontend ja selaimessa suoritettava Javascript on frontend-koodia. Palvelimella taas pyörii backend-koodi.

Tämän kurssin kontekstissa full stack -sovelluskehitys tarkoittaa sitä, että fokus on kaikissa sovelluksen osissa, niin frontendissä kuin backendissä sekä taustalla olevassa tietokannassa. Myös palvelimen käyttöjärjestelmä lasketaan usein osaksi stackia.

Ohjelmoimme myös palvelinpuolta, eli backendia Javascriptilla, käyttäen [Node.js](https://nodejs.org/en/)-suoritusympäristöä. Näin full stack -sovelluskehitys saa vielä uuden ulottuvuuden, kun voimme käyttää samaa kieltä pinon useammassa kerroksessa. Full stack -sovelluskehitys ei välttämättä edellytä sitä, että kaikissa "sovelluspinon" kerroksissa on käytössä sama kieli (Javascript).

Aiemmin on ollut yleisempää, että sovelluskehittäjät ovat erikoistuneet tiettyyn sovelluksen osaan, esim. backendiin. Tekniikat backendissa ja frontendissa ovat saattaneet olla hyvin erilaisia. Full stack -trendin myötä on tullut tavanomaiseksi, että sovelluskehittäjä hallitsee riittävästi kaikkia sovelluksen tasoja ja tietokantaa. Usein full stack -kehittäjän on myös omattava riittävä määrä konfiguraatio- ja ylläpito-osaamista, jotta kehittäjä pystyy operoimaan sovellustaan esim. pilvipalveluissa.

## Javascript fatigue

Full stack -sovelluskehitys on monella tapaa haastavaa. Asioita tapahtuu monessa paikassa ja mm. debuggaaminen on oleellisesti normaalia työpöytäsovellusta hankalampaa. Javascript ei toimi aina niin kuin sen olettaisi toimivan (verrattuna moniin muihin kieliin) ja sen suoritusympäristöjen asynkroninen toimintamalli aiheuttaa monenlaisia haasteita. Verkon yli tapahtuva kommunikointi edellyttää HTTP-protokollan tuntemusta. On tunnettava myös tietokantoja ja hallittava palvelinten konfigurointia ja ylläpitoa. Hyvä olisi myös hallita riittävästi CSS:ää, jotta sovellukset saataisiin edes siedettävän näköisiksi.

Oman haasteensa tuo vielä se, että Javascript-maailma etenee koko ajan todella kovaa vauhtia eteenpäin. Kirjastot, työkalut ja itse kielikin ovat jatkuvan kehityksen alla. Osa alkaa kyllästyä nopeaan kehitykseen ja sitä kuvaamaan on lanseerattu termi [Javascript](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4) [fatigue](https://auth0.com/blog/how-to-manage-javascript-fatigue/) eli [Javascript](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f)-väsymys.

Javascript-väsymys tulee varmasti iskemään myös tällä kurssilla. Onneksi nykyään on olemassa muutamia tapoja loiventaa oppimiskäyrää, ja voimme aloittaa keskittymällä konfiguraation sijaan koodaamiseen. Konfiguraatioita ei voi välttää, mutta seuraavat viikot voimme edetä iloisin mielin vailla pahimpia konfiguraatiohelvettejä.

</div>

<div class="tasks"> 
  <h3>Tehtävät 0.1</h3>
  <h4>HTML ja CSS</h4>

Kertaa HTML:n ja CSS:n perusteet lukemalla Mozillan tutoriaalit [HTML:stä](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics) ja [CSS:stä](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics).

  <h3>Tehtävät 0.2</h3>
  <h4>HTML:n lomakkeet</h4>

Tutustu HTML:n lomakkeiden perusteisiin lukemalla Mozillan tutoriaali [Your first form](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form).

  <h3>Tehtävät 0.3</h3>
  <h4>muistiinpanojen sivu</h4>

Kun käyttäjä menee selaimella osoitteeseen <https://fullstack-exampleapp.herokuapp.com/> voidaan sen seurauksena olevaa tapahtumaketjua kuvata [sekvenssikaaviona](https://en.wikipedia.org/wiki/Sequence_diagram) esim. seuraavasti:

![](./assets/teht/1.png)

Kaavio on luotu [websequencediagrams](https://www.websequencediagrams.com)-palvelussa, seuraavasti:

<pre>
kayttaja->selain:
note left of selain
kayttaja kirjottaa osoiteriville
fullstack-exampleapp.herokuapp.com
end note
selain->palvelin: GET fullstack-exampleapp.herokuapp.com
note left of palvelin
  muodostetaan HTML missä olemassaolevien
  muistiinpanojen lukumäärä päivitettynä
end note
palvelin->selain: status 200, sivun HTML-koodi

selain->palvelin: GET fullstack-exampleapp.herokuapp.com/kuva.png
palvelin->selain: status 200, kuva

note left of selain
 selain näyttää palvelimen palauttaman HTML:n
 johon on upotettu palvelimelta haettu kuva
end note
</pre>

**Tee vastaavanlainen kaavio, joka kuvaa mitä tapahtuu kun käyttäjä navigoi muistiinpanojen sivulle** eli urliin <https://fullstack-exampleapp.herokuapp.com/notes>

Kaavion ei ole pakko olla sekvenssikaavio. Mikä tahansa järkevä kuvaustapa käy.

Kaikki oleellinen tämän ja seuraavien kolmen tehtävän tekemiseen liittyvä informaatio on selitettynä [osan 0](../osa0) tekstissä. Näiden tehtävien ideana on, että luet tekstin vielä kerran ja mietit tarkkaan mitä missäkin tapahtuu. Ohjelman [koodin](https://github.com/mluukkai/example_app) lukemista ei näissä tehtävissä edellytetä, vaikka sekin on toki mahdollista.

  <h3>Tehtävät 0.4</h3>
  <h4>Uusi muistiinpano</h4>

Tee kaavio tilanteesta, missä käyttäjä luo uuden muistiinpanon ollessaan sivulla <https://fullstack-exampleapp.herokuapp.com/notes>, eli kirjoittaa tekstikenttään jotain ja painaa nappia _Talleta_.

Kirjoita tarvittaessa palvelimella tai selaimessa tapahtuvat operaatiot sopivina kommentteina kaavion sekaan.

  <h3>Tehtävät 0.5</h3>
  <h4>Single page app</h4>

Tee kaavio tilanteesta, missä käyttäjä menee selaimella osoitteeseen <https://fullstack-exampleapp.herokuapp.com/spa> eli muistiinpanojen [single page app](../osa0/#single-page-app) -versioon.

  <h3>Tehtävät 0.6</h3>
  <h4>Uusi muistiinpano</h4>

Tee kaavio tilanteesta, missä käyttäjä luo uuden muistiinpanon single page -versiossa.

</div>
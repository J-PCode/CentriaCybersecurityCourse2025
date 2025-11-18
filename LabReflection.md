### Lab: Username enumeration via different responses → Reflection

Tässä labrassa opin, miten erilaiset virheviestit ja vastausten pituuserot voivat paljastaa, onko käyttäjätunnus olemassa vai ei. Olin olettanut, että HTTPS salaisi tämän tyyppiset yksityiskohdat, mutta huomasin, ettei sovelluksen sisäinen logiikka muutu – hyökkääjä näkee silti vasteiden erot. Tehtävä sai pohtimaan, miten brute force -hyökkäyksiltä voidaan suojautua: yhtenäiset virheviestit, yritysrajoitukset, aikaviiveet, IP-kohtaiset rajat, reCAPTCHA ja MFA ovat kaikki tärkeitä keinoja estää käyttäjätunnusten enumerointia ja sanasanabruteforcea.


### Lab: Password reset broken logic → Reflection

Tässä labrassa opin, miten vaarallinen huonosti toteutettu salasanan resetointi voi olla, jos palvelin ei tarkista tokenin oikeellisuutta ollenkaan. Resetointilinkissä oleva token näytti turvalliselta, mutta itse backend ei validoi sitä, jolloin hyökkääjä voi vaihtaa kenen tahansa käyttäjän salasanan pelkästään muokkaamalla POST-pyynnön sisältämää username-arvoa. Tämä labra korosti selvästi, että pelkkä resetointilinkin lähettäminen ei riitä, jos sovellus ei tarkista tokenia jokaisella pyynnöllä. Haastavinta oli ymmärtää, miten pienikin validointivirhe muuttaa koko mekanismin hyökkäykselle täysin avoimeksi.

### Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data → Reflection

Tässä labrassa opin, miten yksinkertainen WHERE-lauseeseen kohdistuva SQL-injektio voi paljastaa piilotettuja tai julkaisemattomia tuotteita. Oli yllättävää huomata, kuinka helposti queryn suodatus voidaan ohittaa lisäämällä OR 1=1 tai kommentoimalla loppu pois merkillä --. Labra havainnollisti hyvin, miten suuri riski syntyy, jos käyttäjän syötettä ei tarkisteta tai parametrisoida lainkaan. Tästä oli helppo nähdä, miksi parametrisoidut kyselyt ja input-sanitointi ovat täysin välttämättömiä. Jäin vähän pohtimaan kannattaako sanitointi tehdä käyttäjän päässä vai palvelimen. ehkäpä molempien.

### Lab: SQL injection vulnerability allowing login bypass → Reflection

Tässä labrassa opin, miten helppoa on ohittaa sisäänkirjautuminen, jos sovellus rakentaa SQL-kyselyn suoraan käyttäjän syötteen perusteella. Riitti, että syötteenä käytin `administrator'--`, jolloin loppu WHERE-lauseesta kommentoitiin pois ja salasanaa ei tarvittu lainkaan. Yllättävää oli, ettei Burpia edes tarvittu, koska injektion pystyi tekemään suoraan käyttöliittymässä. Labra havainnollisti hyvin, kuinka vaarallinen sisäänkirjautumislogiikka on ilman parametrisoituja kyselyjä ja miksi kaikki käyttäjän syöte on käsiteltävä erittäin tarkasti.

### Lab: Unprotected admin functionality → Reflection

Tässä labrassa opin, kuinka vaarallinen täysin suojaamaton admin-polku voi olla, jos sovellus ei tarkista käyttäjän oikeuksia ennen hallintatoimintoihin pääsyä. Oli yllättävää nähdä, että pelkkä robots.txt paljasti koko admin-paneelin sijainnin, minkä jälkeen admin-toimintoihin pääsi ilman minkäänlaista autentikointia. Labra havainnollisti selkeästi vertikaalisen privilege escalation -haavoittuvuuden: tavallinen käyttäjä pystyy suorittamaan admin-toimintoja, kuten poistamaan käyttäjiä. Jäin pohtimaan miten tämä estetään käyttäjätasolla, ilmaiseeko joku token että kuka käyttäjä on kyseessä vai miten komentojen tarkistus tehdään?.

### Lab: User role can be modified in user profile → Reflection

Tässä labrassa opin, miten vaarallista on tallentaa käyttäjän roolitieto sellaiseen paikkaan, jota käyttäjä voi itse muokata. Profiilin päivityspyynnön JSON-runko sisälsi roleid-arvon, ja muuttamalla sen arvoksi 2 sain admin-oikeudet ilman mitään lisätarkistuksia. Haastavinta oli aluksi löytää oikea JSON-body, johon roolitieto oli piilotettu, ja ymmärtää miten helposti tämä voidaan manipuloida Burp Repeaterilla. Labra havainnollisti selvästi, että tällaiset roolitiedot eivät saa koskaan olla client-puolen kontrollissa, vaan niiden tulee tulla palvelimen tietokannasta jokaisella pyynnöllä.
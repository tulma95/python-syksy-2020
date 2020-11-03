# Pipenv ja riippuvuuksien hallinta

Laajoissa ja monimutkaisissa ohjelmistoprojekteissa kaiken koodin tuottaminen itse ei ole enää käytännöllistä. Ei ole esimerkiksi järkevää, että jokaisessa ohjelmistoprojektissa toteutetaan oma ohjelmointirajapinta tietokantaoperaatioille, tai sovelluskehys koodin testaamiseen. Jotta pyörää ei tarvitsisi aina keksiä uudelleen, ovat ohjelmistokehittäjät kehittäneet valtavan määrän avoimen lähdekoodin _kirjastoja_, joita jokainen voi hyödyntää projekteissaan.

Kirjastojen lähdekoodi on usein luettavissa versionhallinta-alustoilla, kuten GitHubissa. Usein kirjastoja päivitetään jatkuvasti ja nämä päivitykset synnyttävät kirjastoista uusia _versioita_. Kirjastojen versioita julkaistaan erilaisiin rekistereihin, joista ne ovat helposti asennettavissa. [The Python Package Index](https://pypi.org/) (PyPI) on eräs tämän kaltainen, Python-kirjastoille tarkoitettu rekisteri.

Jotta kirjastoja olisi helppo asentaa projektiin, on kehitetty erilaisia pakettin asennukseen tarkoitettuja komentorivityökaluja. Pythonin kohdalla suosituin komentorivityökaluja tähän tarkoitukseen on [pip](https://pypi.org/project/pip/). Pip tulee valmiiksi asennettuna uusimpien Python asennusten mukana. Voit varmistaa sen löytymisen komentorivin komennolla:

```bash
pip3 --version
```

Komentoriville tulisi tulostua pipin versio. Jos `pip3`-komentoa ei löydy, kokeile komentoa `python3 -m pip --version`.

Kirjastojen asennus onnistuu pipin komennolla `pip3 install`. Jotta samalla tietokoneella olevien projektien riippuvuuksissa ei syntyisi ristiriitoja, on käytössä usein niin kutsuttaja projektikohtaisia _virtuaaliympäristöjä_. Näitä virtuaaliympäristöjä luodaan ja käytetään [venv](https://docs.python.org/3/library/venv.html) moduulin kautta. Jotta saisimme helposti käyttöömme pipin ja virtuaaliympäristön tuomat edut, voimme käyttää [pipenv](https://pipenv.pypa.io/en/latest/) komentorivityökalua.

## Huomoioita komennoista

Monilla tietokoneilla Python-version kolme komennot suoritetaan `python3`- ja `pip3`-komennoilla komentojen `python` ja `pip` sijaan. Jos komentoja `python3` ja `pip3` ei jostain syystä löydy, tarkista `python`-komennon käyttämä versio komennolla:

```bash
python --version
```

Jos version on alle `3.6.0`, asenna tietokoneellesi [uusin Python-versio](https://www.python.org/downloads/). Muissa tapauksissa, voit käyttää `python`-komentoa `python3`-komennon sijaan.

## Asennus

Pipenv tarjoaa dokumentaatiossaan useita [asennusvaihtoehtoja](https://pipenv.pypa.io/en/latest/#install-pipenv-today). Tavoista ehkä helpoin on suorittaa asennus komentoriviltä pipin avulla:

```bash
pip3 install --user pipenv
```

Jos `pip3`-komentoa ei löydy, kokeile komentoa `python3 -m pip install --user pipenv`.

Voimme varmistaa asennuksen onnistumisen suorittamalla komennon:

```bash
python3 -m pipenv --version
```

Jos haluat suorittaa pipenvin komentoja suoraan, ilman `python3 -m`-komennon suorittamista, tutustu [tarkempiin asennusohjeisiin](https://pipenv.pypa.io/en/latest/install/#pragmatic-installation-of-pipenv). 

## Projektin alustaminen

Harjoitellaan pipenvin käyttöä tekemällä pieni esimerkkiprojekti. Luo hakemisto _pipenv-testi_ haluamaasi hakemistoon. Hakemiston ei tarvitse löytyä Labtooliin rekisteröimästäsi repositoriosta. Avaa hakemisto komentoriviltä ja suorita siellä komento:

```bash
python3 -m pipenv install
```

Komennon suorittaminen tekee projektille vaadittavat alustustoimenpiteet, kuten virtuaaliympäristön alustamisen. Jos projektille olisi määritelty riippuvuuksia, komennon suorittaminen asentaisi myös ne. Tämän vuoksi komento tulee suorittaa aina ennen kuin uutta projekti aletaan käyttämään.

Hakemistoon pitäisi ilmestyä tiedostot _Pipfile_ ja _Pipfile.lock_. Tiedoston _Pipfile_ sisältö on kutakuinkin seuraava:

```
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[dev-packages]

[packages]

[requires]
python_version = "3.9"
```

Tiedostossa luetellaan mm. projektin _riippuvuudet_ `[packages]`-osion alla. Tiedoston `[requires]`-osion alla on määritelty vaadittava Python-version. Ongelmien välttämiseksi eri Python-versioiden kanssa, kannattaa `[requires]`-osio poistaa kokonaisuudessaan. Tiedoston sisältö on tämän jälkeen seuraava:

```bash
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[dev-packages]

[packages]
```

_Pipfile.lock_-tiedosto sen sijaan sisältää kaikkien riippuvuuksien versiotiedot niin, että voimme aina asentaa täsmälleen oikeat versiot. _Pipfile.lock_-tiedoston sisältöä ei tule _missään tapauksessa_ muuttaa, vaan se on täysin pipenvin ylläpitämä.

## Riippuvuuksen asentaminen

Asennetaan seuraavaksi esimerkkiprojektimme ensimmäisen riippuvuus. Riippuvuuksien löytäminen onnistuu helpoiten Googlettamalla ja etsimällä hakutuloksista sopivia GitHub-repositorioita, tai PyPI-sivuja. Asennetaan esimerkkinä projektiimme erittäin hyödyllinen [cowsay](https://pypi.org/project/cowsay/)-kirjasto. Tämä onnistuu komennolla:

```bash
python3 -m pipenv install cowsay
```

Asennuksen komento on siis muotoa `python3 -m pipenv install <KIRJASTO>`. Komennon suorittamisen jälkeen huomaamme, että _Pipfile_-tiedoston `[packages]`-osion alla on uutta sisältöä:

```
[packages]
cowsay = "*"
```

Jos haluaisimme poistaa kirjaston projektimme riippuvuuksen joukosta, se onnistuisi komennolla:

```bash
python3 -m pipenv uninstall cowsay
```

Pidetään kuitenkin cowsay-kirjasto toistaiseksi asennettuna.

## Komentojen suorittaminen virtuaaliympäristössä

Luodaan seuraavaksi _pipenv-testi_-hakemistoon hakemisto _src_ ja luodaan sinne tiedosto _index.py_. Tiedoston sisältö on seuraava:

```python
import cowsay

cowsay.tux("Pipenv is awesome!")
```

Jos suoritamme tiedoston komentoriviltä komennolla:

```bash
python3 src/index.py
```

On lopputuloksena seuravaa virheilmoitus:

```
ModuleNotFoundError: No module named 'cowsay'
```

Tämä johtuu siitä, että emme ole projektin virtuaaliympäristön sisällä, eli Python ei löydä projektimme riippuvuuksia. Asia korjaantuu käyttämällä [run](https://pipenv.pypa.io/en/latest/cli/#pipenv-run) komentoa:

```bash
python3 -m pipenv run python3 src/index.py
```

`python3 -m pipenv run`-komento siis suorittaa annetun komennon virtuaaliympäristössä, jonka sisällä Python löytää riippuvuutumme. Voimme myös siirtyä virtuaaliympäristön sisään kommennolla [shell](https://pipenv.pypa.io/en/latest/cli/#pipenv-shell):

```bash
python3 -m pipenv shell
```

Virtuaaliympäristön sisällä voimme suorittaa komennon "normaalisti":

```bash
python3 src/index.py
```

Voimme lähteä virtuaaliympäristöstä komennolla `exit`.

## Omat skriptit

Komento `python3 -m pipenv run python3 src/index.py` on hieman pitkä ja ei oikeastaan kerro komennon suorittajalle, mitä se tekee. Pipenvin avulla voimme määritellä [omia skriptejämme](https://pipenv.pypa.io/en/latest/advanced/#custom-script-shortcuts). Skriptit määritellään _Pipfile_ tiedostoon `[scripts]` osion alle. Lisätään _Pipfile_ tiedoston `[[source]]` osion alle osio `[scripts]` ja määritellään sinne skripti `start`:

```
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[scripts]
start = "python3 src/index.py"

[dev-packages]

[packages]
cowsay = "*"
```

Lisäämämme skriptin pystyy suorittamaan komennolla:

```bash
python3 -m pipenv run start
```

Komentoja voi olla useita, ja niiden suorittaminen onnistuu komennolla `python3 -m pipenv run <SKRIPTI>`. Huomaa, että skriptit suoritetaan automaattisesti virtuaaliympäristössä.

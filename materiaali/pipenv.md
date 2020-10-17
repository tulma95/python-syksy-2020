# Pipenv

Laajoissa ja monimutkaisissa ohjelmistoprojekteissa kaiken koodin tuottaminen itse ei ole enää käytännöllistä. Ei ole esimerkiksi järkevää, että jokaisessa ohjelmistoprojektissa toteutetaan oma ohjelmointirajapinta tietokantaoperaatioille, tai viitekehys koodin testaamiseen. Jotta pyörää ei tarvitsisi aina keksiä uudelleen, ovat ohjelmistokehittäjät kehittäneet valtavan määrän avoimen lähdekoodin _kirjastoja_, joita jokainen voi hyödyntää projekteissaan.

Kirjastojen lähdekoodi on usein luettavissa versionhallinta-alustoilla, kuten GitHubissa. Usein kirjastoja päivitetään jatkuvasti ja nämä päivitykset synnyttävät kirjastoista uusia _versioita_. Kirjastojen versioita julkaistaan erilaisiin rekistereihin, joista ne ovat helposti asennettavissa. [The Python Package Index](https://pypi.org/) (PyPI) on eräs tämän kaltainen, Python-kirjastoille tarkoitettu rekisteri.

Jotta kirjastoja olisi helppo asentaa projektiin, on kehitetty erilaisia pakettin asennukseen tarkoitettuja komentorivityökaluja. Pythonin kohdalla suosituin komentorivityökaluja tähän tarkoitukseen on [pip](https://pypi.org/project/pip/). Pip tulee valmiiksi asennettuna uusimpien Python asennusten mukana. Voit varmistaa sen löytymisen komentorivin komennolla:

```bash
pip --version
```

Komentoriville tulisi tulostua pip:in versio.

Kirjastojen asennus onnistuu pip:in komennolla `pip install`. Jotta samalla tietokoneella olevien projektien riippuvuuksissa ei syntyisi ristiriitoja, on käytössä usein niin kutsuttaja projektikohtaisia _virtuaaliympäristöjä_. Näitä virtuaaliympäristöjä luodaan ja käytetään [venv](https://docs.python.org/3/library/venv.html) moduulin kautta. Jotta saisimme helposti käyttöömme pip:n ja virtuaalisten ympäristöjen tuomat edut, voimme käyttää [Pipenv](https://pipenv.pypa.io/en/latest/) komentorivityökalua.

## Asennus

Ennen Pipenv:in asennusta, varmista, että Python asennus on kunnossa komennolla:

```bash
python --version
```

Jos komentoa ei löydy, tutustu ensin Python [asennusohjeisiin](./pythonin-asennus.md).

Pipenv tarjoaa dokumentaatiossaan useita [asennusvaihtoehtoja](https://pipenv.pypa.io/en/latest/#install-pipenv-today). Tavoista ehkä helpoin on suorittaa asennus komentoriviltä asennus-skriptin avulla:

```bash
curl https://raw.githubusercontent.com/pypa/pipenv/master/get-pipenv.py | python
```

Tämä on käytännössa sama asia, kuin [asennus-skriptin](https://raw.githubusercontent.com/pypa/pipenv/master/get-pipenv.py) lataaminen omalle tietokoneelle ja sen suorittaminen komentoriviltä komennolla `python get-pipenv.py` asennushakemistossa.

Kun asennus on valmis, käynnistä terminaali-ikkuna uudestaan. Tämän jälkeen varmista, että asennus onnistui komennolla:

```bash
pipenv --version
```

## Projektin alustaminen

Harjoitellaan Pipenv:in käyttöä tekemällä pieni esimerkkiprojekti. Luo hakemisto _pipenv-test_ haluamaasi hakemistoon. Avaa hakemisto komentoriviltä ja suorita siellä komento:

```bash
pipenv install
```

Komennon suorittaminen mm. alustaa projektin virtuaaliympäristön. Hakemistoon pitäisi ilmestyä tiedostot _Pipfile_ ja _Pipfile.lock_. Tiedoston _Pipfile_ sisältö on kuta kuinkin seuraava:

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

Tiedostossa luetellaan mm. projektin _riippuvuudet_ `[packages]` osion alla. _Pipfile.lock_ tiedosto sen sijaan sisältää kaikkien riippuvuuksien versiotiedot niin, että voimme aina asentaa täsmälleen oikeat versiot. _Pipfile.lock_ tiedoston sisältöä ei tule _missään tapauksessa_ muuttaa, vaan se on täysin Pipenv:in ylläpitämä.

## Riippuvuuksen asentaminen

Asennetaan seuraavaksi esimerkkiprojektimme ensimmäisen riippuvuus. Riippuvuuksien löytäminen onnistuu helpoiten Googlettamalla ja etsimällä hakutuloksista sopivia GitHub-repositorioita, tai PyPI-sivuja. Asennetaan esimerkkinä projektiimme erittäin hyödyllinen [cowsay](https://pypi.org/project/cowsay/) kirjasto. Tämä onnistuu komennolla:

```bash
pipenv install cowsay
```

Asennuksen komento on siis muotoa `pipenv install <KIRJASTO>`. Komennon suorittamisen jälkeen huomaamme, että _Pipfile_ tiedoston `[packages]` osion alla on uutta sisältöä:

```
[packages]
cowsay = "*"
```

Jos haluaisimme poistaa kirjaston projektimme riippuvuuksen joukosta, se onnistuisi komennolla:

```bash
pipenv uninstall cowsay
```

Pidetään kuitenkin cowsay kirjasto toistaiseksi asennettuna.

## Skriptien suorittaminen virtuaaliympäristössä

Luodaan seuraavaksi _pipenv-test_ hakemistoon hakemisto _src_ ja luodaan sinne tiedosto _index.py_. Tiedoston sisältö on seuraava:

```python
import cowsay

cowsay.tux('Pipenv is awesome!')
```

Jos suoritamme tiedoston komentoriviltä komennolla:

```bash
python src/index.py
```

On lopputuloksena seuravaa virheilmoitus:

```
ModuleNotFoundError: No module named 'cowsay'
```

Tämä johtuu siitä, että emme ole projektin virtuaaliympäristön sisällä, eli Python ei löydä projektimme riippuvuuksia. Asia korjaantuu käyttämällä [run](https://pipenv.pypa.io/en/latest/cli/#pipenv-run) komentoa:

```bash
pipenv run python src/index.py
```

`pipenv run` komento siis suorittaa annetun komennon virtuaaliympäristössä, jonka sisällä Python löytää riippuvuutumme. Voimme myös siirtyä virtuaaliympäristön sisään kommennolla [shell](https://pipenv.pypa.io/en/latest/cli/#pipenv-shell):

```bash
pipenv shell
```

Virtuaaliympäristön sisällä voimme suorittaa komennon "normaalisti":

```bash
python src/index.py
```

Voimme lähteä virtuaaliympäristöstä komennolla `exit`.

## Omat skriptit

Komento `pipenv run python src/index.py` on hieman pitkä ja ei oikeastaan kerro komennon suorittajalle, mitä se tekee. Pipenv:in avulla voimme määritellä [omia skriptejämme](https://pipenv.pypa.io/en/latest/advanced/#custom-script-shortcuts). Skriptit määritellään _Pipfile_ tiedostoon `[scripts]` osion alle. Lisätään _Pipfile_ tiedoston `[[source]]` osion alle osio `[scripts]` ja määritellään sinne skripti `start`:

```
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[scripts]
start = "python src/index.py"

[dev-packages]

[packages]
cowsay = "*"

[requires]
python_version = "3.9"
```

Lisäämämme skriptin pystyy suorittamaan komennolla:

```bash
pipenv run start
```

Komentoja voi olla useita, ja niiden suorittaminen onnistuu komennolla `pipenv run <SKRIPTI>`. Huomaa, että skriptit suoritetaan automaattisesti virtuaaliympäristössä.

Esimerkkiprojekti löytyy kokonaisuudessaan repositorion hakemistosta [esimerkit/pipenv-test](../esimerkit/pipenv-test).
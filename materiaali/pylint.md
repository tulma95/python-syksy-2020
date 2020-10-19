# Pylint ja koodin laaduun staattinen analyysi

Koodin testauksen lisäksi koodin luettavuuden ylläpitäminen on tärkeää. Tässä hyvänä apuvälineenä on staattisen analyysin työkalu [pylint](https://www.pylint.org/). Pylintin avulla pystytään määrittelemään joukko sääntöjä, joita koodin tulisi noudattaa, ja automaisoidusti tarkastaa noudatetaanko näitä sääntöjä.

## Pylintin käyttöönotto projektissa

Pylint on helppo ottaa käyttöön pipenvillä alustetussa projektissa. Aloitetaan asentamalla pylint riippuvuutena haluamassamme projektissa:

```
python -m pipenv install pylint
```

Pylintille tulee määritellä joukko tarkistettavia sääntöjä. Säännöt määritellään projektin juurihakemiston _.pylintrc_-tiedostossa. Luo kyseinen tiedosto ja kopioi sinne [tämän](./.pylintrc) tiedoston sisältö.

Pylintin laatutarkitukset voi suorittaa komentoriviltä siirtymällä ensin virtuaaliympäristöön komennolla `python -m pipenv shell` ja sen jälkeen suorittamalla komennon `pylint src`. Kyseinen komento suorittaa laatutarkitukset _src_ hakemistossa. Pylint antaa koodille "arvosanan" sen laadun mukaan, joka löytyy tulosteen lopusta:

```
Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00)
```

## Laatutarkistuksien kytkeminen pois päältä

Lähtökohtaisesti pylintin huomauttamat laatuvirheet kannattaa yrittää kaikin keinoin korjata. Tämä johtaa lähes aina laadukkaampaan koodin. Joissain tilanteissa voimme kuitenkin hyväksyä laatuvirheet ja kytkeä tarkastukset pois päältä. Tähän löytyy erilaisia keinoja.

Otetaan esimerkiksi seuraava _src/index.py_-tiedosto:

```python
x = 3
print(x)
```

Komennon `pylint src` suorittaminen paljastaa, että pylint löytää tiedostosta seuraavan virheen:

```
src/index.py:1:0: C0103: Constant name "x" doesn't conform to UPPER_CASE naming style (invalid-name)
```

Eli tiedoston _src/index.py_-riviltä yksi löytyy väärin nimetty muuttuja. Rikottavan säännön nimi on tässä tilanteessa `invalid-name`. Järkevintä olisi vain antaa muuttujalle nimeksi `X`, mutta havainnollistetaan, kuinka säännön tarkistuksen voi ottaa riviltä pois päältä. Lisätään riville seuraava kommentti:

```python
x = 3 # pylint: disable=invalid-name
print(x)
```

Nyt `pylint src`-komennon suorittaminen pitäisi kertoa, ettei virheitä enää löydy.

Voimme myös jättää tarkistuksien ulkopuolelle kokonaisia hakemistoja ja tiedostoja. Muokkaamalla [tätä](https://github.com/Kaltsoon/ohjelmistotekniikka-python/blob/master/materiaali/.pylintrc#L13) riviä _.pylintrc_ tiedossa. Voimme esimerkiksi jättää käyttöliittymästä vastaavan koodin hakemistossa _src/ui_ ja testit hakemistossa _src/tests_ tarkistuksien ulkopuolle:

```
ignore=CVS,ui,tests
```

Älä jätä tarkistamatta mitään muuta kuin käyttöliittymän tai testeihin liittyvää koodia! 

Korjaa ohjelmastasi kaikki pylintin ilmoittavat virheet. Vain harvoissa poikkeustilanteissa säännön kytkeminen pois päältä kommentin avulla on hyvä ratkaisu.

## Laatutarkistuksille oma skripti

[Pipenv-ohjeissa](./pipenv.md) ohjeistettiin, kuinka itse määriteltyä skriptejä voi suorittaa `python -m pipenv run`-komennolla. Tehdään laatutarkistuksien suorittamista varten oma skripti, `lint`. Tämä onnistuu määritellemällä se _Pipfile_-tiedoston `[scripts]`-osiossa:

```
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[scripts]
lint = "pylint src"

...
```

Nyt laatutarkituksien suorittaminen pitäisi onnistua komennolla:

```bash
python -m pipenv run lint
```

## Integrointi editoriin

Monissa editoreissa on lisäosia, jotka huomauttavat laatuvirheistä suoraan koodissa. Tämä tekee niiden huomaamisesta ja korjaamisesta nopeampaa. Jos käytössäsi on [Visual Studio Code](https://code.visualstudio.com/), riittää että varmistat, että [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) lisäosa on asennettu:

![Visual Studio Code Python lisäosa](./kuvat/vscode-python-lisaosa.png)

Tämän jälkeen Visual Studio Coden tulisi huomauttaa laatuvirheistä suoraan koodissa punaisella alleviivauksessa. Viemällä hiiren ongelmallisen koodin päälle pitäisi aueta tarkempaa tietoa virheestä:

![Visual Studio Code pylint](./kuvat/vscode-pylint.png)

Jos integroinnin kanssa ilmenee ongelmia, tutustu Visual Studio Coden [ohjeisiin](https://code.visualstudio.com/docs/python/linting).

## Automaattinen formatointi

Tiettyjen laatukorjausten, kuten sisennysten ja liian pitkien koodirivien korjaaminen tuottaa välillä turhaa manuaalista työtä. Koodin automaattisessa formatoinnissa auttaa [autopep8](https://pypi.org/project/autopep8/)-kirjasto. Kirjasto formatoi koodin automaattisesti [PEP 8](https://www.python.org/dev/peps/pep-0008/)-tyyliohjeiden mukaisesti. Aloitetaan sen käyttö asentamalle se projektin riippuvuudeksi:

```bash
python -m pipenv install autopep8
```

Tämän jälkeen voimme virtuaaliympäristössä formatoida _src_ hakemiston koodin komennolla:

```bash
autopep8 --in-place --recursive src
```

Komennolle voi myös tehdä oman skriptinsä, jolloin suoritus onnistuu esimerkiksi komennolla `python -m pipenv run format`.
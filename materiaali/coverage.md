# Coverage ja testikattavuus

Olemme tyytyväisiä, uskomme että testitapauksia on nyt tarpeeksi. Onko tosiaan näin? Onneksi on olemassa työkaluja, joilla voidaan tarkastaa testien rivi- ja haarautumakattavuus. **Rivikattavuus** mittaa mitä koodirivejä testien suorittaminen on tutkinut. Täydellinen rivikattavuuskaan ei tietenkään takaa että ohjelma toimii oikein, mutta on parempi kuin ei mitään. **Haarautumakattavuus** taas mittaa mitä eri suoritushaaroja koodista on käyty läpi. Suoritushaaroilla tarkoitetaan esimerkiksi if-komentojen valintatilanteita.

## Testikattavuusraportti

Testikattavuuden kerääminen testien suorituksesta onnistuu [coverage](https://coverage.readthedocs.io/en/coverage-4.3.2/index.html)-työkalun avulla. Sen asentamisen projektin riippuvuudeksi onnistuu tuttuun tapaan komennolla:

```bash
python -m pipenv install coverage
```

Testikattavuuden kerääminen `pytest`-komennolla suoritetuista testeistä onnistuu virtuaaliympäristössä komennolla:

```bash
coverage run --branch -m pytest
```

Komennon `--branch` flagillä pystymme keräämään testien [haarautumakattavuuden](https://coverage.readthedocs.io/en/coverage-4.3.2/branch.html). Komennon suorittamisen jälkeen voimme tulostaa komentoriville raportin kerätystä tettikattavuudesta komennolla:

```bash
coverage report -m
```

Tulostuksesta huomaamme, että raportissa on suuri määrä projektin kannalta turhia tiedostoja. Voimme konfiguroida, mistä tiedostoista testikattavuutta kerätään projektin juurihakemiston _.coveragerc_-tiedostossa. Jos haluamme sisällyttää testikattavuuteen vain projektin _src_-hakemiston, on konfiguraatio seuraava:

```
[run]
source = src
```

## Tiedostojen jättäminen raportin ulkopuolelle

Voimme jättää testikattavuuden ulkopuolelle tiedostoja ja hakemistoja. Järkevää voisi olla esimerkiksi jättää testihakemisto, käyttöliittymän koodin hakemisto ja _src/index.py_-tiedosto testikattavuuden ulkopuolle. Tämä onnistuu seuraavalla muutoksella _.coveragerc_-tiedostoon:

```
[run]
source = src
omit = src/tests/**,src/ui/**,src/index.py
```

Nyt komentojen `coverage run --branch -m pytest` ja `coverage report -m` suorittaminen sisällyttää vain haluamamme _src_-hakemiston tiedostot.

Komentoriviltä luettavaa raporttia selkeämmän esitysmuodon voi generoida komennolla:

```bash
coverage html
```

Komennon suorittaminen luo projektin juurihakemistoon hakemiston _htmlcov_. Raporttia voi katsoa selaimessa avaamalla hakemiston tiedoston _index.html_ selaimen kautta.

## Testikattavuusraportin generoimiselle omat skriptit

[Pipenv-ohjeissa](./pipenv.md) ohjeistettiin, kuinka itse määriteltyä skriptejä voi suorittaa `python -m pipenv run`-komennolla. Tehdään testikattavuuden keräämiselle ja raportoinnin omat skript, `coverage` ja `coverage-report`. Tämä onnistuu määritellemällä ne _Pipfile_-tiedoston `[scripts]`-osiossa:

```
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[scripts]
coverage = "coverage run --branch -m pytest"
coverage-report = "coverage html"

...
```

Nyt testikattavuuden kerääminen ja raportin generointi pitäisi onnistua komennoilla:

```bash
python -m pipenv run coverage
python -m pipenv run coverage-report
```

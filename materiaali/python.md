# Muutamia toteutukseen liittyviä vihjeitä

## Sovelluksen käyttöliittymä

## Tietojen tallennus

## Sovelluksen konfiguraatiot

Sovelluksen koodiin ei ole syytä kovakoodata mitään konfiguraatioita, kuten sen käyttämien tiedostojen tai tietokantojen nimiä. Eräs syy tähän on se, että jos konfiguraatiot ovat koodissa, ei ohjelman normaalin käyttäjän (jolla ei ole pääsyä koodiin) ole mahdollista tehdä muutoksia konfiguraatioihin.

Konfiguraatiot on syytä määritellä ohjelman ulkopuolella, esim. erillisissä konfiguraatiotiedostoissa. Pipenv [lataa automaattisesti](https://pipenv-fork.readthedocs.io/en/latest/advanced.html#automatic-loading-of-env) projektin juurihakemiston _.env_ tiedostosta niin kutsuttuja _ympäristömuuttujia_. Näihin ympäristömuuttujiin pääsee koodissa käsiksi `os`-moduulin [getenv](https://docs.python.org/3/library/os.html#os.getenv)-funktion avulla. Tiedoston _.env_ sisältö voisi olla esimerkiksi seuraava:

```
FOO=bar
LOREM=ipsum
```

Tiedostossa määritellään arvot `FOO` ja `LOREM` ympäristömuuttujille. Jokainen muuttuja määritellään omalla rivillään formaatissa `MUUTTUJAN_NIMI=muuttujan_arvo`. Voimme tulostaa muuttujien arvot seuraavalla tavalla:

```python
import os

print(os.getenv("FOO"))
print(os.getenv("LOREM"))
```

Jos ympäristömuuttajalle ei ole annettu arvoa, `getenv`-funktio palauttaa `None`. Jotta koodi pysyisi selkeänä, kannattaa ympäristömuuttujien arvot lukea erillisessä moduulissa, joka toteutetaan esimerkiksi _src/config.py_ tiedostoon:

```python
import os

# jos os.getenv("FOO") palauttaa arvon None, FOO saa arvokseen "default bar"
FOO = os.getenv("FOO") or "default bar"
LOREM = os.getenv("LOREM") or "default ipsum"
```

Muuttujille on hyvä antaa oletusarvot, jos ympäristömuuttujalle ei ole annettu arvoa. Moduulin muuttujat `FOO` ja `LOREM` voidaan importata projektin toisessa tiedostossa seuraavasti:

```python
from config import FOO, LOREM

print(FOO)
print(LOREM)
```

Testeille on usein käytössä eri konfiguraatio, kuin muulla koodilla. Esimerkiksi testien kannattaa käyttää SQLite-tietokannan kanssa eri tiedostoa. Tätä varten voimme tehdä projektin juurihakemistoon erillisen _.env.test_-tiedoston, jonne määritellään testien käyttämät ympäristömuuttujat.

Näiden ympäristömuuttujien lataaminen onnistuu [pytest-dotenv](https://pypi.org/project/pytest-dotenv/)-lisäosalla. Sen asentaminen onnistuu seuraavalla komennolla:

```bash
python -m pipenv install pytest-dotenv
```

Asentamisen lisäksi tulee projektin juurihakemistoon luoda _pytest.ini_-tiedosto, jossa kerrotaan, mistä tiedostosta ympäristömuuttujat ladataan. Tiedoston sisältö tulee olla seuraava:

```
[pytest]
env_override_existing_values = 1
env_files =
    .env.test
```

Nyt testit, jotka suoritetaan `pytest`-komennolla, käyttävät _.env.test_-tiedossa määriteltyjä ympäristömuuttujia.

## Uuden tekniikan harjoittelu ja käyttöönotto
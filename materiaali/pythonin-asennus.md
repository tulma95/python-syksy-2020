# Pythonin asennus

Kurssin materiaali perustuu Pythonin versioon `3.9.0`. Esimerkkien, tehtävien ja projektin toimivuuden kannalta on erittäin tärkeää, että kaikki kurssin opiskelijat käyttävät juuri tätä versiota. Python-version voit tarkastaa komentoriviltä komennolla:

```bash
python --version
```

Huomaa, että Python skriptejä tulee voida suorittaa komennolla `python`, eikä esimerkiksi komennolla `python3`. Jos Python versiosi poikkeaa versiosta `3.9.0`, seuraa tämän materiaalin asennusohjeita.

## Python-versioiden hallinta

Pythonin eri versioiden asennus ja vaihtaminen onnistuu helposti [pyenv](https://github.com/pyenv/pyenv) komentorivityökalun avulla. Pyenv tarjoaa dokumentaatiossaan erilaisia [asennusvaihtoehtoja](https://github.com/pyenv/pyenv#installation). Linux ja macOS käyttöjärjestelmien käyttäjille asennus onnistuu helpoiten [Homebrewin](https://brew.sh/index_fi) avulla. Kun Homebrew on asennettu, pyenv:in asennus onnistuu seuraavilla komennoilla:

```bash
brew update
brew install pyenv
```

Windows käyttöjärjestelmän käyttäjien kannattaa asentaa ja käyttää [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) (WSL) toimintoa viimeisimmän Ubuntu-version kanssa ja seurata [tämän](https://hihigash.com/how-install-pyenv-to-ubuntu-on-windows-adf48cc0577f) artikkelin asennusohjeita.

Asennuksen onnistumisen voi tarkistaa suorittamalla komennon:

```bash
pyenv --version
```

Onnistuneen asennuksen jälkeen oikean Python-version voi asentaa komennolla:

```bash
pyenv install 3.9.0
```

Jonka jälkeen se voidaan asettaa globaalisti käytettäväksi Python-versioksi komennolla:


```bash
pyenv global 3.9.0
```

Tämän jälkeen Python-version tulisi olla oikea. Voit vielä varmistaa asian komennolla:

```bash
python --version
```

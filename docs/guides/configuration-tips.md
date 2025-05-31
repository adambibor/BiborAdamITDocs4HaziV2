---
title: Konfigurációs tippek
sidebar_position: 2
---

# Konfigurációs beállítások

## Tailscale

Először a Tailscale nevű ingyenes VPN (Virtual Private Network – Virtuális Magánhálózat) szoftver szükséges letölteni és telepíteni a szervernek szánt számítógépre, illetve azokra az eszközökre, melyekről el szeretnénk érni később a szerverünket. A Tailscale Windowsra [innen](https://tailscale.com/download/windows) tölthető le, minden más okosezköz esetében pedig a hivatalos alkalmazásáruházakat ([App Store](https://apps.apple.com/us/app/tailscale/id1470499037), [Google Play](https://play.google.com/store/apps/details?id=com.tailscale.ipn)) javasolt használni, mint forrást. Ezen eszközök lehetnek például okostelefonok, táblagépek, laptopok, tv-okosítók, vagy egyes esetekben akár okos tv-k is. 
Telepítés után a Tailscale-be meglévő Google, Microsoft, Github vagy Apple fiókunkkal tudunk regisztrálni, illetve belépni. Ezt követően más dolgunk nincs is, a Tailscale automatikusan felépíti a VPN hálózatot, melyet Tailnetnek hívnak. Érdemes az [admin felületről](https://login.tailscale.com/admin/machines) lejegyezni a szervergépünk rövid domain (short domain) nevét is, mely alapértelmezetten a gép neve, erre még később szükség lesz.

## qBittorrent

Szükségünk lesz egy Bittorrent kliensre, melyek közül az egyik legnépszerűbb és legmegbízhatóbb a qBittorrent. A szoftvert a szervergépre szükséges telepíteni, majd a webes felületet (WebUI) kell engedélyeznünk a beállításokban, melyhez felhasználónevet és jelszót javasolt beállítanunk, valamint a portszámot lejegyeznünk. 
Kategóriákat is meg kell adnunk a szoftvernek, úgy, mint filmek és sorozatok, és ezekhez fix letöltési lokációkat kötni. 
Érdemes ellenőrizni, hogy a WebUI-t elérjük-e egy másik, a Tailneten lévő eszköz böngészőjéből a korábban lejegyzett short domain és portszám megadásával, egy ilyen cím például Windows operációs rendszer esetén valahogy így néz ki: http://desktop-xxxxxxx:8080

## Jackett

Következik a Jackett telepítése a szervergépre, mely egy indexer alkalmazás a választott torrentoldalainkhoz. A Jackettet a http://localhost:9117 címen érhetjük el. Itt a torrentoldalakat szükséges beállítani, amiket használni szeretnénk (privát, vagyis nem nyílt oldalak esetén felhasználónév és jelszó megadása is szükséges lehet), valamint az API kulcsot, és a torrentoldalak „Torznab Feed” -jét kell lejegyeznünk. Maga a felület is ad egy leírást, hogyan tudjuk később hozzákötni a Jackettet a Sonarrhoz és a Radarrhoz.

## Radarr és Sonarr

A Radarrt és Sonarrt szintén a szervergépre kell telepíteni, melyeket a http://localhost:7878 és a http://localhost:8989 alatt érhetünk el, majd a beállításaikban (Settings):
- az Indexereknél össze kell kötni őket előbb a Jackett-tel: 
  - az URL helyére a Jackett Torznab Feed linkje kerül, 
  - az API kulcshoz pedig a Jackett API kulcsa,
- majd pedig Download Clientsnél a qBittorrenttel: 
  - a Hosthoz a qBittorrent WebUI URL-je kerül, 
  - a Porthoz a qBittorrent WebUI portszáma, 
  - a Username/Passwordhöz a qBittorrent WebUI megfelelő autentikációs adatai, 
  - a Categoryhoz pedig Radarr esetén a qBittorrentben létrehozott filmek kategória, Sonarr esetén pedig a qBittorrentben létrehozott sorozatok kategória.

### További beállítások

Ha ezekkel megvagyunk, akkor minőségbeli beállításokat is megadhatunk a beállítások között a Custom Formats és a Quality menüpontok segítségével (pl. FullHD minőség, magyar hangsávval), bár alapértelmezetten is kapunk előbeállításokat. Végül érdemes ellenőrizni / megadni a beállításokon belül a Media Managementnél a Root Foldert (ezt jegyezzük is le, itt fontos, hogy nem lehet ugyanaz a mappa, mint ahova a letöltések kerülnek), a „Rename Movies”, illetve a haladó beállítások (Show Advanced) alatt a „Use Hardlinks instead of Copy” opciókat pedig be kell pipálnunk. 
Ezek azért fontosak, mert ha letöltünk egy tartalmat, akkor abból a Root Folderbe egy úgynevezett hardlinkelt fájl kerül, aminek a neve könnyen értelmezhető és felismerhető a médiacenter számára is, viszont gyakorlatilag az eredetileg letöltött fájlokra mutatnak, többlet helyfoglalás nélkül a háttértárunkon. 
Itt még a Sonarr és Radarr esetén is a beállításoknál a General menüpontban jegyezzük le az API kulcsokat, később szükségünk lesz rájuk.

## Jellyfin Server

Telepíthetjük a Jellyfint is (fontos, hogy a szerver változatát, ne a klienst), ami egy médiacenterként fog funkcionálni a szervergépünkön. A Jellyfint a http://localhost:8096 címen érhetjük el. Itt sok dolgunk nem lesz, a kezdeti lépéseken egy varázsló vezet minket végig, majd a Vezérlőpult / Könyvtárak alatt két könyvtárat kell létrehoznunk (filmek/sorozatok), melyek elérési útjaként a Sonarr/Radarr Root Folder-eit kell majd megadnunk.


## Jellyseerr

A rendszer utolsó komponense a szerverünkön a Jellyseerr, ezen keresztül fogunk tudni egy kifejezetten felhasználóbarát felületen új filmeket vagy sorozatokat hozzáadni a 
gyűjteményünkhöz. A Jellyseerr csak Docker konténerként érhető el (ez egy virtualizációs sztenderd, mely abban különbözik egy virtuális géptől, hogy a hardvert nem, csak szoftveres környezetet emulálja, vagy másképpen virtualizálja, ezáltal sokkal kevésbé erőforrásigényes), így használatához szükséges a Docker Desktop alkalmazás, valamint Windows operációs rendszer esetén a Windows Subsystem for Linux (WSL), ami a Microsoft megoldása Linux környezet emulálására a Windows rendszerén belül.
Ha a Dockert és a WSL-t sikerült telepíteni és beállítani, akkor a parancssorból telepíthető a Jellyseer, ehhez a dokumentációjában megtalálható és parancssorba másolható a 
megfelelő parancs. Telepítés után itt is varázsló fogad majd minket a http://localhost:5055 címen.
- A Jellyfint kiválasztva meg kell adnunk a 
    -  Jellyfin URL-jét (itt tapasztalatom szerint a szervergép Tailnetes IP címét érdemes megadni, minden mást visszautasít a szoftver), 
    -  egy bármilyen e-mail címet,
    -  és a Jellyfinhez tartozó felhasználónevet és jelszót. 
-  A következő képernyőn a Sync Libraries gombra kattintva kiválaszthatjuk, hogy 
mely Jellyfin könyvtárakat (Library) szeretnénk szinkronban tartani a Jellyseerrrel, jelen esetben a filmek és sorozatok nevű könyvtárakat. 
-  A következő (és egyben utolsó) lépés a Jellyseerr összekötése a Radarr/Sonarrral. Itt egy 
    - bármilyen nevet, 
    - a Radarr / Sonarr URL-jét, 
    - a Radarr / Sonarr API kulcsát, 
    - az alapértelmezett minőségi profilt (Quality Profile) 
    - és a Root Foldert kell beállítanunk.
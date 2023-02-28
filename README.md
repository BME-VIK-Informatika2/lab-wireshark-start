# Wireshark labor

## Felkészülés
A laborra felkészüléshez olvassa el a segédletet és próbáljon meg válaszolni az ellenőrző kérdésekre.

## Útmutató
Ebbe a könyvtárba készítse el a wireshark labor megoldásait. A megoldásokat pull request formájában adja be a határidőre úgy, hogy reviewerként hozzárendeli a laborvezetőjét.
Figyeljen rá a labor elején, hogy hozzon létre egy új git branchet, mert a pull request létrehozásánal azt tudja majd a master ággal összehasonlítani.

Amennyiben a labor szöveges válaszokat kér kérdésekre, azokat a válaszokat ide írja be!
Mivel ezen a mérésen nem forráskód készül, a szöveges válaszokat is írja le ide!

## 1. ARP címlekérés vizsgálata

Az address resolution protocol (ARP) feladata a HW és az IP cím összerendelés feloldása. Segítségével
kideríthetjük, hogy az egyes IP címekhez milyen host tartozik.

A mérési feladat megvalósításához a Wireshark programban a következő **Capture Filter** beállítással
indítsuk el a monitorozást:

```
arp host <saját ip cím>
```
A terminálban futtassuk le a `ping` programot a mérésvezető által megadott IP címmel. Azonban
mielőtt ezt megtennénk, ellenőrizzük az `arp -a` paranccsal, hogy az adott IP cím nem szerepel-e már
az ARP gyorsító tárban. Amennyiben szerepel, akkor más címet kell választanunk, különben a
vizsgálódásunk eredménytelen lesz.

A `ping` parancs kiadása után állítsuk le a monitorozást és elemezzük a kapott csomagokat!

1. A kérés csomag Ethernet kerete milyen cél címet tartalmaz és miért?
2. A kérés csomag ARP keretében milyen HW és IP címeket találunk?
3. A válasz csomag Ethernet kerete milyen cél címet tartalmaz?
4. A válasz csomag ARP keretében milyen HW és IP címeket találunk?

## 2. Ping vizsgálata

A `ping` program feladata, hogy kiderítse egy másik gép hálózati elérhetőségét. Segítségével
megvizsgálhatjuk, hogy a csomagjaink eljutnak-e a megadott géphez, illetve a válasz csomagok
visszajutnak-e.

A mérési feladat megvalósításához a Wireshark programban a következő **Capture Filter** beállítással
indítsuk el a monitorozást:

```
icmp
```
A terminálban futtassuk le a `ping` programot a mérésvezető által megadott IP címmel.

A `ping` parancs kiadása után állítsuk le a monitorozást és elemezzük a kapott csomagokat!

1. Melyik ICMP csomag típust használja a ping program a kommunikáció ellenőrzésére?
2. A távoli gép milyen csomaggal jelzi a kommunikációs kapcsolat meglétét?

## 3. Név feloldás vizsgálata

A DNS feladata az IP cím, gép név összerendelés feloldása. Segítségével az IP címek helyett
használhatunk neveket is a cél gépek megadásánál.

A mérési feladat megvalósításához a Wireshark programban a következő **Capture Filter** beállítással
indítsuk el a monitorozást:

```
port 53
```
A terminálban futtassuk le a `ping` programot a mérésvezető által megadott gépnévvel.


A `ping` parancs kiadása után állítsuk le a monitorozást és elemezzük a kapott csomagokat!

1. Milyen protokoll csomagokat küld ki és kap a gépünk a név feloldás során?
2. Mi a kérés csomag cél portja és a válasz csomag forrás portja?
3. Keresse meg a kérés csomagban a lekérdezett gép nevét és a válasz csomagban a név szerver
    válaszát!

## 4. Traceroute vizsgálata

A `traceroute` , vagy az MS Windows implementációja a `tracert` a csomagok hálózati útvonalát követi
vissza. Segítségével kideríthetjük, hogy a hálózati csomagjaink milyen útvonalon haladnak, mely
routereket érintve jutnak el a célba. Alapbeállításban minden routert 3-szor vizsgál meg, majd
visszaadja egy listában a címüket és a kommunikációs időket.

A mérési feladat megvalósításához a Wireshark programban a következő **Capture Filter** beállítással
indítsuk el a monitorozást (Linux alatt: „not broadcast and not multicast and not arp”):

```
icmp
```
A terminálban futtassuk le a `tracert` programot a mérésvezető által megadott IP címmel a következő
módon (Linux alatt: `traceroute –n –q 1 <ip cím>`) :

```
tracert –d <ip cím>
```
A miután véget ért a parancs állítsuk le a monitorozást és elemezzük a kapott csomagokat!

1. Milyen protokoll csomagokat küld a program a távoli gépnek?
2. Hogyan változik a kiküldött csomagok „Time to live” értéke?
3. Honnan és milyen protokoll üzeneteket kapunk vissza?
4. Hogyan deríti ki ez a mechanizmus a kommunikációs vonalban lévő egyes csomópontok címét?

## 5. TCP kapcsolat felépülése és lebontása

A mérési feladat megvalósításához a Wireshark programban a következő **Capture Filter** beállítással
indítsuk el a monitorozást:

```
tcp port 80
```
A terminálban építsünk fel, majd bontsunk le egy TCP kapcsolatot a mérésvezető által megadott web
szerverrel a következő parancsokkal:

```
telnet gépnév 80
<CTRL+]>
quit
```
A miután véget ért a parancs állítsuk le a monitorozást és elemezzük a kapott csomagokat!

1. Vizsgálja meg a gépünk által a szervernek küldött első csomag, a szerver válasz csomagjának,
    majd a gépünk viszont válasz csomagjának TCP Flags mező értékét! Hogyan változnak a Flag
    értékek a kapcsolat felépülése során?
2. Vizsgálja meg a többi csomag (a kapcsolat lebontás csomagjainak) TCP Flags értékét! Hogyan
    változnak a Flag értékek a kapcsolat lebontása során?

## 6. HTTP protokoll vizsgálata

A mérési feladat megvalósításához a Wireshark programban a következő **Capture Filter** beállítással
indítsuk el a monitorozást:

```
tcp port 80
```
Nyissunk meg egy web böngészőt és hozzuk be a mérésvezető által megadott weboldalt!

A miután véget ért a weboldal betöltése állítsuk le a monitorozást és nézzük meg a kapott
csomagokat. A TCP kommunikáció egy csomagját kiválasztva fűzzük össze a kommunikáció
csomagjait a **Follow TCP Stream** menüpont használatával.

A megjelenő ablakban vizsgáljuk meg a kliens és a szerver üzeneteit!

1. Keresse meg a kliens kérését, azon belül a címet és a protokoll verziót!
2. Keresse meg a szerver válaszában a státuszkódot!
3. Keresse meg a web oldal HTML forrását a szerver válaszában! (A HTML forrás azonosításában
    segíthet, ha a web böngészőben lekéri a web oldal forrását.)



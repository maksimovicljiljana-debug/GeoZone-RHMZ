# GeoZone–RHMZ | Stanice podzemnih i površinskih voda i meteorološke stanice Srbije

Objedinjeni QGIS projekat za pregled lokacija stanica, njihovih osnovnih karakteristika i dostupnih linkova ka izvorima. Namenjen je kolegama kojima je potreban praktičan početni pregled osmatračkih mesta na području istraživanja.

**Priprema i GIS obrada:** Ljiljana Maksimović — GeoZone  
**Izvor podataka:** Republički hidrometeorološki zavod Srbije (RHMZ) i navedeni zvanični registri  
**Verzija paketa:** 1. oktobar 2026.

Ovo je nezavisno obrađen pregled javno dostupnih podataka, a ne zvaničan proizvod RHMZ-a. Paket ne predstavlja celu državnu mrežu niti prikaz merenja u realnom vremenu.

## Brzo otvaranje

1. Preuzmite ZIP i raspakujte **ceo folder**.
2. U QGIS-u otvorite **GeoZone-RHMZ.qgz**.
3. Uključite željene slojeve. Alatom **Identify Features** kliknite na stanicu za pregled atributa; atributnu tabelu otvorite desnim klikom na sloj.
4. Izvorne stranice dostupne su kroz polja `source_url`, `link` i druga URL polja, gde su popunjena.

Projekat je sačuvan u QGIS-u **3.40.14 Bratislava**. Putanje do sva tri GeoPackage fajla su relativne: držite ih uz projekat, u istom folderu. Stilovi, oznake i podešavanja prikaza sačuvani su u QGIS projektu.

Podloge **OpenStreetMap** i **Google Satellite** povezane su kao internet servisi; njihovi snimci nisu sadržani u paketu. Za podloge je potrebna internet veza, dok se lokalni slojevi stanica mogu koristiti bez nje. Dostupnost podloga zavisi od pružalaca servisa; poštujte njihove uslove i atribuciju.

[Pregled interaktivne mape u Google My Maps](https://www.google.com/maps/d/viewer?mid=1UuagLuV2ha0PrCeFf0x6xigWC_A9VMQ)

## Sadržaj paketa

| Datoteka | Sadržaj | Broj zapisa |
|---|---|---:|
| `GeoZone-RHMZ.qgz` | QGIS projekat sa stilovima, oznakama i internet podlogama | — |
| `Piezometers_RHMZ.gpkg` | Sloj `points_from_table`: stanice podzemnih voda / piezometri | 415 |
| `surface_water_RHMZ.gpkg` | Sloj `stanice_sa_koordinatama`: površinske vode | 189 |
| `surface_water_RHMZ.gpkg` | Tabela `sve_stanice`: registar površinskih voda, bez geometrije | 432 |
| `meteo_2024_RHMZ.gpkg` | Sloj `meteo_2024_priblizno`: meteorološke stanice iz godišnjaka 2024. | 55 |

Ukupno je u tri prostorna sloja **659 tačaka**. Tabela od 432 zapisa uključuje 189 stanica prikazanih na karti; nije dodatnih 432 lokacije. Preostalih 243 stanice površinskih voda nemaju geometriju u ovom paketu.

## Podzemne vode — piezometri

Sloj sadrži naziv i oznaku stanice, područje, dostupne podatke o konstrukciji i geološkoj sredini, kotama, režimu osmatranja i karakterističnim nivoima, kao i linkove ka RHMZ stranici i pratećim prikazima.

| Polja | Značenje |
|---|---|
| `name`, `code` | Naziv i oznaka stanice |
| `max_level_cm`, `mean_level_cm`, `min_level_cm` | Karakteristični nivoi, iskazani kao dubine u cm prema referentnoj tački na vrhu cevi |
| `max_level_raw`, `mean_level_raw`, `min_level_raw` | Izvorni zapisi, uključujući nenumeričke oznake |
| `z0_m`, `terrain_m`, `above_m` | Kota referentne tačke, kota terena i visina referentne tačke iznad terena |
| `easting`, `northing` | Metričke koordinate korišćene za geometriju |
| `source_url`, `profile_url`, `map_url` | Linkovi ka izvornoj stranici, profilu i kartografskom prikazu, gde postoje |
| `qa`, `crs_note` | Napomene o podacima i njihovom poreklu |

**Maksimalni nivo vode odgovara najmanjoj dubini do vode.** Vrednosti u centimetrima nisu apsolutne kote nivoa. Ne treba ih direktno porediti kao hidrauličke potencijale niti interpolirati bez usklađivanja referentnih kota, perioda i uslova osmatranja.

Status u atributima prenet je iz izvora; nije nezavisno proveravan na terenu. Izvorni podaci preuzeti su 29. septembra 2026.

## Površinske vode

Prostorni sloj ima 189 stanica sa koordinatama iz registra lokacija iz **2013. godine**. Uparivanje sa stranicama RHMZ-a dokumentovano je u atributima `coord_match`, `coord_name`, `coord_waterbody`, `coord_year` i `coord_source`.

Tabela `sve_stanice` ima 432 zapisa: **220 aktivnih, 209 ukinutih i 3 sa nesaglasnim statusom u izvorima**, prema prikupljenim podacima. To su statusi izvora u trenutku prikupljanja, ne potvrda rada na terenu.

Važna polja su `name`, `river`, `status`, `z0_m` (kota nule vodomera), `catchment_km2` (površina sliva), `mouth_dist_km`, podaci o periodima merenja i `source_url`. Polje `measurements_url` sadrži dodatni link tamo gde je pronađen.

**Ovaj paket ne sadrži vremenske serije vodostaja ili proticaja niti njihove min/max vrednosti.** Kota nule vodomera nije vodostaj. Metapodaci su preuzeti 30. septembra 2026; koordinatni izvor je stariji i ne potvrđuje da lokacije od 2013. nisu menjane. Uočene razlike zapisane su u `qa`.

## Meteorološke stanice

Spisak i položaji 55 stanica potiču iz **Meteorološkog godišnjaka 1 — Klimatološki podaci za 2024**, štampane strane 11–13 (PDF strane 17–19). Ovaj sloj ne obuhvata kompletnu mrežu padavinskih stanica.

Prva atributna polja su naziv, minimalna i maksimalna temperatura, minimalne i maksimalne padavine i link. Za **26 stanica** preuzeti su temperaturni ekstremi i maksimalne padavine sa pojedinačnih RHMZ stranica, uz datume. Za ostale ta polja nisu popunjena; link vodi ka godišnjaku.

- `temp_min_C`, `temp_max_C`: vrednosti u °C.
- `padavine_max_mm`: maksimalne padavine u mm prema stranici stanice.
- `padavine_min_mm`: prazno za sve stanice jer minimum nije objavljen u korišćenom izvoru.
- `temp_min_datum`, `temp_max_datum`, `padavine_max_datum`: datumi preneti iz izvora.
- `tip`: glavna ili obična stanica; nije oznaka trenutne aktivnosti.
- `period_rada`: period naveden u godišnjaku; ne garantuje neprekidnost serije.

Ekstremi sa stranica **nisu godišnji ekstremi za 2024**. Stranice ne navode zajednički referentni period ni garantovanu ažurnost rekorda. Trajanje akumulacije uz maksimalne padavine nije eksplicitno navedeno na korišćenim stranicama: vrednost ne treba samostalno tumačiti kao godišnju sumu ili potvrđen 24-časovni maksimum. Stranice su preuzete 30. septembra 2026.

## Koordinatni sistemi i tačnost položaja

| Sloj | Zapis u dostavljenom paketu |
|---|---|
| Projekat | EPSG:3909 |
| Piezometri | U projektu EPSG:3909; GeoPackage koristi interni SRS ID 100000 sa definicijom MGI 1901 / Balkans zone 7 i oznakom EPSG:3909 u WKT-u |
| Površinske vode | GeoPackage EPSG:3909; projekat za prikaz sloja navodi EPSG:6316 |
| Meteo | EPSG:4326 kao radna pretpostavka; izvorni geodetski datum nije potvrđen |

Definicije za površinske vode u projektu i datoteci imaju iste parametre projekcije MGI 1901 / Balkans zone 7, ali različit zapis redosleda osa. Oznake su dokumentovane **kako su dostavljene**, bez prepisivanja CRS-a ili transformisanja geometrije. Geometrijske X/Y vrednosti svih 415 piezometara i svih 189 tačaka površinskih voda proverene su prema poljima `easting`/`northing` i poklapaju se.

Meteo lokacije su **približne**: izvorne koordinate date su do celog minuta. Pretvaranje u decimalne stepene ne povećava njihovu tačnost. EPSG:4326 je praktična pretpostavka za pregledni prikaz, a ne potvrđen datum izvora. Nije garantovana maksimalna greška od 2,5 km.

Primer nesaglasnosti: godišnjak za Crni Vrh navodi 44°08′ N, 21°58′ E, dok stranica stanice navodi 44°07′ N, 21°57′ E, približno 2,3 km razlike. Uzrok nije potvrđen. U sloju je dosledno korišćen godišnjak.

Položaji nisu provereni terenskim snimanjem. Za precizno lociranje objekata, projektovanje ili analize osetljive na položaj potrebno je dodatno proveriti koordinate i aktuelno stanje.

## Izvori i tumačenje nedostajućih podataka

- [RHMZ Srbije](https://www.hidmet.gov.rs/) — pojedinačni URL-ovi sačuvani su u atributima.
- [RHMZ — stanice površinskih voda](https://www.hidmet.gov.rs/ciril/hidrologija/povrsinske/index.php).
- [Registar lokacija hidroloških stanica, Prilog 2, koordinatni izvor 2013.](https://reg.pravno-informacioni-sistem.rs/api/Attachment/prilozi/246717/2prilozi.html).
- [RHMZ — mreža sinoptičkih stanica](https://www.hidmet.gov.rs/ciril/meteorologija/moss_mreza.php).
- [Meteorološki godišnjak 2024.](https://www.hidmet.gov.rs/data/meteo_godisnjaci/Republika%20Srbija%20-%20Meteorolo%C5%A1ki%20godisnjak%201%20-%20klimatoloki%20podaci%20-%202024.pdf).

Prazno polje / NULL znači da vrednost nije dostupna u ovom paketu, a ne da je jednaka nuli. Sačuvane izvorne tekstualne oznake ne treba automatski zamenjivati nulama. Prazno polje `qa` nije potvrda tačnosti lokacije. Linkovi vode ka spoljnim izvorima i mogu se vremenom promeniti.

Pri daljem korišćenju navedite **RHMZ kao izvor podataka** i **GeoZone — Ljiljana Maksimović kao autora objedinjavanja i GIS obrade**. Ovaj README ne dodeljuje novu licencu nad izvornim RHMZ podacima.

## Provera paketa

Provereni su integritet tri SQLite/GeoPackage baze, broj zapisa, relativne putanje u projektu i poklapanje metričke geometrije sa koordinatnim atributima. Projektni i podatkovni fajlovi sačuvani su neizmenjeni; dodat je ovaj README. U ovoj proveri nije izvršeno vizuelno otvaranje projekta u desktop QGIS-u niti terenska kontrola lokacija.

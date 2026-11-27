# RideShare - Java 11

## Çfarë ndërtova

Shtova njoftime brenda aplikacionit kur shoferi pronar pranon ose refuzon kërkesën e udhëtarit. Çdo njoftim ruan `kerkesa_id`, statusin e përgjigjes dhe marrësin. Kufizimi unik `(kerkesa_id, status)` ndalon njoftime të dyfishta për të njëjtën përgjigje. Kërkesa kalohet në API me ID-në e vet, prandaj një njoftim nuk hap një kërkesë tjetër të udhëtimit.

Kutia e njoftimeve shfaqet te Paneli im dhe rilexohet çdo 5 sekonda. Kur udhëtari hap një njoftim, faqja kërkon kërkesën përkatëse nga serveri dhe shënon atë njoftim si të lexuar. Nuk përdora Web Push; funksioni nuk varet nga mbështetja për push dhe nuk kërkon leje të shfletuesit.

Ekzekuto `schema-java11.sql` në Neon pas `schema-java10.sql`.

## Provat

Të dhëna prove të sugjeruara: udhëtimi fiktiv 2, një llogari udhëtari dhe llogaria e shoferit që zotëron udhëtimin. Mos përdor fjalëkalime reale në raport.

### 1. Përgjigjja shfaqet në njoftim

**Pritja:** Pas pranimit shihet `Kërkesa u pranua`; pas refuzimit shihet `Kërkesa u refuzua`.

**Ndodhi realisht:** Nuk u ekzekutua provë me dy llogari dhe Neon në këtë sesion. Build-i dhe kontrolli i tipeve kaluan; teksti i njoftimit dhe ruajtja janë të implementuara, por kjo nuk provon sjelljen në databazë reale.

### 2. Njoftimi hap kërkesën e duhur dhe bëhet i lexuar

**Pritja:** Klikimi hap kërkesën me të njëjtën ID që ruhet te njoftimi. Pas leximit të suksesshëm nga serveri, njoftimi nuk numërohet më si i palexuar.

**Ndodhi realisht:** Nuk u ekzekutua provë manuale në shfletues. Në kod, URL-ja përmban ID-në e kërkesës dhe API-ja shënon të lexuar vetëm rreshtin për çiftin përdorues–kërkesë.

### 3. Njoftime të përsëritura dhe pa Web Push

**Pritja:** Rifreskimet e panelit nuk krijojnë kopje. Veprimi i përsëritur mbi kërkesën e përfunduar nuk shton njoftim tjetër. Aplikacioni vazhdon pa kërkuar mbështetje ose leje për push.

**Ndodhi realisht:** Nuk u ekzekutua provë manuale në Neon. Kufizimi unik në skemë dhe veprimi server-side janë implementuar. Nuk ka kod Web Push ose kërkesë lejeje.

## Skedarët kryesorë

- `schema-java11.sql` — statusi `e_refuzuar` dhe tabela e njoftimeve.
- `src/app/api/udhetimet/[id]/kerkesa/route.ts` — përgjigjja e shoferit, krijimi i njoftimit dhe shënimi si i lexuar.
- `src/app/api/njoftimet/route.ts` — leximi i njoftimeve të përdoruesit të kyçur.
- `src/components/Njoftimet.tsx` — inbox-i dhe rifreskimi periodik.
- `src/components/GjendjaKerkeses.tsx` — prano/refuzo dhe hapja e kërkesës specifike.

## Kufizimet dhe verifikimi

`npm run build` kaloi. Nuk u krye test me koleg, Neon ose dy sesione reale në këtë sesion; prandaj rezultatet e mësipërme shënohen si të paekzekutuara dhe jo si sukses manual. Para dorëzimit, ekzekuto migrimin në Neon dhe provo të tre skenarët me të dhëna fiktive. Nëse ndonjë provë dështon, shëno rezultatin real këtu.

## Ndihma nga AI

Përdora AI për të zgjeruar API-në dhe skemën, për të shtuar inbox-in dhe për të shkruar këtë raport. Kontrollova build-in dhe kodin; nuk paraqita testet manuale si të kryera.

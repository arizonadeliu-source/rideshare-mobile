# RideShare - Java 10

## Rezultati i pritshëm

Shtova një kërkesë reale në Neon dhe një lexim automatik prej 5 sekondash për dy rolet. Shoferi pronar mund të pranojë kërkesën; udhëtari dhe shoferi lexojnë të njëjtën gjendje të udhëtimit dhe statusin e kërkesës.

## Çfarë ndërtova

- `schema-java10.sql` krijon tabelën `kerkesat_udhetimi` me status, udhëtar, udhëtim dhe një kërkesë unike për çiftin udhëtar–udhëtim.
- `/api/udhetimet/[id]/kerkesa` lexon gjendjen, krijon kërkesën dhe pranon kërkesën nga serveri.
- Pranimi përdor kontroll për shoferin pronar dhe vendet pozitive. Statusi ndryshohet dhe një vend zvogëlohet në të njëjtën rrjedhë.
- `GjendjaKerkeses.tsx` rilexon çdo 5 sekonda, anulon lexime të mbivendosura dhe pastrohet kur largohet komponenti.
- Nëse leximi dështon, ndërfaqja ruan vlerat e fundit dhe shfaq paralajmërimin `Po rilidhemi`. Kthehet në internet dhe rilexon menjëherë, pa Refresh manual.

## Provat

ID-ja e udhëtimit të përdorur: Nuk u ekzekutua provë manuale në këtë sesion; plotësohet pas testimit me Neon.
ID-ja e llogarisë udhëtare: Nuk u ekzekutua provë manuale në këtë sesion.
ID-ja e llogarisë shoferit: Nuk u ekzekutua provë manuale në këtë sesion.

### Rasti 1: Dërgo dhe shiko pa Refresh

**Statusi:** Nuk u ekzekutua manualisht në këtë sesion.

Parashikimi: dy llogaritë shohin 2 vende, udhëtari dërgon kërkesën dhe shoferi e sheh `Në pritje` pas rileximit.

Vëzhgimi real: Nuk u vëzhgua manualisht; `.env.local` dhe dy llogaritë testuese nuk ishin të disponueshme në këtë sesion.

### Rasti 2: Prano dhe mbro një rezervim

**Statusi:** Nuk u ekzekutua manualisht në këtë sesion.

Parashikimi: të dy ekranet pas pranimit shfaqin 1 vend dhe `U pranua`. Përsëritja nuk ul vendet sërish dhe me 0 vende kërkesat nuk pranohen.

Vëzhgimi real: Nuk u vëzhgua manualisht; databaza dhe sesionet e roleve nuk ishin të disponueshme për vëzhgim.

### Rasti 3: Offline te udhëtari, pastaj rilidhje

**Statusi:** Nuk u ekzekutua manualisht në këtë sesion.

Parashikimi: udhëtari offline mban pamjen e fundit, shoferi online pranon kërkesën, dhe pas kthimit të rrjetit udhëtari merr `U pranua` dhe 1 vend pa Refresh manual.

ID-ja e udhëtimit të dytë: Nuk u ekzekutua provë manuale në këtë sesion.
Vëzhgimi real: Nuk u vëzhgua manualisht; DevTools Offline dhe dy sesione reale nuk ishin të disponueshme në këtë sesion.

## Skedarët kryesorë

- `src/app/api/udhetimet/[id]/kerkesa/route.ts` — leximi, POST-i i kërkesës dhe PATCH-i i pranimit.
- `src/components/GjendjaKerkeses.tsx` — timer-i, bllokimi i dërgimeve, offline dhe rikthimi i gjendjes.
- `src/app/udhetimi/[id]/kerkesa/page.tsx` — faqja e kërkesës.
- `schema-java10.sql` — tabela dhe kufizimet e kërkesës.

## Kufizimet

Kreditencat reale të GitHub dhe Neon nuk janë të disponueshme në këtë kopje, prandaj tri rastet manuale nuk mund të konfirmohen si ekzekutim në shfletues. Build-i i Next.js kaloi. Pas konfigurimit të `.env.local`, ekzekuto `schema-java10.sql` në Neon dhe kryej tri rastet manuale për të plotësuar ID-të dhe vëzhgimet reale.

## Ndihma nga AI

Përdora AI për skicimin e API-së atomike, UI-së periodike dhe skemës PostgreSQL, si dhe për këtë raport. Verifikova build-in dhe kodin e implementuar; rezultatet manuale dhe të dhënat reale nuk u paraqitën si të kryera pa vëzhgim në shfletues.

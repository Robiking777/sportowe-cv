# Sportowe CV – MVP: bieganie, siłownia, wędkarstwo

Specyfikacja projektu dla Claude Code. Czytaj ten plik na początku każdej sesji i trzymaj się go.

## 1. Produkt w jednym akapicie

Sportowe CV to strona-wizytówka zawodnika pod adresem `[DOMENA]/nazwa`, którą wkleja się do bio na Instagramie i TikToku. Odwiedzający w kilka sekund widzi rekordy, historię startów, najbliższe starty z celem i sprzęt. MVP obsługuje trzy dyscypliny: **bieganie**, **siłownię** (trójbój, ciężary, kalistenika) i **wędkarstwo**. Jedna osoba może mieć kilka dyscyplin i przełączać się między nimi na profilu. Wyniki dzielą się na **nieoficjalne** (trening, zdjęcie) i **zweryfikowane** (z linkiem do oficjalnych wyników zawodów). To NIE jest dziennik treningowy: brak feedu, mapek GPS, czatu, polubień i komentarzy.

Prawie 100% ruchu to telefony po kliknięciu linku w bio. Profil publiczny musi ładować się błyskawicznie.

## 2. Jak ze mną pracować

- Nie jestem programistą. Tłumacz prostym językiem po polsku, bez żargonu albo z krótkim wyjaśnieniem.
- Przed każdą większą zmianą przedstaw plan w punktach i poczekaj na moją akceptację.
- Pracuj małymi krokami. Po każdym kroku: powiedz, co zrobiłeś, jak to sprawdzić na telefonie (adres podglądu z Vercela) i zrób commit z jasnym opisem.
- Gdy potrzebujesz ode mnie czegoś (konto, klucz, ustawienie w panelu Supabase lub Vercela), podaj instrukcję klik po kliku.
- Nie dodawaj funkcji spoza zakresu MVP (sekcja 4). Pomysły zapisuj w pliku `LATER.md`.
- Interfejs po polsku. Kod, nazwy plików, tabel i zmiennych po angielsku.
- Nigdy nie umieszczaj kluczy ani haseł w kodzie. Tylko zmienne środowiskowe (`.env.local`, ustawienia Vercela). `.env.local` musi być w `.gitignore`.

## 3. Technologia

- **Next.js** (App Router, najnowsza stabilna wersja) + **TypeScript** + **Tailwind CSS**.
- **Supabase**: baza Postgres, logowanie (Auth), zabezpieczenia wierszy (RLS), Storage na zdjęcia dowodowe. Projekt w regionie UE (Frankfurt). Integracja przez `@supabase/ssr`.
- **Vercel**: hosting, podgląd każdej zmiany. Region funkcji: Frankfurt (`fra1`).
- **GitHub**: repozytorium.
- Fonty przez `next/font/google`: **Barlow Condensed** (600, 700, 800) do liczb i nagłówków, **Inter** (400, 500, 600, 700) do tekstu.
- Obrazki podglądu linku: `next/og` (`opengraph-image`).
- Statystyki: Vercel Web Analytics.
- Bez dodatkowych ciężkich bibliotek (UI kit, biblioteki wykresów, animacji). Ikony jako proste inline SVG w stylu „stroke”. Kompresję zdjęć rób w przeglądarce przez `<canvas>`, bez zewnętrznych usług.
- Migracje bazy jako pliki SQL w `supabase/migrations/`, żeby dało się je odtworzyć.

## 4. Zakres MVP

| Funkcja | W MVP |
| --- | --- |
| Profil publiczny `/[username]` z awatarem z inicjałów | tak |
| 3 dyscypliny: bieganie, siłownia, wędkarstwo (do 3 na profil) | tak |
| Przełącznik dyscyplin pod bio (ukryty, gdy dyscyplina jest jedna) | tak |
| Rekordy dopasowane do dyscypliny (czas / ciężar i powtórzenia / okaz) | tak |
| Historia startów i połowów z filtrem „Wszystkie / Tylko zweryfikowane” | tak |
| Planowane starty i zawody z odliczaniem dni i celem | tak |
| Weryfikacja: link do oficjalnych wyników lub protokołu, status ustawia admin | tak |
| Dowód: zdjęcie (wgrywane, kompresowane) i link do wideo (Instagram, TikTok, YouTube) | tak |
| Sprzęt: nazwa, kategoria, link, przypisanie do dyscypliny | tak |
| Waga ciała i kategoria wagowa w siłowni (waga domyślnie ukryta) | tak |
| Podgląd linku (Open Graph) z imieniem i rekordami | tak |
| Logowanie linkiem z e-maila (magic link) | tak |
| Panel edycji dla użytkownika | tak |
| Panel admina: tworzenie i edycja profili za zawodników, weryfikacja wyników | tak |

**Poza MVP** (tylko do `LATER.md`): inne dyscypliny (sporty walki, kolarstwo, triathlon, pływanie), wgrywanie wideo, zdjęcie profilowe, wykres progresu (waga, obwody, VO2 max), logowanie Google/Apple, płatności, statystyki odwiedzin profilu, afiliacja, jasny motyw, powiadomienia e-mail, grafika na Stories.

## 5. Wygląd (z makiety)

Tylko ciemny motyw. Mobile-first: projektuj na 390 px szerokości. Na komputerze treść wyśrodkowana, maks. 480 px szerokości.

Kolory (zdefiniuj jako zmienne w Tailwind):

| Token | Wartość | Użycie |
| --- | --- | --- |
| `bg` | `#0A0B0D` | tło strony |
| `surface` | `#15171B` | karty, kafelki |
| `surface-2` | `#1E2126` | ikony, pola, awatar, tło zdjęć |
| `border` | `#23262C` | obramowania kart |
| `border-strong` | `#2E323A` | przyciski obrysowane |
| `text` | `#F4F5F7` | tekst główny |
| `text-2` | `#C5CAD2` | tekst drugorzędny |
| `muted` | `#9BA1AB` | etykiety, opisy |
| `accent` | `#D4FF3A` | akcent: odznaka „zweryfikowany”, wyróżnione liczby, aktywna dyscyplina, główny przycisk |
| `on-accent` | `#0A0B0D` | tekst na akcencie |

Typografia:
- Liczby (wyniki, ciężary, wagi ryb, odliczanie, statystyki) i nagłówki sekcji: Barlow Condensed, 700–800, nagłówki sekcji WIELKIMI LITERAMI, 24 px.
- Imię w nagłówku profilu: Barlow Condensed 800, 38 px, wielkie litery, w dwóch liniach (imię / nazwisko).
- Tekst: Inter 13–15 px. Etykiety kafelków: 11–12 px, wielkie litery, odstęp liter 0.06–0.08em, kolor `muted`.
- Jednostki przy liczbach (`kg`, `cm`) mniejsze i w kolorze `muted`.

Komponenty:
- **Karta**: tło `surface`, obramowanie 1 px `border`, zaokrąglenie 16–20 px.
- **Kafelek rekordu**: etykieta (dystans, bój, gatunek) u góry, wynik dużą liczbą Barlow Condensed, pod spodem jedna linia szczegółów (tempo, data, „zawody: 220”). Cały kafelek jest linkiem do szczegółów wyniku.
- **Odznaka zweryfikowany**: kółko 18–22 px w kolorze `accent` z ciemnym „ptaszkiem”; w listach pigułka „Zweryfikowany” (tło `accent`, tekst `on-accent`, 11 px, pogrubiony).
- **Pigułka nieoficjalna**: obrys 1 px `#3A3F47`, tekst `text-2`, 11 px. Napis zależy od dyscypliny: „Trening” (bieganie, siłownia) albo „Nieoficjalny” (wędkarstwo). Ikona wideo lub aparatu, gdy wynik ma dowód.
- **Pigułka PB**: obrys i tekst w kolorze `accent`.
- **Przełącznik dyscyplin**: pigułki 40 px wysokości z ikoną i nazwą. Aktywna: tło i obrys `accent`, tekst `on-accent`. Nieaktywna: tło `surface`, obrys `border-strong`. Element `<nav aria-label="Dyscypliny">`, aktywna z `aria-current="page"`.
- **Chipy** (podkategorie, np. „Maraton”, „Kategoria do 93 kg”, „Złów i wypuść”): wysokość 30 px, tło `surface`, obrys `border`.
- **Zdjęcie dowodowe**: w karcie, `object-fit: cover`, etykieta w lewym górnym rogu („Zdjęcie na macie”) na półprzezroczystym ciemnym tle.
- **Przyciski i linki dotykowe**: min. 44 × 44 px. Główny przycisk: tło `accent`, tekst `on-accent`, zaokrąglenie 14 px.
- **Logo**: mały skośny kwadrat w kolorze `accent` + napis „SPORTOWE CV” (Barlow Condensed 800, odstęp liter 0.06em).
- Bez gradientów, cieni i emoji.

Zrzuty ekranów makiety leżą w folderze `design/` (ekran telefonu 390 px, zapisany w podwójnej rozdzielczości):

- `profil-bieganie-1.png` … `profil-bieganie-4.png` – profil biegaczki od góry do dołu; kolejne części nachodzą na siebie o kilka linii,
- `profil-wedkarstwo-1.png` … `profil-wedkarstwo-3.png` – ten sam profil w zakładce wędkarstwo,
- `rekord.png` – szczegóły wyniku,
- `szablon-silownia.png` – profil trójboisty,
- `szablon-wedkarz.png` – profil wędkarza z jedną dyscypliną (bez przełącznika).

Odwzoruj je możliwie wiernie. Pomiń elementy spoza MVP widoczne na zrzutach: sekcję „Progres” z wykresem i napis „Linki partnerskie” przy sprzęcie. Dane na zrzutach to dane demo z sekcji 13.

## 6. Dyscypliny

Dyscypliny opisz w jednym pliku konfiguracyjnym `lib/disciplines.ts`. Dodanie kolejnej dyscypliny w przyszłości ma polegać głównie na dopisaniu wpisu do tego pliku. Każdy wpis: `slug`, nazwa, ikona, typ wyniku, lista konkurencji, etykiety sekcji, statystyki, napis dla wyniku nieoficjalnego, podpowiedzi chipów.

| | Bieganie | Siłownia | Wędkarstwo |
| --- | --- | --- | --- |
| `slug` w adresie | `bieganie` | `silownia` | `wedkarstwo` |
| Typ wyniku | `time` – mniej = lepiej | `weight_reps` – więcej = lepiej | `catch` – więcej = lepiej |
| Konkurencje | 5 km, 10 km, półmaraton, maraton, inny dystans (km) | przysiad, wyciskanie leżąc, martwy ciąg, total (trójbój), rwanie, podrzut, podciąganie (powtórzenia), inny bój | karp, amur, szczupak, sandacz, okoń, sum, leszcz, pstrąg, inny gatunek |
| Pola wyniku | czas, kategoria, miejsce w kategorii, miejsce open | ciężar (kg), powtórzenia (domyślnie 1), waga ciała w dniu startu (opcjonalnie) | waga (kg, 1 miejsce po przecinku) i/lub długość (cm), metoda (karpiowa, spinning, spławik, feeder, mucha), region (np. „woj. warmińsko-mazurskie”), wypuszczony tak/nie |
| Rekord (PB) | najlepszy czas na dystansie | największy ciężar przy 1 powtórzeniu; podciąganie: najwięcej powtórzeń | najcięższa ryba danego gatunku; bez wagi – najdłuższa |
| Sekcja rekordów | „Rekordy życiowe”: siatka 2 × 2 (5 km, 10 km, półmaraton, maraton), inne dystanse pod spodem | „Rekordy życiowe”: Total na całą szerokość, pod nim 3 kafelki (przysiad, wyciskanie, martwy ciąg), reszta pod spodem | „Gablota rekordów”: najcięższa ryba jako duża karta ze zdjęciem, pozostałe gatunki jako kafelki 3 w rzędzie |
| Statystyki (3 kolumny) | lata stażu, starty, zweryfikowane | kategoria wagowa lub waga ciała, lata stażu, starty | lata z wędką, zawody, gatunki |
| Wynik nieoficjalny | „Trening” | „Trening” | „Nieoficjalny” |
| Dodatkowe w profilu dyscypliny | – | kategoria wagowa (np. „do 93 kg”), waga ciała + przełącznik „pokaż publicznie” (domyślnie wyłączony) | – |
| Nagłówek historii | „Historia startów” | „Historia startów” | „Historia połowów i zawodów” |
| Nagłówek startów | „Najbliższe starty” | „Najbliższe starty” | „Najbliższe zawody” |

Szczegóły:
- Wyniki z więcej niż 1 powtórzeniem w siłowni pokazuj w historii jako „200 kg × 3”, ale nie biorą udziału w rekordzie ciężaru.
- Total w trójboju zawodnik wpisuje jako osobny wynik z zawodów. Nie sumuj automatycznie najlepszych bojów z różnych dni.
- Tempo (tylko bieganie) = czas / km, format `m:ss /km`. Dystanse: 5, 10, 21,0975, 42,195 km.
- Lokalizacja łowiska jest zawsze na poziomie regionu, nigdy dokładne miejsce. Wędkarze chronią swoje łowiska.

## 7. Ekrany i adresy

### Publiczne
- `/` – strona główna: krótki nagłówek („Twoje wyniki. Jeden link w bio.”), przykładowe profile demo, przycisk „Załóż swoje Sportowe CV”, linki do regulaminu i polityki prywatności.
- `/[username]` – profil w pierwszej dyscyplinie zawodnika. `/[username]/[slug-dyscypliny]` – profil w wybranej dyscyplinie. Sekcje w tej kolejności:
  1. Pasek górny: logo + przycisk „Udostępnij” (Web Share API, w przeciwnym razie skopiuj link).
  2. Nagłówek osoby (wspólny dla wszystkich dyscyplin): awatar z inicjałów (88 px, pierścień w kolorze `accent`), imię i nazwisko, `@username`, miasto, bio (maks. 200 znaków).
  3. Przełącznik dyscyplin (tylko przy 2+ dyscyplinach). Kliknięcie zmienia adres na `/[username]/[slug]` bez przeładowania całej strony.
  4. Od tego miejsca wszystko dotyczy wybranej dyscypliny: chipy, statystyki, rekordy (sekcja 6), najbliższe starty, historia, sprzęt.
  5. **Najbliższe starty / zawody**: pierwszy jako duża karta (data, miejsce, nazwa, odliczanie dni dużą liczbą w kolorze `accent`, cel), kolejne jako kompaktowe wiersze. Tylko daty dziś lub później. W bieganiu dodatkowo „Obecna życiówka X – do celu brakuje Y”.
  6. **Historia**: przełącznik „Wszystkie / Tylko zweryfikowane”, grupowanie po latach, wiersz: dzień i miesiąc, nazwa zawodów (lub „Trening”, „Zasiadka”), jedna linia szczegółów, pigułki statusu i PB, wynik po prawej. Najpierw 5 ostatnich, przycisk „Pokaż całą historię”.
  7. **Mój sprzęt**: sprzęt przypisany do tej dyscypliny lub do wszystkich. Wiersz: ikona kategorii, nazwa, opis, przycisk linku zewnętrznego.
  8. Stopka: logo, „Wyniki, rekordy i historia startów. Jeden link w bio.”, przycisk „Stwórz swoje Sportowe CV” → `/`.
- `/[username]/wynik/[id]` – szczegóły wyniku: dowód na górze (zdjęcie na pełną szerokość albo karta wideo z przyciskiem „Odtwórz” otwierającym link w nowej karcie; dla YouTube miniatura z `img.youtube.com`), konkurencja + PB, wynik bardzo dużą liczbą, linia szczegółów (bieganie: tempo i różnica do poprzedniej życiówki; siłownia: powtórzenia i waga ciała, jeśli publiczna; wędkarstwo: długość, metoda, wypuszczony), karta weryfikacji z linkiem „Oficjalne wyniki”, szczegóły (data, zawody, kategoria i miejsce, sprzęt), „Droga do rekordu” – kolejne rekordy w tej konkurencji z latami.
- `/regulamin`, `/prywatnosc` – strony z tekstem zastępczym oznaczonym wyraźnie „[DO UZUPEŁNIENIA PRZEZ WŁAŚCICIELA]”. Nie wymyślaj treści prawnych.
- Strona 404 w stylu aplikacji.

### Dla zalogowanych
- `/login` – pole e-mail, wysyłka linku logowania.
- `/onboarding` – po pierwszym logowaniu: nazwa profilu (sprawdzanie dostępności na żywo), imię i nazwisko, miasto, bio, wybór 1–3 dyscyplin (kolejność = kolejność w przełączniku), rok rozpoczęcia dla każdej; obowiązkowe zgody: regulamin i polityka prywatności, oświadczenie „mam co najmniej 16 lat”.
- `/panel` – na górze wybór dyscypliny (ten sam przełącznik), pod nim zakładki: **Wyniki**, **Starty**, **Sprzęt**, **Profil**. Linki „Zobacz mój profil” i „Kopiuj link do bio”. W zakładce Profil: dane osoby, dodawanie i usuwanie dyscyplin, kolejność, chipy, a w siłowni kategoria wagowa i waga ciała z przełącznikiem widoczności.
- Formularz wyniku zależny od typu (sekcja 6). Wspólne pola: data, nazwa zawodów (puste = trening lub zasiadka), link do oficjalnych wyników lub protokołu, zdjęcie, link do wideo, sprzęt z listy. Czas wpisywany jako `18:32` lub `3:04:55`. Dodanie wyniku ma zająć mniej niż minutę.
- Po dacie zaplanowanego startu w panelu pojawia się karta „Dodaj wynik z [nazwa]” z formularzem wypełnionym danymi startu.

### Admin
- `/admin` – dostęp tylko dla e-maili z tabeli `admins`.
  - **Do weryfikacji**: wyniki ze statusem `pending` ze wszystkich dyscyplin, podgląd zdjęcia, link do oficjalnych wyników, przyciski „Zweryfikuj” i „Odrzuć”.
  - **Profile**: lista profili, „Utwórz profil za zawodnika” (bez konta, z polem `claim_email`), edycja dowolnego profilu tymi samymi formularzami co w panelu, usuwanie zdjęć.

## 8. Zdjęcia dowodowe

- Supabase Storage, bucket `proofs`, publiczny odczyt, ścieżka `{profile_id}/{result_id}.webp`.
- Przed wysłaniem kompresja w przeglądarce: dłuższy bok maks. 1600 px, WebP (zapasowo JPEG), jakość ok. 0,8, wynik maks. 1,5 MB. Przyjmuj tylko obrazy (JPEG, PNG, WebP, HEIC, jeśli przeglądarka umie go odczytać).
- Ponowne zakodowanie przez `<canvas>` usuwa dane EXIF, w tym położenie GPS. To wymóg, nie dodatek: zdjęcie nie może zdradzać łowiska ani adresu.
- Jedno zdjęcie na wynik. Usunięcie wyniku usuwa zdjęcie.
- Zapis do bucketa tylko dla właściciela profilu i admina (polityki Storage).
- Na profilu zdjęcia ładowane leniwie (`loading="lazy"`), oprócz karty najcięższej ryby.

## 9. Model danych

```
profiles
  id uuid pk default gen_random_uuid()
  owner_user_id uuid null references auth.users  -- null = profil założony przez admina
  claim_email text null                         -- e-mail osoby, która przejmie profil
  username text unique not null                 -- małe litery, a–z 0–9 . _ -, 3–30 znaków
  display_name text not null
  city text null
  bio text null (max 200)
  is_demo boolean default false
  created_at, updated_at timestamptz

profile_disciplines
  id uuid pk, profile_id references profiles on delete cascade
  discipline text check in ('bieganie','silownia','wedkarstwo')
  position int                                  -- kolejność w przełączniku
  since_year int null
  chips text[] default '{}'
  weight_class text null                        -- siłownia, np. 'do 93 kg'
  body_weight_kg numeric null                   -- siłownia
  body_weight_public boolean default false
  unique (profile_id, discipline)

results
  id uuid pk, profile_id references profiles on delete cascade
  discipline text                               -- jak wyżej
  event_key text not null                       -- np. '5k', 'squat', 'carp', 'other'
  event_label text null                         -- dla 'other': np. '15 km', 'Wyciskanie żołnierskie', 'Węgorz'
  -- typ time
  time_seconds int null
  distance_km numeric null                      -- tylko dla 'other' w bieganiu
  -- typ weight_reps
  weight_kg numeric null
  reps int null
  body_weight_kg numeric null
  -- typ catch
  length_cm numeric null                        -- waga ryby trafia do weight_kg
  method text null
  water_region text null
  released boolean null
  -- wspólne
  date date not null
  event_name text null                          -- null = trening / zasiadka
  category text null
  place_category int null
  place_open int null
  status text check in ('unofficial','pending','verified','rejected')
  official_url text null
  video_url text null
  photo_path text null
  gear_id uuid null references gear on delete set null
  created_at, updated_at
  -- check: wymagane pola zależnie od discipline (time_seconds dla biegania; weight_kg lub reps dla siłowni; weight_kg lub length_cm dla wędkarstwa)

events                                           -- planowane starty i zawody
  id uuid pk, profile_id, discipline, name, date, city null,
  event_key text null, goal_seconds int null, goal_text text null   -- np. 'Total 560 kg', 'Karp 20+ kg'

gear
  id uuid pk, profile_id, discipline text null (null = wszystkie),
  name, category text check in ('shoes','watch','rod','reel','belt','other'),
  description text null, url text null

admins
  email text pk
```

Reguły:
- Status przy zapisie przez użytkownika: jest `official_url` → `pending`, brak → `unofficial`. Tylko admin ustawia `verified` lub `rejected`. Wymuś to triggerem w bazie, nie tylko w interfejsie. Zmiana wartości wyniku lub `official_url` w wyniku zweryfikowanym przywraca `pending`.
- Rekord (PB) liczony w zapytaniu według reguł z sekcji 6, wśród wyników o statusie innym niż `rejected`. Nie zapisuj go osobno. Kafelek pokazuje odznakę statusu wyniku-rekordu.
- Pigułka PB w historii: wynik, który w chwili swojej daty był najlepszy w tej konkurencji.
- Statystyki liczone per dyscyplina: lata = bieżący rok − `since_year`; starty / zawody = liczba wyników z `event_name` i statusem innym niż `rejected`; zweryfikowane = liczba `verified`; gatunki = liczba różnych `event_key` w wędkarstwie.
- Przejęcie profilu: gdy użytkownik loguje się e-mailem równym `claim_email` profilu bez właściciela, ustaw `owner_user_id` i pomiń onboarding.
- Zarezerwowane nazwy profili: `admin, panel, login, onboarding, api, wynik, regulamin, prywatnosc, auth, _next, demo, www, app, help, pomoc, bieganie, silownia, wedkarstwo`.

## 10. Bezpieczeństwo (RLS)

- Włącz RLS na wszystkich tabelach.
- Odczyt publiczny: `profiles`, `profile_disciplines`, `events`, `gear`, `results` ze statusem innym niż `rejected`. Niepubliczne: `claim_email` oraz `body_weight_kg`, gdy `body_weight_public = false` (użyj widoków albo osobnych kolumn/tabel, nie ukrywaj tylko w interfejsie).
- Zapis: tylko właściciel (`owner_user_id = auth.uid()`) albo admin. To samo dla bucketa `proofs`.
- Funkcja `is_admin()`: sprawdza, czy e-mail z tokenu (`auth.jwt() ->> 'email'`) jest w tabeli `admins`. Tabela `admins` niedostępna z przeglądarki.
- Linki od użytkowników (`official_url`, `video_url`, `url` sprzętu) akceptuj tylko z `https://`. Zewnętrzne linki otwieraj z `rel="noopener noreferrer nofollow"`.
- Po każdym etapie przejdź przez test: drugie konto nie może zmienić cudzego profilu, wgrać zdjęcia do cudzego folderu ani nadać sobie statusu `verified`.

## 11. Formatowanie po polsku

- Daty: `11 paź 2026`; w historii dzień dużą liczbą i skrót miesiąca: sty, lut, mar, kwi, maj, cze, lip, sie, wrz, paź, lis, gru.
- Liczby dziesiętne z przecinkiem: `42,195 km`, `24,0 kg`, `92,1 kg`.
- Ciężary: `190 kg`, `200 kg × 3`, podciąganie `15 powt.`. Ryby: `19,4 kg`, `96 cm`.
- Odliczanie: „1 dzień”, „N dni do startu” / „N dni do zawodów”, „start dziś”. Liczone w strefie Europe/Warsaw.
- Czasy: `18:32`, `1:24:10`, `3:04:55`.

## 12. Wydajność i SEO

- Profil publiczny renderowany na serwerze, z odświeżaniem po zmianie danych (`revalidatePath` po zapisie w panelu i adminie).
- Cel: profil otwiera się na telefonie w mniej niż 2 sekundy.
- Metadane: tytuł „Imię Nazwisko – Sportowe CV”, opis z rekordami wybranej dyscypliny.
- `opengraph-image` profilu 1200 × 630: logo, imię, do 4 rekordów pierwszej dyscypliny (lub wybranej, dla adresu z dyscypliną), kolory z makiety.

## 13. Dane demo

Seed z dwoma profilami demo (`is_demo = true`), zgodnymi z makietą. Linki zastąp adresami `https://example.com/...`. Zdjęć w demo nie ma: pokaż szary prostokąt z podpisem „Zdjęcie dowodowe”.

**`marta`** – Marta Kowalczyk, Warszawa, bio: „Biegam od 2017 roku, a ryby łowię od dziecka. W 2027 chcę złamać 3 godziny w maratonie.” Dyscypliny: bieganie (od 2017), wędkarstwo (od 2008).
- Bieganie, wyniki: 5 km 18:32 (Grand Prix Warszawy, 17.05.2026, K30 2. miejsce, zweryfikowany), 10 km 38:47 (trening, 14.06.2026), półmaraton 1:24:10 (Półmaraton Warszawski, 29.03.2026, K30 5. miejsce, zweryfikowany), maraton 3:04:55 (Poznań Maraton, 11.10.2026, K30 7. miejsce, open kobiet 41., zweryfikowany, z linkiem do wideo), 10 km 39:02 (Bieg Niepodległości, 11.11.2026, K30 4. miejsce, zweryfikowany), maraton 3:11:20 (2025) i 3:18:02 (2024).
- Bieganie, starty: Półmaraton Warszawski 28.03.2027 cel < 1:22:00; Maraton Warszawski 26.09.2027 cel < 3:00:00.
- Bieganie, sprzęt: Nike Vaporfly 3 (startówki), Garmin Forerunner 965 (zegarek, wszystkie dyscypliny), ASICS Novablast 4 (treningowe).
- Wędkarstwo, wyniki: karp 19,4 kg (zawody karpiowe 72 h, 13.09.2026, drużynowo 3. miejsce, wypuszczony, zweryfikowany), amur 14,2 kg (Otwarcie sezonu, 24.05.2026, 5. miejsce, zweryfikowany), szczupak 96 cm (zasiadka, 19.07.2026, nieoficjalny), okoń 44 cm (spinning, 12.04.2026, nieoficjalny).
- Wędkarstwo, zawody: Zawody karpiowe 48 h, 21.05.2027, cel „Karp 20+ kg”.
- Wędkarstwo, sprzęt: wędka karpiowa 12 ft 3 lb, kołowrotek z wolnym biegiem, mata i waga cyfrowa.

**`kuba`** – Kuba Nowak, Kraków, bio: „Trójbój klasyczny od 2019 roku.” Dyscyplina: siłownia (od 2019), chipy: „Trójbój klasyczny”, „Kategoria do 93 kg”, waga ciała 92,1 kg (publiczna).
- Wyniki: total 540 kg, przysiad 190 kg, wyciskanie leżąc 130 kg i martwy ciąg 220 kg (Mistrzostwa Małopolski, 20.09.2026, zweryfikowane); martwy ciąg 225 kg (trening, 18.10.2026, z linkiem do wideo).
- Start: Grand Prix Krakowa, 14.03.2027, cel „Total 560 kg”.
- Sprzęt: pas 10 mm, opaski na kolana, buty do przysiadu.

## 14. Etapy budowy

Każdy etap kończy się listą rzeczy do sprawdzenia przeze mnie na telefonie.

1. **Etap 1 – szkielet**: projekt Next.js + Tailwind z tokenami kolorów i fontami, połączenie z Supabase, `lib/disciplines.ts`, migracje (tabele, RLS, trigger statusu, `is_admin()`, bucket `proofs` z politykami), logowanie linkiem z e-maila, onboarding z wyborem dyscyplin, zarezerwowane nazwy, seed demo, wdrożenie na Vercel.
2. **Etap 2 – profil publiczny**: `/[username]` i `/[username]/[slug]` z przełącznikiem, sekcje rekordów dla trzech typów wyniku według makiety, `/[username]/wynik/[id]`, podgląd linku (OG), strona główna z demo, 404. Panel admina: tworzenie i edycja profili za zawodników.
3. **Etap 3 – panel użytkownika**: zakładki Wyniki / Starty / Sprzęt / Profil z wyborem dyscypliny, formularze dla trzech typów wyniku, wgrywanie zdjęć z kompresją i usuwaniem EXIF, karta „Dodaj wynik z …”, przejmowanie profilu przez `claim_email`, kolejka weryfikacji w adminie.
4. **Etap 4 – szlify**: strony regulaminu i prywatności (tekst zastępczy), Vercel Analytics, przegląd bezpieczeństwa RLS i Storage z listą problemów, sprawdzenie wydajności profilu na telefonie, porządek w `LATER.md`.

## 15. Gotowe, gdy

- Mogę założyć konto, wybrać nazwę i dwie dyscypliny, a w 5 minut zbudować profil z rekordami, startem i sprzętem w każdej z nich.
- Profil pod `/nazwa` wygląda jak makieta na telefonie, a przełącznik dyscyplin działa i zmienia adres.
- Biegacz widzi siatkę życiówek, trójboista total i trzy boje, wędkarz gablotę z najcięższą rybą na zdjęciu.
- Wgrane zdjęcie jest skompresowane i nie zawiera danych GPS.
- Wklejony link pokazuje podgląd z rekordami.
- Jako admin tworzę profil za zawodnika, a on przejmuje go, logując się swoim e-mailem.
- Wynik z linkiem do oficjalnych wyników trafia do kolejki, a po mojej akceptacji ma odznakę „Zweryfikowany”.
- Drugie konto nie może edytować cudzych danych, wgrać zdjęcia do cudzego folderu ani nadać sobie weryfikacji.

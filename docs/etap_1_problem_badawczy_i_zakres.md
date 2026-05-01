# Etap 1: Problem badawczy i zakres pracy

Tytul pracy: **Behavioral Work Continuity Index: Development of a Measure and Comparison of Machine Learning Models**

## 1. Stale zalozenia pracy

Praca dotyczy **behawioralnej ciągłości pracy** i **ryzyka jej przerwania**, a nie bezposredniego pomiaru skupienia, koncentracji, uwagi ani stanu psychicznego uzytkownika.

Dane BEHACOM nie zawieraja twardej etykiety psychologicznej typu `focused / distracted`. Z tego powodu zmienna docelowa musi byc traktowana jako **proxy**: mierzalne przyblizenie widocznych w telemetrii wzorcow zachowania, ktore moga wskazywac na stabilnosc lub pogorszenie ciaglosci pracy.

Glowna wartosc pracy ma wynikac z:

- konstrukcji indeksu Behavioral Work Continuity Index,
- uzasadnienia i walidacji przyjetej operacjonalizacji,
- porownania modeli uczenia maszynowego,
- analizy stabilnosci wynikow w czasie i miedzy uzytkownikami,
- interpretacji sygnalow behawioralnych stojacych za predykcja.

## 2. Cel glowny

Celem pracy jest opracowanie behawioralnego indeksu ciaglosci pracy oraz porownanie modeli uczenia maszynowego, ktore na podstawie danych telemetrycznych z komputera estymuja i przewiduja epizody pogorszenia ciaglosci pracy uzytkownika w krotkim horyzoncie czasowym.

Zakladany horyzont predykcji: **5-15 minut**.

Dane bazowe: **BEHACOM**, czyli minutowo agregowane dane dotyczace aktywnosci klawiatury, myszy, uzycia aplikacji oraz podstawowych metryk systemowych.

## 3. Problem badawczy

Glowny problem badawczy:

> Czy na podstawie behawioralnych danych telemetrycznych uzytkownika komputera mozna zbudowac spojny indeks ciaglosci pracy oraz przewidywac epizody pogorszenia tej ciaglosci w krotkim horyzoncie czasowym?

Problem nie zaklada, ze model wykrywa rzeczywisty stan psychiczny uzytkownika. Zaklada jedynie, ze obserwowalne sygnaly, takie jak przerwy w interakcji, przelaczanie aplikacji, zmiennosc rytmu pracy oraz dynamika wejscia klawiatura-mysz, moga tworzyc uzyteczne proxy ciaglosci pracy.

## 4. Pytania badawcze

RQ1. Ktore grupy cech behawioralnych najlepiej opisuja ciaglosc pracy uzytkownika?

RQ2. Czy mozna zbudowac Behavioral Work Continuity Index, ktory jest spojny, stabilny i interpretowalny jako proxy ciaglosci pracy?

RQ3. Czy modele uczenia maszynowego potrafia przewidywac pogorszenie ciaglosci pracy w horyzoncie 5-15 minut lepiej niz proste reguly bazowe?

RQ4. Ktore modele sa najbardziej adekwatne dla tego problemu: modele liniowe, modele tablicowe typu Random Forest / XGBoost / LightGBM, czy modele sekwencyjne?

RQ5. Czy modele trenowane globalnie uogolniaja sie na nowych uzytkownikow, czy konieczna jest personalizacja?

RQ6. Jak wrazliwe sa wyniki na definicje indeksu, dlugosc okna, prog zdarzenia oraz horyzont predykcji?

RQ7. Czy wskazania indeksu i modeli sa zgodne z zewnetrznym punktem odniesienia z malej walidacji wlasnej, takiej jak experience sampling albo kontrolowane przerwania?

## 5. Definicje robocze

### Behavioral Work Continuity Index

Behavioral Work Continuity Index, dalej **BWCI**, to liczbowy indeks opisujacy ciaglosc pracy widoczna w danych telemetrycznych. Indeks powinien laczyc sygnaly dotyczace:

- ciaglosci interakcji, np. idle time, dlugosc przerw, stabilnosc aktywnosci,
- stabilnosci kontekstu, np. liczba przelaczen aplikacji, czas w aplikacji dominujacej, entropia aplikacji,
- dynamiki wejscia, np. tempo klawiatury i myszy, zmiennosc rytmu, wzorce korekt.

BWCI nie jest miara produktywnosci ani psychologicznego skupienia. Jest operacyjna miara ciaglosci zachowania przy komputerze.

### Epizod pogorszenia ciaglosci pracy

Epizod pogorszenia ciaglosci pracy to zdarzenie wyznaczone na podstawie przyszlej wartosci BWCI lub zmiany BWCI w zadanym horyzoncie predykcji.

Przykladowe warianty definicji:

- BWCI spada ponizej ustalonego progu,
- BWCI spada wzgledem wartosci bazowej o co najmniej ustalona wartosc,
- segmentacja stanu wskazuje przejscie do stanu `disrupted`,
- metoda detekcji punktow zmiany wykrywa istotne pogorszenie wzorca pracy.

Wariant podstawowy powinien byc prosty i interpretowalny. Warianty latentne lub change-point detection sa rozszerzeniem, jezeli czas i wyniki podstawowe to uzasadnia.

## 6. Zakres minimalny

1. Przygotowanie i audyt danych BEHACOM.
2. Definicja BWCI jako zmiennej proxy.
3. Definicja zadania regresji BWCI oraz klasyfikacji epizodu pogorszenia ciaglosci pracy.
4. Inzynieria cech czasowych w oknach rolling.
5. Modele bazowe: reguly progowe, Linear Regression, Logistic Regression.
6. Modele tablicowe: Random Forest oraz jeden model gradient boosting, preferencyjnie LightGBM albo XGBoost.
7. Ewaluacja regresji: MAE, RMSE, korelacja, zgodnosc trendu.
8. Ewaluacja klasyfikacji: Precision, Recall, F1, PR-AUC.
9. Walidacja czasowa i leave-one-user-out.
10. Analiza wrazliwosci na okno, prog i horyzont predykcji.
11. Interpretacja cech przez permutation importance albo SHAP.
12. Krotka walidacja zewnetrzna na wlasnych danych

## 7. Zakres rozszerzony


- porownanie kilku definicji etykiety: heurystyka, HMM, change-point detection,
- modele sekwencyjne: LSTM, GRU albo TCN,
- metody nienadzorowane: Isolation Forest albo One-Class SVM,
- demonstrator w Streamlit lub FastAPI,
- bardziej rozbudowana aplikacja do zbierania walidacji zewnetrznej.

## 8. Glowne ryzyka metodologiczne

### Brak ground truth

BEHACOM nie informuje, czy uzytkownik byl skupiony, rozproszony ani jakie zadanie wykonywal. Odpowiedz: praca nie twierdzi, ze mierzy skupienie. Mierzy proxy behawioralnej ciaglosci pracy i waliduje je posrednio oraz zewnetrznie.

### Arbitralnosc indeksu

BWCI moze zostac uznany za sztuczny, jesli bedzie oparty wylacznie na arbitralnych wagach. Odpowiedz: trzeba porownac warianty definicji, przeprowadzic analize wrazliwosci i opisac uzasadnienie kazdego komponentu.

### Cyrkularnosc modelowania

Jesli model przewiduje BWCI z tych samych cech, z ktorych BWCI zostal bezposrednio policzony, wyniki moga byc trywialne. Odpowiedz: nalezy przewidywac przyszly BWCI lub przyszle zdarzenie, pilnowac separacji czasowej oraz raportowac baseline autoregresyjny.

### Przeuczenie pod uzytkownikow

Zachowania moga byc silnie osobnicze. Odpowiedz: obowiazkowo porownac scenariusz globalny, personalizowany i leave-one-user-out.

### Niezbalansowanie klas

Epizody pogorszenia ciaglosci pracy moga byc rzadkie. Odpowiedz: stosowac PR-AUC, Precision, Recall i F1 zamiast accuracy jako glownej metryki.

### Prywatnosc

Dane o aktywnosci uzytkownika moga byc wrazliwe nawet bez tresci wpisywanych znakow. Odpowiedz: minimalizacja danych, pseudonimizacja, praca na agregatach i jasne ograniczenia interpretacyjne.

## 9. Kryteria sukcesu Etapu 1

Etap 1 uznajemy za zakonczony, jezeli:

- zatwierdzony tytul jest zapisany jako obowiazujacy,
- problem badawczy jest sformulowany bez odnoszenia sie do pomiaru stanu psychicznego,
- pytania badawcze pokrywaja konstrukcje indeksu, modelowanie, walidacje i generalizacje,
- zakres minimalny jest oddzielony od rozszerzen,
- znane ryzyka metodologiczne sa jawnie nazwane,
- nastepny etap moze przejsc do organizacji repozytorium i danych.

## 10. Podsumowanie i ustalenia

Na potrzeby dalszej realizacji przyjmujemy:

- nazwa indeksu w pracy: **Behavioral Work Continuity Index**,
- skrot roboczy: **BWCI**,
- termin dla zdarzenia: **work continuity deterioration event** albo **work disruption event**; wybor finalnej nazwy zostanie doprecyzowany przy definicji etykiet,
- podstawowy horyzont predykcji: 5, 10 i 15 minut,
- podstawowe okna historii: 10, 20 i 30 minut,
- modele sekwencyjne i HMM pozostaja rozszerzeniem, nie warunkiem ukonczenia pracy.

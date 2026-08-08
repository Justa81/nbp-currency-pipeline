# 📈 NBP Currency ELT Pipeline & BigQuery Analytics

Projekt typu **ELT (Extract, Load, Transform)** napisany w Pythonie i SQL, który pobiera dane finansowe z REST API NBP, zasila hurtownię danych Google BigQuery, a następnie przeprowadza transformację i analizę zmienności kursów walut.

## 🛠️ Użyte technologie
* **Python 3.12.3** (`pandas`, `requests`, `google-cloud-bigquery`, `db-dtypes`)
* **REST API NBP** (źródło danych)
* **Google Cloud Platform (GCP)** – BigQuery (chmurowa hurtownia danych)
* **SQL (ANSI SQL / BigQuery)** – funkcje okienkowe (`LAG`), podzapytania `WITH (CTE)`
* **Jupyter Notebook**

## 🚀 Architektura i Przepływ Danych (ELT)
1. **Extract & Load (Python):** Skrypt pobiera dzienne kursy EUR oraz USD za cały lipiec z API NBP, tworzy spójną ramkę danych i ładuje surowe dane (`WRITE_TRUNCATE`) do tabeli `kursy_lipiec_surowe` w BigQuery.
2. **Transform (BigQuery SQL):** Tworzony jest trwały widok analityczny `widok_analiza_lipiec`, który w chmurze wylicza:
   * Kurs krzyżowy EUR/USD,
   * Dzienne zmiany procentowe kursów przy użyciu funkcji okienkowej `LAG() OVER (ORDER BY data)`,
   * Relację dynamiki obu walut dzień do dnia (`CASE WHEN`).
3. **Reporting (Python / BI):** Przeliczone dane są odczytywane bezpośrednio z widoku BigQuery do notatnika Jupytera.

## 🔒 Bezpieczeństwo
Dostęp do GCP odbywa się za pomocą konta usługowego (Service Account). Klucz autoryzacyjny JSON jest ukryty w pliku `.gitignore` i nie trafia do publicznego repozytorium.

## 📊 Wyniki analizy

Na podstawie danych pobranych z API NBP przeanalizowano dzienne zmiany kursów EUR i USD oraz relację pomiędzy obiema walutami.

Analiza obejmuje:

1. Zmienność dziennych kursów EUR i USD,
2. Największe dzienne wzrosty i spadki,
3. Zmiany procentowe kursów z wykorzystaniem funkcji LAG(),
4. Kształtowanie się kursu krzyżowego EUR/USD,
5. Porównanie dynamiki zmian obu walut w analizowanym okresie.

Wyniki analizy zostały przedstawione w formie tabel i wizualizacji w Jupyter Notebook.

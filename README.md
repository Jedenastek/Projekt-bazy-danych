# Sklep internetowy Cubezilla.pl

## Opis projektu
**Cubezilla.pl** to internetowy sklep oferujący szeroki wybór łamigłówek logicznych. Projekt obejmuje stworzenie systemu informatycznego z bazą danych, umożliwiającego obsługę sprzedaży, monitorowanie zamówień oraz zarządzanie opiniami klientów.

## Cele projektu
Celem systemu jest zapewnienie wygodnej i efektywnej sprzedaży produktów, umożliwienie klientom śledzenia zamówień oraz gromadzenie opinii o produktach. System jest zgodny z zasadą **SMART**:
- **S**konkretyzowany – obejmuje konkretne funkcjonalności e-commerce,
- **M**ierzalny – pozwala na analizę zamówień, płatności i opinii klientów,
- **A**osiągalny – możliwy do wdrożenia w praktyce,
- **R**ealistyczny – zgodny z wymaganiami rynkowymi,
- **T**erminowy – możliwy do wdrożenia w określonym czasie.

## Wymagania funkcjonalne
### Dla klienta:
- Rejestracja/logowanie (e-mail, Google, Facebook).
- Przeglądanie i wyszukiwanie produktów według kategorii i producentów.
- Dodawanie produktów do koszyka i składanie zamówień.
- Wybór metody płatności (BLIK, karta, przelew, PayPal).
- Wybór sposobu dostawy (kurier, paczkomat, odbiór osobisty).
- Śledzenie statusu zamówienia.
- Wystawianie ocen i recenzji.
- Kontakt z obsługą klienta.

### Dla administratora:
- Logowanie do systemu (dwustopniowa autoryzacja).
- Zarządzanie użytkownikami i ich zamówieniami.
- Zarządzanie produktami i kategoriami.
- Moderowanie komentarzy i recenzji.
- Kontrola stanów magazynowych.

### Dla pracownika:
- Dodawanie nowych produktów.
- Aktualizacja dostępności produktów.
- Obsługa zgłoszeń technicznych.

## Model bazy danych
### Główne encje:
- **Klient** (ID, e-mail, hasło, historia zamówień).
- **Koszyk** (ID, klient, produkty).
- **Płatność** (ID, metoda, kwota, termin).
- **Dostawa** (ID, metoda, adres dostawy).
- **Zamówienie** (ID, klient, produkty, płatność, dostawa, status).
- **Produkt** (ID, nazwa, producent, kategoria, cena, promocja).
- **Recenzja** (ID, klient, produkt, ocena, komentarz).
- **Stan zamówienia** (ID, zamówienie, status).

### Relacje między encjami:
- **Zamówienie** ← **Klient** (każde zamówienie przypisane do klienta).
- **Koszyk** ← **Produkt** (każdy koszyk zawiera produkty).
- **Zamówienie** ← **Płatność** (każde zamówienie ma metodę płatności).
- **Zamówienie** ← **Dostawa** (każde zamówienie ma przypisaną metodę dostawy).
- **Produkt** ← **Recenzja** (każdy produkt może mieć recenzje klientów).

## Model relacyjny
Każda encja posiada określone atrybuty, klucze główne i klucze obce, zapewniające integralność danych. Poniżej przedstawiono najważniejsze powiązania:

| Encja | Klucz główny | Klucze obce |
|--------|--------------|-------------|
| Klient | idKlienta | - |
| Koszyk | idKoszyka | idKlienta |
| Płatność | idPłatności | - |
| Dostawa | idDostawy | idZamówienia |
| Zamówienie | idZamówienia | idKlienta, idKoszyka, idPłatności, idDostawy |
| Produkt | idProduktu | idProducenta, idKategorii |
| Recenzja | idRecenzji | idKlienta, idProduktu |

## SQL – tworzenie bazy danych
### Przykładowa struktura tabeli `Zamówienie`:
```sql
CREATE TABLE Zamowienie (
    idZamowienia INT PRIMARY KEY,
    idKlienta INT,
    idKoszyka INT,
    idPlatnosci INT,
    idDostawy INT,
    kosztZamowienia DECIMAL(10,2),
    FOREIGN KEY (idKlienta) REFERENCES Klient(idKlienta),
    FOREIGN KEY (idKoszyka) REFERENCES Koszyk(idKoszyka),
    FOREIGN KEY (idPlatnosci) REFERENCES Platnosc(idPlatnosci),
    FOREIGN KEY (idDostawy) REFERENCES Dostawa(idDostawy)
);
```

## Uruchamianie projektu
1. **Sklonuj repozytorium:**
   ```sh
   git clone https://github.com/user/cubezilla.git
   ```
2. **Przejdź do katalogu projektu:**
   ```sh
   cd cubezilla
   ```
3. **Skompiluj bazę danych:**
   ```sh
   mysql -u root -p < schema.sql
   ```
4. **Uruchom aplikację:**
   ```sh
   python app.py
   ```

## Autorzy
- **Piotr Chrabąszcz**
- **Krzysztof Charuk**
- **Dominik Długołencki**

---
*Projekt realizowany w ramach przedmiotu „Wprowadzenie do systemów baz danych” na Politechnice Lubelskiej.*

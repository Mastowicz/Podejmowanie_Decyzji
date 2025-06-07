# System ekspertowy od diagnozowania problemów z uruchomieniem komputera

## 1. Tematyka bazy

Zestaw przyczyn mogących uniemożliwić uruchomienie komputera lub systemu operacyjnego.
Celem bazy jest identyfikacja źródła problemu na podstawie analizy informacji podanych przez użytkownika.

### Założenie:

System potrafi rozwiązać problem, jeżeli dotyczy on:
1) Problemu z zasilaniem
2) Problemu z pamięcią RAM
3) Problemu z dyskiem twardym
4) Problemu z układem BIOS
5) Problemu z front panelem

## 2. Powód wybrania zagadnienia

Program pomoże użytkownikowi znaleźć przyczynę przez którą komputer nie działa.

## 3. Proces zbierania informacji

Wykorzystane w projekcie informacje na temat diagnostyki komputerów pochodzą z własnych doświadczeń oraz wiedzy nabytej na zajęciach.

Umiejętność tworzenia baz wiedzy w programie PC-Shell nabyto z dokumentacji i przykładów dostarczonych razem z programem.

## 4. Proces powstawania

#### 1. Wybór tematyki projektu  
Wybierając temat projektu należało znaleźć problem, który można rozwiązać na różne sposoby bazując na jednym zestawie pytań. Diagnozując komputer często przechodzi się przez ten sam proces sprawdzania najbardziej oczywistych usterek, dlatego wybrany został problem niedziałającego komputera i znalezienia tego przyczyny.
#### 2. Ustalenie założeń  
Temat diagnostyki komputerów jest bardzo rozległy. Często objawy nie wskazują jednoznacznie na konkretną usterkę i wymagana jest dalsza diagnoza. Dlatego na potrzeby zrealizowania projektu w określonym czasie, zestaw rozwiązań problemu został ograniczony do najpowszechniejszych i najbardziej oczywistych opcji.
#### 3. Opracowanie bazy reguł  
Mając ustalone założenia i znając możliwe do zdiagnozowania dla programu usterki należało określić zestaw warunków dla których te usterki występują. Dobierając warunki staraliśmy się pamiętać o tym aby były one łatwe do sprawdzenia dla przeciętnego użytkownika.

Reguła 1:
```
Jeśli reakcja_na_przycisk_power = "nie"
i     kabel_zasilania_jest_podłączony = "nie"
to    problem = "Niepodłączony kabel zasilania"
```

Reguła 2:
```
Jeśli reakcja_na_przycisk_power = "nie"
i     kabel_zasilania_jest_podłączony = "tak"
i     przełącznik_na_zasilaczu_jest_włączony = "nie"
to    problem = "Niewłączony zasilacz"
```

Reguła 3:
```
Jeśli reakcja_na_przycisk_power = "nie"
i     kabel_zasilania_jest_podłączony = "nie"
i     przełącznik_na_zasilaczu_jest_włączony = "tak"
to    problem = "Problem z front panelem"
```

Reguła 4:
```
Jeśli reakcja_na_przycisk_power = "tak"
i     ekran_wyświetla = "nie"
to    problem = "Prawdopodobnie problem z pamięcią RAM"
```

Reguła 5:
```
Jeśli reakcja_na_przycisk_power = "tak"
i     ekran_wyświetla = "tak"
i     system_działa = "nie"
i     boot_error = "tak"
to    problem = "Problem z układem BIOS"
```

Reguła 6:
```
Jeśli reakcja_na_przycisk_power = "tak"
i     ekran_wyświetla = "tak"
i     system_działa = "nie"
i     boot_error = "nie"
to    problem = "Problem z dyskiem twardym"
```

Reguła 7:
```
Jeśli reakcja_na_przycisk_power = "tak"
i     ekran_wyświetla = "tak"
i     system_działa = "tak"
to    problem = "Nie wykryto problemu"
```
#### 4. Utworzenie drzewa decyzyjnego  
W celu utworzenia diagramu wystarczyło ułożyć pytania sprawdzające występowanie ustalonych wcześniej warunków, ustalić sensowną kolejniość, a następnie przedstawić całość graficznie. Do stworzenia ostatecznej wersji diagramu użyte zostało przeglądarkowe oprogramowanie "draw.io".

![diagram](https://github.com/Mastowicz/Podejmowanie_Decyzji/blob/main/diagram.svg)

#### 5. Zapoznanie się z programem PC-Shell  
...
#### 6. Analiza przykładowych baz wiedzy  
#### 7. Opracowanie kodu źródłowego  

## 5. Kod źródłowy
<details>
    
```
knowledge base diagnozapc                            

    facets
    
        single yes;
    
        problem:
            val oneof { "Niepodłączony kabel zasilania",
                        "Niewłączony zasilacz",
                        "Prawdopodobnie problem z pamięcią RAM",
                        "Problem z dyskiem twardym",
                        "Problem z układem BIOS",
                        "Problem z front panelem",
                        "Nie wykryto problemu"};
    
        reakcja_na_przycisk_power:
            query "[BCzy jest reakcja na przycisk power?:"
            val oneof { "tak","nie"};
            
        ekran_wyswietla:
            query "[BCzy coś wyświetla się na ekranie?:"
            val oneof { "tak","nie"};
            
        system_dziala:
            query "[BCzy system operacyjny się włącza?:"
            val oneof { "tak","nie"};
            
        boot_error:
            query "[BCzy komputer zawiesza się przy włączaniu?:"
            val oneof { "tak","nie"};
            
        kabel_zasilania_jest_podłączony:
            query "[BCzy komputer jest podłączony do prądu?:"
            val oneof { "tak","nie"};
    
        przełącznik_na_zasilaczu_jest_włączony:
            query "[BCzy zsilacz jest włączony?:"
            val oneof { "tak","nie"};
    
    end;
    
    rules
    
        01:problem = "Niepodłączony kabel zasilania" if
            reakcja_na_przycisk_power = "nie",
            kabel_zasilania_jest_podłączony = "nie";
    
        02:problem = "Niewłączony zasilacz" if
            reakcja_na_przycisk_power = "nie",
            kabel_zasilania_jest_podłączony = "tak",
            przełącznik_na_zasilaczu_jest_włączony = "nie";
            
        03:problem = "Problem z front panelem" if
            reakcja_na_przycisk_power = "nie",
            kabel_zasilania_jest_podłączony = "tak",
            przełącznik_na_zasilaczu_jest_włączony = "tak";
    
        04:problem = "Prawdopodobnie problem z pamięcią RAM" if
            reakcja_na_przycisk_power = "tak",
            ekran_wyswietla = "nie";
    
        05:problem = "Problem z układem BIOS" if
            reakcja_na_przycisk_power = "tak",
            ekran_wyswietla = "tak",
            system_dziala = "nie",
            boot_error = "tak";
            
        06:problem = "Problem z dyskiem twardym" if
            reakcja_na_przycisk_power = "tak",
            ekran_wyswietla = "tak",
            system_dziala = "nie",
            boot_error = "nie";
    
        07:problem = "Nie wykryto problemu" if
            reakcja_na_przycisk_power = "tak",
            ekran_wyswietla = "tak",
            system_dziala = "tak";
    
    end;
    
    control
    
        setSysText( problem, "[2Co jest nie tak z komputerem?" );
    
        run;
    
        setAppWinTitle("Diagnostyka");
        createAppWindow;
        vignette( "Diagnostyka komputera","Autorzy:\nPatryk Wilk\nKrystian Zając","Projekt na przedmiot: sztuczna inteligencja");
    
        int Odp;
        Odp:=1;
    
        menu "Menu"
        1. "Diagnostyka problemu"
        2. "Wyjście"
        case 1:
            while(Odp == 1)
                begin
                    goal( "problem = X" );
                    confirmBox( 0, 0, "Kontynuacja","Wybierz \"OK\" aby kontynuować diagnostykę\nlub wybierz \"Anuluj\" aby zakończyć", Odp );
                    delNewFacts;
                end;
        case 2:
            exit;
    
        end;
    end;
end;
```

</details>

## 6. Przykłady wnioskowania

### Konsultacja 1
> System: Czy jest reakcja na przycisk power?
> 
> Użytkownik: Tak
> 
> System: Czy coś wyświetla się na ekranie?
> 
> Użytkownik: Tak
> 
> System: Czy system operacyjny się włącza?
> 
> Użytkownik: Nie
> 
> System: Czy komputer zawiesza się przy włączaniu?
> 
> Użytkownik: Nie
> 
> System: Problem z dyskiem twardym

### Konsultacja 2
> System: Czy jest reakcja na przycisk power?
> 
> Użytkownik: Nie
> 
> System: Czy komputer jest podłączony do prądu?
> 
> Użytkownik: Tak
> 
> System: Czy zasilacz jest włączony?
> 
> Użytkownik: Dlaczego?
> 
> System: Hipoteza: Problem z front panelem lub wyłączony zasilacz
> 
> System: Czy zasilacz jest włączony?
> 
> Użytkownik: Tak
> 
> System: Problem z front panelem
> 
> Użytkownik: Jak?
> 
> System: Reguła 3

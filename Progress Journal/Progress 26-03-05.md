---
created: 2026-03-05
modified: 2026-03-06
type: journal
---


### Initial goal
- Projít Tutorial AthenaK
- Opakovat výsledky z tutoriálu

### Results and Conclusion
- Dodán zápis Krtička
- Tutoriál nebyl splněn!!
  problém je se spuštěním s pomocí inputfile `hohlraum_1d.athinput`, je v ní definovaný problém *hohlraum*, tento problém ale není rozeznám generátorem problémů `pgen.cpp` a tedy jej není možné nagenerovat. Ze stejného důvodu tedy nefunguje ani spuštění `hohlraum_2d.athinput`.
  
  Můžeš se též podívat na `pgen.hpp`, hohlraum není mezi predefined problems.

  Toto je od něčeho, co by měl být Tutoriál vstřícný novým uživatelům (byť  z akedemického prostředí) dosti zákeřný krok.
  
  Zkusil jsem též vhodit `rad_hohlraum.cpp` jako problem file, ale pak není definována class *ProblemGenerator*.
- Byla nainstalována i Athena++ z důvodu lepší dokumentace a možnosti využít její wiki. Její framwork je ale trochu odlišný.

#### Výsledek 
Jsem schopen spustit simulaci s předdefinovaný problem file, jsme schopen to propojit s input file, sledovat, že mi to vyhazuje a asi i nějak funguje.

Podstata toho, co je vyhazováno v rámci souborů je poněkud zastřena. Není jisté, jak k tomuto problému přistoupit.

### To Do

+ Known Bugs

+ To Do In the Future
	+ Pokusit se rozchodit tutoriál
	+ S athenak měli přijít i nějaké analyzující skripty, pokusit se rozchodit ty -- linear wave, lze začít od toho issue.
	+ Podívat se, jak ve vim hledat pořádně v souborech

----------------------------------------------------
### Addendum
date: 06-03-2026

Podařilo se mi replikovat výsledky v tutoriálu, podrobnosti v [[AthenaK#How to make Hohlraum work?]]

Dále byl vytvořen Inicializován git v athena adresáři. Nová vytvořená větev *hohlraum*, která uložila změny nutné ke zprovoznění tutoriálu (viz odkaz výše).
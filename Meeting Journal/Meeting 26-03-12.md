---
event_date: 2026-03-12
created: 2026-03-12
modified: 2026-03-12
type: journal
---
Note:
### Příprava
- Přečtený Krtička cca 100 stránek
- C jazyk -- studované základní funkce, jednoduché programy
- [[Zingale, hydrostatic models, article]]
	- Zejména rozmyšlen Fluff, overview nějakých základních hydrostat rovnic (hodnota off -- myslím, že souvisí s derivacemi)
- Lane-Emden
	- ~~Nejsem si jistý s polytropic eq.~~~ => dnes odvozeno, trochu trik jak zapsat škálu procesů.  
	- Jinak jsem i s Krtičkou počítal profily atmosféry za zjednodušených podmínek s hydrostatickou rovnicí
		- konst rho, lineární hmotnost, isothermal.
- Athenak
	- stažena a sestavena
	- jednoduchý výpočet něco vyhazoval
	- Pokusil jsem se replikovat výsledky z wikipedie (zcela prázdná až na entry), nešlo replikovat --> uprava makefile a další --> úspěšná replikace => jak propojit fyziku se simulacemi, kde získat informace o obsahu datových douborů?

Otázky a cíle:
- C: lepší pochopení jak seskládat a build program z různých souborů -- include, oddělený compile, deklarace funkcí
- ~~Lane-Emden: propojit polytropic eq. s něčím, co už znám, proč právě toto je dobrý model?~~ 
- AthenaK: 
	- schopnost chápat fyziku za simulacemi
	- schopnost chápat a číst výsledná data
	- Dokumentace -> Athena++?
- Číst články, Krtičku, a Zingale
### Průběh
Pejcha velmi rád povídal, nepamatuju si nutně mnoho, budu muset vrazit do zdi.
### Cíle
Zejména další krok athena, pokračovat
### Úkoly pro Pejchu

### Úkoly pro mě
De fakcto pokračovat ve vytyčeném směru, spustit simulace a zkusit vykreslit. V simulacích zkusit zjistit, k čemu odkazují akronymy. 
Spec. pozornost:
- Kraus, hydrostatická bublina

Jak počítat zvuk?
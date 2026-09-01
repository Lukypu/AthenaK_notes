---
created: 2026-02-24
modified: 2026-08-28
---
note:
==Important== 
# Mikulášek, Krtička: Základy fyziky hvězd 

## Metadata
 @book{Mikulasek_Hvezdy,
 author = {Mikulasek, Zdeněk and Krtička, jiří},
 title = {Základy fyziky hvězd }, 
 year =  { 2005 },
 publisher =  { },
 address ={ }, 
 edition =  { },
 language =  { cz },
 ISBN =  { },
 }
 - Added: 24-02-2026
 - links:
	 - www:
	 - doi:
- [x] Downloaded?
- [ ] Read?

## What and why read?
%%which sections to read and why%%
Základy fyziky hvězd, doporučeno Pejchou. Důraz na následující kapitoly:
- 2 Stavba hvězd
- 4 Vznik a vývoj hvězd
- 6 Fyzika dvojhvězd
- Ostatní taky zajímavé


## Conclusions
%%what is it that we have learned%%
#### 1 Úvod
###### Elmag záření
Elektromagnetické záření, chová se jako částice i vlna. Chrakteristické vlné délky a vzdálenosti: Comptonova délka. Různé polarizace, Brzdné záření (Potřeba dvě částice). 
###### Tepelné záření
Hvězdy v prvním přiblížení jako absolutně černá tělesa -- zářivý výkon a skladba vlnových délek (jemně ovlivněna prvky ve hvězdě). Planckův zákon, Wien/Stefan (UV IF katastrofy), Wienův posunovací zákon.
(V případě potřeby načíst z jiného zdroje).
###### Výkon hvězd a měření
Měření problematické -- hvězdy moc daleko. Používají se veličiny jak absolutní tak ty, které souvisejí s omezeným měření (např. svítivost dle možností lidského oka). Typicky celková veličina vůči veličině měřené **bolometricky**, pro luminositu: $L = \int_{S(r)} F  d S = 4\pi r^2 F$, kde $F$ je *bolometrická* jasnost (plošná luminosita), analogicky velké množství dalších veličin, zejm.
- hvězdná velikost $m=-2.5 \log\frac{j}{j_0}$ , j referenční jasnost (výkon), (dovozuje se vizuální jasnost)
- Efektivní teplota hvězd: teplota černého tělesa stejného poloměru a výkonu jako pozorovaná hvězda.

###### Měření hvězd
date: 01-03-2026

Hvězdy se vyznačují několika zásadními parametry, zejm.:
- Spektrální čáry (zastoupené fotky *abundance*), původné (staré hvězdy) se soudí, že nemají prvky těžší než železo, ie jen H a He
- Barva (souvisí s teplotou a svítivostí): spektrální zářivý výkon
- Teplota $T$ , spec. *Efektivní teplota*
- Jasnost/zářivý výkon, svítivost ($L$ je zářivý výkon alias svítivost. Jasnost je svítivost ve směru/úhlová svítivost)
- Poloměr $R$
- Hmotnost $M$

Při studium hvězdné oblohy se uplatňuje výběrový efekt -- vidíme jen hvězdy, které mají velkou svítivost. Kategorizace hvězd: Hertzsprung-Russell diagram (H-R diagram). 
Chceme porovnávat základní charkteristiky, tj. $M$, $R$, $T$ a $L$. Veličiny jsou na sobě závislé. Typicky: HR diag jako závislost $L$ a $T$ v log-log škále. Různé varianty, např. wiki $L$ ("colour").

Základní charakteristika HR: Bílí trpaslíci, hlavní sekvence, obři, superobři. Diagram je nesmírně důležitý pro měření vzdáleností. Zespektrálního profilu je možné získat typ hvězdy, zařadit ji na HR, tam porovnáme absolutní svítivost hvězd daného druhu s pozorovanou svítivostí a dopočteme vzdálenost hvězdy.

#### 2 Stavba hvězd
date: 05-03-2026 
###### Rovnováha
- Definice Hvězdy -- na hvězdě je nejdůležitější její hmotnost, ie definice přes dostatečnou hmotnost
- Jsou vázána gravitační silou -- proti ní působí zejm. hydrostatický tlak (odstřev obvykle zanedb)
- rce hydrostat rvn pro hvězdy
	  $$-\frac{dp}{dr} = \rho \frac{GM}{r^2}$$
		( $GM/r^2$ jako gravitační zrychlení, tlak jako síla)
- kulová symetrie -- nerotující hvězdy tvoří koule 
- Pokud je hydrostatická rovnováha narušena --> toky energie, zrychlování elementů. Obvykle rovnováha tž stabilní, jinak exploze/imploze -- např zhroucení plynu při tvorbě, výbuch supernovae.
- "Globální" termodynamická rovnováha vzácná -- lokálně se jí však ve vymezeném prostotu a čase můžeme libovolně blížit -- pojem "LTE" -- local thermodynamic equilibrium.
- Vlastnosti ideálního plynu: Důležité je, že i plasma, reps. superhorká superhustá hmota ve hvězdě se může blížit ideálnímu plynu -- čím vyšší teplota, tím spíše ideální plyn
- Elektronová degenerace -- hmota je udržována fermiho principem -- zpravidla trpaslíci -- vypálen palivo. Teplota je principiélně nízká, vlastnosti silně závisí na hustotě
- Fotonový plyn: TODO shrnutí (reps. nemyslím si, že mi nyní jeho číslo metačství výrazně pomůže)
###### Zdroje energie
- Zdroje energie hvězdy -- nejenom teromo reakce -- trpaslíci vyhořelé palivo, ie i další zdroje
  
  Jsou zahřáty na velikou teplotu (třeba i tření?!) a pozorujeme photosféru. 
- Energie vázaného systému -- viriálový teorém -- Luminosita potom jako změna vnitřní energie -- vnitřní energie klesá, ale jelikož díky viriálovému toerému vždy platilo $U<0 \; and \: U = -E_k$ , kinetická energie roste --> záře
- Jaderné cylky -- prominentní H-H cyklus a CNO (přeskočeno)
###### Energetická rovnováha

#### 3 Hvězdné atmosféry

###### Slupkový model
Slupky hvězdy:
- Jádro
- Obal (obvykle jen konduktivní, obři a jiné konvektivní; trpaslíci a jiné chybí -- jen degenerované jádro)
- Svrchní vrstvy atmosféry
- Akreční disk (very optional)

*Hvězdné atmosféry se zajímají výhradně o **Svrchní vrstvy** atmosféry*.

Energetická rovnováha:
- Prvotní hvězdy – ohřev dle viriálovému teorému
- Energetická rovnováha 
	- inputs: Spalování H (H-H, CNO), later spalování He (triple alpha), termodynamické smršťování gravitací
	- outputs: záření, Sluneční větry,  termodynamické rozpínání

Složení hvězdné atmosféry: 
- svrchní vrstvy ze zkoumání čar prvků
- spodní vrstvy – ouvej

*Ze spektrálních čar vychází klasifikace hvězd. Původně ABC… pak se jim to malinko rozbilo*: výsledek – OBAFGKM

##### Atmosféra Slunce
Asi lze brát jako typickou atmosféru.

Součástí:
- Fotosféra – zdroj slunečního záření pozorovaného na Zemi. Dobrá definice povrchu Hvězdy
- Chromosféra – navazuje na Fotosféru, řídká, růst teploty!!! -> přenos teploty zvukovými vlnami??
- Koróna – nejsvrchnější vrstva
- Sluneční vítr – proud ionizovaných částic vyvržených ze Slunce

#### 4 Vznik a vývoj hvězd
###### Počátek
Mezihvězdný plyn fluktuací zhuštěn na kritickou úroveň. Začne se vlastní gravitací zhušťovat (Oppenheimer v Semerák) a dle věty o viriálu se začne zahřívat. Složení mezihvězdného plynu závisí na jeho historii, ale typicky ~98% H, ~2% He.

Zhařeje-li se dostatečně – zažehnut HH cyklus. Jinak hnědý trpaslík či jiné pa-stádium. Před zhroucením brání vytvořený gradient tlaku z termodynamiky.

=> *Protohvězda*

###### Hlavní posloupnost
88% času.  Spalování H v jádru, jak dochází, jádro se zahušťuje –> zvyšování teploty –> zvyšování rychlosti reakce (v extrému převládne CNO). Zvýšený výkon <-> větší poloměr, větší L

Čas na hlavní posloupnosti lze odhadnout:
	$\tau \approx \frac{E}{L} \approx 10^{10} \, \mathrm{years} \, \left(\frac{M}{M_{sun}}\right)^{2.6}$ 
E je celkové množství zásob jaderné energie, tj. efektivně kolik má paliva a jak rychle jej spotřebovává?

###### Spálení Vodíku
Spálení vodíku –> žádné další termonuk reakce –> hvězda se začne smršťovat a zahřívat. Jádro se gravitační silou začne degenerovat.

V případě Slunce –> zažehnutí reakcí ve vrstvách kolem jádra, kde sátle dost vodíku. To způsobí expanzi obalu. Postupem času až takovou, že pohltí Merkur (až 0.8 AU). Červený obr.

Postupně dojde k degeneraci He jádra a dalšímu zvyšování teploty v jeho blízkosti tž zapálení spalování He –> normální obr. Tzv. He záblesk, část E ze záblesku na sejmutí degenerace jádra.

###### Po spálení He
Analogická situace jako u spálení H. Vyváří se degenerované C-O jádro a vrstva hořícího He. Zdrojem E stále slupkové hoření H ve vyšších vrstvách. 

Postupně čím dál více přenos energie konvekcí. Čím dál hlubší vrstvy až nakonec velké tepelné ztráty a silné Sluneční větry.

Nestabilita jádra a pulsy odvrhnou atmosfétru

###### Konec
Chladnoucí degenerované C-O jádro. Bílý trpaslík -> černý trpaslík.

Slunce má v celku typickou a exemplární vývoj. Alternativou menší hvězdy, u nichž nedojde k zapálení He a těží hvězdy s mnohem dynamičtějším zakončením. Bude se hodit vytvořit si jakýsi diagram hvězdných vývojů.


#### 5 Závěrečná stádia vývoje hvězd
Závěrečné neaktivní stavy hvězd:
- Surpenovae remnant
- Brown Dwarf
- White dwarf
- Neutronstar
- Black hole


#### 6 Binární systémy hvězd



## Shortcomings and Ideas
%%what didnt work and what was not understood though should be%%



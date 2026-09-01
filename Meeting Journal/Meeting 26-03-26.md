---
event_date: 2026-03-26
created: 2026-03-26
modified: 2026-04-02
type: journal
---
Note: Doplněno retroaktivně z poznámek dne: 02-04-2026

### Průběh
##### Riemannovy Solvery
Viz [[Zingale, comp_hydro, book]]: HLL, HLLC + vyhlazování PLM (piecewise linear), PPM (piecewise polynomial)

##### Spuštění Reyleigh-Taylor
- Athena++ See python configure.py make for rt
- Potřeba make new rt.cpp ?????
- pozor souřednice -- chceš *kartézské*

##### HDF5
- h5.py ??
- obecně by tam měl být skript, co bin --> hdf5
- Pejcha si myslí, že vynechání hdf5 má souvislost s optimalizací, konkrétně s paralelním zápisem

### Cíle

### Úkoly pro Pejchu

### Úkoly pro mě
- [ ] Přečíst Zingale -- Sodova trubice
- [ ] Zingale -- základy Riemann solverů HLL, HLLC -- testovat na Sodovy (also PLM PPM)
- [ ] Číst Krtičku
- [ ] Spustit Reyleigh-Taylor nestabilitu
- [ ] Přečíst binární soubory

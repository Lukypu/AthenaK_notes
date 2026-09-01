---
created: 2026-04-28
modified: 2026-04-29
type: journal
---


### Initial goal
- zjistit proč Rayleigh-Taylor dává špatné výsledky?
- Správná Rayleigh-Taylor

### Results and Conclusion
- Zjištěna příčina – špatné čtení meshblocků
- Nové video, taky nefunguje správně, nyní více do fyziky

- I po spravení meshblocků, numerická simulace stále neprobíhá správně
- **Pokus**: nový build, zopakovat postup, výsledky jsou shodné.
- Build options:
```
 This Athena++ executable is configured with:  
 Problem generator:          rt  
 Floating-point precision:   double  
 MPI parallelism:            OFF  
 OpenMP parallelism:         OFF
```
- Možnosti:
	- Compatibility issue, see [https://github.com/IAS-Astrophysics/athenak/wiki/Requirements]
	- MPI (Message Passing Interface), what does it exactly do??
	- Because I'm using meshblocks, it might be required for me to use MPI: Turn on with
		- `-D Athena_ENABLE_MPI=ON` when build
		- or just turn off mesh blocks in input file

- **Pokus2**: build 2, vypnout meshblocks
	- No good "bin_convert" needs at least minimal info about mbs
	- 
### To Do

+ Known Bugs

+ To Do In the Future

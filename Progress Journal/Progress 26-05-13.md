---
created: 2026-05-13
modified: 2026-05-14
type: journal
---


### Initial goal
- Design a test for different Riemann solvers and different Reconstructors
### Results and Conclusion
- Turns out that "Double Mach Reflection" test is not natively implemented in athenak
- trying everything for everything inpractical
- found interesting page at least partially explaining what athena is doing: (https://princetonuniversity.github.io/Athena-Cversion/AthenaDocsUGRiemann.html)
- Idea: do Brio-Wu Tube 
	- 2 Riemann + All reconstruc
	- 2 Reconstruct + All Riemann

Now problem: How to automatically run these tests??

- Created a bash script to mass execute the test. Though unexpected errors:
	"advect" and "roe" rsolvers are not aligned with the "dynamic" problems
	Rest of the tests concluded successfully

### To Do

+ Known Bugs

+ To Do In the Future

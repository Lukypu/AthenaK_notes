---
created: 2026-05-17
modified: 2026-05-18
type: journal
---


### Initial goal
Create images and videos of Brio-Wu tests
	 - test batches:
		 - bw_recon_tests
		 - bw_rsol_tests
### Results and Conclusion
- Script used was not yet generalized for versitile use
- On the other hand it worked. Different images and videos were created
- I am not quite sure how to qualitativaly look at it and which details I should put emphasis on. Not to mention I am not sure what various of the options actually mean.
- It holds though that only few of the ccombinations actually make sense
- clear things first:
	- dc reconstructor is very stable, but also smooths solution too much
	- plm perhaps offers a healthy combination of stability and smoothing
	- it seems that reconstructors have more definite impact on the simulation while rsolvers having their much more subtle.

- What is possible to improve for the next time
	- simulations could last longer in the simulation time
	- time step could be smaller (although time step should be in correspondence with space discretisation)
	- how much is the simulation going to be impacted by having smoother discretisation?
	- better naming convention test1_bw is not ideal. Differentiate between different test batches
	- better use of integrator "order of integrator should correspond to order of reconstructor"

Todo:
- the same simulations but:
	- add dc in case of rsol
	- let integrator and polynomial order be the same

### To Do

+ Known Bugs

+ To Do In the Future

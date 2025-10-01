# Summary
In this chapter, we covered the three types of LOD expressions available in Tableau (FIXED, INCLUDE, and EXCLUDE) and studied how we can use them to aggregate data at a level that is either more granular (in the case of INCLUDE) or less granular than the dimensions already in the view. We also looked at the order of operations to explain the differences between these calculations and the ones seen previously (such as table calculations). For instance, we now hold the tools to calculate contributions to a total, create cohorts based on first order date, and aggregate customer-level information starting from item-level data.

Here is a quick reminder:
|   | Fixed | INCLUDE | EXCLUDE |
|---|-------|---------|---------|
| Order of operations  | Before dimension filters      |  After dimension filters       | After dimension filters        |
| Purpose  | Calculate across the dataset along selected dimensions      |  Calculate results using a dimension that is not part of the view       | Calculate results excluding one of the dimensions that is part of the view        |



		
			



This chapter closes a three-chapter journey into the different calculations that we can use in Tableau, and we are now ready to use some of the analytical tools to add some insights to our worksheets.
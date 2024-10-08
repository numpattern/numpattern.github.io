---
layout: post
title: Dealing with large sub-blocked vein models
#subtitle: An essential part of Resources Evaluation. GSLIB Cell Based Method.
tags: [Modeling]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_flattening.PNG
---

Sometimes we deal with sub-blocked models generated with a software that is not available to us, and we have access only to GSLIB. When using GSLIB's programs, we must consider: (1) they use structured unrotated grids, and (2) write to disk. This may not discourage to practitioners as it's fairly possible to simulate using conventional GSLIB routines for real cases without falling in long waits or depleting our storage. Here, I summarize practical steps to deal with large sub-blocked narrow vein models to quickly simulate and up-scale to original parent cells. 

### **Task**
Use GSLIB to generate a simulated model, and average up to the parent cells of a given irregularly sub-blocked model **BM**. Flatten composite points **cmp** prior estimation/simulation. The following issues were encountered:
- Our software may not handle the grid definition to import an irregular sub-blocked model
- The orientation of the model (e.g. narrow structure) may not be aligned with a major plane  (XY, YZ, XZ), this results in large size in disk due to a large GSLIB griddef
- Composite data consists of centroid coordinates (no survey, trace, top or bottom coordinates)

### **Data specifications**
- The composites and sub-blocked model are provided
- The grid definition was not rotated, this prevented us to do any rotation
- **BM** can be exported including XYZ coordinates of populated cells (do not consider unpopulated cells). Do not rely on any previous indexing
- **BM** is coded with the domain

### **Steps**
1. Flatten sub-blocked grid model by aligning to a principal axis  
	1.1 Regularize the sub-blocked model to **BM1**, using small cell sizes.  
	1.2 Flatten BM1 to BM1_ by projecting the grid cells along one axis (e.g. Y) to its orthogonal plane (e.g. XZ)  
	1.3 Create a dictionary **key2coord**  with **key**:( X-Z coordinate ), **value**: lowest  Ymn (or highest Ymx) of grouped X-Z, and use it to map Ymn to all BM1 cells. 
	1.4 Flatten BM1_:  Y_projected = Y - Ymn

2. Project composites using block model coordinates  
	2.1 Regularize **BM** to **BM2** using smallest cells for higher precision  
	2.2 Flatten **BM2** to BM2_ by projecting the grid cells along one axis (e.g. Y), to the perpendicular plane (e.g. XZ)  
	2.3 For **BM2**, obtain **key2coord2** with **key**:( X-Z coordinate ), **value**: lowest  Ymn (or highest Ymx) of grouped X-Z  
	2.4 Attach the closest XYZ coordinates from **BM2** to the composite points  
	2.5	Flatten the composites **cmp** to **cmp_**  by projecting to an axis, using **key2coord2** dictionary: Y_projected = Y-Ymn  

3. Create a GSLIB griddef that matches the **BM1_**
4. Impose all 1D-idx's (structured GSLIB griddef) to **BM1_**, and generate a keyout file
5. Simulate 
6. Unflatten **BM1_**  to **BM1** using **key2coord**, with the simulated values
7. Up-scale **BM1** 

### **Conclusions**
A large sub-blocked model of a narrow structure was used as reference domain to generate a simulated model and scale-up to match the original parent cells. The impact of the coordinate projection and precision must be understood and is case-specific. GSLIB programs can successfully used to simulate unstructured large grids.

### **References**
1. [A flexible sequential Gaussian simulation program: USGSIM. Computers & geosciences, 41, 208-216](https://www.sciencedirect.com/science/article/abs/pii/S0098300411002755)

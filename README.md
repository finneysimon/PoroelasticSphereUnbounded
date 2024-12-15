# Poroelastic Sphere Unbounded
The small deformation of a weakly deformable poroelastic particle in a general unbounded Stokes flow. Mathematical details may be found in (insert DOI when published).

There are two notebooks: 'GeneratePorjectionTerms.nb' and 'Weakly_deformable_poroelastic_sphere_in_a_general_unbounded_Stokes_flow.nb'. The former is used purely to generate the file 'ProjectionTerms.wl' and does not need to be edited if using a background shear or Poiseuille flow. 'ProjectionTerms.wl' stores the terms in Equation III.36 as projected to a spherical harmonic basis via Equation III.37. The latter notebook may be edited and describes the fluid and solid problems to be solved (Equations III.10-III.13 for the fluid problem, III.32-III.35 for the solid problem). 

### Weakly_deformable_poroelastic_sphere_in_a_general_unbounded_Stokes_flow.nb
We provide a brief description of how to use our notebook to solve the leading-order fluid flow and the corresponding particle deformation. Please download the file and load into Mathematica to view due to symbolic language used in the notebook.
 - The variables \[CapitalPhi]StarTable, \[CapitalPsi]StarTable, and \[CapitalChi]StarTable define the background flow profile. Two examples are given in our notebook corresponding to shear flow and Poiseuille flow. For each variable, the first index corresponds to L=0, the second L=1, and so on. Within each index (value of L), values correspond to m from -n to n. 
 - To implement a new background flow, these variables must be updated such that when substituted into Equation III.2 & III.3 (L is replaced with n in the paper) we recover the desired background flow without the particle. 
 - The parameter L_max must be set to be 2 higher than the highest non-zero term in \[CapitalPhi]StarTable, \[CapitalPsi]StarTable, or \[CapitalChi]StarTable. For example, shear flow, which has a non-zero value when L=2, we set L_max=4; for Poiseuille flow we instead set L_max=5. NOTE: projection terms must also be calculated using a value grater than or equal to your desired L_max. In the current saved version we set L_max=5 when generating the projection terms, making it suitable for both shear flow and Poiseuille flow.
   
### Fluid problem
 - The fluid problem is defined by the statements EQN1, EQN2, EQN3, EQN4, and EQN5, which are equivalent to III.10-III.13.
 - To solve the fluid problem we use the built-in Mathematica function 'Solve', calculating the unknown coefficient tables \[CapitalPhi]HatTable, \[CapitalPsi]HatTable, \[CapitalChi]HatTable, PHatTable.
 - We then additionally impose the force and torque balances III.19-III.21 to calculate the particle's velocity and rotation, noting that the particle's velocity must appear in the background flow as {Uy, Uz, Ux} (L=1).
### Solid problem
 - We first load the projection terms from 'Projection_Terms.wl', ensuring an appropriate L_max was used during their generation.
 - BC1, BC2, and BC3 correspond to the reformulated boundary conditions III.32-III.35.
 - BC0123Rules solves these boundary conditions simultaneously, exploiting the orthogonality of the solid spherical harmonics.
 - The outputs UCoefList, VCoefList, WCoefList correspond to the desired unknown coefficients in III.28, (\[Psi]^{x, y, z}_{n, m} in paper).
### Visualising solutions
 - After we have solved the solid problem, we simply substitute all the calculated coefficients back into the general solutions to calculate the fluid pressures and velocities, as well as the solid displacement.
 - A few sanity checks are also implemented to ensure the boundary conditions were applied correctly.

NOTE: Due to the size of the simultaneous equations, solving and simplifying them for large L_max can cause issues in Mathematica leading to long run times, especially during simplification. This can often be because Mathematica attempts to solve the problem with no restrictions on the unknowns, and can sometimes be avoided by imposing assumptions on all variables. For example, \[Theta] being between 0 and Pi. This can be done by adding to the line '$Assumptions = x \[Element] Reals && y \[Element] Reals && z \[Element] Reals;' at the top of the notebook.

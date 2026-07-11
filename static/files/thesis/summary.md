Briefing Document: Seismic Performance of Reinforced Concrete Wall Structures
Date: October 26, 2023

Author: AI Assistant

Source: Excerpts from "cf-cordero_fo.pdf" (Author: C. Cordero, Advisor: L. Massone)

1. Executive Summary
This document reviews a study focused on the seismic performance of reinforced concrete (RC) wall structures, particularly those commonly found in Chile. The research utilizes fiber models and time-history analyses to simulate structural behavior under seismic loads. Key themes include the detailed characterization of structural components (walls, slabs), material properties (concrete and steel), model validation against experimental data, and a comprehensive parametric study investigating the influence of various design parameters on structural response. The study highlights the importance of confinement, axial load, and foundation conditions on the ductility and capacity of these structures during earthquakes.

2. Study Objectives
The specific objectives of this work are outlined as follows:

Define structural typologies to be modeled and their dimensional ranges.
Specify how materials and their properties will be considered in the fiber model, as well as the vertical distribution of elements. Coupling elements present in the studied structures will also be characterized.
Vary different relevant parameters of the considered structures and subject them to various acceleration records through time-history analyses.
3. Description of Earthquake and Observed Damage (Conceptual Framework)
While the source doesn't detail a specific earthquake event, it sets the stage for understanding damage patterns by referencing observed damage and typical structural characteristics.

3.1. General Description of Damaged Structures
Layout (Planta): Structures are typically regular and rectangular, with aspect ratios between 2:1 and 3:1.
Wall Distribution: Characterized by a low quantity of practically continuous, long walls in the longer direction, and a high quantity of rectangular and T-shaped walls in the shorter direction.
Wall Thicknesses: Primarily 20 cm (60% of cases reviewed by Estay (2008) for Chilean RC buildings between 1996-2006, specifically those 15 stories or more from 2000 onwards) and 25 cm (17%). Lesser percentages for 15 cm and 30 cm thicknesses.
Wall Lengths: Vary widely, concentrating between 50 cm and 650 cm.
Elevation: Not detailed in provided excerpts, but listed as a sub-section.
Typical Dimensions of Damaged Walls (Summary):Wall Thicknesses: Vary between 17 cm and 30 cm.
Wall Lengths: Flange lengths vary between 5 m and 8 m. Web lengths average 5 m, with variations of no more than 15%.
Floor Height: Between 2.5 m and 3.2 m.
Reinforcement Meshes: Vary between φ8 mm at 25 cm to φ12 mm at 17 cm.
Longitudinal Boundary Reinforcement: Varies from 4φ16 to 10φ25. Calculation of reinforcement quantity can follow ACI318-2005.
Material Quality: For analyzed structures, materials are H30 concrete (f'c = 25 MPa) and A63-42H steel (fy = 420 MPa).
Observed Damage: Figures 2.6 and 2.7 show observed damage in buildings in Macul and Ñuñoa, indicating real-world relevance.
4. Model Validation
The study rigorously validates its computational models (developed using OpenSees) against experimental results from existing literature.

4.1. Cross Sections and Element Discretization
Fiber Model: Cross-sections are modeled using a fiber model approach, where the section is discretized into individual fibers, each assigned a material constitutive model (e.g., concrete, steel).
Discretization: Concrete is discretized into rectangular fibers, and steel reinforcement is modeled as concentrated fibers.
4.2. Material Characterization
Steel (Reinforcing Bars):Steel02 Model: Used for general steel behavior, incorporating parameters like yield strength (Fy), initial elastic modulus (E0), and strain hardening ratio (b).
ReinforcingSteel Model (Massone, 2009): Based on Chang and Mander (1994) [3], this model simulates bar buckling between stirrups, which causes degradation of compression capacity. It includes parameters such as ultimate stress (fu) and strain at ultimate capacity (εu).
Buckling Influence: The "Gomes and Appleton" parameters [7] are used to incorporate the buckling of longitudinal bars.
Experimental Validation: Models compare well with experimental data (e.g., A1 and C1 tests) for stress-strain behavior.
Concrete:Concrete02 Model (OpenSees): Incorporates both compression and tension capacity for monotonic behavior.
Compression: Assumes an initial tangent stiffness and defines post-peak behavior with a descending straight line. Residual capacity is zero for unconfined concrete and 20% of f'cc for well-confined concrete.
Tension: Considers tensile strength (ft, ~10% of f'c) and a descending straight line post-cracking (slope estimated at 1000 MPa).
Confinement Effect: Concrete strength under confinement (fcc) is calculated using equations based on f'c, effective confinement pressure (fle), and a factor k1. Effective confinement pressure is derived from stirrup area (Asi), yield strength (fy), confined width (bci), and stirrup spacing (s), multiplied by an effectiveness factor k2.
Fracture Energy (Gc_f): Plays a role in defining the post-peak discharge curve, particularly relevant for confined and unconfined concrete based on integration element length.
4.3. Calibration of Models (RW2 and TW2 Walls)
Walls Tested: RW2 (Rectangular Wall 2) and TW2 (T-shaped Wall 2) are used for calibration, based on experimental data (modified from [10] and [14]). Dimensions and reinforcement details are provided.
Instrumentation: Strain gauges at the base of the wall are used to measure deformations.
Damage Observed: Pictures (Fig. 3.19) show damage to RW2 and TW2 at 2.5% drift, confirming the models' ability to simulate damage.
Model Implementation: Fiber models are used for cross-sections.
Calibration Results:Global Response (TW2): The model shows good agreement with experimental data for drift vs. basal shear. However, the model predicts higher shear capacity when the web is in compression (about 10% higher) and slightly lower capacity when the flange is in compression (about 5% lower). The model also shows less capacity loss due to cycling compared to the experiment.
Unitary Deformation (TW2): The model shows higher tensile deformations relative to compressive deformations (approx. 3 times) compared to the experiment (approx. 2 times).
RW2 Global Response: Calibration also shows reasonable agreement with experimental data.
4.4. Parametric Study of Response (TW2 Wall)
Four parameters influencing the global response of the TW2 wall were evaluated:

Quantity of Confinement:Effect: Confinement significantly improves the lateral load capacity and ductility of the wall, as shown in Figure 3.27. Unconfined concrete leads to a more brittle response.
Quality of Concrete (f'c):Effect: Decreasing f'c by 30% reduces lateral load capacity but "does not imply a loss of ductility" due to material degradation (Fig. 3.28).
Longitudinal Bar Buckling (between stirrups):Effect: Modeling buckling, especially when considering its influence on confined concrete quality (Fig. 3.29), impacts the load-displacement response and deformation capacity. Comparison with Figure 3.30 (without concrete quality influence) shows subtle differences, but both indicate that buckling affects the response.
Axial Load Level:Effect: Increasing axial load (from 0.075 f'c·Ag (base case) to 0.15 f'c·Ag and 0.3 f'c·Ag) leads to an increase in lateral force capacity but rapidly reduces ductility. At 0.3 f'c·Ag, the structure becomes brittle, with a "violent decay of capacity before reaching 1% drift." Uniaxial deformations at the base increase almost proportionally with increased axial load (Fig. 3.31).
Length of Confinement Zone:Effect: Increasing the length of the confined zone (15%, 30%, 50% of web length) generally improves the ductility and capacity of the wall (Fig. 3.32).
5. Modeling of Structures
Two main structural typologies, Structure A and Structure B, are analyzed through time-history simulations.

5.1. Description of Analyzed Structures
General Materials: H30 concrete (f'c = 25 MPa) and A63-42H steel (fy = 420 MPa) are used for all analyzed structures.
Structure A:Composed of four different transverse sections (two in each T-shaped wall).
Sections are key for evaluating damage due to seismic action.
Sections: Table 4.1 provides details on sections (e.g., thickness, reinforcement).
Floor Heights: Table 4.2 lists floor heights.
Structure B:Sections: Table 4.3 provides details on sections.
Floor Heights: Table 4.4 lists floor heights.
5.2. Description of Modeling
Perimeter Walls: Modeled as linear elastic elements with a concrete elastic modulus of 25 GPa.
Slabs: Modeled as elasto-plastic elements in flexure, axially undeformable. Plastic moments and corresponding curvatures are specified for 15 cm and 18 cm thick slabs.
Axial Load Application: Described as a specific modeling step.
Time-History Analyses: Implemented using specific acceleration records (e.g., RENADIC-UCH, Concepción EO).
5.3. Analysis Cases (Parametric Study)
The study explores the impact of several parameters on the seismic response of Structures A and B.

5.3.1. Variation of Number of Floors
Structure A: Various scenarios (constant axial load/longitudinal reinforcement, proportional variation of mass/axial load, proportional variation of longitudinal reinforcement, proportional variation of both) are studied for 10, 15, 20, and 25 stories.
General Observation: The figures (5.15-5.18) illustrate changes in shear, moment, and unitary deformation envelopes across floors, as well as roof drift and spectral comparisons. The spectral comparison (Fig. 5.19) shows how different variations in the number of floors affect the dynamic response relative to elastic spectra.
Structure B: Similar variations to Structure A were conducted.
General Observation: Figures 5.28-5.31 show similar trends. For all four combinations of variations, it is observed that "the response is not controlled by shear, since it never exceeded the capacity of the sections," as seen in the shear envelopes.
5.3.2. Axial Load and Confinement
Structure A (without confinement): Increasing axial load (0.1, 0.15, 0.3 f'c·Ag) significantly impacts shear, moment, and deformation profiles (Fig. 5.20). Higher axial loads lead to higher forces but reduced ductility as seen in the unitary deformations and drift.
Structure A (with ACI confinement): With confinement (ACI equivalent, 30% of web free edge), the structure shows improved performance compared to no confinement, especially at higher axial loads (Fig. 5.21).
5.3.3. Length of Confined Zone
Structure A: Varying the confined length (0.15 L conf., 0.3 L conf., 0.5 L conf.) impacts the distribution of moments and deformations (Fig. 5.22).
Structure B: Similar results (Fig. 5.35).
5.3.4. Foundation Type and Perimeter Wall Thickness
Structure A:Foundation Type: Comparison between rigid (fixed base) and flexible foundation (Balasto constant of 10 Kg/cm³) shows differences in structural response (Fig. 5.23).
Perimeter Wall Thickness (Fixed Base): Increasing perimeter wall thickness (40, 80, 100 cm) affects moment and deformation profiles (Fig. 5.24).
Perimeter Wall Thickness (Flexible Foundation): Similar study with flexible foundation (Fig. 5.25). An important finding is that for the largest thickness, "maximum drifts are reduced to a value close to 0.6%, which also translates into less flexibility and ductility of the structure." This suggests that modeling the support condition and the interaction of perimeter walls is crucial.
5.3.5. Different Acceleration Records
Structure A (without confinement): The response varies significantly depending on the acceleration record (Maipu, Santiago Centro, Vina, Concepcion), particularly in terms of roof drift and deformation profiles (Fig. 5.26).
Structure A (with ACI confinement): With confinement (ACI, 50% web length), the response is generally more controlled compared to the unconfined case across different records (Fig. 5.27).
Structure B (without confinement and with confinement): Similar observations regarding the influence of different seismic records and the benefits of confinement (Figs. 5.39, 5.40).
6. Conclusions and Final Comments
(Section 6 is listed in the index but details are not provided in the excerpt. However, based on the results presented in Section 5 and the parametric studies, the following general conclusions can be inferred:)

Fiber models are effective for simulating the non-linear behavior of RC wall structures under seismic loads.
Confinement is critical for improving the ductility and load-carrying capacity of concrete walls, especially under high axial loads. Inadequate confinement can lead to brittle failure.
Axial load has a significant impact on structural ductility. While higher axial loads can increase lateral force capacity, they drastically reduce the system's ability to deform inelastically without severe degradation.
Foundation conditions and the interaction of perimeter walls play a role in the overall structural response, affecting drift and flexibility.
The choice of seismic record influences the dynamic response, highlighting the importance of considering multiple ground motion scenarios for a robust design.
The study underscores that for the analyzed structures, shear capacity was generally not exceeded, implying that flexural behavior and associated deformations were the primary concerns for damage.
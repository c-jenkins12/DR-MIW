DR MIW: Dispersion Relation Magneotinductive Waveguide Design Tool

A MATLAB-based tool for the synthesis of magnetoinductive waveguide (MIW) designs based on circuit parameter estimation and analytical models for MIW dispersion.

**SETUP**

*.MLAPP VERSION*
1) Ensure MATLAB 2023b or later is installed on your PC along with the signal processing and symbolic math toolboxes
   Note: Older versions of MATLAB may work, but have not been verified
2) Download the .mlapp file and Images.zip from the repository
3) Unzip Images.zip to the same directory as the .mlapp file
4) Open the .mlapp file using MATLAB
5) Press run - the images directory should be automatically detected by the script

*.exe version*

The .exe version of the script is contained in the IntegratedDispersionToolvXXX.zip file. For further information on how to use this, along with the MATLAB Runtime Compiler see the available mathworks documentation

*.prj version*

A .prj version of this script is also available. See the appropriate mathworks documentation to setup the tool via the .prj file.

**HOW TO USE**

For a detailed operation and explanation of background theory and relevant approximations, please refer to the article titled "DR MIW: Dispersion Relation Magneotinductive Waveguide Design Tool" published in IEEE Antennas and Propagation Magazine. A brief explanation is contained below:

On Startup, the user is provided with several available options. The image and parameter fields dynamically change depending on the selected MIW Type (axial, planar, dual-layer planar, and axial-planar), Configuration Setup (circuit model or geometric model), Slider Setup (circuit model or geometric model) and selected Loop Shape (circular or rectangular). 
For each possible configuration, pressing the Default button will automatically fill out the parameter fields corresponding to an example MIW design. Pressing the Reset button will return the program to the startup state. When a value is entered into the Starting Values field, the minimum and maximum values are automatically calculated, but can be changed by the user without error. 

Once the necessary parameters are inserted, the Resonant Frequency field will automatically update based on the Capacitance and Self-Inductance of the selected model and the typical resonance definition. Since the resonant frequency of the structure is very important to MIW function, this field can be used to adjust values until the desired resonance is achieved without requiring a full solution of the dispersion relation. 
Once the Confirm button is pressed, a series of calculations will occur in the background depending on the configuration setup to determine the equivalent circuit model parameters. From here, the dispersion relation of interest is solved either analytically (first order coupling only) or numerically (higher-order coupling) for the propagation constant.

When the solution is complete, the tab will automatically switch to the Dispersion Relations tab. If any error is present in the setup, the tab will not be switched and instead an error message will display. Note that pressing the Confirm button is the only time the two tabs interact with one another, and will overwrite any active design in the Dispersion Relations tab. If the button is not pressed, no changes made in the Startup tab will affect the Dispersion Relations tab.

Four figures are generated in the Dispersion Relations tab showing the real and imaginary parts of the propagation constant and terminal impedance across frequency. Five text areas are also filled with important specifications related to the current design: minimum loss, bandwidth, resonant frequency, terminal impedance, and solution order used. These specification fields can be removed by accessing the Display tool bar and deselecting the field(s) of concern. The units used in the figures and in the specification fields can be changed in the Units tool bar. Specifically, the frequency can be listed in Hz, MHz, GHz and as a fraction of the resonant frequency and the loss can be listed in linear or log scale. In the Calculation tool bar, the bandwidth calculation can be switched between a theoretical limit as determined by the first-order dispersion relation or by a change from the minimum loss. In the same menu, the terminal impedance can either be listed at the resonant frequency or at the frequency of minimum loss.

Each parameter from the Startup tab has a corresponding slider and field on the Dispersion Relation tab. Adjusting any slider leads to the generation of a new solution and an update of the figures and specification fields. Entering in a new value into one of the parameter fields will have the same effect, but it will also cause the limits of the corresponding slider to change such that the new value is the center value of the slider.

At any point while using the DR MIW Tool, the state of the app can be saved by accessing the Save As button under the File toolbar. The state will be saved as a .mat file which can then be loaded at any time by pressing the Open button under the File toolbar and selecting the appropriate file in the pop-up window. If the Save As button has been used or if a file has been opened, the Save button will overwrite the file with the new state of the application if pressed.

The Circuit Model configuration setup allows the user to directly input the equivalent circuit parameters of the MIW (e.g., resistance, mutual inductance, etc.). This implementation is particularly useful for the design of non-loop based MIWs as the dispersion relations are generalizable to arbitrary shapes so long as the circuit parameters are accurate, and the shape is electrically small. To reduce complexity of user interaction, the current Circuit Model configuration is limited to first-order solutions as any higher orders would require direct input of many more mutual inductance values. This reduction in complexity also allows the solution to be solved analytically in both the single-loop and dual-loop element MIW designs, drastically decreasing computational load and iteration time. As such, this model is also suitable for experienced designers who have a strong understanding of how changes in equivalent circuit parameter values correspond to changes in geometry. Additionally, because the geometry is not considered in this mode, selecting the Axial MIW type will yield the same solution as selecting the Planar MIW type and likewise with the Dual-Layer Planar MIW type and Axial-Planar MIW type since each pair has the same dispersion relation.
The Geometric Model configuration setup is significantly more complex than the Circuit Model. Before the dispersion relation can be solved, the equivalent circuit parameters must first be estimated based on the entered geometry. The three parameters of interest are: resistance, self-inductance, and mutual inductance.
 
Many of these calculations, particularly those for mutual inductance, can take much longer to complete than the Circuit Model configuration. As such, the Geometric Model is always slower than the Circuit Model, and greater complexity in geometry leaves to longer computation times (e.g., a geometric dual-layer planar MIW solution would take longer to calculate than a geometric axial MIW).

Higher-order coupling is only available for Geometric Model inputs. As such, for higher-order coupling solutions, the tool solves these numerically using built in MATLAB functions. These solutions can be unstable and lead to spurious results, particularly as N increases because of the potential for multiple solutions. A strange result of the numerical solution is that even values of N cannot be used as the results are severely incorrect. These same errors become more pronounced for the dual-loop element MIWs. A solution to controlling this error has not been found for these MIWs. As such, higher-order coupling is not currently implemented for dual-loop element MIWs.

To reach a higher-order solution for the single-loop element MIWs, the first-order solution is first solved for analytically. Then, N mutual inductance values are calculated. N can either be set exactly by the user or can be determined based on the percent change of the mutual inductance values from the initial first-order value and a user-defined threshold. The mutual inductance values are then used to create a symbolic expression in terms of the propagation constant according to the multi-order dispersion relation. The propagation constant is then solved for numerically, using the first-order analytic solution as an initial guess to speed up the solver. 

Because several mutual inductance values must be calculated, and then a potentially complex symbolic expression must be solved numerically, higher-order solutions take significantly longer to reach than first-order solutions. As such, it is not recommended to use the higher-order solution mode throughout the entire design process. Instead, use the first-order solution to reach a strong starting point and then use the higher-order solutions to make any additional small adjustments.


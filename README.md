# University Project

This project which I wrote for my University Dissertation explores how to create a program that identifies radionuclides
based on their gamma-ray spectrometry. It investigates the mathematics behind nuclear decay and derives the decay equation. In doing so, this project
provides a structured method in which to analyse gamma-spectra and the
reasoning behind doing so. The importance of this analysis has many applications in medicinal imaging and use by LabLogic Systems. Analysis within
this report contains many details and methods that are useful to this application.

## Science Behind Programming

This project will discuss the creation of an algorithm for LabLogic Systems Ltd, which can identify radioactive isotopes based on their gamma spectroscopy. When gamma spectroscopy is performed on these radioactive isotopes, the resulting keV values can be plotted on a graphical interface to produce Gaussian curves. The software provided by LabLogic Systems, known as ‘Laura’, plots the energy levels (keV) against the counts of the corresponding keV values within a given time. These Gaussian curves have peak energy values, which can be used to identify any radioactive isotope. The specific radioactive isotope can be identified from the gamma spectroscopy peaks. Peak energy values can be compared to the tables of collated data provided by the Laboratoire National Henri Becquerel (LNHB). Comparing the characteristics of radioactive isotopes from the LNHB and cross-examining those characteristics can identify which radioactive isotopes are present within the sample. In addition, a quantitative purity analysis can be produced for any given gamma spectra. This can be used to decompose a given isotope into its decaying components. In addition, a quantitative purity analysis for any given gamma spectra and decompose a given isotope into its decaying components.

##Explanation Behind How The Program Works

The final version of my program uses two rolling standard deviations, to create an array of the top 15 standard deviations in each half of the dataset. If within these top 15 standard deviations there exists a high density of keV values around where a peak is expected for a given isotope, the program will identify these spectra as that isotope and return this to the user. Every radionuclide has unique properties where the gamma transitions occur. Using this property, we can identify any given radionuclide. The top standard deviations, which indicate a peak, can be utilised to complete this identification.
The outputs of the program and the data from the LNHB are included in Figure 2.7 and Figure 2.8.


![Screenshot 2025-03-27 030139](https://github.com/user-attachments/assets/10f2b518-0910-4a4c-badb-08529b831972)
![Screenshot 2025-03-27 030200](https://github.com/user-attachments/assets/366593ef-dbca-46fa-8c8a-c38761440c8a)

# University Project

This project which I wrote for my University Dissertation explores how to create a program that identifies radionuclides
based on their gamma-ray spectrometry. It investigates the mathematics behind nuclear decay and derives the decay equation. In doing so, this project
provides a structured method in which to analyse gamma-spectra and the
reasoning behind doing so. The importance of this analysis has many applications in medicinal imaging and use by LabLogic Systems. Analysis within
this report contains many details and methods that are useful to this application.

## Science Behind Programming

This project will discuss the creation of an algorithm for LabLogic Systems Ltd, which can identify radioactive isotopes based on their gamma spectroscopy. When gamma spectroscopy is performed on these radioactive isotopes, the resulting keV values can be plotted on a graphical interface to produce Gaussian curves. The software provided by LabLogic Systems, known as ‘Laura’, plots the energy levels (keV) against the counts of the corresponding keV values within a given time. These Gaussian curves have peak energy values, which can be used to identify any radioactive isotope. The specific radioactive isotope can be identified from the gamma spectroscopy peaks. Peak energy values can be compared to the tables of collated data provided by the Laboratoire National Henri Becquerel (LNHB). Comparing the characteristics of radioactive isotopes from the LNHB and cross-examining those characteristics can identify which radioactive isotopes are present within the sample. In addition, a quantitative purity analysis can be produced for any given gamma spectra. This can be used to decompose a given isotope into its decaying components. In addition, a quantitative purity analysis for any given gamma spectra and decompose a given isotope into its decaying components.

## Explanation Behind How The Program Works

The final version of my program uses two rolling standard deviations, to create an array of the top 15 standard deviations in each half of the dataset. If within these top 15 standard deviations there exists a high density of keV values around where a peak is expected for a given isotope, the program will identify these spectra as that isotope and return this to the user. Every radionuclide has unique properties where the gamma transitions occur. Using this property, we can identify any given radionuclide. The top standard deviations, which indicate a peak, can be utilised to complete this identification.
The outputs of the program and the data from the LNHB are included in Figure 2.7 and Figure 2.8.


![Screenshot 2025-03-27 030139](https://github.com/user-attachments/assets/10f2b518-0910-4a4c-badb-08529b831972)
![Screenshot 2025-03-27 030200](https://github.com/user-attachments/assets/366593ef-dbca-46fa-8c8a-c38761440c8a)

## Code Walkthrough
In this section, I will discuss the program I wrote in MATLAB, making comments where necessary and providing the inputs needed to run the program. The first section of the program takes the input of text files. These text files contain the data provided by LabLogic systems and are then imported into arrays. These arrays are labelled according to which radionuclide the data belongs to. Additionally, these arrays contain the name of the detector model used to collect the data. For example, cs1373b3p can be decomposed to be Cs-137 3x3 planar. Since MATLAB does not allow ‘x’ in array names adjacent to numbers, instead of ‘x’, the program uses ‘b’. Arrays containing 2b2p are short for 2x2 planar, and 2b2w is short for 2x2 well.

```MATLAB
keVvalues3b3p = importdata("keVvalues3x3.txt");
cs1373b3p=importdata("cs137-3x3.txt");
ba1333b3p=importdata("ba133-3x3.txt");
na223b3p=importdata("na22-3x3.txt");
co603b3p=importdata("co60-3x3.txt");
keVvalues2b2p = importdata("keVvalues2b2p.txt");
ba1332b2p = importdata("ba1332b2p.txt");
cs1372b2p = importdata("cs1372b2p.txt");
co602b2p = importdata("co602b2p.txt");
keVvalues2b2w = importdata("keVvalues2b2w.txt");
ba1332b2w = importdata("ba133well.txt");
cs1372b2w = importdata("cs137well.txt");
co602b2w = importdata("co60well.txt");
na223b2w = importdata("na22-2b2w.txt");
```
The next section of the code asks the user to select the detector type used, whether 2x2 planar or 3x3 planar. The user is then asked which isotope they would like to analyse (the user could alternatively import a different text file.) If they would like to analyse a spectra without knowing its identity. Finally, the variable q takes the size of the count array, known as data.

```MATLAB
keVvalues = input("Enter detector model ") ;
data = input("Enter Radionuclide to identify ");
q=(size(data,2));
```
The following code is the code for the rolling standard deviation of the
data. This section takes the standard deviation iteratively of the first point
in the array data, then the first and second points, followed by the points
x0,x1 and x2. It then stores these in an array which I have labelled result and
result2. The first for loop repeats over half of the array data and the second
for loop repeats over the second half. The reason for doing this process twice
is that for certain isotopes, such as co60, the program will create higher
standard deviations weighted towards the beginning of the loop. Clearing
the loop allows the rolling standard deviation to more accurately identify
peaks in the second half of the dataset.
The variable ‘sd’ takes the standard deviation of the current point x<sub>i</sub> and
‘average’ takes the average of all points before and including x<sub>i</sub>.
.

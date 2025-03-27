# University Project

This project which I wrote for my University Dissertation explores how to create a program that identifies radionuclides
based on their gamma-ray spectrometry. It investigates the mathematics behind nuclear decay and derives the decay equation. In doing so, this project
provides a structured method in which to analyse gamma-spectra and the
reasoning behind doing so. The importance of this analysis has many applications in medicinal imaging and use by LabLogic Systems. Analysis within
this report contains many details and methods that are useful to this application. All code attatched can be pasted into MATLAB for analysis, any data however was forbidden to share and protected under a non-disclosure agreement and so cannot be shared.

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
x<sub>0</sub>,x<sub>1</sub> and x<sub>2</sub>. It then stores these in an array which I have labelled result and
result2. The first for loop repeats over half of the array data and the second
for loop repeats over the second half. The reason for doing this process twice
is that for certain isotopes, such as co60, the program will create higher
standard deviations weighted towards the beginning of the loop. Clearing
the loop allows the rolling standard deviation to more accurately identify
peaks in the second half of the dataset.
The variable ‘sd’ takes the standard deviation of the current point x<sub>i</sub> and
‘average’ takes the average of all points before and including x<sub>i</sub>.
```MATLAB
result =[];
sd=0.0;
sum=0.0;
average=0.0;
for i = 1:q/2
sum = sum + data(i);
average = sum/(i);
sd = sqrt((data(1,i)-average)^2)/(i);
result(end+1)=(sd);
end
result2 =[];
sd=0.0;
sum=0.0;
average=0.0;
for i = ((q/2)+1):q
23
sum = sum + data(i);
average = sum/(i);
sd = sqrt((data(1,i)-average)^2)/(i);
result2(end+1)=(sd);
end
```
Variables ‘I’ and ‘J’ are the top 15 highest standard deviations from the loop
that created result and result2 respectively. Finding the positions of these
standard deviations, relative to the corresponding keV values, is important
to return the peak and label the isotope at the end of the program.
```MATLAB
I=maxk(result,15);
J=maxk(result2,15);
```
The loop in the following code writes the keV values and their corresponding counts to a 2d array. This means that the 2d array contains the top
highest standard deviations from earlier, which are the peaks of the gamma
spectra. These keV values and counts are then written to peaks and peaks2
respectively.
```MATLAB
peaks=[];
peaks2=[];
for p = 1:15
linearIndices1 = find(result==I(1,p));
linearIndices2 = find(result2==J(1,p));
peaks=[peaks;keVvalues(1,linearIndices1),data(1,linearIndices1)];
peaks2=[peaks2;keVvalues(1,(linearIndices2+(q/2))),data(1,(linearIndices2+512))];
end
```
The following code plots the gamma spectra. Similar to the program Laura,
the gamma spectra are displayed in a window when the program has finished
running. Axes are labelled with appropriate labels relating to the counts and
keV values.
```MATLAB
plot(keVvalues,data);
xlabel(’keV Value’)
ylabel(’Counts’)
title(’Gamma Spectroscopy’)
```
Below are the criteria for identifying which radionuclide has been analysed. This data is taken directly from the LNHB[1]. The criteria for Cs-137
states that, if there exists a keV value in peaks between 655 and 665, then
it must be Cs-137. This is because only Cs-137 would have a peak between
655 and 665.
```MATLAB
ba133=peaks(peaks>88.0 & peaks<92.0);
co60=peaks2(peaks2<1317.0 & peaks2>1302.0);
na22=peaks2(peaks2<1280.0 & peaks2>1270.0);
cs137=peaks(peaks<665.0 & peaks>655.0);
```
The final section of code below is a collection of selective statements, which
determine what to return to the user. If the criteria for Ba-133 is satisfied,
then the variable ‘ba133’ will not be empty, then the program will return a
statement telling the user that the sample must contain Ba-133.
```MATLAB
if isempty(cs137)== false
disp("the sample contains cs137")
elseif (isempty(ba133) == false && length(ba133)>=length(co60))
disp("the sample contains ba133")
elseif (isempty(co60) == false && length(co60)>length(ba133))
disp("the sample contains co60")
elseif isempty(na22) == false
disp("the sample contains na22")
else
disp("error")
end
```
## Inputs and Results

To run this program, only two inputs are needed, the first input is the detector
type. The detector types are listed below.
```
Input           Detector
keVvalues3b3p - 3x3 planar
keVvalues2b2p - 2x2 planar
keVvalues2b2w - 2x2 well
```
The user will be prompted to input the radionuclide they want to identify,
this could be a text file of their own or one of the text files that I have
imported into the program. This text file should contain all the counts of
the energy values. As can be seen in Figure 4.1, the program will then output the data plotted against the corresponding keV values. Additionally, the program will
return the radionuclide that it identifies.

![3](https://github.com/user-attachments/assets/782d75f1-1165-4856-bdea-59f85ce1bc3d)

If the program is run in a modern high-level programming language, the arrays will be accessible and will arrays contain the peaks of individual spectra.
Information about the standard deviations can also be accessed within the
corresponding arrays that I have created. An example of this can be seen in
Figure 2.8 (See Above).


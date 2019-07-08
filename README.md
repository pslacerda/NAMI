NAMI
====

GUI-based Python program for thermal shift assay data analysis and interpretation

-----------------------------

Please cite: Groftehauge et al. 2014. Acta Cryst. D

-----------------------------

### 1. Installation

*If you cannot get the Python code to run (the instructions are terribly out-dated to be honest) please contact me on Twitter https://twitter.com/DrGroftehauge for pre-compiled executable files for Linux, Mac, or Windows. If you have a good suggestion for where to host the files for distribution please contact me.*

This chapter explains how to install the required programs to run NAMI on either Linux, Mac, Windows, or preferably on a Anaconda system.

NAMI runs through Python and currently requires the installation of Python 2.7 and the common modules matplotlib, numpy, scipy, and pyqt. Python 3 is not supported.

**1.1 Anaconda**

If Anaconda is installed on the system, an environment can be created to install NAMI.

```bash
conda create --name nami python=2.7
conda activate nami
conda install scipy=1.2.0 icu pyqt=4 matplotlib # or maybe icu=56.1 matplotlib=2.0.0b1

# required by the StepOnePlus convert script
conda install pandas=0.23.4 xlrd=1.2.0
```

To start NAMI you could run:

```bash
conda activate nami
python ./NAMI.py
```

***Anaconda multi-user Windows install***

For sharing an Anaconda environment between multiple users, ensure that Anaconda was installed system-wide with adminstrator privileges the environemnt and copy the environement to the `C:\Users\Public\AppData` folder. The environment to be copied probably is at `C:\Users\YourUser\Anaconda3\envs\nami`.

These general tips can be used to share any Anaconda environement. If your computer is registred at some domain, call the support team.

```bat
@REM example file, place it into C:\Users\Default\Desktop\NAMI.bat
call "C:\ProgramData\Anaconda3\Scripts\activate.bat" C:\Users\Public\AppData\nami
python C:\Users\Default\AppData\nami\NAMI\NAMI.py
```

Or you may want:
```bat
@REM example file, place it into C:\Users\Default\Desktop\NAMI.bat
@REM use it to run a specific conversion script right before start NAMI
@REM drag the original file and drop over this script
call "C:\ProgramData\Anaconda3\Scripts\activate.bat" C:\Users\Public\AppData\nami
python C:\Users\Default\AppData\nami\NAMI\ConvertScripts\steponeplus_v2.2-to-NAMI.py %1
cd %~dp1
python C:\Users\Default\AppData\nami\NAMI\NAMI.py
```

**1.2 Linux**

Most Linux distributions have Python installed as default. However, an older distribution might have Python 2.6 or earlier and it should not be replaced since so many programs depend upon it. The simplest way forward is to upgrade Linux, use virtualenv, or possibly use the Enthought Python Distribution (EPD). The use of virtualenv or EPD to run parallel Python versions is outside the scope of this manual however. 

The libraries matplotlib, numpy, scipy, and pyqt4 should all be available in the package manager but may be prefixed with "python-". 

If the version available in the package manager is not recent enough to run NAMI you can easily install the updated version using ‘pip’. Here’s an example from Ubuntu:
```
sudo apt-get install pip
sudo apt-get remove python-matplotlib
sudo apt-get build-dep python-matplotlib
sudo pip install matplotlib
```
Build-dep adds all the things that matplotlib depends on to run - it might not be necessary but it won’t hurt. Pip is preferable to easy-install as it has a better uninstall function (pip remove). 

**1.3 Mac**

Instructions coming. Enthought is also available for Mac but there doesn't seem to be anything as simple as WinPython. 

**1.4 Windows**

The simplest way is to use WinPython which comes with the relevant modules built in ([http://winpython.sourceforge.net/](http://winpython.sourceforge.net/)). Download the most recent 2.7 version.
Even without administrator privileges you can still launch the WinPython Command Prompt and run python and NAMI.

Otherwise Enthought supplies a Python distribution that easily enables the installation of 100+ common Python libraries. They have two free versions - one called Enthought Canopy Express with a reduced number of libraries ([http://www.enthought.com/canopy-express/](http://www.enthought.com/canopy-express/)) and a yearly renewing free academic license for Enthought Canopy ([http://www.enthought.com/products/canopy/academic/](http://www.enthought.com/products/canopy/academic/)). 

### 2. Running NAMI

**2.1 Execution** 

Open a terminal (Linux / Mac) or command prompt (Windows) and navigate to the directory where you want to output your results. If you have placed NAMI.py in that directory (or placed it in a directory that your PATH points to) executing it is as simple as writing 

`python NAMI.py`

otherwise you can prefix the path to NAMI.py. Example for Linux / Mac:

`python /home/user/somedirectory/NAMI.py`

Example for Windows:

`python C:\\somedirectory\NAMI.py `

**2.2 Convert input files**

NAMI flexibly reads text files saved as comma separated values (.csv), skipping the first line to avoid headers. All spreadsheets, e.g. LibreOffice Calc or Microsoft Office Excel, can save in .csv format but remember to use comma as the field separator. We are working to add Python scripts to convert the raw, exported data from any qPCR software but we need examples to work from. Python script contributions are gratefully accepted - please remember to include the qPCR machine model and software version number. An example is given in the ConvertScripts folder. The syntax is 

`python convert_script.py exported_data.csv`

**2.3 Set parameters**

Click "Set Input Parameters". Click “Raw data” to choose the file containing the fluorescence intensities.  Example raw data files are provided in the directory ExampleFiles, e.g. glucoseisomerase-salt-Mg.csv. 

Click "Solutes file" to provide a file with information about the experimental contents of each well. This is required to plot the temperature of hydrophobic exposure (T<sub>h</sub>) vs concentration or pH etc. but not necessary for determination of T<sub>h</sub>. An example raw data files is provided in the directory ExampleFiles, e.g. solsalt.sol. 

Click "Results from a previous run" to provide a text file from a previous run with T<sub>h</sub> and skip the data processing and go directly to the plotting tools. Raw data are still required since the waterfall plot option uses the raw data. **Note: resume from previous results is temporarily broken**

Specify which columns contain well numbers, temperatures, and fluorescence intensities. On some machines the temperature is not specified but only the number of the reading is given. So if the first reading was at 24 degrees then a temperature offset of 23 should be input. You could also use this to get your results in Kelvin rather than Celsius.

Specify what the temperature step increment multiplier is. We have excellent results with 1 degree steps but some qPCR machines are extremely fast and some people prefer to sample fluorescence more often. This should be 1 in most cases. It is only neccessary to change if you both have a temperature increment that is different from 1 and your machine doesn't output temperatures but only reading number. 

Specify the number of wells. If your data file contains fewer wells than you input then NAMI will crash. However, you can also specify fewer wells than are present in the data file to avoid spending time processing them.

Click "Apply" and “Close”.

**2.4 Data processing**

Click "Begin Data Analysis". The tick boxes “Automatic” and “Save Graphs” respectively control whether to run the data processing one well at a time or without user input and whether to save the graphs or not. Graphs are saved as individual .png files. 

At the very bottom of the window is a text box which allows the user to override the automatic algorithm and specify a T<sub>h</sub> or write that no signal is found. In particular, the algorithm for detecting when there isn’t a signal in the data is not infallible and may require user input.

After data processing is completed a results.csv file is written out with three columns, first melting point, second melting point (if detected), and user input (if specified).

**2.5 96-well table**

At the end of data processing a pop-up allows the input of a reference temperature and a significant divergence from that reference. Click "Show table" and a colour-coded 96-well table shows up in the main window with the T<sub>h</sub> displayed. All wells with T<sub>h</sub> within +/- the significant divergence of the reference temperature are white, the ones below are graded from yellow to red and the ones above are graded from light blue to dark blue; So red indicates destabilising conditions while blue indicates stabilising conditions. Grey wells either (a) did not have a discernible signal, “N/S”, or (b) were beyond the number of wells set to be processed, “N/A”. Wells are coloured green when NAMI detects two melting transitions. 

If a .sol file was provided before data processing the contents of each well is displayed when the mouse pointer hovers over a well in the table.

**2.6 Various plots**

After setting the reference temperature and generating a table there are various additional plotting options available. 

Go to the "Data" menu and click “Plots for Analysis”. Choose one or more solutes and click one of the plot options in the bottom of the window. The x-axis can be set to a linear or log scale. Log scale is needed for dilution series (keep in mind that ligand affinity must be higher than binding site concentration if approximating binding affinity unless “affinity better than this value” is good enough - and also that the T<sub>h</sub> is usually quite far from the physiological temperature).

**Waterfall** plots allows you to inspect raw data for a single solute at various concentrations more quickly than by finding each graph saved as a png. This allows you to easily estimate if the noise in the data or two-stage denaturation changed with solute concentration. The graphs are colour-coded in the same scheme as the 96-well table but uses purple instead of white when there is no difference from the reference. The graph can be rotated with the mouse.

**T<sub>h</sub> vs concentration** accepts any number of solutes and gives you an idea of how the influence of each solute on thermal stability changes with concentration. For salts the influence of both anions and cations are of course aggregated so if a hypothetical protein is destabilised by, say, Cl<sup>-</sup> and unaffected by Na<sup>+</sup>, NH<sub>4</sub><sup>+</sup>, and SO<sub>4</sub><sup>2-</sup> then both NH<sub>4</sub>Cl and NaCl should destabilise it while (NH<sub>4</sub>)<sub>42</sub>SO<sub>4</sub> and Na<sub>2</sub>SO<sub>4</sub> should not.

**T<sub>h</sub> vs pH** accepts any number of solutes and corrects the pH by the T<sub>h</sub> (since pK<sub>a</sub> is temperature dependent) before plotting the values. As with salts the effect of H<sup>+</sup> concentration is reflected in aggregate with the effect of a buffer molecule. Sometimes a buffer molecule will bind to a protein and influence the energy required to denature that protein. 

Click "Save" to save the current view of any plot.

**2.7 Output**

The output is results.csv a text/csv file of T<sub>h</sub>, the 96-well table as a .png, and (optionally) each plot as a .png. All files are saved in the directory from which you ran python.

### 3. Interpretation

**3.1 High affinity divalent cations**

Metal-binding proteins are quite common. If a metal chelator, e.g. EDTA or EGTA, destabilises your protein then it is quite likely that the protein co-purified with a divalent cation. If you also observe a stabilising effect from one or more divalent cations then it is even more likely. For the follow-up assay you should add the metal chelator to your protein solution and then do a buffer exchange to a buffer without the chelator or the divalent metal ion. Then perform either your initial screen again or a dose-response screen based on the stabilising cations from the first screen.

You can use various assays to determine the effects of stabilising ions. Stabilising ions can both be activators and inhibitors of activity. Destabilising ions are typically non-specific protein denaturants. The transition metal ions are particularly effective at this. 

EDTA is often used in lysis buffers as a protease inhibitor but if the folding of the protein of interest is highly dependent on divalent metal ions then EDTA could both leave it vulnerable to proteases and liable to spontaneously denature. 

This information is also highly valuable to crystallographers since phase information can be retrieved from bound metal ions (some of them). 

**3.2 High concentration salts**

Typically salts with divalent anions **may** have a stabilising effect at high concentration. For protein storage there will be an optimal salt concentration at which the protein is most soluble and it is not necessarily the concentration at which the protein is most stable. 

NaF is interesting for circular dichroism experiments and for crystallisation you can try to use a protein buffer with maybe 1 M of a very stabilising salt. You may move some screening conditions from vapor diffusion to batch crystallisation but as long as you get diffracting crystals it doesn’t actually matter how. 

**3.3 pH screening**

For pH screening NAMI will correct by delta-pK<sub>a</sub> / degree when plotting T<sub>h</sub> vs pH. Delta-T<sub>h</sub> depends on both the pH and the buffer molecule used so try multiple buffers at the same pH to determine what changes to attribute to pH and what to attribute to the buffer molecule. pH correction is important since delta-pK<sub>a</sub> differs highly between buffer molecules, e.g. the pKa of Tris decreases by 1.86 between 20 and 80 degrees Celsius (and obviously the pH of a solution buffered by Tris would decrease by the same amount). The pK<sub>a</sub>s of the protein's ionisable groups also change with temperature but will change the same in each experiment since the protein is the same. This is by the way one of the many reasons you should not use Tris unless you've tried HEPES, MOPS, bis-tris (propane and methane), tricine, bicine and they didn't work in your experiment.

In our experience protein stability always changes somewhat with pH but is not predictive of crystallising conditions. But a specific buffer molecule may be neccessary for crystallisation because it binds in some site (even if the buffer molecule is a weak ligand, it is present at 100 mM). 

-----------------------------

Please cite: Groftehauge et al. 2014. Acta Cryst. D

-----------------------------

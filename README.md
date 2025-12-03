# Thembisa (version 4.8)

This repository contains code and supporting files for the Thembisa HIV/TB model. Thembisa is a mathematical model of the South African HIV epidemic, designed to answer policy questions relating to HIV prevention and treatment. Thembisa is also a demographic projection model and a source of demographic statistics. Recently the model has also been extended to include tuberculosis.

The model was developed by Prof Leigh Johnson, Prof Rob Dorrington, and Dr Mmamapudi Kubjane. More details can be found at the official Thembisa website: https://www.thembisa.org/.

# Software Requirements

The model is written in C++ and built using Microsoft Visual Studio (Windows) or via compatibility-modified source files on Linux/MacOS. For Linux/MacOS users, make sure to implement compatibility changes (see below).

# Directory Structure

```
├── ./
│   .DS_Store  
│   .gitignore  
│   AssemblyInfo.cpp  
│   README.md  
│   StatFunctions.cpp  
│   StatFunctions.h  
│   THEMBISA.cpp  
│   THEMBISA.h  
│   THEMBISA.sln  
│   app.ico  
│   app.rc  
│   mersenne.cpp  
│   randomc.h  
│   stdafx.cpp  
│   stdafx.h  
│   ├── calibration-outputs/
│   ├── outputs/
│   │   ├── Backup/
│   │   ├── Debug/
│   │   ├── Release/
│   │   ├── Template/
│   │   ├── ipch/
│   ├── inputs/
│   │   ├── TBinputs/
```

# Running the Model

1. **Setup**

    This project uses Unix-style LF line endings for all text files. These settings ensure consistent behavior across all platforms.
    Run this once: ``git config --global core.autocrlf false``. Please do not adjust the `.gitattributes` file.

    Clone the full directory structure, including inputs, outputs, and calibration-outputs: ``git clone <repo>``

    Ensure compatibility modifications are applied if not using Windows (see section: Compatibility Changes)

2. **Compilation**

    Open THEMBISA.sln in Visual Studio and build the solution.

    Alternatively, compile on Linux/MacOS using g++ with required modifications.

# National HIV Model
## Calibration

There are data calibration outputs needed to simulate HIV transmission both nationally and for each province. Calibration outputs include: 

```
├── calibration-outputs/
│   ARTerror.txt  
│   Cholesky.txt  
│   Cholesky1.txt  
│   Diagnostics.txt  
│   Distance.txt  
│   FractionUnique.txt  
│   Inverse.txt  
│   Inverse1.txt  
│   LogL.txt  
│   LogLxWeight.txt  
│   MixtureMean.txt  
│   ModelParameters.txt  
│   NextMode.txt  
│   PosteriorMeans.txt  
│   RandomParameter.txt  
│   RandomUniform.txt  
│   Sorted.txt  
│   VRbiasTB.txt  
│   Weights.txt  
│   WeightedCov.txt  
```

For the national calibration of the model, ensure that in **`THEMBISA.cpp`** , the following lines are commented and uncommented respectively: 

```{bash, eval=FALSE}
//RunSample();	
runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}
int FixedUncertainty = 0;
const int VaryFutureInterventions = 0; 
const int VaryFutureInterventionsTB = 0; 
const int FixedARTinitiation = 0;
const int InputARTinitiationRates = 0; 
```

and: 

```{bash, eval=FALSE}
const int ProvModel = 0; 
string ProvID = "KZ"; // variable selection ignored if ProvModel=0
const int UseBrassLogit = 0;
const int IncludeTB = 0; 
const int IncludeDR_TB = 0; 
```


In addition, please ensure the following calibration settings are all set to 1:

* CalibAdultPrev
* CalibANCprev (and also InclANCpre1997 and InclAS_ANCprov)
* CalibDeathsA
* CalibARTbyAge
* CalibARTbyAgeP2
* CalibARTcoverage

Furthermore, the following lines should be specified as: 

```{bash, eval=FALSE}
const int ARTdataPoints = 194; 
const int ARTdataPointsP = 137; 
const int ARTdataPointsM = 4; 
```

and, 

```{bash, eval=FALSE}
double LogLikelihood;
const int MCMCdim = 49; 
const int MaxPriors = 145; 

```

and, 


```{bash, eval=FALSE}
const int nCSWstudies = 35;
double CSWstudyDetails[nCSWstudies][3];
const int nMSMstudies = 20;
double MSMstudyDetails[nMSMstudies+1][4];
```

## Simulation


For the national-level HIV simulation, please ensure that the national calibration has been completed first so that the appropriate calibration-output files are available. Ensure that in the C++ program, **`THEMBISA.cpp`** , the following lines are uncommented and commented respectively: 

```{bash, eval=FALSE}
RunSample();	
//runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}
int FixedUncertainty = 1;
```
Simulation outputs include: 

```
├── outputs/
│   ├── Backup/
│   ├── Debug/
│   ├── Release/
│   ├── Template/
│   ├── ipch/
│   AddnalOutput.txt  
│   AddnalTBoutput.txt  
│   AgeDbnOnART_F.txt  
│   AgeDbnOnART_M.txt  
│   DHScalib2016.txt  
│   ETRbias.txt  
│   ErrorVar.txt  
│   FSW_HIVprofile.txt  
│   FSWincidence.txt  
│   Females15to49.txt  
│   FinalSimplex.txt  
│   HCTbyAge.txt  
│   HSRCcalib.txt  
│   HSRCcalib2002.txt  
│   HSRCcalib2005.txt  
│   HSRCcalib2008.txt  
│   HSRCcalib2012.txt  
│   HSRCcalib2017.txt  
│   HSRCcalib2022.txt  
│   Males15to49.txt  
│   NegChildrenU5.txt  
│   NegLogL.txt  
│   NewHIVafterBirth.txt  
│   NewHIVatBirth.txt  
│   NewHIVinANCandBF.txt  
│   ORdiagDeathsPIP.txt  
│   OutputByAge.txt  
│   OutputByAge2.txt  
│   PrevFSW15to24.txt  
│   PrevFSW25plus.txt  
│   PrevTested05.txt  
│   PrevTested08.txt  
│   PrevTested09.txt  
│   PrevTested12.txt  
│   PrevTested16.txt  
│   PrevTested17.txt  
│   PrevTested22.txt  
│   PropnScreened.txt  
│   PropnScreened2.txt  
│   SummaryOutput.txt  
│   SummaryTBoutput.txt  
│   TBoutputByAge.txt  
│   TestingBias.txt  
│   TotOnCABLA.txt  
│   TotOnPrEP.txt  
│   TotalNewHIV.txt  
```

# Provincial HIV Model

## Calibration


For the provinciall calibration of the model, ensure that in the C++ program, **`THEMBISA.cpp`** , the following lines are commented and uncommented respectively: 

```{bash, eval=FALSE}
//RunSample();	
runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}
int FixedUncertainty = 0;
const int VaryFutureInterventions = 0; 
const int VaryFutureInterventionsTB = 0; 
const int FixedARTinitiation = 0;
const int InputARTinitiationRates = 1; 
```

and: 


```{bash, eval=FALSE}
const int ProvModel = 1; 
string ProvID = "KZ"; // Choose from EC, FS, GT, KZ, LM, MP, NC, NW, WC
const int UseBrassLogit = 0;
const int IncludeTB = 0; 
const int IncludeDR_TB = 0; 
```


In addition, please ensure the following calibration settings are all set to 1:

* CalibAdultPrev
* CalibANCprev (and also InclANCpre1997 and InclAS_ANCprov)
* CalibDeathsA
* CalibARTbyAge
* CalibARTbyAgeP2
* CalibARTcoverage

Furthermore, the following lines should be specified as: 

```{bash, eval=FALSE}
double LogLikelihood;
const int MCMCdim = 18; 
const int MaxPriors = 145; 

```

and, for each varying province, the variables: 

```{bash, eval=FALSE}
const int ARTdataPoints = 194; 
const int ARTdataPointsP = 171; 
const int ARTdataPointsM = 8; 
```

should be: 

| Province  | ARTdataPoints | ARTdataPointsP | ARTdataPointsM |
|-----------|---------------|----------------|----------------|
| EC        | 194           | 180            | 8              |
| FS        | 199           | 180            | 8              |
| GA        | 194           | 180            | 8              |
| KZ        | 194           | 171            | 8              |
| LM        | 194           | 180            | 8              |
| MP        | 194           | 180            | 8              |
| NC        | 194           | 135            | 4              |
| NW        | 150           | 137            | 8              |
| WC        | 194           | 180            | 15             |
| National  | 194           | 137            | 4              |

**Table:** ARTdataPoints, ARTdataPointsP and ARTdataPointsM to be specified in the header file.



Similarly for the number of CSW and MSM studies, the lines: 

```{bash, eval=FALSE}
const int nCSWstudies = 10;
double CSWstudyDetails[nCSWstudies][3];
const int nMSMstudies = 2;
double MSMstudyDetails[nMSMstudies+1][4];
```

should be specified by province as: 
| Province  | CSW Studies | MSM Studies |
|-----------|------------|------------|
| EC        | 4          | 0          |
| FS        | 1          | 1          |
| GA        | 12         | 8          |
| KZ        | 10         | 2          |
| LM        | 1          | 1          |
| MP        | 1          | 2          |
| NC        | 1          | 0          |
| NW        | 2          | 2          |
| WC        | 5          | 4          |
| National  | 35         | 20         |

**Table:** CSWstudies and MSMstudies parameters to be specified in the header file.


## Simulation



For the provinciall simulation of the model, ensure that in the C++ program, **`THEMBISA.cpp`** , the following lines are uncommented and commented respectively: 

```{bash, eval=FALSE}
RunSample();	
//runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}
int FixedUncertainty = 1;
```



# National TB Model

## Calibration

For the calibration of the national-level TB model, ensure that in the C++ program, **`THEMBISA.cpp`** , the following lines are commented and uncommented respectively: 

```{bash, eval=FALSE}
//RunSample();	
runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}

int FixedUncertainty = 0;
const int VaryFutureInterventions = 0; 
const int VaryFutureInterventionsTB = 0; 
const int FixedARTinitiation = 0;
const int InputARTinitiationRates = 0; 

const int PropnalImmART = 1; 
const int ExcludeInterrupters = 1; /
const int UseNumbersTests = 1; 
const int ProvModel = 0; 
string ProvID = "NW"; //will be ignored
const int UseBrassLogit = 0; 
int PrEPorVM = 0; 

const int IncludeTB = 1; 
const int IncludeDR_TB = 0; 
const int FixedTBscreening = 0;
double RRtestingDiagnosed = 1.0; 

const int CalibPaedPrev = 0;
const int CalibAdultPrev = 0; 
const int CalibANCprev = 0; 
const int InclANCpre1997 = 1; 
const int InclAS_ANCprov = 1; 
const int CalibFSWprev = 0; 
const int CalibMSMprev = 0; 
const int CalibCD4 = 0; 
const int CalibCD4ANC = 0; 

const int CalibHCT_HH = 0; 
const int CalibHCT_ANC = 0; 
const int CalibHCTprev = 0; 
const int CalibHCTprevP = 0; 
const int CalibHCTtotP = 0; 
const int CalibHCTageSex = 0; 
const int CalibDeathsA = 0; 
const int AgeLimitMortCalib = 60; 
const int CalibDeathsP = 0; 
const int CalibAIDStrend = 0; 
const int CalibAIDSage = 0; 
const int CalibARTtotals = 0; 
const int CalibARTtotalsP = 0; 
const int CalibCD4atARTstart = 0; 
const int CalibARTbyAge = 0; 
const int CalibARTbyAgeP = 0; 
const int CalibARTbyAgeP2 = 0; 
const int CalibChildPIP = 0; 
const int CalibARTcoverage = 0; 
const int CalibMarriageData = 0; 
const int CalibANC_ART = 0; 
const int CalibAHDpaedART = 0; 

const int CalibTBdeathsA = 1; 
const int CalibTBdeathsPLHIV = 1; 
const int CalibETRdeathsA = 1; 
const int CalibTBcasesA = 1; 
const int CalibTBdiagnosesA = 1; 
const int CalibHIVprevETR = 1; 
const int CalibTBprev = 1; 
const int CalibRifResPropn = 0; 
const int CalibSmPosHH = 0; 
const int CalibRRscreen = 0; 
const int CalibTB_HIV_OR = 1; 
```

and, 

```{bash, eval=FALSE}
const int ARTdataPoints = 194; 
const int ARTdataPointsP = 137; 
const int ARTdataPointsM = 4; 
```

and, 

```{bash, eval=FALSE}
double LogLikelihood;
const int MCMCdim = 25; 
const int MaxPriors = 63; 

```

and, 

```{bash, eval=FALSE}
const int nCSWstudies = 35;
double CSWstudyDetails[nCSWstudies][3];
const int nMSMstudies = 20;
double MSMstudyDetails[nMSMstudies+1][4];
```


## Simulation

For the national-level TB simulation, please ensure that the national calibration has been completed first so that the appropriate calibration-output files are available. Ensure that in the C++ program, **`THEMBISA.cpp`** , the following lines are uncommented and commented respectively: 

```{bash, eval=FALSE}
RunSample();	
//runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}
int FixedUncertainty = 1;
```

# Variables that should not be changed 

There are some variables specified in the **Thembisa.h** file that should not be changed. These variables include: 

- `StartYear = 1985`
- `VaryFutureInterventions = 0`
- `FixedARTinitiation = 0`
- `ExcludeInterrupters = 1`
- `UseNumbersTests = 1`
- `PrEPorVM = 0`

# Compatibility changes

### Linux/MacOS Compatibility Changes Table

| File               | Lines    | Windows                                                                 | Linux/MacOS                                                                 |
|--------------------|----------|--------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `Thembisa.cpp`     | 4–6      | `#include "stdafx.h"`<br>`#using <mscorlib.dll>`<br>`using namespace System;` | `using namespace std;`<br>`#include <iostream>`<br>`#include <cstring>`<br>`#include <sstream>`<br>`#include <string>` |
| `Thembisa.h`       | 7        | `#using <mscorlib.dll>`                                                 | _Delete_                                                                    |
| `StatFunctions.cpp`| 13–15    | `#include "stdafx.h"`<br>`#using <mscorlib.dll>`                         | _Delete_                                                                    |
| `StatFunctions.h`  | 8        | `#using <mscorlib.dll>`                                                 | _Delete_                                                                    |
| `mersenne.cpp`  | 8        | `#include "stdafx.h`                                                 | _Delete_                                                                    |


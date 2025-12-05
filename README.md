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
    

# Compatibility changes

### Linux/MacOS Compatibility Changes Table

| File               | Lines    | Windows                                                                 | Linux/MacOS                                                                 |
|--------------------|----------|--------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `Thembisa.cpp`     | 4–6      | `#include "stdafx.h"`<br>`#using <mscorlib.dll>`<br>`using namespace System;` | `using namespace std;`<br>`#include <iostream>`<br>`#include <cstring>`<br>`#include <sstream>`<br>`#include <string>` |
| `Thembisa.h`       | 7        | `#using <mscorlib.dll>`                                                 | _Delete_                                                                    |
| `StatFunctions.cpp`| 13–15    | `#include "stdafx.h"`<br>`#using <mscorlib.dll>`                         | _Delete_                                                                    |
| `StatFunctions.h`  | 8        | `#using <mscorlib.dll>`                                                 | _Delete_                                                                    |
| `mersenne.cpp`  | 8        | `#include "stdafx.h`                                                 | _Delete_                                                                    |


# National HIV Model

For the national HIV simulation of the model, ensure that in **`THEMBISA.cpp`** , the following lines are commented and uncommented respectively: 

```{bash, eval=FALSE}
RunSample();	
//runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}
int FixedUncertainty = 1;
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


# Provincial HIV Model

## Simulation

For the provincial-level HIV simulation, ensure that in the C++ program, **`THEMBISA.cpp`** , the following lines are uncommented and commented respectively: 

```{bash, eval=FALSE}
RunSample();	
//runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}
int FixedUncertainty = 1;
```

and: 


```{bash, eval=FALSE}
const int ProvModel = 1; 
string ProvID = "KZ"; // Choose from EC, FS, GT, KZ, LM, MP, NC, NW, WC
const int UseBrassLogit = 0;
const int IncludeTB = 0; 
const int IncludeDR_TB = 0; 
```


# National TB Model

## Simulation

For the simulation of the national-level TB model, ensure that in the C++ program, **`THEMBISA.cpp`** , the following lines are commented and uncommented respectively: 

```{bash, eval=FALSE}
RunSample();	
//runIMIS(0.0);
```

In the header file, **`THEMBISA.h`**, ensure

```{bash, eval=FALSE}

int FixedUncertainty = 1;
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

```

# Variables that should not be changed 

There are some variables specified in the **Thembisa.h** file that should not be changed. These variables include: 

- `StartYear = 1985`
- `VaryFutureInterventions = 0`
- `FixedARTinitiation = 0`
- `ExcludeInterrupters = 1`
- `UseNumbersTests = 1`
- `PrEPorVM = 0`



The clas12Tags are a series of clas12 specific tags of the GEMC source code and geometry, installed in /group/clas12/gemc.

![Alt CLAS12](clas12.png?raw=true "The CLAS12 detector in the simulation. The electron beam is going from left to right.")


For Q/A on CLAS12 simulations you can use the [CLAS12 Discourse](https://clas12.discourse.group).

###### The CLAS12 detector in the simulation. The electron beam is going from left to right.



<br>

### Current PRODUCTION version: **4.3.0**, compatible with **COATJAVA 5.7.4** and above. JLAB_SOFTWARE version: 2.3

This points GEMC_DATA_DIR (geometry location) to /group/clas12/gemc/4.3.0

<hr>

To use the latest production tag (currently 4.3.0):

```source /group/clas12/gemc/environment.csh```


<br>

To run gemc use the tagged gcard:

```gemc /group/clas12/gemc/4.3.0/clas12.gcard -N=nevents -USE_GUI=0 ```



<br><br>

Magnetic Fields
---------------

You can scale magnetic fields using the SCALE_FIELD option. To do that copy the gcard somewhere first, then modify it. The gcard can work from any location.
Example on how to run at 80% torus field (inbending) and 60% solenoid field:

```
 <option name="SCALE_FIELD" value="TorusSymmetric, -0.8"/>
 <option name="SCALE_FIELD" value="clas12-newSolenoid, 0.6"/>
```

<br><br>

Hydrogen, Deuterium or empty target
-----------------------------------



Starting with 4.3.1: 
By default the target cell is filled with liquid hydrogen by specifying the "lh2" target variation. 
To use liquid deuterium instead use the variation "lD2" instead.

To use an empty target instead, use the SWITCH_MATERIALTO option.
```
 <option name="SWITCH_MATERIALTO" value="G4_lH2, G4_Galactic"/>
```

In 4.3.0 to use the LD2 target you still need to set SWITCH_MATERIALTO to LD2, as the variation is not there:
```
 <option name="SWITCH_MATERIALTO" value="G4_lH2, LD2"/>
```

<br><br>


Removing a detector or a volume
-------------------------------

You can remove/comment out the ```<detector>``` tag in the gcard to remove a whole system.
To remove individual elements, use the existance tag in the gcard. For example, to remove the forward micromegas:

```
       <detector name="FMT">
                <existence exist="no" />
        </detector>
```

<br><br>

How to install
----------------

Starting with 4a.2.4 gemc is distributed  <a href="https://gemc.jlab.org/gemc/html/docker.html"> using docker</a>.

<br><br>

How to get and compile the clas12Tags
-------------------------------------

The clas12tags can be installed on top of an existing [jlab installation.
For 4.3.0 it's JLAB_VERSION 2.3](https://www.jlab.org/12gev_phys/packages/sources/ceInstall/2.3_install.html):

1. clone https://github.com/gemc/clas12Tags
2. cd to the tag you want to use
3. type `<scons -j4 OPT=1>`

This will compile a gemc executable in that directory. Remember to use the full path to that executable when running gemc,
otherwise the OS will pick up the default from the $GEMC env variable.

Each tag has the production gcard inside its directory. To use: change the path from 
``` /group/clas12/gemc/4.3.0 ``` to the proper location of your disk.

<br><br>

GCARDS and Various CLAS12 experiments
=====================================

See  <a href="https://github.com/gemc/clas12Tags/tree/master/gcards"> gcards and experiments</a>.

<br><br>


Software, Geometry Tags
=======================

<br>
	
Production:
-----------

<br>

- 4.3.0: **COATJAVA: 5.7.4**, **JLAB_VERSION: 2.3**:

	- Updated DC geometry using latest survey (May 18 Entry in DB) 
	- Fixed bug that prevented material name from being displayed in the GUI
	- 3d cartesian field map support
	- new geant4 version: 10.4.p02
	- 51 micron tungsten shield (for bst) surrounding the target
	- calorimeters: reading ecal effective velocity from CCDB
	- change htcc time offset table to use the same used in reconstruction
	- Tony Forest: Added polarized target geometry/material and cad volume.


<br>


In development:
---------------

<br>

- 4.3.1:

	- FTOF Time resolution updated based on data
	- Option  <a href="https://gemc.jlab.org/gemc/html/documentation/rerunEvents.html">SAVE_SELECTED, RERUN_SELECTED</a> to save RNG state for certain particles, detector
	- Option  <a href="https://gemc.jlab.org/gemc/html/documentation/ancestry.html"> SAVE_ALL_ANCESTORS </a> to save complete particles hierarchy in output (evio2root also updated)
	- gcards for rg-a different run-periods
	- gcards for rg-b different run-periods
	- ec, pcal digitization removed obsolete constants
	- moved ftof shield in the correct position
	- Option written in JSON format 
	- rga_fall2018 variations for: FTOF, EC, PCAL, CTOF geometry services 
	- default variation for DC geometry service 
	- ltcc variarions for different run periods :soon:
	- target position added to BMT, CTOF digitization position shift


<br>

- 4.3.2:

	- FILTER_HADRONS option to write out events that have hit from specific hadrons in them
	- Passing geometry variation to digitization routines :soon:
	- Geometry variation as a gcard option :soon:
	- Extend beam background merging to all detectors :soon:
	- arbitrary number of sequential rotations in the detector definition :soon:
	- Time propagation in DC digitization :soon:
	- BMT digitization with global coordinates instead of locals :soon:
	- 3D Cylindrical map field :soon:


<br>

- 4.4.0:

	- geant4 10.5 support

<br>




Previous Tags:
--------------

- 4a.2.4: **COATJAVA: 5b.6.1**, **JLAB_VERSION: 2.2**:

	- Use new torus field map
	- FMT shift by 8mm
	- use run number 11 as default in the gcard
	- FMT background hits
	- production cut set for individual volumes in the options
	- new geant4 version
	- env variable "GEMC_DATA_DIR" as a base path in the gcard (gcard is now portable to other systems)
	- bst tungsten and heat shield
	- LTCC Nose CAD model
	- magnetic field map displacements and rotations with command line options
	- FAST MC mode 10, 20 output fixed
	- new solenoid field map used by default (scaled by -1)

<br>


- 4a.2.3 (compatible with COATJAVA 5b.3.3): Same as 4a.2.2 with in addition:

	- ctof, ftof banks: 1 ADC output / pmt instead of ADCL/ADCR for a single paddle)
	- CTOF, FTOF Paddle to PMT digitization for FADC
	- background merging algorithm framework
	- background merging algorithm implementation in digitization: DC, BST and MM.
	- Correct field geant4-caching
	- Solenoid integration method: G4ClassicalRK4 to fix some geant4 navigation issues in the field. Slower but more reliable (should have less crashes)
	- SYNRAD option to activate synchrotone radiation in vacuum (SYNRAD=1) or any material (SYNRAD=2)
	- dc gas material changed to 90% Ar, 10% G4_CARBON_DIOXIDE.
	- RF shift from target center: added option RFSTART: Radio-frequency time model. Available values are:
	  - "eventVertex, 0, 0, 0" (default): the RF time is the event start time + the light time-distance of the first particle from the point (0,0,0)
	  - "eventTime".....................: the RF time is identical to the event start time


<br>

- 4a.2.2: Same as 4a.2.1 with in addition:

	- target from CAD
	- htcc wc invisible
	- no transparency in MM
	- threshold mechanism
	- rotate LUND bank to flat
	- final beamline configuration
	- LTCC sector 4 removed
	- LTCC sector 1 removed
	- DC: Removed unused lines and calculation of smeared doca
	- generator user information are now in two dedicate banks: user header (TAG 11), and user particle infos (TAG 22)
	- MM overlaps with target fixed
	- New numbering scheme for CTOF, CND

<br>

- 4a.2.1: Same as 4a.2.0 with in addition:

	- fixes to CAD modeling of both the beamline and the torus
	- forward carriage volume fixed to accomodate beamline and shielding
	- fixed CND / CTOF overlaps
	- updated latest micromegas geometry
	- MM: Adjust transverse diffusion and ionization potential
	- updated latest BST
	- 3 + 3 configuration BST + MM
	- new vacuum pipe
	- new shield downsttream of the torus
	- LTCC box hierarchy fixed
	- LTCC frame is CAD + copies
	- corrected mini-stagger values for DC
	- Added TDC calibration constants to ec,pcal hitprocess
	- FADC mode 1 (still tuning it to make it exactly like Serguei DAQ)
	- reading tdc_conv for ctof from database
	- fixed an issue with the header bank where the LUND info index was not correct

- 4a.2.0: Same as 4a.1.0 with in addition:
 
  - use JLAB_VERSION 2.1, with new mlibrary
  - Micromegas: updated  geometry and digitization
  - DC: realistic time to distance function, reading constants from CCDB
  - DC: ministagger for DC region 3: "even closer" layers 1,3,5: +300um, SL 2,4,6: -300um
  - LTCC: Reading CCDB SPE calibration constants
  - LTCC: Smearing ADC based on calibration constants
  - CTOF javacad instead of cad (should be indentical)
  - Fixed smeared infor in generated summary for FASTMC mode
  - improved/fixed CND digitization routine
  - updated RF timing (mlibrary) 
  - BST+MM: using 3+3 configurations (no FMT)
  

- 4a.1.0: Same as 4a.0.2 with in addition:

  - fixed a bug that affect output file size 
  - fixed bug that affected multiple cad imports
  - added micromegas geometry and hit processes
  - RF output correct frequency in the clas12 gcard
  - updated FT geometry and hit process
  - updated ftof geometry
  - added reading of FTOF reading of tdc conversion constants from the database
  - check if strip_id is not empty in bmt_ and fmt_strip.cc, otherwise fill it.


- 4a.0.2: Same as 4a.0.1 with in addition: 
  
  - full (box, mirrors, pmts shields and WC) LTCC geometry 
  - LTCC hit process routine.

- 4a.0.1: Same as 4a.0.0 with in addition: 

  - FTOF geometry fix

- 4a.0.0: KPP configuration: 

  - Fixes in source and hit process for FTOF
  - added EC gains. 
  - Java geometry uses now coatjava 3. 
  - Database fixed for DC geometry. 
  - Linear time-to-distance for DC.
  - CTOF in the KPP position configuration in the new kpp.gcard.
  
- 3a.1.0: same as 3a.0.2 with in addition:

  - DC time-to-distance implementation.
  
- 3a.0.2: same as 3a.0.1 with  in addition: 

  - CND fix.
 
- 3a.0.1: same as 3a.0.0 with  in addition:

  - ctof has status working.
  
- 3a.0.0: git commit d3a5dc1, Dec 2 2016. Includes:

  - FTOF and CTOF paddle delays from CCDB
  - CTOF center off-set.

<br>

Other notes
===========

<br>

FTOn, FTOff configurations
--------------------------

The default configuration for the first experiment is with "FTOn" (Figure 1, Left): complete forward tagger is fully operational.
The other available configuration is "FTOff" (Figure 1, Right): the Forward Tagger tracker is replaced with shielding, and the tungsten cone is moved upstream.

The simulations in preparation of the first experiment should use the default version FTOn.
FTOff will be used only by experts for special studies in preparation for the engineering run.

<a href="url"><img src="https://github.com/gemc/clas12Tags/blob/master/ftOn.png" align="left" width="400" ></a>
<a href="url"><img src="https://github.com/gemc/clas12Tags/blob/master/ftOff.png" align="left" width="400" ></a>

<br><br>
<br><br>
<br><br>
<br><br>
<br><br>
<br>


###### Figure 1. Left: FT On configuration: Full, OperationalForward Tagger. Right: FT Off configuration: FT Tracker replaced by shielding, Tungsten Cone moved upstream, FT if turned off.

<br>

To change configuration from FTOn to FTOff, replace the keywords and variations from:


```
<detector name="ft" factory="TEXT" variation="FTOn"/>
<detector name="beamline" factory="TEXT" variation="FTOn"/>
<detector name="cadBeamline/" factory="CAD"/>
```

to:


```
<detector name="ft" factory="TEXT" variation="FTOff"/>
<detector name="beamline" factory="TEXT" variation="FTOff"/>
<detector name="cadBeamlineFTOFF/" factory="CAD"/>
```


<br>

Different tag environment
-------------------------

You can specific a different tag in the environment command line:

```source /group/clas12/gemc/environment.csh <tag>  ```





<br>


To produce:
-----------

1. create new tag dir
2. cp experiments and gcard form old tag to keep track of new changes
3. make links from ../gcards
4. change environment.csh to point to the new tag
5. copy $GEMC to source and clean up. From the tag:

	- cd $GEMC
	- scons -c
	- cd -
	- cp -r $GEMC source ; cd source ; rm -rf .git* ; rm .sconsign.dblite
	- find ./  -type f  -name .DS_Store  -exec rm -f {} \;
	- rm -rf api ; cp -r /opt/projects/gemc/api . ; cd api ;  rm -rf .git*

5. change gemc.cc tag to new tag

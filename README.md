# FCC Study Event Data in ROOT

Archive of FCCC Study event data in ROOT, generated from [FCC Tutorials](https://hep-fcc.github.io/fcc-tutorials/main/2-gen-and-fastsim/2-2-fastsim-delphes/README.html).

## Tutorials

### Event simulation

From [FCC: Getting started with simulating events in Delphes](https://hep-fcc.github.io/fcc-tutorials/main/2-gen-and-fastsim/2-2-fastsim-delphes/README.html).

1. Source a Key4hep stack

2. Download official pythia cards for the various processes

```bash
# e+ e- -> ZH -> Z and H to anything
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/main/FCCee/Generator/Pythia8/p8_ee_ZH_ecm240.cmd

# e+ e- -> ZZ -> Z to anything
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/main/FCCee/Generator/Pythia8/p8_ee_ZZ_ecm240.cmd

# e+ e- -> WW -> W to anything
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/main/FCCee/Generator/Pythia8/p8_ee_WW_ecm240.cmd

wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/main/FCCee/Delphes/card_IDEA.tcl
wget https://raw.githubusercontent.com/HEP-FCC/FCC-config/main/FCCee/Delphes/edm4hep_IDEA.tcl
```

3. Simulate events with DelphesEDM4Hep

```bash
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_ZH_ecm240.cmd p8_ee_ZH_ecm240.root
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_ZZ_ecm240.cmd p8_ee_ZZ_ecm240.root
DelphesPythia8_EDM4HEP card_IDEA.tcl edm4hep_IDEA.tcl p8_ee_WW_ecm240.cmd p8_ee_WW_ecm240.root
```

### Full simulation

From [FCC-ee Full Sim General Overview](https://hep-fcc.github.io/fcc-tutorials/main/full-detector-simulations/FCCeeGeneralOverview/FCCeeGeneralOverview.html#towards-full-sim-physics-analyses-with-cld).

1. Source a Key4hep stack

2. Clone repo

```bash
git clone https://github.com/Key4hep/CLDConfig.git
cd CLDConfig/CLDConfig
```

3.  Retrieve Z(mumu)H(X) MC generator events

```bash
# e⁺e⁻ → Z(μμ)H(inclusive)

wget https://fccsw.web.cern.ch/fccsw/tutorials/MIT2024/wzp6_ee_mumuH_ecm240_GEN.stdhep.gz
gunzip wzp6_ee_mumuH_ecm240_GEN.stdhep.gz
```

4. Run the Geant4 simulation (1 event)

```bash
ddsim -I wzp6_ee_mumuH_ecm240_GEN.stdhep -N 1 -O wzp6_ee_mumuH_ecm240_CLD_SIM.root --compactFile $K4GEO/FCCee/CLD/compact/CLD_o2_v07/CLD_o2_v07.xml --steeringFile cld_steer.py
```

5. Running CLD reconstruction (1 event)

```bash
sed -i "s/DEBUG/INFO/" CLDReconstruction.py
k4run CLDReconstruction.py --inputFiles wzp6_ee_mumuH_ecm240_CLD_SIM.root --outputBasename wzp6_ee_mumuH_ecm240_CLD --num-events -1

```

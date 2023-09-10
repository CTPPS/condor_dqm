  Being on AFS create a cmssw release workspace

```
source /cvmfs/cms.cern.ch/cmsset_default.sh
cmsrel CMSSW_13_2_3
cd CMSSW_13_2_3/src
```

Set properly the grid proxy to be visible by Condor, following instruction from https://twiki.cern.ch/twiki/bin/view/Sandbox/UsingGrid:

```
export X509_USER_PROXY=${HOME}/private/.x509up_${UID}
voms-proxy-init -voms cms -rfc -out ${HOME}/private/.x509up_${UID} -valid 192:00
```

Clone here this repo then prepare the scripts for submission.
Adjust dataset name and lumi.txt file (there you find list of run number and lumisections).
Adjust also expected output file name DQM_V0001_CTPPS_R000373088.root

```
./cmsCondor.sh -py=dqm.py -dataset=/ZeroBiasNonColliding/Run2023F-v1/RAW -json=lumi.txt -output=DQM_V0001_CTPPS_R000373088.root
```

Submit the job using:

```
cd condor_ZeroBiasNonColliding_D_Run2023F-v1_D_RAW_230910_233700; condor_submit job.jdl; cd -
```

To check the job status type:
```
condor_q
```

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
cd condor_ZeroBiasNonColliding_D_Run2023F-v1_D_RAW_230910_235330; condor_submit job.jdl; cd -
```

To check the job status type:
```
condor_q
```

The results should be in

```
[grzankal@lxplus700 src]$ ls -alh condor_ZeroBiasNonColliding_D_Run2023F-v1_D_RAW_230910_235330/condorOut/
total 3.5M
drwxr-xr-x. 2 grzankal zh 2.0K Sep 11 00:01 .
drwxr-xr-x. 5 grzankal zh 2.0K Sep 10 23:56 ..
-rw-r--r--. 1 grzankal zh 3.5M Sep 11 00:01 DQM_V0001_CTPPS_R000373088_1801932_0.root
```

The script to submit the jobs is based on https://github.com/jeongeun/SimpleNtuplizer/blob/main/Run3Ntuplizer/test/cmsCondor.sh

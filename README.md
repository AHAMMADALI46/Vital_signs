# Vital_signs
# Vital_signs_New 
Libname raw "data path link";
Libnmae sdtm "data path link";

data vs1;
set raw.vital_raw;
studyid=strip(studyid);
domain="VS";
subjid=strip(site);
usubjid=studyid//"-"//siteid//"-"//subjid;
vsdtc=put(RECDT,IS8601DA.)//"T"//Put(Recdtm, tod8.);
VSTPT=Strip(TP);
if vstpt="check-in" then vstptnum=1;
else if vstpt="0" then vstptnum=2;
else if vstpt="2" then vstpnum=3;
else if vstpt="8" then vstpnum=4;
else if vstpt="Check-out" then vstptnum=5;
if visit="screening" then visitnum=0;
else visit="Period-1" then visitnum=1;
else visit="Period-2" then visitnum=2;
else visit="Period-3" then visitnum=3;
run;

Proc sort data=vs1 out=vs2;
by studyid domain usubjid visitnum visit vstpnum vstpt vsdtc;
run;

proc transpose data=vs2 out=vs3;
by studyid domain usubjid visitnum visit vstpnum vstpt vsdtc;
var pulse temp sys dia;
run;

Data vs4;
set vs3;
if _Name_="pulse" then do; Vstestcd="Pulse" ; Vtes"Pulse rate"; end;
if _Name_="Temp" then do; Vstestcd="Pulse" ; Vtes"temperature"; end;
if_Name_="sys" then do; Vstestcd="sysbp" ; Vtes"systolic blood pressure"; end;
if_Name_="DIA" then do; Vstestcd="DIABP" ; Vtes"Pulse rate"; end;
vsorres=strip(put (col1, Best.));

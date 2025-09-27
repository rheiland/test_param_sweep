# test_param_sweep

From the the cloned repo:
```
cd ~/git/test_param_sweep/PhysiCell-1.14.2
make -j2   # this will create the "project" executable
make save PROJ=MODEL1   # optional, if you really plan to have multiple models

cp beta/params_run.py params_run.py
cp beta/params_run.txt params_run.txt
```

Edit these files to be relevant to the model in this repo, e.g.:

```
(base) M1P~/git/test_param_sweep/PhysiCell-1.14.2$ cat params_run.txt 
# File to be used with params_run.py
# Allows for changing parameters in .xml, running sim, and writing results to different folders.
# <key> <value> pairs, where <key> is the first unique node name found in the xml.
#max_time 120
#full_data.enable false
folder  run1 
cell_definitions.cell_definition[@name='cancer'].phenotype.cycle.phase_durations.duration 1444
run_it foobar
#
folder  run2 
cell_definitions.cell_definition[@name='cancer'].phenotype.cycle.phase_durations.duration 1440
cell_definitions.cell_definition[@name='invasive'].phenotype.cycle.phase_durations.duration 99999
run_it foobar
```

Running them should result in a `/run1` and `/run2` output directories
```
(base) M1P~/git/test_param_sweep/PhysiCell-1.14.2$ python params_run.py project params_run.txt
```

```
(base) M1P~/git/test_param_sweep/PhysiCell-1.14.2$ diff config/PhysiCell_settings.xml run1/config.xml 
30c30
<         <folder>output</folder>
---
>         <folder>run1</folder>
91c91
<                         <duration index="0" fixed_duration="false">1440</duration>
---
>                         <duration index="0" fixed_duration="false">1444</duration>
(base) M1P~/git/test_param_sweep/PhysiCell-1.14.2$ 
(base) M1P~/git/test_param_sweep/PhysiCell-1.14.2$ 
(base) M1P~/git/test_param_sweep/PhysiCell-1.14.2$ diff config/PhysiCell_settings.xml run2/config.xml 
30c30
<         <folder>output</folder>
---
>         <folder>run2</folder>
237c237
<                         <duration index="0" fixed_duration="false">42000</duration>
---
>                         <duration index="0" fixed_duration="false">99999</duration>
(base) M1P~/git/test_param_sweep/PhysiCell-1.14.2$ 
```


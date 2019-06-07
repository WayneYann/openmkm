---
layout: default
---

# Input Files

## Introduction
OpenMKM uses two input files supplied as two arguments to the OpenMKM executable.

1. The first argument *rctr.yaml* is the name of yaml file specifying the reactor model parameters, 
operating conditions, and the names of thermodynamic phases (which are defined in the Cantera XML file 
supplied as second argument) and the starting composition and coverages of gas and surface phases 
respectively. For more details on the yaml file format and the input options refer to the Input section. 

2. The second argument *input.xml* is Cantera input file in XML format which provides the 
definitions of species, reactions, interactions, and gas, solid and  catalyst surface phases. 
Cantera defines an additional input format called CTI, which is easier to work with.
To assist those migrating from Chemkin, scripts are available to convert Chemkin input files to Cantera files. 

## YAML Input File.

The first argument (called *rctr.yaml* throughout this document, but could be named anything) 
uses [YAML](https://yaml.org) format, which is a human friendly data format. YAML is a superset of 
JSON format, so the input could be specified in JSON format also. The data specified in YAML file can 
be thought of as a nested python  dictionary. A brief snippet of a pseudo YAML file containing 
most of the YAML format features used for *rctr.yaml* are given below.

```python
key1 : value1
key2 : 
    nested_key1: value2
    nested_key2:
       - array_value1
       - array_value2
```

In the above snippet, the YAML data contains two level-1 keys (also called YAML nodes), *key1, key2*.
*key1* has a single data value, *value1* assigned to it. On the other hand *key2* consists of 
nested compound data (which is again a dictionary) as value. The keys of nested dictionary can be
thought of as level-2 keys. Intersting would be the value for 
*nested\_key2*, which is a sequence (also called array) of two values, 
*array\_value1* and *array\_value2*. 

YAML is a free format. So *key1 : value1* line could have been given below the data for *key2*. Also any text after *\#* is a comment.

*rctr.yaml* specifies 3 mandatory and one optional level-1 nodes (or keys). The 
mandatory nodes are *reactor*, *simulation*, *phases*, and the optional node is 
*inlet_gas*.

1. **reactor**: This node specifies the reactor parameters such as its type, dimensions, operational mode, catalyst size, state (temperature and pressure) etc.

2. **simulation**: This node specifies the simulation parameters including those supplied to numerical solvers such as simulation time, stepping options for advancing (time for CSTR & batch, and distance for PFR), solver tolerances etc.

3. **phases**: OpenMKM expects 1 or 3 types of phases. For purely gas phase 
mechanism, a gas phase has to be specified. For heterogeneous reactions, two 
additional types of phases, one bulk solid phase and one or more surface phases 
have to be specified. The phases are actually defined in the Cantera input file, which is
supplied as the second argument. Since a cantera input XML file allows for defining 
arbitrary number of phases, the names of the phase definitions to be used in the 
simulation are specified in the *rctr.yaml*. The Cantera XML file can contain 
defitions of additional phases not specified in the *rctr.yaml* file. This way 
one could use a single XML file and just the phase definitions for different runs 
if required. In addition to the names of the phase definitions, this node is also 
used to specify the initial composition of the phases.

4. **inlet_gas**: This node is required only for CSTR and PFR models. These models require a continous input of feed with a flow rate and composition, which can be defined by this node.

Predefined templates are available for batch, CSTR, PFR, TPD and temperature 
profiled PFR reactor models in the *\<OpenMKM_ROOT\>/data/input_files/* folder. 
The users can quickly modify those template files rather than trying to write 
*rctr.yaml* from scratch. The *rctr_tmplt.yaml* file in the same folder 
contains full explanation of all the possible keywords given as comments next to 
each of the keywords.


3. Some sample working simulations are
*CANTERA_ROOT/interfaces/cython/cantera/ck2cti.py*, which parses gas.inp, surf.inp and thermdat files
to generate the Cantera input file in CTI format. The CTI file needs to converted again into XML file
using *CANTERA_ROOT/interfaces/cython/cantera/ctml_writer.py*.
For more information on the Cantera input file formats, refer to 
[Cantera documentation on input file format](https://cantera.org/tutorials/input-files.html).

3. The chemkin input files are not sometimes parsed by ck2cti.py script. Refer to 
[step by step guide](ck_conversion.md) on how to overcome those issues. 

4. Coverage effects need to be incorporated into the xml file. Since it is easy to work with CTI files, users have the option of specifying coverage effects in CTI file and then convert the CTI file into XML file with the python script *ctml_writer.py*.



2. For syntax of the yaml file, refer to the [rctr_tmplt.yaml](rctr_tmplt.yaml) file, 
which provides extensive comments on the keywords required by hetero_ct in the supplied yaml file.
Multiple yaml files for various reactor models and operating modes are in located in *OpeMKM_Root/test_files* folder. 


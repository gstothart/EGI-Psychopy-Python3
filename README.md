# EGI-Psychopy-Python3
EGI Psychopy library - Python 3 compatibility update

Psychopy contains a library to support interfacing with EGI EEG equipment. The library was only been compatible with Python 2. We have updated the library to make it compatible with Python 3. We have documented the changes below, and include example code snippets demonstrating how to now set up the EGI connections and send triggers. 

Coding updates were completed in the Fastball lab, https://osf.io/r5tns/, at the University of Bath  by Abhiram Anand Thiruthummal, aat63@bath.ac.uk

# Installing
Make a backup of your existing egi library, typically found in the following path C:\Program Files (x86)\PsychoPy2\Lib\site-packages\egi, delete the egi folder and replace with the version provided. 

# Setting up your connection
The code below establishes a connection between the Psychopy stimulus presentation computer and the EEG recording computer (Mac) running EGI Netstation Acquisition. 
```
from egi import simple as egi 
ms_localtime = egi.ms_localtime     
ns = egi.Netstation()
ns.connect('10.10.10.42', 55513) # sample address and port -- change according to your network settings
ns.BeginSession()     
ns.sync()
ns.StartRecording()
```
# Sending a trigger
Note that the trigger type has been changed from string to byte in order to make it compatible with Python3.
```
ns.sync()     
ns.SendSimpleTimestampedEvent(b'0051')
```
# Code changes from Python2

The following documentation outlines the changes made to the code and supporting libraries to make it compatible with Python environment, PsychoPy 3.2.4 (Python 3.6). 
The trivial syntax change for print() was implemented wherever necessary. The non-trivial changes are documented below. The exact lines in the code where critical code changes are made, are marked by the tag #bugfix within the code.

Changelog
## EGI\__init__.py

Version number updated to 3.0.0

Email address added to description (docstrings) for future support.

## EGI\simple.py

Syntax change: importing socket_wrapper module.
from socket_wrapper -> from egi.socket_wrapper

Module Removed: exceptions.
class Exception moved to builtins from module exceptions.

Definition change: inheriting class Exception by class Eggog
class Eggog( exceptions.Exception ) -> class Eggog( Exception )

Datatype removed: long int / type(1L).
Python 3 only supports a single integer datatype (int).

Datatype change: string to bytes object
struct.pack() only accepts int, float or bytes object, as its 2,3,4… arguments. All strings passed as 2,3,4… arguments to struct.pack() in simple.py and Fastball_st_mem_objects_condition1.py  converted to bytes object using string prefix b or bytes() with utf-8 encoding.

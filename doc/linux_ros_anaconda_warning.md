### A Warning to Linux users who already have ROS installed:

Upon completion of the Anaconda install, you should have a new Anaconda environment called `RoboND` with all the packages you need.  However, if you have ROS installed on your Linux machine you will most likely run into a conflict between Python installations due to the `PYTHONPATH` variable being set in your `/opt/ros/kinetic/setup.bash` file which gets sourced in your `.bashrc` file.  

The hallmark of this conflict is getting an import error when you go to import packages that you just installed using Anaconda.  To test whether you have this problem, first you fire up Python at the command line:

```sh
$ python
Python 3.5.2 |Anaconda custom (x86_64)| (default, Jul  2 2016, 17:52:12) 
[GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
Next you go to import a package (OpenCV or `cv2` in this case) and you get an `ImportError` like this (or similar):

```
>>> import cv2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so: undefined symbol: PyCObject_Type
```

The problem here is that Python is looking for a package in `/opt/ros/kinetic/lib/python2.7/dist-packages/` when it should be looking in the Anaconda installation directories.  The reason for this is that with your ROS install, your `.bashrc` file contains the line `source /opt/ros/kinetic/setup.bash`, which sets your `PYTHONPATH` variable to the `/opt/ros/kinetic/...` location.  

The solution is to unset your `PYTHONPATH` variable before sourcing your Anaconda environment:

```
$ unset PYTHONPATH
$ source activate RoboND
```
And it should work!  But in this case, you'll need to remember to do this each time you want to use your Anaconda Python install.  

For a more permanent solution, you can add the `unset PYTHONPATH` line to your `.bashrc` file, but you need to make sure you drop it into the right location, after sourcing ROS.  To do this, open your `.bashrc` file (should be located in your `~/` directory) and find the line that says `source /opt/ros/kinetic/setup.bash` and make this edit:

```
source /opt/ros/kinetic/setup.bash
unset PYTHONPATH
```

## Recommended ROS Approach

Another way to do it if you prefer to leave your ROS environment intact is to create a shell script and stick it in ~/bin:
Make sure when you first install anaconda and it asks you if you want to add it to your path on install you click **NO**.
If you made this mistake then go ahead and delete references to anaconda from your ~/.profile and ~/.bashrc.

###Summary:

1. Install anaconda and choose not to add it to your path
2. Create a script named: ~/bin/RoboND, with the following contents:
	```
	#!/bin/bash
	unset PYTHONPATH
	export PATH=$HOME/anaconda3/bin:$PATH
	source activate RoboND
	```
3. Login out and log back in to trigger the ~/bin path to be automatically to your PATH variable.
	Then you can run:
	```
	. RoboND
	```
	anywhere on your system and it just works.


See [this post](https://stackoverflow.com/questions/43019951/after-install-ros-kinetic-cannot-import-opencv) and [this post](https://stackoverflow.com/questions/17386880/does-anaconda-create-a-separate-pythonpath-variable-for-each-new-environment) for more information on the matter.  

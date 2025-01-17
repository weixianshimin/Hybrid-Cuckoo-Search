#SCRAPs: A Multicomponent Alloy Structure Design Tool
#A novel Hybrid CUCKOO SEARCH determines combinatorial global optimum
#SuperCell Random APproximates(SCRAPs)for specified point and pair
#correlations in high-entropy alloys having proper distributions.

#Copyright (c) 2020. Duane D. Johnson. All rights reserved.
#Developed by: Duane D. Johnson, Rahul Singh, Prashant Singh, Aayush Sharma, Ganesh Balasubramanian.
#Iowa State University; www.iastate.edu

Input files needed from the user: 
(I) INPUT  - input file 

The input file to initialize the structure-

TYP         3       	 # Type of atoms
GITER       1000     	 # Global iterations for global optimization
ELEMENT     18 18 18 	 # number of atoms in the cell
Cell DIM    3 3 3    	 # cell dimension 
NESTS       24        	 # number of nests used in cuckoo search
MCSTEP      750       	 # number of monte-carlo steps needed for local optimization 
TOTALIter   10        	 # total iterations for global+local optimziation
MAXSHELLNUM 3            # maxium number oc cell
WEIGHT      2.0 1.0 1.0  # weight

Weight: First number in the weight section defines the longest distance, or cut-off to calculate 
	the correlation functions. For example, in the case of f.c.c random solid solution with 
	lattice parameter 1- (a) 2^.5/2=0.71 is the the first nearest neighbor distance; 
	(b) 1.0 is second nnb distance, and (c) 1.5^.5=1.2 is the third nnb distance. 
	Higher cutoff, i.e., with more shells included leads to better disorder, however, chooising
	up to three shells are enough.

(II) POSCAR - position file depending on target structure, 
e.g., face-centered cubic, body-centred cubic, and/or hexagonal closed packed structures.

Format of structure file:

Example crystal structure      comment line
  1.0             	       universal scaling factor
 [x1] [y1] [z1]	 	       first  Bravais lattice vector
 [x2] [y2] [z3]  	       second Bravais lattice vector
 [x3] [y2] [z3]  	       third  Bravais lattice vector
 2               	       number of atoms per cell in the conventional unit cell
Direct           	       direct or cart (only first letter is significant)
 [atom1x] [atom1y] [atom1z]    atom position 1
 [atom2x] [atom2y] [atom2z]    atom position 2
   ---      ---      ---         
 [atomnx] [atomny] [atomnz]    atom position n

Exmaple position files:
(a) body centered cubic

BCC structure    comment line
 1.0             
 1.0 0.0 0.0     
 0.0 1.0 0.0     
 0.0 0.0 1.0     
 2               
Direct           
 0.0 0.0 0.0     
 0.5 0.5 0.5     

(b) face-centred cubic

FCC structure    
1.0             
 1.0 0.0 0.0     
 0.0 1.0 0.0     
 0.0 0.0 1.0     
 4               
Direct           
 0.0 0.0 0.0     
 0.5 0.0 0.5
 0.0 0.5 0.5
 0.5 0.5 0.0


(c) hexagonal closed pack

HCP structure                  
 1.0  0.0       0.0
-0.5  0.866025  0.0
 0.0  0.0       1.629932
 2 
Direct 
 0.0  0.0       0.0
 0.0  0.577350  0.816497


#------SCRAPS source code and compilation information-----------------------------------
(1) main.cpp
(2) Lattice.cpp (Lattice.hpp)
(3) readInput.cpp (readInput.hpp)

HPP is a file extension for a header file format that contains
variables, constants and functions referenced by source code.



# Compilation with c++11 libraries, also provide path for eigen-file directory:

#-Eigen program uses instructions
#Eigen consists only of header files, hence there is nothing to compile
#before you can use it. Moreover, these header files do not depend on your
#platform, they are the same for everybody. 
#You can use right away the headers in the Eigen/ subdirectory.

export PATH=/opt/gcc-6.1.0/bin:$PATH LD_LIBRARY_PATH=/opt/gcc-6.1.0/lib64:$LD_LIBRARY_PATH
/opt/openmpi-1.10.2-gcc-6.1.0/bin/mpic++ -std=c++11 main.cpp Lattice.cpp readInput.cpp -I ~/eigen-eigen-7e6036736698/

# Output is the executable e.g. a.out or user can create his/her own

# Prepairing a submit script:

export PATH=/opt/gcc-6.1.0/bin:$PATH LD_LIBRARY_PATH=/opt/gcc-6.1.0/lib64:$LD_LIBRARY_PATH

MPIRUN="/bin/mpirun"
SCRAPS="~/directory-path/a.out"

${MPIRUN}  ${SCRAPS} -p -np 12 >& SCRAPs.log.

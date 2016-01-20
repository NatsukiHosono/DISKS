# DISKS

DISKS is a tool that automatically generates 
an optimal placement of equal-mass particles 
to realize disks of a user-specified density 
distribution by placing particles in concentric 
rings. All you have to do is choice of a density 
profile and parameters of a disk.  
If you have any question or comment, please do not 
hesitate to contact us. Our e-mail address is 
<yamamoto.s.an-at-geo.titech.ac.jp>.

## Usage

1. SYNOPSIS  
Please enter "make" only once and "./a.out". 


2. DESCRIPTION
	In default, the parameters are given below
	density profile = r^(-1)
	inner edge = 1.0
	outer edge = 4.0
	cut   edge = 1.0
	the number of ring at cut edge = 20 
	the number of particles in a ring = 40
	(it is used with only power is -2 at cut edge.)
	tolerance = 10^(-8)

	
	2.1 Density Profile options: 
	
	-d, —-denstype=TYPE 
		create the density profile specified by TYPE.

		TYPE: density profile type               function  
		1   : power-law with slope != -2         S * r^(P)  
		2   : power-law with slope = -2          S * r^(-2)  
		3   : exponential density profile        S * e^(P * r)  
		4   : post giant impact disk             S * (r - 1) * e^(P * r)  
		5   : user defined  

	-P, —-powernumber=VALUE
		change the parameter P to VALUE.

	-S, —-coefficient=VALUE
		change the parameter S to VALUE.

	Examples:  
	-Enter "./a.out -d 3 -S 4.0".  
	The density profile is set "4.0 * e^(-r)",
	-Enter "./a.out -d 1 -S 2.0 -P -1.2".  
	The density profile is set "2.0 * r^(-1.2 * r)",
	-Enter "./a.out -S 2.0 -P -1.2".  
	The density profile is set "2.0 * r^(-1.2 * r)".  

	If you want to choose user defined density 
	profile, please edit "userdefined.h". Some 
	examples are written in the file.   
	
	
	2.3 Disk Parameters options: 

	-I, —-inneredge=VALUE
		change the position of inner edge to VALUE. 
		It must be a non-negative number or 0.

	-O, —-outeredge=VALUE
		change the position of outer edge to VALUE. 
		It must be larger than 0.

	-C, —-cutedge=VALUE
		change the position of cut edge to VALUE. 
		It must be larger than 0.

	-i, —-icut=VALUE
		change the number of ring at a cut edge to VALUE. 
		It must be integer greater than 0.

	-n, —-npzero=VALUE
		change the number of particles in a ring to VALUE. 
		It must be integer greater than 0 and is used with only power is -2 at cut edge.

	Examples:
	-Enter "./a.out -I 2.0 -O 5.0 -C 3.0 -i 30".  
	A disk whose range is r = [2.0:5.0] is generated.
	Placement of particles is stated at r = 3.0.
	The number of rings at the cut edge is set to 30.  
	-Enter "./a.out -O 5.0 -i 30".  
	A disk whose raige is r = [1.0:5.0] is generated.
	Placement of particles is stated at r = 1.0.
	The number of rings at the cut edge is set to 30.  


	2.3 Other options:  
	 
	-o —-output=FILE
        	write result to FILE instead of standard output [filename].  
	
		Example:  
		-Enter "./a.out -o test.dat".

	-t —-tolerance=VALUE
		change the value of tolerance to VALUE. 
	
	-p —-outputdens
		calculate the density of particles.
		Note that there are "large error" near edges because of 
		low-order of SPH approximation. In addition, it takes "N*N  
		times", where N is the number of particles.

	—h —-help 
		display help. 

Examples:

-Enter "./a.out ./a.out -d 2 -i 10 -O 5.0 -n 40 ".
You can get the left-hand side in figure 5 in the article.

-Enter "./a.out -P -1.9 -i 60 -O 5.0 ".
You can get the right-hand side in figure 5 in the article.

-Enter "./a.out -P -1.45 ".
You can get the right-hand side in figure 6 in the article.

-Enter "./a.out -d 3 -P -1.0 -i 15 ".
You can get the left-hand side in figure 9 in the article.

-Enter "./a.out -d 4 -P -1.0 -I 2.0 -C 2.0 -i 10 ".
You can get the right-hand side in figure 9 in the article.

-Enter "./a.out -d 3 -S 1.0 -P -0.5 -I 0.0 -O 3.0 -C 0.1 -i 1 -o test.dat".  
A disk whose density profile is “1.0 * e^(-0.5 * r)" and range is r = [0.0:5.0] is generated.
Placement of particles is stated at r = 0.1.
The number of rings at cut edge is set to 1.  
The output file is "./test.dat".  

-Enter "./a.out -S 4.0 -P -3.0 -I 2.0 -C 2.0 -i 50".  
A disk whose density profile is "4.0 * r^(-3.0 * r)" and range is r = [2.0:4.0] is generated.
Placement of particles is stated at r = 2.0.
The number of rings at cut edge is set to 50.  

-Enter "./a.out —d 2 -n 40.  
A disk whose density profile is “1.0 * r^(-2)" and range is r = [1.0:4.0] is generated.
Placement of particles is stated at r = 1.0.
The number of rings at cut edge is set to 20.  
The number of particles for each ring is set to 40.


3. Output Data  
The parameters of particles are outputted. From left to 
right, the values mean  
(particle id, x-coordinate, y-coordinate, radius, density).
If you do not choose the option -p, values of density is set to 0.


4. Errors  
If there are errors in this program, an error message is outputted.  
	4.1 Error: The number of iterations for deriving parameters of the ring is over (number).  
	We put a ceiling to iteration for calculating the parameters of particles for the degrees 
	of freedom of tolerace.  
	If you want to change the maximum number of iterations, please change the parameter 
	max_iteration (l.30 in main.cc).
	
	4.2 Error: Please increase the parameter of max_number_of_ring (l.12 in main.cc).
	There is an maximum value of the memory of the number of rings. If the error is outputted, 
	please change the parameter of max_number_of_ring (l.12 in main.cc).
	We are going to apply dynamic memory allocation to this program as soon as possible.
	
	4.3 Error: Please increase the number of a ring at the cut edge or decrease the value of  
	the cut edge by changing the parameter of the option -i or -C.
	The optimal number of particles at i-th ring is negative. It can be avoided to change the 
	parameter of the option -i or -C.

	4.4 Error: The number of particles in a ring is zero. Please change the parameter of the option -n.  
	The number of particles in a ring is zero. Please change the parameter of the option -n.
	
	4.5 Error: Please change the cut edge or the inner edge by changing the parameter of the option -C or -I.
	The mass is non-number or negative. This can be avoided to change the parameter of option 
	-C or -I.
		
	4.6 Error: Fail to open (file name). Please change the filename by the option -f.
	The file-name is invalid. Please change file name.  
	
	4.7 Error: Invalid option '(option)'.  
	You choose an invalid option, but it dose not effect the program.  
	
	4.8 Error: Invalid type of the density profile. Please change the parameter of the option -d.
	You choose an invalid type of a density profile. The type of density profile is 1, 2, 3, 4 or 5 (see 2.1). 
	
	4.8 Error: Invalid value of the inner edge. Please change the parameter of the option -I.
	You choose an invalid value of the inner edge. The value must be non-negative or zero.

	4.9 Error: Invalid value of the outer edge. Please change the parameter of the option -O.
	You choose an invalid value of the outer edge. The value must be large than 0.

	4.9 Error: Invalid value of the cut edge. Please change the parameter of the option -C.
	You choose an invalid value of the cut edge. The value must be large than 0.

	4.10 Error: Invalid number of a ring at cut edge. Please change the parameter of the option -i.
	You choose an invalid number of a ring at cut edge. It must be integer greater than 0.


We describe the detail of this algorithm in "in preparation".  

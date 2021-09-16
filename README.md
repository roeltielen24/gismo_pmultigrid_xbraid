# pMultigrid

In this example, we consider the *p*-multigrid method as considered in [1, 2]. *p*-multigrid methods are a class of iterative solvers that use a low-order correction to update the high-order solution.  At each level of the hierarchy, a basic iterative method (e.g.  Gauss-Seidel,ILUT) is applied which results in an overall highly efficient method.  Figure 1 illustrates a single iteration of a *p*-multigrid method.

![GitHub Logo](images/p_multigrid.png)

As a model problem, we consider the convection-diffusion-reaction (CDR) equation, where  Dirichlet  (essential)  and  Neumann  (natural)  boundary  conditions  can be prescribed,  respectively. 

In this tutorial we will explain how one can use a *p*-multigrid method to solve this model problem. We will explain how one can define the benchmark problem, set-up the *p*-multigrid method and adjust it to your own preference. More information can be found in [1] and [2].

Most of the parameters required to define the model problem and *p*-multigrid method can be provided via the command-line. Alternatively, one can adjust them in the main file, but this requires (re)-compiling the code. In the following, we will briefly discuss the different command-line arguments.

## Benchmark

Using the argument **-b \<INT\>** one can choose the considered benchmark problem.  Eachproblem  is  defined  by  the  exact  solution  ’solutionexact’,  considered  right  hand  side ’rhsexact’ and the underlying geometry ’geo’.  In total, five benchmarks can be adopted, but new ones can easily be added by the user. Typing **-b 2** in the command line leads for example to:
  
![GitHub Logo](images/code.png)

A  second  argument, **-P \<INT\>** defines  the  number  of  patches  considered. Starting from a single patch geometry, the geometry is split uniformly in each direction P times, with a maximum of 4 (= 64 patches). The benchmark problem above, where the geometry consists of 4 patches can thus be defined as follows:
  
 **-b 2 -P 2**

## p-multigrid hierarchy
  
To set up the *p*-multigrid hierarchy, one first has to define the high-order discretization by  defining  the  degree  of  the  B-spline  basis  functions  and  the  refinement  level  by  the command line  arguments **-p \<INT\>** and **-r \<INT\>**,  respectively. Then, a *p*-multigrid hierarchy  is  created  by  setting  the  number  of  levels  in  the  multigrid  hierarchy  (using **-l \<INT\>**) and the coarsening strategy **-z \<STRING\>**. The string should have length **l−1** and defines if *p*-coarsening (”p”) or *h*-coarsening should be applied (”h”). From left to right, the string starts with the coarsest level and ends with the finest one. 
  
The considered *p*-multigrid in Figure 1 is based on applying *h*-coarsening twice followed by applying *p*-coarsening twice afterwards and can be defined as follows:
  
**-p 3 -r 5 -l 5 -z "hhpp"**
  
In case a so-called direct projection [3] is adopted, the user should still provide the number  of  levels  and  coarsening  strategy  as  shown  above. The *p*-multigrid  hierarchy based on a direct projection will be formed internally by providing the argument **-D 1**, see next section. 
  
## p-multigrid options

The  number  of  smoothing  steps  adopted  within  the *p*-multigrid  method  is  defined  by **-v \<INT\>**. It should be noted that the type of smoother chosen by **-S \<INT\>** is only used at high-order levels.  At level p=1, Gauss-Seidel is always adopted as a smoother. One could adjust the *p*-multigrid method as follows:
  
**-s 1 -S 1 -D 1 -L 1 -d 1**
  
All other parameters and their meaning are provided in the table below.

  ![GitHub Logo](images/table.png)
  
## Example
  
As an example, one can run the following code in the terminal:
  
**$./bin/pMultigridexample -p 2 -r 6 -l 4 -S 1 -b 2 -z "hhp"**
  
This would lead to the following output on the terminal:
  
![GitHub Logo](images/output.png)
  
Hence,  Poisson’s  equation  is  solved  on  a  quarter  annulus  using  a *p*-multigrid  method combined  with  an  ILUT  smoother. Of  course,  the  times  shown  in  the  example  above differ from device to device. Note that the output in the terminal describes the benchmark problem, the *p*-multigrid hierarchy and details about the solver. By typing
  
**$./bin/pMultigridexample -p \<INT\> -r \<INT\> -l \<INT\> -S \<INT\> -b 2 -z \<STRING\>**
  
one can obtain the results for different discretizations. Table 1 shows the results for different values of *p* and *h* which can be reproduced by varying the different parameters in the terminal. The red colored 4 is the result corresponding to the output above.
  
 ![GitHub Logo](images/results.png)
 
 
 ## Bibliography
 
 [1]  R. Tielen, M. M&ouml;ller, D. G&ouml;ddeke, C. Vuik, *p*-multigrid methods and their comparisonto h-multigrid methods within Isogeometric Analysis, *Computer Methods in AppliedMechanics and Engineering*, Volume 372, 2020
 
 [2]  R. Tielen, *p*-multigrid methods for Isogeometric Analysis, *PhD thesis*, 2021
 
 [3]  R. Tielen, M. M&ouml;ller, C. Vuik, A Direct Projection to Low-Order Level for *p*-Multigrid Methods in Isogeometric Analysis, *Lecture  Notes  in  Computational Science and Engineering: Numerical Mathematics and Advanced Applications ENUMATH 2019*, 2021
  



#  BEAST:  A Bayesian Ensemble Algorithm for Change-Point Detection and Time Series Decomposition
 ###  BEAST is a Bayesian model averaging algorithm to decompose time series or 1D sequential data into individual components, such as abrupt changes, trends, and periodic/seasonal variations, as described in [Zhao et al. (2019)](https://go.osu.edu/beast2019). BEAST is useful for changepoint detection (i.e., breakpoints or structural breaks), nonlinear trend analysis, time series decomposition, and time series segmentation

> **BEAST** was impemented in C/C++. Check the "Source" folder for the source code. R and Matlab interfaces are also provided and can be found under the "R" and "Matlab" folders above. Or follow the instructions below to install and run BEAST in R or Matlab.

---- 
# Installation
## R


   1. from **CRAN**: An R package **`Rbeast`** has been deposited at [CRAN](https://CRAN.R-project.org/package=Rbeast). (On CRAN, there is another Bayesian time-series package named "beast", which has nothing to do with the BEAST algorithim. Our package name is `Rbeast`.) Install `Rbeast` in your R console as follows:
      ```R
       install.packages("Rbeast")
      ```

   2. from **GitHub**: The latest versions of package files for **Rbeast** are available here at [GitHub](https://github.com/zhaokg/Rbeast). Alternative ways to install Rbeast are:

      ```R
        # Windows x86 or x64 (install from binary)
        install.packages("https://github.com/zhaokg/Rbeast/raw/master/R/Windows/Rbeast_0.9.4.zip" ,repos=NULL)
         
        # Linux/Mac (install from source)
        install.packages("https://github.com/zhaokg/Rbeast/raw/master/R/Rbeast_0.9.4.tar.gz", repos = NULL, type="source")
      ```
  

#### Run and test Rbeast

The main functions in Rbeast are `beast(Y, ...)` and `beast123(Y, metadata, prior, mcmc, extra)`. The code snippet below  provides a starting point for the basic usage of Rbeast.

  ```R
      library(Rbeast)
      data(Nile)                       #  annual streamflow of the Nile River    
      out = beast(Nile, season='none') #  'none': trend-only data without seasonlaity   
      print(out)                   
      plot(out)
      ?Rbeast          # See more details about the usage of `beast`                 
 ```
---- 
## Matlab

#### Installation and usage (Windows x64 only)

The C code of BEAST has also been compiled into a Matlab mex library (i.e., Rbeast.mexw64) with some wrapper matlab functions similar to the R interface, all available at the Rbeast\Matlab folder above. Download the files to your local drive and run BEAST. Alternatively, run the following Matlab code to automatically download and copy the files your local drive:

  ```Matlab
  % Installation path of your choice; Write permission needed; the var name has to be 'beastPath'
  beastPath = 'c:\rbeast\';                    
  eval( webread('http://go.osu.edu/rbeast', weboptions('cert','')) );
  help beast
  help beast123  
  ```

The Matlab API is similar to those of R. Below is a quick example:
  ```Matlab
   load('Nile.mat')                     % annual streamflow of the Nile River startin from year 1871
   out = beast(Nile, 'season', 'none','start', 1871)  % trend-only data without seasonality
   printbeast(out)
   plotbeast(out)
  ```
We generated the Matlab mex library only for the Windows 64 OS. Mex libraries for other OS systems such as Linux and Mac can be compiled from the source code files under "\Rbeast\Source". If needed, we are happy to work with you to compile for your specific OS or machines. Additional informaiton on compliation from the C source is also given below.

---- 
## Python

A wrapper in Python is being developed: We wecolme contributions and help from interested developers. If interested, contact Kaiguang Zhao at zhao.1423@osu.edu.


---- 
## Julia

A wrapper in Julia is also being developed: We wecolme contributions and help from interested developers. If interested, contact Kaiguang Zhao at zhao.1423@osu.edu

## Description
Interpretation of time series data is affected by model choices. Different models can give different or even contradicting estimates of patterns, trends, and mechanisms for the same data–a limitation alleviated by the Bayesian estimator of abrupt change,seasonality, and trend (BEAST) of this package. BEAST seeks to improve time series decomposition by forgoing the "single-best-model" concept and embracing all competing models into the inference via a Bayesian model averaging scheme. It is a flexible tool to uncover abrupt changes (i.e., change-points), cyclic variations (e.g., seasonality), and nonlinear trends in time-series observations. BEAST not just tells when changes occur but also quantifies how likely the detected changes are true. It detects not just piecewise linear trends but also arbitrary nonlinear trends. BEAST is applicable to real-valued time series data of all kinds, be it for remote sensing, economics, climate sciences, ecology, and hydrology. Example applications include its use to identify regime shifts in ecological data, map forest disturbance and land degradation from satellite imagery, detect market trends in economic data, pinpoint anomaly and extreme events in climate data, and unravel system dynamics in biological data. Details on BEAST are reported in [Zhao et al. (2019)](https://go.osu.edu/beast2019). The paper is available at https://go.osu.edu/beast2019.

## Reference
Zhao, K., Wulder, M. A., Hu, T., Bright, R., Wu, Q., Qin, H., Li, Y., Toman, E., Mallick B., Zhang, X., & Brown, M. (2019). [Detecting change-point, trend, and seasonality in satellite time series data to track abrupt changes and nonlinear dynamics: A Bayesian ensemble algorithm.](https://go.osu.edu/beast2019) Remote Sensing of Environment, 232, 111181. 

---- 
## Additonal Notes

1. **Computation**

As a Bayesian algorithm, BEAST is fast and is possibly among the fastest implementations of Bayesian time-series analysis algorithms of the same nature. For applications dealing with a few to thousands of time series, the computation won't be an practical concern at all. But for remote sensing applications that may easily involve millions or billions of time series, compuation will be a big challenge for Desktop computer users. We suggest first testing BEAST on a single time series or small image chips first to determine whether BEAST is appropriate for your applications and, if yes, estimate how long it may take to process the whole image. We also welcome consulation with Kaiguang Zhao (zhao.1423@osu.edu) to give specific suggestions if you see some value of BEAST for your applications.

2. **Complifation from source code**

The BEAST source code appears more complicated than neccessary, mainly because the same source is used for both R and Matlab interfaces as well as for various different compliation settigns (e.g., compiler variants, alternative library dependencies, cross-platform compatibility, mixed language interfaces, and Win32 API native interfaces). The compiliation control variables are defined as MARCOs in abc_marco.h. Of the soure code files, there are dozens of "abc_xxxx.c" files, which are some auxliary files; the BEAST aglrotihim itself is coded in beast_multipleChain_fast2.c; and the R and Matlab inferfaces are coded in glue.c.


We tested our source code under many common compliers (e.g., MSVC, gcc, clang, and Oracle Developer Studio) and all succesfully passed (e.g., see the [Rbeast package status report](https://cran.r-project.org/web/checks/check_results_Rbeast.html)). To complie for R, you need to make sure your machine has a C and a Fotran compiler appropriately set up. For example, see [Package Development Prerequisites](http://www.rstudio.com/ide/docs/packages/prerequisites) for the tools needed for your operating system. In particular, on Windows platforms, the most convenient option is to go with the Rtools toolkit. To complile for Matlab, the appropriate C/C++ header files (e.g., mex.h) have to be correctly specified. Below are some compliation schemes using the gnu compilers as an example.

* To create the Matlab libray, run the following steps.

     1. Go to abc_macro.h and change the contorl macros as follows:
     
        ```C
        #define R_INTERFACE 0       // Disable  the R interface
        #define M_INTERFACE 1       // Enable the Matlab interface
        #define MYMAT_LIBRARY 1     // Use the default math libary provided in sfloatMath.f
        #define MKL_LIBRARY   0     // don't use Intel's MKL library
        #define MATLAB_LIBRARY 0    // Don't use Matlab's own math library
        #define MYRAND_LIBRARY  1   // Use our own random-number generating library (i.e.,abc_rand_pcg.c)
        #define MKLRAND_LIBRARY 0   // Don't use Intel's MKL random-number library
        #define R_RELEASE   0       // Disable the R infterface
        ```

    2. Go to your local folder  where the source files are saved. Complie them into object files
    

        `gcc -c -fPIC *.c -DMATLAB_MEX_FILE  -I"Path to your local matlab include folder (i.e., the include folders containing mex.h, etc.)"`
        
    3. Compile the fortan math library
     
        `gfortran -c -fPCI sfloatMath.f`
        
       
    4. Link all the objects into the Matlab mex library
     
        
        `gcc -shared -o beast_default.mexw64 *.o -lmx -lmex -lmat -L"Path to your local matlab static library folder"`
         

* To create the R libray (which is part of the R pacakge but not the whole R pacakge itself), run the following steps.

     1. Go to abc_macro.h and change the contorl macros as follows:
     
        ```C
        #define R_INTERFACE 1       // Enable  the R interface
        #define M_INTERFACE 0       // Disable the Matlab interface
        #define MYMAT_LIBRARY 1     // Use the default math libary provided in sfloatMath.f
        #define MKL_LIBRARY   0     // don't use Intel's MKL library
        #define MATLAB_LIBRARY 0    // Don't use Matlab's own math library
        #define MYRAND_LIBRARY  1   // Use our own random-number generating library (i.e.,abc_rand_pcg.c)
        #define MKLRAND_LIBRARY 0   // Don't use Intel's MKL random-number library
        #define R_RELEASE   1       // Enable the R infterface
        ```

    2. Go to your local folder where the source files are saved. Complie them into object files
    
        `gcc -c -fPIC *.c  -I"Path to your local R include folder (i.e., the include folders containing Rinternal.h, etc.)""`
        
    3. Compile the fortan math library
     
        `gfortran -c -fPCI sfloatMath.f`
        
       
    4. Link all the objects into the R dll library
        
        `gcc -shared -o Rbeast.dll *.o -lR -lRblas -lRlapack -L"Path to your local R static library folder"`
         
 

## Reporting Bugs

BEAST is distributed as is and without warranty of suitability for application. The one distribubuted above is a beta version, with potentail room for further improvement. If you encounter flaws with the software (i.e. bugs) please report the issue. Providing a detailed description of the conditions under which the bug occurred will help to identify the bug. *Use the [Issues tracker](https://github.com/zhaokg/Rbeast/issues) on GitHub to report issues with the software and to request feature enchancements. Alternatively, you can directly email its maintainer Dr. Kaiguang Zhao at zhao.1423@osu.edu

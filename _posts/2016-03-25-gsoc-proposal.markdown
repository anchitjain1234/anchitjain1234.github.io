---
layout: post
comments: true
title:  "GSoC Proposal"
date:   2016-03-25 20:00:00 +0530
category: code
tags: gsoc
---
Here is my GSoC proposal. I have removed some of the personal information from it.

# Student Info

**Name**  Anchit Jain   
**Github** [anchitjain1234](https://github.com/anchitjain1234)  
**IRC Nick** anchitjain@freenode   
**Time Zone** IST (UTC + 05:30)  
**GSOC Blog URL** [http://www.anchitja.in/blog/](https://www.anchitja.in/blog/)  
**Blog RSS feed** [http://www.anchitja.in/atom.xml/](http://www.anchitja.in/atom.xml)  
**University information**
  
  * **University name** : [Birla Institute of Technology and Science Pilani](http://www.bits-pilani.ac.in/) 
  * **Major** : Computer Science, Economics 
  * **Current Year** : 4th 
  * **Graduation Date** : May 2017
  * **Degree** : BE (Hons) Computer Science, MSc (Hons) Economics 

# Code Sample

I have submitted following patches to ``astropy`` 

##### Merged :

1. [#4606](https://github.com/astropy/astropy/pull/4606) - Support for Path objects in ``io`` packages.  
2. [#4561](https://github.com/astropy/astropy/pull/4561) - Remove new line characters after last row of data in ``ascii.latex.AASTex``.
3. [#4532](https://github.com/astropy/astropy/pull/4532) - Fix to generate zero-length copy of current table.
4. [#4482](https://github.com/astropy/astropy/pull/4482) - Displaying Compound model expressions when printing compound model instances.
5. [#4474](https://github.com/astropy/astropy/pull/4474) - Fix for ``CDS`` reader requiring at least two description characters.

##### Under review :

1. [#4585](https://github.com/astropy/astropy/pull/4585) - Error handling in ``io.fits.compression`` module.
2. [#4574](https://github.com/astropy/astropy/pull/4574) - Allow passing of tuples for setting constraints on model parameters.
3. [#4551](https://github.com/astropy/astropy/pull/4551) - Explicitly set ``BeautifulSoup`` parser to ``html.parser`` to avoid warning.
4. [#4505](https://github.com/astropy/astropy/pull/4505) - Fix for allowing ``WCS`` to take Image HDU.

# Project Info

* ### Project Title  
  Implement Public API for ERFA

* ### Project Abstract  
  Astropy currently uses wrapped version of ERFA library which covers some functions from original C library but not all.  
  If this wrapped library is extended to wrap all functions under ERFA and exposed as a public API, this would help astropy to leverage a validated and trusted library to do computational "heavy-lifting" which might also lead to performance improvements.  
  The two major aims of this project are
  1. Create a separate package under ``liberfa`` which would be having 100% coverage for ERFA functions, good test coverage and documentation such that it follows all the guidelines for ``astropy`` affiliated package.
  2. Use the new package in astropy replacing ``_erfa`` and discuss for replacing overlapping code between ``astropy`` python code and ERFA.  

* ### Deliverables
  1. Working python based ERFA package wrapping original C based ERFA package, strictly adhering to ``astropy`` affiliated package guidelines.
  2. Removal of ``astropy._erfa`` and use of this new python based ERFA package as external (but bundled) dependency.

* ### Proposal Detailed Description  
  ERFA is Essential Routines for Fundamental Astronomy which implements high-quality astronomical routines in C.  
	Currently astropy uses ERFA in ``coordinates`` and ``time`` subpackages for some angle utilities, conversions and time conversion, formatting by using ``_erfa`` subpackage.  
	But it has been under discussion in community for a long time ([#3123](https://github.com/astropy/astropy/pull/3123), [#4571](https://github.com/astropy/astropy/pull/4571)) to use ERFA as an external package in ``astropy`` by exposing ERFA as public API. Recently ([#4664](https://github.com/astropy/astropy/pull/4664)) there has been also some discussion to whether replace python code in coordinates subpackage with ERFA code.  
	Therefore exposing ERFA as public API would be beneficial for Astropy and the larger scientific Python community.  
	I propose to do the following changes in two phases:

	1. #### Phase 1(7 weeks) - ERFA as separate package  
    Right now for ERFA functionality, astropy uses ``_erfa``   subpackage which wraps ERFA functions only from ``Astronomy`` section, ``AngleOps`` and ``SphericalCartesian`` subsections under ``VectorMatrix`` section.   
      In this phase I propose to increase wrapping and test coverage, improve function names and documentation.  
      I intend to do this in 3 subphases:  
        1. ###### Extension of wrapping
          * First I would start with Astropy affiliated package template and using the existing ``_erfa`` code in it, to create a new package possibly named ``pyerfa``. Then I intend to extend it(wrapping) to all the remaining sections/subsections such that package could be imported as independent package, and all the ERFA  functions would work without worrying about correctness of the results, just get every ERFA function working.
          * Currently functions under ``CopyExtendExtract`` under ``VectorMatrix`` do give some weird error on wrapping. So my first task would be to parse functions under ``CopyExtendExtract`` correctly.
          * Functions under other subsections were getting compiled correctly so next task would be to check that they are parsed correctly i.e. check if they do work and if not correct them.
          * Existing ``docstring`` parser would need to be modified such that new parts to the ``docstrings`` could be added which are not present currently in original C library.
        2. ###### Testing of wrappings
          * In this subphase I intend to test rigorously all the new wrappings extended to ensure result correctness following Astropy guidelines for testing.
          * This would involve testing the wrappings under different values of arguments and input types etc. using ``py.test`` module and following Astropy standard for testing.  
          * Existing C test suite for ERFA (``t_erfa_c.c``) would be ported to Python by automatic conversion in the similar manner, by creating parsing templates for test suite and test suite generator. Parts which couldn’t be ported, would be hand-written. Through this approach we would be having exactly same test suite as of original SOFA/ERFA tests.
          * Overall objective would be to create a test suite having coverage >= 90%.
        3. ###### Create name aliases and improve documentation
          * In this subphase I would work on creating name aliases for the functions from ERFA as current names are very cryptic which don't suit well for code readability and understanding. For example. ``eraAb`` is a function in ERFA that performs aberration to transform natural direction into proper direction but this isn't very much clear from this name. Documentation improvement would go hand in hand with name aliasing
          * Name aliasing would involve manual work of checking code, work to be done by function, docs, arguments for function etc. and most importantly discussion with mentors to be done for every function.
          * Documentation improvement would include updating of current ``docstrings`` generated by C comments to include examples following ``doctest``, short summary and other notes  to follow ``numpy`` style ``docstrings``.

  2. ##### Phase 2(5 weeks) - ERFA as separate package
      Under this phase my focus would be on to use this newly generated package within Astropy.  
      I intend to do this in 2 subphases:
      1. ###### Update ``_erfa`` usage
          * As ``_erfa`` would no longer be needed therefore update usages of ``_erfa`` by the new package and get everything working
          * This would be done by creating new ERFA package as external dependency by placing it in ``astropy/extern`` and update its usages. Then ``_erfa`` would be removed. 
          
      2. ###### Overlapping code replacement
          * This subphase would involve discussion to whether replace python code with ERFA based on the results of performance analysis. 
          * Here first I would try to find parts of code where ERFA could be used, mainly in ``coordinates``, ``time`` etc. This would be done by carefully looking at the existing codebase to check whether ERFA function would suit here. 
          * Then using performance analysis and discussion with mentors replace the code for the parts where agreement comes. 
          * Performance analysis would be done using [``asv``](https://github.com/spacetelescope/asv) by writing benchmarks first for existing code and then for code using ERFA. 

# Timeline 

| Week | Work |
| ---  | ---  |
| **Community bonding period (22 Apr - 22 May)** | Check ``astropy._erfa`` and ``liberfa/erfa`` code closely, discuss concrete approach with mentors, read documentation for ``asv``, ``cython`` . |
| **Week 1 (23 May - 29 May)** | 1. Setup new repository using ``astropy`` affiliated package guidelines and ``_erfa`` existing code.<br/> 2. Make this repository work independently without Astropy. |
| **Week 2 (30 May - 5 June)** | 1. Extend to ``CopyExtendExtract`` subsection.<br/> 2.Modify parsing templates. |
| **Week 3 (6 June - 12 June)** | 1. Extend to new sections of ERFA.<br/> 2.Make whole package working. |
| **Week 4 (13 June - 19 June)** | Port original test suite(``t_erfa_c.c``) to Python is similar manner (automated like wrapping). |
| **Week 5 (20 June - 26 June)** | 1. Convert manually tests which couldn't be converted by automation.<br/> 2.Ensure good coverage. |
| **Weeks 6-7 (27 June - 10 July)** | 1. Create name aliases after discussion with mentors. <br/> 2.Improve documentation to Astropy standard. |
| **Week 8 (11 July - 17 July)** | 1. Use new ERFA python wrappings package.<br/> 2.Remove existing ``_erfa`` subpackage from ``astropy``. |
| **Week 9 (18 July - 24 July )** | Check carefully astropy code, mainly in sections ``coordinates`` and ``time`` where existing python code could be replaced by ERFA function. |
| **Weeks 10-11 (25 July - 7 August)** | Write benchmarks to evaluate performance of existing code and code after replacing with ERFA. |
| **Week 12-13 (8 August - 23 August)** | 1. Discuss with mentors after performance analysis which parts to replace and which not.<br/> 2.Complete all the remaining spilled work from previous phases. |

# Other Commitments

1. **Do you have any other commitments during the main GSoC time period, May 23rd to August 23rd?**
    * My end semester examinations would be from 4 May - 15 May, so I would not be able to do much research then.
    * My new semester would be starting from 1 August, this might decrease my productivity, but I would try my best to not let it affect me.


2. **Have you applied with any other organizations?**  
No. I have just applied to Astropy. 

3. **Have you participated previously in GSoC? when? with which project?**  
No. This is first time I am applying for GSoC.

# Extra information 

**Link to Resume** [resume](http://www.anchitja.in/assets/resume.pdf/)

**Alternate contacts**

  * **Facebook** [anchit.jain.1234](https://www.facebook.com/anchit.jain.1234)
  * **Twitter** : [anchitjain1234](https://twitter.com/anchitjain1234) 



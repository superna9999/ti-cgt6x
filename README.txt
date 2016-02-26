TMS320C6000 C/C++ CODE GENERATION TOOLS
7.3.4 Release Notes
March 2012


===============================================================================
Contents
===============================================================================
1) Support Information
   1.1) Defect History
   1.2) Compiler Wiki
   1.3) Compiler Documentation Errata
   1.4) TI E2E Community
   1.5) C67x Fast Run-Time Support (RTS) Library
   1.6) Defect Tracking Database
2) Early Adopter Version of Prelink Utility - prelink6x
3) Maximum number of open files increased
4) Performance Improvements
5) 32-bit wchar_t support  
6) divf/divd RTS routines improvements for C67xx ISAs
7) Fewer pre-built libraries, automatic library building
8) C6000 Linux ABI Support

-------------------------------------------------------------------------------
1) Support Information
-------------------------------------------------------------------------------

1.1) Defect History
-------------------

The list of defects fixed in this release as well as known issues can
be found in the file DefectHistory.txt.

1.2) Compiler Wiki
------------------

A Wiki has been established to assist developers in using TI Embedded
Processor Software and Tools.  Developers are encouraged to read and
contribute to the articles.  Registered users can update missing or 
incorrect information.  There is a large section of compiler-related 
material.  Please visit:

http://tiexpressdsp.com/wiki/index.php?title=Category:CGT

1.3) Compiler Documentation Errata
----------------------------------

Errata for the "TMS320C6000 Optimizing Compiler User's Guide" and the
"TMS320C6000 Assembly Language User's Guide" is available online at the
Texas Instruments Embedded Processors CG Wiki:

http://tiexpressdsp.com/wiki/index.php?title=Category:CGT

under the 'Compiler Documentation Errata' link.

1.4) TI E2E Community
---------------------

Questions concerning TI Code Generation Tools can be posted to the TI E2E
Community forums.  The "Development Tools" forum can be found at:

http://e2e.ti.com/support/development_tools/f/default.aspx

1.5) C67x Fast Run-Time Support (RTS) Library
---------------------------------------------

These libraries are no longer included in the C6000 Code Generation Tools.
They are available for download at the following Texas Instruments page:

http://focus.ti.com/docs/toolsw/folders/print/sprc060.html

Or search the ti.com site for the tag: tms320c67x fastrts library

1.6) Defect Tracking Database
-----------------------------

Compiler defect reports can be tracked at the Development Tools bug
database, SDOWP. The log in page for SDOWP, as well as a link to create
an account with the defect tracking database is found at:

https://cqweb.ext.ti.com/pages/SDO-Web.html

A my.ti.com account is required to access this page.  To find an issue
in SDOWP, enter your bug id in the "Find Record ID" box once logged in.
To find tables of all compiler issues click the queries under the folder:

"Public Queries" -> "Development Tools" -> "TI C-C++ Compiler"

With your SDOWP account you can save your own queries in your
"Personal Queries" folder.


-------------------------------------------------------------------------------
2) Early Adopter Version of Prelink Utility - prelink6x
-------------------------------------------------------------------------------

The C6x CGT 7.3 release includes an early adopter version of a prelink utility,
prelink6x, which can be used in the development flow of a dynamic system to 
offload tasks that are normally performed by the dynamic linker/loader at
load time. By reducing the work required of the dynamic linker/loader, the
prelinker helps to reduce the load and/or startup time of a dynamic
application.

A more detailed description of the prelink utility's functionality can be 
found in the "prelink" sub-directory of this release distribution. Please
see the prelink_readme.txt in that directory for more details about the 
prelink utility and some walk-through examples of how it is used.

-------------------------------------------------------------------------------
3) Maximum number of open files increased
-------------------------------------------------------------------------------

The maximum number of files that can be simultaneously opened has increased
from 10 to 20 in this release. This means that users can have up to 17 files
open at the same time. Three file handles are used for STDIN/STDOUT/STDERR by 
default.

-------------------------------------------------------------------------------
4) Performance Improvements
-------------------------------------------------------------------------------

This 7.3.0 release contains notable performance improvements:

The performance of certain C6600-specific loops has been improved.

The performance of certain control-code loops has been improved.

-------------------------------------------------------------------------------
5) 32-bit wchar_t support  
-------------------------------------------------------------------------------
This 7.3.0 supports an option to generate 32-bit wchar_t type. The 
default compiler behavior is unchanged; it continues to generate 16-bit wchar_t
by default. When the --wchar_t=32 option is specified, the compiler generates
32-bit wchar_t type. Here are the details:

* The --wchar_t=32 option is allowed only in EABI mode. If this option is 
  specified in COFF ABI mode, the following warning is generated and the
  option is ignored

     warning: Option --wchar_t=32 is not valid without --abi=eabi (ignored)

* 16-bits wchar_t objects are not compatible with the 32-bit wchar_t objects. 
  If the 32-bit wchar_t objects are mixed with 16-bit wchar_t objects the 
  following error is generated:

     fatal error #16032: object files have incompatible wchar_t types
        ("file1.obj = --wchar_t=16, "file2.obj" = --wchar_t=32)

* When the shell option --linux is specified, it also implies --wchar_t=32

Background:
-----------
C/C++ supports the type wchar_t to represent wider characters.  The C99 and C++
standards leave the size of wchar_t 'implementation defined'. The requirement 
is that the underlying type be an integer type that can represent the distinct 
codes for all members of the largest extended character set from the supported 
locale. Hence the platform which implements the locale influences the size of 
whcar_t. Linux uses 32-bit extended characters and hence requires that the 
wchar_t type is 32-bits wide. So, when building an application to run on C6x 
Linux wchar_t should be 32-bits. 

There are two motivations to support this option. One is to support building
applications or part of application using TI compiler to run on C6x Linux. The 
other is to support interlinking with C6x GCC compiler generated objects. GCC 
by default generates 32-bit wchar_t types. 

-------------------------------------------------------------------------------
6) Floating point divide routines improvements for C67xx ISAs
-------------------------------------------------------------------------------

In this 7.3.0 release, the floating point divide routines (divf/divd) 
have been improved:

* All NaN/INF/ZERO inputs are handled correctly.

* Overflow and underflow of the calculation is handled correctly. 

* Divide routines on floating point parts are about 3x faster.

* We expect the precision is at least at the same level as the previous 
  implementation.  We have verified that the floating point divide routines 
  are generating the correct result up to the least significant mantissa bit 
  for our test cases.

  However, it is impossible to test all combinations. We make no guarantees
  that our floating point divide routines are conforming to the IEEE 752-2008 
  standard.  There is still possibility that the last mantissa bit can be 
  different from the exact result due to rounding errors. However, we expect 
  that case to be very rare.

-------------------------------------------------------------------------------
7) Fewer pre-built libraries, automatic library building
-------------------------------------------------------------------------------

This release provides fewer pre-built compiler run-time support libraries,
which makes the CCS package download size much smaller, reducing download
time.  Users are expected to build less-commonly used libraries as needed.
The RTS source code is provided in each compiler release, which allows the
user to build libraries with custom command-line options, and it also allows
the linker to automatically build missing libraries.  

Users with shared or read-only installation directories need to take special
action to build (at installation time) libraries that will be needed.

More details about the mechanism behind rebuilding libraries can be found at
the TI Embedded Processors Wiki http://processors.wiki.ti.com/index.php/Mklib
      
-------------------------------------------------------------------------------
8) C6000 Linux ABI Support
-------------------------------------------------------------------------------

This release includes support for the creation of object files and dynamic
object modules that are compliant with the C6000 Linux ABI as specified in
the C6000 EABI Specification (SPRAB89). For further details on the C6000
Linux ABI, please see the C6000 EABI Specification (SPRAB89). For further
details about support for the C6000 Linux ABI in the C6000 Code Generation
Tools, please see the C6000 Linux Support wiki site:

   http://processors.wiki.ti.com/index.php/C6000_Linux_Support

-- End Of File --

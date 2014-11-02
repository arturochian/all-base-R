---
layout: page
permalink: /all-base-R/dot-Call/
---

## __.Call__

#### _Modern Interfaces to C/C++ code_

### Usage

{% highlight r %}

.C(.NAME, ..., NAOK = FALSE, DUP = TRUE, PACKAGE, ENCODING)
.Fortran(.NAME, ..., NAOK = FALSE, DUP = TRUE, PACKAGE, ENCODING)

{% endhighlight %}

### Arguments

`.NAME`
a character string giving the name of a C function or Fortran subroutine, or an object of class "NativeSymbolInfo", "RegisteredNativeSymbol" or "NativeSymbol" referring to such a name.

`...`
arguments to be passed to the foreign function. Up to 65.

`NAOK`
if TRUE then any NA or NaN or Inf values in the arguments are passed on to the foreign function. If FALSE, the presence of NA or NaN or Inf values is regarded as an error.

`DUP`
if TRUE then arguments are duplicated before their address is passed to C or Fortran.

`PACKAGE`
if supplied, confine the search for a character string .NAME to the DLL given by this argument (plus the conventional extension, ‘.so’, ‘.dll’, ...).
This is intended to add safety for packages, which can ensure by using this argument that no other package can override their external symbols, and also speeds up the search (see ‘Note’).

`ENCODING`
For back-compatibility, accepted but ignored.

### Notes

These functions can be used to make calls to compiled C and Fortran 77 code. Later interfaces are .Call and .External which are more flexible and have better performance.

These functions are both primitive, and .NAME is always matched to the first argument supplied (which should not be named). The other named arguments follow ... and so cannot be abbreviated. For clarity, should avoid using names in the arguments passed to ... that match or partially match .NAME.

### Values
A list similar to the ... list of arguments passed in (including any names given to the arguments), but reflecting any changes made by the C or Fortran code.

### Argument types
The mapping of the types of R arguments to C or Fortran arguments is

R -- C	-- Fortran
integer	-- int* -- integer
numeric -- double* -- double precision
-- or -- -- float* -- real
complex -- Rcomplex* -- double complex
logical -- int* -- integer
character -- char** -- [see below]
raw -- unsigned char* -- not allowed
list -- SEXP* -- not allowed
other -- SEXP -- not allowed

Numeric vectors in R will be passed as type double * to C (and as double precision to Fortran) unless the argument has attribute Csingle set to TRUE (use as.single or single). This mechanism is only intended to be used to facilitate the interfacing of existing C and Fortran code.

The C type Rcomplex is defined in ‘Complex.h’ as a typedef struct {double r; double i;}. It may or may not be equivalent to the C99 double complex type, depending on the compiler used.

Logical values are sent as   (FALSE), 1 (TRUE) or INT_MIN = -2147483648 (NA, but only if NAOK = TRUE), and the compiled code should return one of these three values: however non-zero values other than INT_MIN are mapped to TRUE.

Note: The C types corresponding to integer and logical are int, not long as in S. This difference matters on most 64-bit platforms, where int is 32-bit and long is 64-bit (but not on 64-bit Windows).

Note: The Fortran type corresponding to logical is integer, not logical: the difference matters on some Fortran compilers.

Missing (NA) string values are passed to .C as the string "NA". As the C char type can represent all possible bit patterns there appears to be no way to distinguish missing strings from the string "NA". If this distinction is important use .Call.

.Fortran passes the first (only) character string of a character vector is passed as a C character array to Fortran: that may be usable as character*255 if its true length is passed separately. Only up to 255 characters of the string are passed back. (How well this works, and even if it works at all, depends on the C and Fortran compilers and the platform.)

Lists, functions are other R objects can (for historical reasons) be passed to .C, but the .Call interface is much preferred. All inputs apart from atomic vectors should be regarded as read-only, and all apart from vectors (including lists), functions and environments are now deprecated.
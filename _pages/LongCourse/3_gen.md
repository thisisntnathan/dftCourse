---
layout: page
title: Custom Basis Sets and Effective Core Potentials in <kbd>Gaussian</kbd>
sub-title: The DFT Long Course
sidebar_link: true
sidebar_sort_order: 3
permalink: /LongCourse/gen.html
---
<!-- markdownlint-disable-file MD040 -->

### Specifying additional/alternative basis functions

<kbd>Gaussian</kbd> has an extensive collection of built-in options for [basis sets](https://gaussian.com/basissets/). The included basis sets will be sufficient for the vast majority of computational needs; however, the need may arise for additional basis functions or further customization, which can be accomplished using the `gen` and `extraBasis` keywords.  

#### `gen`

`gen` allows you to specify your own basis set specifications to the atom level. It is used in place of a basis set keyword in the routing line (e.g. `#N M062X/gen`). The basis set specification is then read *after* the molecule specification. The basis definition input can either be entered directly into the input file or read into the input stream using <kbd>g16</kbd>'s include (`@`) function as described [earlier](/dftCourse/LongCourse/bashScripting.html) in the course.

The basis specification can either utilize <kbd>g16</kbd>'s built-in basis functions or custom-defined basis definitions, but they must follow a specific format:  

```
...
{Molecule geometry}
<<Blank line>>
H 0                                                 ! {atomSymbol} 0
S    3   1.00                                       ! these are the basis functions
      0.122518D+02           0.233371D-01           ! from Jensen's pcseg-1
      0.186871D+01           0.159150D+00
      0.418208D+00           0.500000D+00
S    1   1.00
      0.106100D+00           1.0000000
P    1   1.00
      0.100000D+01           1.0000000
****                                                ! **** tells g16 that its done
He 0                                                ! reading functions for this atom
S    3   1.00                                       ! and to move to the next
      0.368654D+02           0.273556D-01
      0.558021D+01           0.174855D+00
      0.119170D+01           0.500000D+00
S    1   1.00
      0.268925D+00           1.0000000
P    1   1.00
      0.145000D+01           1.0000000
****
<<More atoms>>                                      ! add as many as you need
****                                                ! this section ends with ****
<<Blank line>>                                      ! the g16 input file ends with
<<Blank line>>                                      ! 2 blank lines
```

>If <kbd>Gaussian</kbd> encounters an atom that doesn't appear in the molecule it will kill the run (this is a safety feature meant to detect input mistakes). This can be circumvented by reformatting the atom definition line from `{atomSymbol} 0` to `-{atomSymbol}`, (in fact, this is how <kbd>g16</kbd>'s own built-in basis sets are defined). This is useful when creating standard basis set files for amy atoms which are intended to be read directly into the input stream using the include (`@`) function.

By default specifying a basis set in the routing line applies it to **all** atoms in the calculation.

<!-- TODO: Add code snippet showing how to define basis functions by atom -->

#### `extraBasis`

<!-- TODO: Add description of extraBasis (adds functions to already defined centers) w/ code snippet -->

<br />

#### References

(1) [`gen`](https://gaussian.com/gen/)  
(2) Unifying General and Segmented Contracted Basis Sets. Segmented Polarization Consistent Basis Sets by Frank Jensen[*J. Chem. Theory Comput.* **2014**, *10* (3), 1074](https://pubs.acs.org/doi/10.1021/ct401026a)

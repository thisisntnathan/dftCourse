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

`gen` allows you to specify your own basis set specifications to the atom level. It is used in place of a basis set keyword in the routing line (e.g. `#N M062X/gen`). The basis set specification is then read *after* the molecule specification. The basis specification can either 

##### A naïve example

#### `extraBasis`


<br />

#### References

(1) [`gen`](https://gaussian.com/gen/)

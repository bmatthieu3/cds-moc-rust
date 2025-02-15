# `moc` Change Log

## 0.11.3 (no bug fix, but minor add)

Released 2023-03-31

### Added

* Methods for frequency MOCs in store


## 0.11.2

Released 2023-03-06

### Fixed

* Wrong constant (pi/2 instead of pi) when checking elliptical cone position angle



## 0.11.1

Released 2023-02-17

### Fixed

* Dumb initialization of the counts in the store


## 0.11.0

Released 2023-02-17

### Added

* Operations `fill_holes` and `fill_holes_smaller_than`
* Add reference count in store to handle external MOC copies
  (e.g. when using python multiprocessing silnetly resorting 
   on pickle, which cause bugs since the MOC store index is no
   more uniq and is may be dropped)



## 0.10.1

Released 2023-02-13

### Fixed

* Remove spurious "WARNING: Keyword 'TTYPE1' found more than once in a same HDU! We use the first occurrence."
  when readding FITs files


## 0.10.0

Released 2023-02-13

### Fixed

* Computation of T-MOCs and F-MOCs from ranges

### Added

* Add computation of a MOC mean centre and the maximum distance from a given point.
* Add generation of PNG files to visualize a MOC
    + add the mapproj crate dependency
    + add the png crate dependency
* Supports ring indexed skymaps
* Switch to standard ASCII serialisation in RangeMOC
* Add `TTYPE1=RANGE` keyword in FITS files (TTYPE is optional in the FITS standard but without
  it astropy seems not to be able to read the file)
* Add the `CellHpxMOCIterator` trait to easily save S-MOC in FITS files compatible with v1.0
  of the MOC standard.
* Add the store features (for MOCPy, MOCWasm, ...)

## 0.9.0

Released 2022-09-09

### Bug correction

* Print the deepest order in JSON output even when it contains no cell 


## 0.9.0-alpha

Released 2022-06-17

### Added

* Add Frequency MOCs


## 0.8.0

Released 2022-04-13

### Added

* Add the multi `or` operation
* Add MOC from mulitple cones (a lot of small cones or a resonnable number of large cones)
* Add support for specific FITS skymaps (possibly gzipped)
* Add gzip support for FITS Multi-Order Map


## 0.7.1

Released 2022-03-22 (not a bug fix)

### Added

* Possibility to perform operations on borrowed ranges in addition to owned ranges


## 0.7.0

Released 2022-02-04

### Added

* Add MOC fromm ring
* Add the possibility to choose indirect neighbours (8, instead of the 4 direct neighbours) when splitting a MOC

### Enhancement

* Make FITS deserialization more robust for UNIQ indices
  (to cope with a -- now fixed -- Aladin bug adding trailing '0' uniq indices)  


## 0.6.1

Released 2021-11-15

### Modification

* Remove the wasm/nowasm `create_from_time_ranges_spatial_coverage` instead of wasm only (used in MOCPy) 

## 0.6.0

Released 2021-11-15

### Added

* Add well formed ASCII/Json tests at deserialization
* `Split` a disjoint MOC into joint MOCs
* Add direct support for FITS Multi-Order Map

## 0.5.0

Released 2021-10-18

### Added

* ST-MOC union on iterators
* ST-MOC builder from (time\_idx, pos\_idx) iterator
* ST-MOC builder from (time\_range, pos\_idx) iterator
* Options to MocFromValuedCells

### Bug correction

* Correct a bug in MocFromValuedCells
* Several other bug corrections

## 0.4.0

Put apart from MOCPy: 2021-08-16


### Previous versions

The original code was part of [MOCPy](https://github.com/cds-astro/mocpy).
It has evolved and put in this separated crate.
See `src/interval` (then renamed `src/moc`)
in the [mocpy](https://github.com/cds-astro/mocpy) project.


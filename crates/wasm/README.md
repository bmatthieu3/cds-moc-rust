# `moc-wasm`

WebAssembly Library to read/write/create/manipulate HEALPix **M**ulti-**O**rder **C**overage maps (**MOC**s) in a web page.

## About

This [wasm](https://webassembly.org/) library is made from the [Rust MOC library](https://github.com/cds-astro/cds-moc-rust).
It implements the v2.0 of the [MOC standard](https://ivoa.net/documents/MOC/),
including (S-)MOCs, T-MOCs and ST-MOCs.

If you are developing a web page using:
* **JavaScript**: you can use this library (see next section for the list of available JS functions);
* **Rust --> wasm**: the code can be an example of how to directly use the Rust MOC library.

Technically, this project relies on [wasm-bindgen](https://rustwasm.github.io/docs/wasm-bindgen/): a big
thank you to the developers.

For tools able to display MOCs, see:
* the [Aladin Desktop](https://aladin.u-strasbg.fr/) sky atlas in Java (also supports MOC operations) 
* the [Aladin Lite](https://aladin.u-strasbg.fr/AladinLite/), "a lightweight version of the Aladin Sky Atlas running in the browser".
* [MOCPy](https://cds-astro.github.io/mocpy/), a python wrapper using the very same Rust MOC library.



## Use it

### Try it

From this [demo page (old version)](http://cdsxmatch.u-strasbg.fr/lab/mocwasm/).
From this [demo page (new version)](http://cdsxmatch.u-strasbg.fr/lab/mocwasm/).


### Put it in your own Web page

Download the last `moc-wasm-vxx.tar.gz` from the[github release page](https://github.com/cds-astro/cds-moc-rust/releases).
Put it in the same directory of you web page and decompress it:
```bash
tar xvzf moc-wasm-vxx.tar.gz
```
And add this in your HTML body:
```html
<script type="module">
    import init, * as moc from './pkg/moc.js';
    async function run() {
        const wasm = await init().catch(console.error);
	window.moc = moc;
    }
    run();
</script>
```
This [the example page](https://github.com/cds-astro/cds-moc-rust/blob/main/crates/wasm/index.html).


### Used by

Example of web page(s) using MOCwasm:
* [Gravitational-Wave Sky Localizations](https://virgo.pg.infn.it/maps/index.html)


### Use it in you project with NPM

TBW


## Available JavaScript methods  

Following the provided [index.html](index.html) example, 
use the `moc` prefix to, call the following methods (e.g. `moc.list()`):
```bash
# In case of failure, enable Rust stack trace
debugOn();

# Info
# - List the name of the MOCs loaded in memory.
list() -> Array<String>
# - Returns the quantity type (space, time or space-time) of the MOC having the given name
qtype(name) -> String
# - Returns information on the MOC having the given name
info(name) -> Object
# - Remove from memory the MOC of given name
drop(name)

# Load
# - fires the select file dialog (see JS file for more details) to load a local MOCs
fromLocalFile(empty|'space'|'time'|'space-time')
# - load the MOC from a FITS file content in the provided UInt8Array 
fromFits(name, data: UInt8Array)
# - load the MOC stored in a FITS file of given URL, and store it with the given name 
fromFitsUrl(name, url) // A string of supported mimi types can be passed in an additional last parameter
# - load S/T/ST-MOC from a ASCII String or an ASCII file
smocFromAscii(name, data: String)
smocFromAsciiUrl(name, url)
tmocFromAscii(name, data: String)
tmocFromAsciiUrl(name, url)
stmocFromAscii(name, data: String)
stmocFromAsciiUrl(name, url)
# - load S/T/ST-MOC from a JSON String or a JSON file
smocFromJson(name, data: String)
smocFromJsonUrl(name, url)
tmocFromJson(name, data: String)
tmocFromJsonUrl(name, url)
stmocFromJson(name, data: String)
stmocFromJsonUrl(name, url)

# - fires the select dialogto load a multi-order map FITS file and create a MOC from the given parameters
fromLocalMultiOrderMap(from_threshold: f64, to_threshold: f64, asc: bool, not_strict: bool, split: bool, revese_recursive_descent: bool)
# - load a multi-order map FITS file and create a MOC from the given parameters
fromFitsMulitOrderMap(name, data: UInt8Array, from_threshold: f64, to_threshold: f64, asc: bool, not_strict: bool, split: bool, revese_recursive_descent: bool)
# - load a multi-order map FITS file of given URL and create a MOC from the given parameters
fromMultiOrderMapFitsUrl(name, url, from_threshold: f64, to_threshold: f64, asc: bool, not_strict: bool, split: bool, revese_recursive_descent: bool) // A string of supported mime types can be passed in an additional last parameter

# Save a MOC
# - get the FITS binary representation of the MOC of given name  
toFits(name) -> Uint8Array
# - get the ASCII representation of the MOC of given name  
toAscii(name, fold: null|int) -> String
# - get the JSON representation of the MOC of given name  
toJson(name, fold: null|int) -> String
# - fires the download dialog to save the MOC in an ASCII/JSON or FITS file.
toFitsFile(name)
toAsciiFile(name, fold: null|int)
toJsonFile(name, fold: null|int)

# Create MOC
# - create a S-MOC from a geometric shape
fromCone(name, depth, lon_deg, lat_deg, radius_deg) 
fromRing(name, depth, lon_deg, lat_deg, internal_radius_deg, external_radius_deg) 
fromEllipse(name, depth, lon_deg, lat_deg, a_deg, b_deg, pa_deg)
fromZone(name, depth, lon_deg_min, lat_deg_min, lon_deg_max, lat_deg_max)
fromBox(name, depth, lon_deg, lat_deg, a_deg, b_deg, pa_deg)
fromPolygon(name, depth, vertices_deg: Float64Array, complement: boolean)
# - create a S-MOC from a list of coordinates
fromCoo(name, depth, coos_deg: Float64Array)
# - create a T-MOC from a list of Julian Days
fromDecimalJDs(name, depth, jd: Float64Array)
# - create a T-MOC from a list of Juliand Days range
fromDecimalJDRanges(name, depth, jd_ranges: Float64Array)
# - reate a new S-MOC from the given lists of UNIQ and Values (i.e. from a Multi-Order Map)
fromValuedCells(name, depth,  density: bool, from_threshold: f64, to_threshold: f64, asc: bool,  not_strict: bool, split: bool, revese_recursive_descent: bool, uniqs: Float64Array, values: Float64Array)

# Single MOC operations
# S/T-MOC
not(name, out_name) / complement(name, out_name)
degrade(name, depth, out_name)
# - S-MOC only
extend(name, out_name)
contract(name, out_name)
externalBorder(name, out_name)
internalBorder(name, out_name)
# -- split considering the 4 direct neighbours (NE, NW, SE, SW) as being part of the same region
splitCount(name)
split(name, out_name_prefix)
# -- split considering the 4 direct neighbours (NE, NW, SE, SW) plus the 4 indirect neighbours (S, N, E, W) as being part of the same region
splitIndirectCount(name)
splitIndirect(name, out_name_prefix)

# Two MOCs operations
or/union(left_name, right_name, out_name)
and/intersection(left_name, right_name, out_name)
xor/symmetric_difference(left_name, right_name, out_name)
minus/difference(left_name, right_name, out_name)

# Operation on ST-MOC
timeFold(tmoc_name, st_moc_name, out_smoc_name)
spaceFold(smoc_name, st_moc_name, out_tmoc_name)

# Filter operations (return arrays containing 0 (out of the MOC) or 1 (in the MOC))
filterCoos(name, coos_deg: Float64Array) -> Uint8Array
filterJDs(name, jds: Float64Array) -> Uint8Array

```

## Example 1: 2MASS and SDSS DR12 MOCs

In the [index.html](index.html) web page put behind a server (see next section), 
simply copy/paste those line the web browser console:
```java
// Load 2MASS and SDSS DR12 MOCs from CDS      
await moc.fromFitsUrl('2mass', 'http://alasky.u-strasbg.fr/footprints/tables/vizier/II_246_out/MOC');
await moc.fromFitsUrl('sdss12', 'http://alasky.u-strasbg.fr/footprints/tables/vizier/V_147_sdss12/MOC');

// List MOCs loaded in the page
console.log(moc.list());
      
// Init a timer
console.time('timer');
// Performs MOC intersection
moc.and('2mass', 'sdss12', '2mass_inter_sdss12');
// Log time
console.timeLog('timer', 'Intersection');
// Performs MOC union
moc.or('2mass', 'sdss12', '2mass_union_sdss12');
// Log time
console.timeLog('timer', 'Union');
// Degrade to order 2 the result of the intersection      
moc.degrade('2mass_inter_sdss12', 2, '2mass_inter_sdss12_d2')
// Remove timer
console.timeEnd('timer');
      
// List MOCs loaded in the page
console.log(moc.list());
      
// Print the ASCII and JSON serializations of '2mass_inter_sdss12_d2'
console.log(moc.toAscii('2mass_inter_sdss12_d2'));
console.log(moc.toJson('2mass_inter_sdss12_d2'));

// Save the result of the intersection in a FITS file
moc.toFitsFile('2mass_inter_sdss12');
```

## Example 2: Build a MOC from a Multi-Order Map and split it into disjoint MOCs

```java
var n;
// Load a multi-order map and create a MOC on-the-fly
await moc.fromMultiOrderMapFitsUrl('lalmap', 'http://cdsxmatch.u-strasbg.fr/lab/moc/LALInference.multiorder.fits', 0.0, 0.9, false, false, false, false);
// List MOCs loaded in the page
console.log(moc.list());

// Init a timer
console.time('timer');
// Count the number of disjoint MOCs in the MOC
n = moc.splitCount('lalmap');
console.log("n sub_mocs: " + n);
console.timeLog('timer', 'Spit count');
// Do split the MOC in 10 sub-MOCs
moc.split('lalmap', 'lalmap_sub');
console.timeLog('timer', 'Spit');
// Remove timer
console.timeEnd('timer');
        
// List MOCs loaded in the page
console.log(moc.list());

// Get info on sub-MOCs
for (let i = 0; i < n; i++){
    console.log("Coverage percentage sub "+i+": "+moc.info("lalmap_sub_"+i).coverage_percentage);
}
```

## Install/run locally

Checkout the git project.

[Install rust](https://www.rust-lang.org/tools/install)
(check that `~/.cargo/bin/` is in your path), 
or update the Rust compiler with:
```bash
rustup update
``` 

Install the `wasm32-unknown-unknown` toolchain (done automatically with wasm-pack?):
```bash
rustup target add wasm32-unknown-unknown
````

Install [wasm-pack](https://rustwasm.github.io/wasm-pack/) following [those instructions](https://rustwasm.github.io/wasm-pack/installer/) 
or using (automatic download of sources and local compilation):
```bash
cargo install wasm-pack
```

Run tests:
```bash
wasm-pack test --node
```

Build the project, see wasm-bindgen [doc](https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html)
```bash
wasm-pack build --out-name moc --target web --no-typescript 
wasm-pack build --out-name moc --target web --no-typescript --release
```

Run a local server
* either using the static server from https://crates.io/crates/https
```bash
http
```
* or use python
```bash
python2 -m SimpleHTTPServer
python3 -m http.server
```

And load the web page [http://0.0.0.0:8000/](http://0.0.0.0:8000/) in our favorite (firefox?) web browser.

## Publish on NPM (self reminder)

See [here](https://rustwasm.github.io/docs/book/game-of-life/publishing-to-npm.html)
```bash
wasm-pack build --out-name moc --release
wasm-pack login
wasm-pack publish
```

## ToDo list

* [ ] Implement `difference (xor)` for `ST-MOCs`
* [ ] Implement `complement (not)` for `ST-MOCs` (complement on Space only or also on Time with allsky S-MOCs?)
* [ ] Implement `degradeSpace` (?), `degradeTime` (?), `degradeSpaceAndTime` (?) for `ST-MOCs`
* [ ] Build a `ST-MOCs` from an array of `(jd, lon, lat)`
* [ ] Add possibility to filter an array of `(jd, lon, lat)` with a `ST-MOCs`
* [ ] Add `overlap/contains(MOC, MOC)` methods? (use cases?)
* [ ] (Internal change for performances) add native operations on `RangeMOC2` instead of transforming in `TimeSpaceMoc`
* [ ] Implement an [egui](https://crates.io/crates/egui) interface

## License

Like most projects in Rust, this project is licensed under either of

* Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
  http://www.apache.org/licenses/LICENSE-2.0)
* MIT license ([LICENSE-MIT](LICENSE-MIT) or
  http://opensource.org/licenses/MIT)

at your option.


## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in this project by you, as defined in the Apache-2.0 license,
shall be dual licensed as above, without any additional terms or conditions.


## Acknowledgements

This work has been supported by the ESCAPE project.  
ESCAPE - The **E**uropean **S**cience **C**luster of **A**stronomy & **P**article Physics **E**SFRI Research Infrastructures -
has received funding from the **European Union’s Horizon 2020** research and innovation programme under **Grant Agreement no. 824064**.


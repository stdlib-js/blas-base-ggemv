<!--

@license Apache-2.0

Copyright (c) 2025 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# ggemv

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Perform one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`.



<section class="usage">

## Usage

```javascript
import ggemv from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-ggemv@deno/mod.js';
```
The previous example will load the latest bundled code from the deno branch. Alternatively, you may load a specific version by loading the file from one of the [tagged bundles](https://github.com/stdlib-js/blas-base-ggemv/tags). For example,

```javascript
import ggemv from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-ggemv@v0.1.1-deno/mod.js';
```

You can also import the following named exports from the package:

```javascript
import { ndarray } from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-ggemv@deno/mod.js';
```

#### ggemv( order, trans, M, N, α, A, LDA, x, sx, β, y, sy )

Performs one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`, where `α` and `β` are scalars, `x` and `y` are vectors, and `A` is an `M` by `N` matrix.

```javascript
var A = [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ];
var x = [ 1.0, 1.0, 1.0 ];
var y = [ 1.0, 1.0 ];

ggemv( 'row-major', 'no-transpose', 2, 3, 1.0, A, 3, x, 1, 1.0, y, 1 );
// y => [ 7.0, 16.0 ]
```

The function has the following parameters:

-   **order**: storage layout.
-   **trans**: specifies whether `A` should be transposed, conjugate-transposed, or not transposed.
-   **M**: number of rows in the matrix `A`.
-   **N**: number of columns in the matrix `A`.
-   **α**: scalar constant.
-   **A**: input matrix stored in linear memory.
-   **LDA**: stride of the first dimension of `A` (a.k.a., leading dimension of the matrix `A`).
-   **x**: first input vector.
-   **sx**: stride length for `x`.
-   **β**: scalar constant.
-   **y**: second input vector.
-   **sy**: stride length for `y`.

The stride parameters determine how operations are performed. For example, to iterate over every other element in `x` and `y`,

```javascript
var A = [ 1.0, 2.0, 3.0, 4.0 ];
var x = [ 1.0, 0.0, 1.0, 0.0 ];
var y = [ 1.0, 0.0, 1.0, 0.0 ];

ggemv( 'row-major', 'no-transpose', 2, 2, 1.0, A, 2, x, 2, 1.0, y, 2 );
// y => [ 4.0, 0.0, 8.0, 0.0 ]
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

// Initial arrays...
var x0 = new Float64Array( [ 0.0, 1.0, 1.0 ] );
var y0 = new Float64Array( [ 0.0, 1.0, 1.0 ] );
var A = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );

// Create offset views...
var x1 = new Float64Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element
var y1 = new Float64Array( y0.buffer, y0.BYTES_PER_ELEMENT*1 ); // start at 2nd element

ggemv( 'row-major', 'no-transpose', 2, 2, 1.0, A, 2, x1, -1, 1.0, y1, -1 );
// y0 => <Float64Array>[ 0.0, 8.0, 4.0 ]
```

#### ggemv.ndarray( trans, M, N, α, A, sa1, sa2, oa, x, sx, ox, β, y, sy, oy )

Performs one of the matrix-vector operations `y = α*A*x + β*y` or `y = α*A^T*x + β*y`, using alternative indexing semantics and where `α` and `β` are scalars, `x` and `y` are vectors, and `A` is an `M` by `N` matrix.

```javascript
var A = [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ];
var x = [ 1.0, 1.0, 1.0 ];
var y = [ 1.0, 1.0 ];

ggemv.ndarray( 'no-transpose', 2, 3, 1.0, A, 3, 1, 0, x, 1, 0, 1.0, y, 1, 0 );
// y => [ 7.0, 16.0 ]
```

The function has the following additional parameters:

-   **sa1**: stride of the first dimension of `A`.
-   **sa2**: stride of the second dimension of `A`.
-   **oa**: starting index for `A`.
-   **ox**: starting index for `x`.
-   **oy**: starting index for `y`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying buffer, the offset parameters support indexing semantics based on starting indices. For example,

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

var A = [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ];
var x = [ 0.0, 1.0, 2.0, 3.0 ];
var y = [ 7.0, 8.0, 9.0, 10.0 ];

ggemv.ndarray( 'no-transpose', 2, 3, 1.0, A, 3, 1, 0, x, 1, 1, 1.0, y, -2, 2 );
// y => [ 39.0, 8.0, 23.0, 10.0 ]
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   `ggemv()` corresponds to the [BLAS][blas] level 2 function [`dgemv`][dgemv] with the exception that this implementation works with any array type, not just Float64Arrays. Depending on the environment, the typed versions ([`dgemv`][@stdlib/blas/base/dgemv], [`sgemv`][@stdlib/blas/base/sgemv], etc.) are likely to be significantly more performant.
-   Both functions support array-like objects having getter and setter accessors for array element access (e.g., [`@stdlib/array-base/accessor`][@stdlib/array/base/accessor]).

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import discreteUniform from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-array-discrete-uniform@deno/mod.js';
import ggemv from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-ggemv@deno/mod.js';

var opts = {
    'dtype': 'generic'
};

var M = 3;
var N = 3;

var A = discreteUniform( M*N, 0, 255, opts );
var x = discreteUniform( N, 0, 255, opts );
var y = discreteUniform( M, 0, 255, opts );

ggemv( 'row-major', 'no-transpose', M, N, 1.0, A, N, x, 1, 1.0, y, 1 );
console.log( y );

ggemv.ndarray( 'no-transpose', M, N, 1.0, A, N, 1, 0, x, 1, 0, 1.0, y, 1, 0 );
console.log( y );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/blas-base-ggemv.svg
[npm-url]: https://npmjs.org/package/@stdlib/blas-base-ggemv

[test-image]: https://github.com/stdlib-js/blas-base-ggemv/actions/workflows/test.yml/badge.svg?branch=v0.1.1
[test-url]: https://github.com/stdlib-js/blas-base-ggemv/actions/workflows/test.yml?query=branch:v0.1.1

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/blas-base-ggemv/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/blas-base-ggemv?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/blas-base-ggemv.svg
[dependencies-url]: https://david-dm.org/stdlib-js/blas-base-ggemv/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/blas-base-ggemv/tree/deno
[deno-readme]: https://github.com/stdlib-js/blas-base-ggemv/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/blas-base-ggemv/tree/umd
[umd-readme]: https://github.com/stdlib-js/blas-base-ggemv/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/blas-base-ggemv/tree/esm
[esm-readme]: https://github.com/stdlib-js/blas-base-ggemv/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/blas-base-ggemv/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/blas-base-ggemv/main/LICENSE

[blas]: http://www.netlib.org/blas

[dgemv]: https://www.netlib.org/lapack/explore-html/d7/dda/group__gemv_ga4ac1b675072d18f902db8a310784d802.html

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/blas/base/dgemv]: https://github.com/stdlib-js/blas-base-dgemv/tree/deno

[@stdlib/blas/base/sgemv]: https://github.com/stdlib-js/blas-base-sgemv/tree/deno

[@stdlib/array/base/accessor]: https://github.com/stdlib-js/array-base-accessor/tree/deno

<!-- <related-links> -->

<!-- </related-links> -->

</section>

<!-- /.links -->

ocamlconf
=========

[Autoconf](https://en.wikipedia.org/wiki/Autoconf) for OCaml codebases.
`ocamlconf` takes a directory of OCaml files and produces a `Makefile`
to compile them together. You can think of it as a compiler frontend for
OCaml that targets the `make` backend. Under the hood, it uses
[ocamldep](https://caml.inria.fr/pub/docs/manual-ocaml/depend.html) to
build the dependency graph.

### Dependencies

+ `python >= 2.7`
+ `ocamldep`
+ `ocamlfind`
+ `ocamlopt`

### Installation

`ocamlconf` is distributed as a single python script. Put it anywhere
and add it to your `$PATH`.

### Usage

```
ari@ari-MacBookAir:~/Documents/ocamlconf$ ./ocamlconf --binary main --package "cohttp,cohttp-lwt-unix,lwt" /home/ari/Documents/vector/ml/src
[ocamlconf] Found 2 OCaml sources in /home/ari/Documents/vector/ml/src.
[ocamlconf] Built dependency graph: {'/home/ari/Documents/vector/ml/src/main': ['/home/ari/Documents/vector/ml/src/router'], '/home/ari/Documents/vector/ml/src/router': []}.
[ocamlconf] Writing Makefile.
[ocamlconf] Done!
ari@ari-MacBookAir:~/Documents/ocamlconf$ cat Makefile
PACKAGE="cohttp,cohttp-lwt-unix,lwt"

define link
	ocamlfind ocamlopt -package $PACKAGE -o $@ -linkpkg $^
endef

define compile
	ocamlfind ocamlopt -package $PACKAGE -I /home/ari/Documents/vector/ml/src -c $<
endef

main: /home/ari/Documents/vector/ml/src/router.cmx /home/ari/Documents/vector/ml/src/main.cmx
	$(link)

/home/ari/Documents/vector/ml/src/main.cmx: /home/ari/Documents/vector/ml/src/main.ml /home/ari/Documents/vector/ml/src/router.cmx
	$(compile)

/home/ari/Documents/vector/ml/src/router.cmx: /home/ari/Documents/vector/ml/src/router.ml
	$(compile)
```

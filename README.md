## A python parser for OBO ontology files

[![Build Status](https://travis-ci.org/dhimmel/obonet.svg?branch=master)](https://travis-ci.org/dhimmel/obonet)

This repository contains a python package for handling OBO serialized ontologies.
The function `obonet.read_obo()` takes an `.obo` file and returns a [`networkx.MultiDiGraph`](http://networkx.readthedocs.io/en/stable/reference/classes.multidigraph.html) representation of the ontology.

The parser aims to be compatible with OBO versions [1.2](https://owlcollab.github.io/oboformat/doc/GO.format.obo-1_2.html) and [1.4](https://owlcollab.github.io/oboformat/doc/GO.format.obo-1_4.html).

## Usage

This package is designed and tested on python ≥ 3.4.
OBO files can be read from a path, URL, or open file handle.
Compression is inferred from the path's extension.
See example usage below:

```python
import networkx
import obonet

# Read the taxrank ontology
url = 'https://github.com/dhimmel/obo/raw/master/tests/data/taxrank.obo'
graph = obonet.read_obo(url)

# Or read the xz-compressed taxrank ontology
url = 'https://github.com/dhimmel/obo/raw/master/tests/data/taxrank.obo.xz'
graph = obonet.read_obo(url)

# Number of nodes
len(graph)

# Number of edges
graph.number_of_edges()

# Check if the ontology is a DAG
networkx.is_directed_acyclic_graph(graph)

# Mapping from term ID to name
id_to_name = {id_: data['name'] for id_, data in graph.nodes(data=True)}
id_to_name['TAXRANK:0000006']  # TAXRANK:0000006 is species

# Find all superterms of species. Note that networkx.descendants gets
# superterms, while networkx.ancestors returns subterms.
networkx.descendants(graph, 'TAXRANK:0000006')
```

## Installation

The latest version of the package can be installed using `pip` with:

```sh
pip install git+https://github.com/dhimmel/obonet.git#egg=obonet
```

## Contributing

We welcome feature suggestions and community contributions.
Currently, only reading OBO files is supported.
Please open an issue if you're interested in writing OBO files in Python.

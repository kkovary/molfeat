<div align="center">
    <img src="docs/images/logo-title.svg" width="100%">
</div>

<p align="center">
    <b>molfeat - all your molecular featurizers in one place </b>. <br />
</p>
<p align="center">
  <a href="https://datamol-io.github.io/molfeat/stable/index.html" target="_blank">
      Docs
  </a> | 
  <a href="https://www.molfeat.datamol.io/" target="_blank">
      Homepage
  </a>
</p>

---

[![license](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/datamol-io/datamol/blob/main/LICENSE)
[![GitHub Repo stars](https://img.shields.io/github/stars/datamol-io/molfeat)](https://github.com/datamol-io/molfeat/stargazers)
[![GitHub Repo stars](https://img.shields.io/github/forks/datamol-io/molfeat)](https://github.com/datamol-io/molfeat/network/members)
[![test](https://github.com/datamol-io/molfeat/actions/workflows/test.yml/badge.svg)](https://github.com/datamol-io/molfeat/actions/workflows/test.yml)
[![code-check](https://github.com/datamol-io/molfeat/actions/workflows/code-check.yml/badge.svg)](https://github.com/datamol-io/molfeat/actions/workflows/code-check.yml)
[![doc](https://github.com/datamol-io/molfeat/actions/workflows/doc.yml/badge.svg)](https://github.com/datamol-io/molfeat/actions/workflows/doc.yml)
[![release](https://github.com/datamol-io/molfeat/actions/workflows/release.yml/badge.svg)](https://github.com/datamol-io/molfeat/actions/workflows/release.yml)

Molfeat is a python library to simplify molecular featurization. It supports a wide variety of molecular featurizers out-of-the-box and can be easily extended to add your own.

- :snake: Simple pythonic API.
- :rocket: Fast and efficient featurization.
- :arrows_counterclockwise: Unifies pre-trained embeddings and hand-crafted featurizers in a single package.
- :heavy_plus_sign: Easily extend Molfeat with your own featurizers through plugins.
- :chart_with_upwards_trend: Benefit from increased performance through a trouble-free caching system.

Visit our website at https://molfeat.datamol.io.

## Installation

### Installing Molfeat

Use mamba:

```bash
mamba install -c conda-forge molfeat
```

_**Tips:** You can replace `mamba` by `conda`._

_**Note:** We highly recommend using a [Conda Python distribution](https://github.com/conda-forge/miniforge) to install Molfeat. The package is also pip installable if you need it: `pip install molfeat`._ 

### Optional dependencies
Not all featurizers of the Molfeat core package are supported by default. Some featurizers require additional dependencies. If you try to use a featurizer that requires additional dependencies, Molfeat will raise an error and will tell you which dependencies are missing and how to install these. 

- To install `dgl`: run `mamba install -c dglteam dgl`
- To install `dgllife`:  run `mamba install -c conda-forge dgllife`
- To install `fcd_torch`: run `mamba install -c conda-forge fcd_torch`
- To install `pyg`: run `mamba install -c conda-forge pytorch_geometric`
- To install `graphormer-pretrained`: run `mamba install -c conda-forge graphormer-pretrained`
- To install `map4`: see https://github.com/reymond-group/map4
- To install `bio-embeddings`: run `mamba install -c conda-forge 'bio-embeddings >=0.2.2'`


If you install Molfeat using pip, there are optional dependencies that can be installed with the main package. For example, `pip install "molfeat[all]"` allows installing all the compatible optional dependencies for small molecule featurization. There are other options such as `molfeat[dgl]`, `molfeat[graphormer]`, `molfeat[transformer]`, `molfeat[viz]`, and `molfeat[fcd]`. See the [optional-dependencies](./pyproject.toml) for more information.


### Installing Plugins

The functionality of Molfeat can be extended through plugins. The usage of a plugin system ensures that the core package remains easy to install and as light as possible, while making it easy to extend its functionality with plug-and-play components. Additionally, it ensures that plugins can be developed independently from the core package, removing the bottleneck of a central party that reviews and approves new plugins. Consult the Molfeat documentation for more details on how to [create](developers/create-plugin.md) your own plugins.

This, however, does imply that the installation of a plugin is plugin-dependent: Please consult its documentation to learn more.


## API tour

```python
import datamol as dm
from molfeat.calc import FPCalculator
from molfeat.trans import MoleculeTransformer
from molfeat.store.modelstore import ModelStore

# Load some dummy data
data = dm.data.freesolv().sample(500).smiles.values

# Featurize a single molecule
calc = FPCalculator("ecfp")
calc(data[0])

# Define a parallelized featurization pipeline
trans = MoleculeTransformer(calc, n_jobs=-1)
trans(data)

# Easily save and load featurizers
trans.to_state_yaml_file("state_dict.yml")
trans = MoleculeTransformer.from_state_yaml_file("state_dict.yml")
trans(data)

# List all available featurizers
store = ModelStore()
store.available_models

# Find a featurizer and learn how to use it
model_card = store.search(name="ChemBERTa-77M-MLM")[0]
model_card.usage()

# Load a featurizer through the store
trans, model_info = store.load(model_card)
```

## How to cite

Please cite Molfeat if you use it in your research: [![DOI](https://zenodo.org/badge/613548667.svg)](https://zenodo.org/badge/latestdoi/613548667).

## Contribute

See [developers](docs/developers/) for a comprehensive guide on how to contribute to `Molfeat`

## Changelogs

See the latest changelogs at [CHANGELOG.rst](./CHANGELOG.rst).

## Maintainers

* @cwognum
* @maclandrol
* @hadim


## License

Under the Apache-2.0 license. See [LICENSE](LICENSE).



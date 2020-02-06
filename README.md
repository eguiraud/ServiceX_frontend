# ServiceX_frontend
 Client access library for ServiceX

[![GitHub Actions Status](https://github.com/gordonwatts/ServiceX_frontend/workflows/CI/CD/badge.svg)](https://github.com/gordonwatts/ServiceX_frontend/actions)
[![Code Coverage](https://codecov.io/gh/gordonwatts/ServiceX_frontend/graph/badge.svg)](https://codecov.io/gh/gordonwatts/ServiceX_frontend)

[![PyPI version](https://badge.fury.io/py/ServiceX_frontend.svg)](https://badge.fury.io/py/ServiceX_frontend)
[![Supported Python versions](https://img.shields.io/pypi/pyversions/ServiceX_frontend.svg)](https://pypi.org/project/ServiceX_frontend/)

# Introduction

Given you have a selection string, this library will manage submitting it to a ServiceX instance and retreiving the data locally for you.
The selection string is often generated by another front-end library, for example:

- lib1
- lib2

# Prerequisites

Before you install this library you'll need:

- An environment based on python 3.7 or later
- A ServiceX end-point. For example, `http://localhost:5000/servicex`.

# Usage

The following lines will return a `pandas.DataFrame` containing all the jet pT's from an ATLAS xAOD file containing Z->ee Monte Carlo:

```
    query = "(call ResultTTree (call Select (call SelectMany (call EventDataset (list 'localds:bogus')) (lambda (list e) (call (attr e 'Jets') 'AntiKt4EMTopoJets'))) (lambda (list j) (/ (call (attr j 'pt')) 1000.0))) (list 'JetPt') 'analysis' 'junk.root')"
    dataset = "mc15_13TeV:mc15_13TeV.361106.PowhegPythia8EvtGen_AZNLOCTEQ6L1_Zee.merge.DAOD_STDM3.e3601_s2576_s2132_r6630_r6264_p2363_tid05630052_00"
    r = ServiceX_fe.get_data(query , dataset, servicex_endpoint=endpoint)
    print(r)
```
And the output in a terminal window from running the above script (takes about 1-2 minutes to complete):
```
python scripts\run_test.py http://localhost:5000/servicex
            JetPt
entry
0       38.065707
1       31.967096
2        7.881337
3        6.669581
4        5.624053
...           ...
710183  42.926141
710184  30.815709
710185   6.348002
710186   5.472711
710187   5.212714

[11355980 rows x 1 columns]
```

If your query is badly formed or there is an other problem with the backend, an exception will be thrown.

If you'd like to be able to submit multiple queries and have them run on the ServiceX back end in parallel, it may be best to use the `asyncio` interface, which has the identical signature, but is called `get_data_async`.

# Features

Implemented:

- Accepts a `qastle` formatted query
- Exceptions are used to report back errors of all sorts from the service to the user's code.
- Data is return as a `pandas.DataFrame`
- Complete returned data must fit in the process' memory
- Run in an async or a non-async environment and non-async methods will accomodate automatically (including `jupyter` notebooks).
- Support up to 100 simultanious queries from a laptop-like front end without overwhelming the local machine (hopefully ServiceX will be overwhelmed!)
- Start downloading files as soon as they are ready (before ServiceX is done with the complete transform).

Comming:

- Data is returned as an `awkward` array, or a list of ROOT files located in a specified directory
- Make it easy to submit the same query for 100 different datasets

# Testing

This code has been tested in several environments:

- Windows, Linux, MacOS
- Python 3.7
- Jupyter Notebooks (not automated), regular python command-line invoked source files

# Development

For any changes please feel free to submit pull requests!

To do development please setup your environment with the following steps:

1. A python 3.7 development environment
1. Pull down this package, XXX
1. `python -m pip install -e .[test]`
1. Run the tests to make sure everything is good: `pytest`.

Then add tests as you develop. When you are done, submit a pull request with any required changes to the documentation and the online tests will run.

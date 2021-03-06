# solar-data-tools

[![PyPI release](https://img.shields.io/pypi/v/solar-data-tools.svg)](https://pypi.org/project/solar-data-tools/)
[![Anaconda Cloud release](https://anaconda.org/slacgismo/solar-data-tools/badges/version.svg)](https://anaconda.org/slacgismo/solar-data-tools)

Tools for performing common tasks on solar PV data signals. These tasks include finding clear days in
a data set, common data transforms, and fixing time stamp issues. These tools are designed to be
automatic and require little if any input from the user. Libraries are included to help with data IO
and plotting as well.

There is close integration between this repository and the [Statistical Clear Sky](https://github.com/slacgismo/StatisticalClearSky) repository, which provides a "clear sky model" of system output, given only measured power as an input. 

See [notebooks](/notebooks) folder for examples.

## Setup

### Recommended: Set up `conda` environment with provided `.yml` file

_Updated March 2021_

We recommend setting up a fresh Python virtual environment in which to use `solar-data-tools`. We recommend using the [Conda](https://docs.conda.io/projects/conda/en/latest/index.html) package management system, and creating an environment with the environment configuration file named `pvi-user.yml`, provided in the top level of this repository. This will install the `statistical-clear-sky` package as well.

Additional documentation on setting up the Conda environment is available [here](https://github.com/slacgismo/pvinsight-onboarding/blob/main/README.md).

Please see the Conda documentation page, "[Creating an environment from an environment.yml file](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file)" for more information.

### Installing this project as PIP package

```sh
$ pip install solar-data-tools
```

As of March 6, 2019, it fails because scs package installed as a dependency of cxvpy expects numpy to be already installed.
[scs issue 85](https://github.com/cvxgrp/scs/issues/85) says, it is fixed.
However, it doesn't seem to be reflected in its pip package.
Also, cvxpy doesn't work with numpy version less than 1.16.
As a work around, install numpy separatly first and then install this package.
i.e.
```sh
$ pip install 'numpy>=1.16'
$ pip install statistical-clear-sky
```

#### Solvers


Currently, this sofware package requires the use of a commercial software package called MOSEK. The included YAML file will install MOSEK for you, but you will still need to obtain a license. More information is available here:

* [mosek](https://www.mosek.com/resources/getting-started/)stall -f https://download.mosek.com/stable/wheel/index.html Mosek
    ```

### Installing this project as Anaconda package

```sh
$ conda install -c slacgismo solar-data-tools
```

If you are using Anaconda, the problem described in the section for PIP package above doesn't occur since numpy is already installed. And during solar-data-tools installation, numpy is upgraded above 1.16.

#### Solvers

By default, ECOS solver is used, which is supported by cvxpy because it is Open Source.

However, it is found that Mosek solver is more stable. Thus, we encourage you to install it separately as below and obtain the license on your own.

* [Free 30-day trial](https://www.mosek.com/products/trial/)
* [Personal academic license](https://www.mosek.com/products/academic-licenses/)

### Using this project by cloning this GIT repository

From a fresh `python` environment, run the following from the base project folder:

```bash
$ pip install -r requirements.txt
```


## Usage

Users will primarily interact with this software through the `DataHandler` class.

```python
from solardatatools import DataHandler
from solardatatools.dataio import get_pvdaq_data

pv_system_data = get_pvdaq_data(sysid=35, api_key='DEMO_KEY', year=[2011, 2012, 2013])

dh = DataHandler(pv_system_data)
dh.run_pipeline(power_col='dc_power')
```
If everything is working correctly, you should see something like the following

```
total time: 16.67 seconds
--------------------------------
Breakdown
--------------------------------
Preprocessing              6.52s
Cleaning                   8.62s
Filtering/Summarizing      1.53s
    Data quality           0.23s
    Clear day detect       0.19s
    Clipping detect        0.21s
    Capacity change detect 0.91s
```


## Versioning

We use [Semantic Versioning](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/bmeyers/solar-data-tools/tags).

## Authors

* **Bennet Meyers** - *Initial work and Main research work* - [Bennet Meyers GitHub](https://github.com/bmeyers)

See also the list of [contributors](https://github.com/bmeyers/solar-data-tools/contributors) who participated in this project.

## License

This project is licensed under the BSD 2-Clause License - see the [LICENSE](LICENSE) file for details

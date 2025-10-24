<!--
SPDX-FileCopyrightText: 2024-2025 Bryn Pickering <bp325@cam.ac.uk>

SPDX-License-Identifier: CC-BY-4.0
-->

# energy-system-masterclass

Energy Technologies MPhil ETA1 masterclass in Energy Systems Modelling.

This repository contains a subset of the [Euro-Calliope pre-built model](https://euro-calliope.readthedocs.io), containing two countries in the European energy system.
It is built for use in [_Calliope_ v0.7](https://calliope.readthedocs.io/en/latest/).
The model is available on the national spatial resolution and daily temporal resolution, with a subset of technologies available compared to the base Euro-Calliope.

Euro-Calliope models like this can be built manually, which adds more configuration options.
To learn how to build Euro-Calliope manually, head over to [Euro-Calliope's documentation](https://euro-calliope.readthedocs.io).

## At a glance

The base Euro-Calliope is a model of the European energy system, with each node representing an administrative unit.
It is built on three spatial resolutions: on the continental level as a single node, on the national level with 34 nodes, and on the regional level with 497 nodes.
At each spatial _node_, renewable generation capacities (wind, solar, bioenergy) and balancing capacities (battery, hydrogen) can be invested in to meet electricity demand, and heat pumps or conventional natural gas can be invested in to meet building heat demand.
In addition, a fixed amount of hydro electricity and pumped hydro storage is available, based on what already exists today.
Once invested in, technologies can satisfy energy demands at each node, where demand is based on historic data.
Nodes are connected through electricity transmission lines, which may have unrestricted capacity or be limited based on real data.
Using [Calliope](https://www.callio.pe), the model is solved as a linear optimisation problem with total monetary cost of all capacities and their operation as the minimisation objective.

## Prepare

First, you will need to install software on your device:

1. [VSCode](https://code.visualstudio.com/download).
This gives you access to an Interactive Development Environment (IDE) in which to edit code and to interact with your device's terminal.
On Windows, I recommend you follow the _command prompt_ instructions.
2. [pixi](https://pixi.sh/latest/).
This gives you access to `pixi` in your device's terminal, with which you can create isolated Python environments to work in.
3. [The Gurobi solver license](https://www.gurobi.com/features/academic-named-user-license/).
This gives you access to a high-performance optimisation solver.

> [!TIP]
> You can ignore steps 2 and 3 of the instructions (downloading the installer) and go straight to requesting a "named user" license after registering.
> See our [Gurobi license section](#set-the-gurobi-license) for details on installing the license.

> [!NOTE]
> You can only request and set up the license while connected to the university network (i.e., connected to eduroam and not behind a personal VPN).

### Set up VSCode

With this software installed, you can then set up your project:

1. Open VSCode.
1. Open a new terminal window (`Terminal` top-bar tab -> `New Terminal`, it will open at the bottom of the screen).

> [!NOTE]
> You will be using `pixi run` before every command you make in the terminal.
> This ensures you run your command in an isolated environment, giving you access to commands like `git`, `calliope` and `calligraph`.

### Clone the GitHub repository

With VSCode set up, you can "clone" (i.e. copy) this repository to your device:

1. In the VSCode terminal, call `pixi run git clone https://github.com/brynpickering/energy-system-masterclass.git <output-directory>/energy-system-masterclass`.
`<output-directory>` should be a directory on your device where you want to store cloned GitHub repositories (often something like `%USERPROFILE%\Repositories` on Windows or `~/Repositories` on Linux/MacOS).
1. Then you can open that cloned repository (i.e. downloaded folder) in VSCode (`File` top-bar tab -> `Open Folder` -> navigate to `<output-directory>/energy-system-masterclass`).

### Set the Gurobi license

You can then call the `grbgetkey` command in the terminal to install the Gurobi license that you have requested, e.g.:

```bash
pixi run grbgetkey ae36ac20-16e6-acd2-f242-4da6e765fa0a
```

The license key can be found on the Gurobi portal, under the `Licenses` tab and then the `show installation instructions` button.

> [!NOTE]
> If this just isn't working, you can use the fall-back solver `cbc`.
> Just use `--scenario=use_cbc` in the `calliope` commands below (or `--scenario=add_nuclear,use_cbc` when adding multiple scenarios).

## Run

We recommend you run the models from the terminal.
They will be available on your device after following the [preparation steps](#prepare), and visible in the VSCode `Explorer` when you have the `<output-directory>/energy-system-masterclass` folder open in VSCode.
To run a model and save the outputs in the NetCDF format (a storage convention for multi-dimensional data) _and_ as a set of CSVs:

```sh
pixi run calliope run "model-gbr-irl/model.yaml" --save_netcdf "model-gbr-irl.nc" --save_csv "model-gbr-irl-outputs"
```

### Run a scenario

There is one pre-defined "scenario" in the GBR+IRL model, to add in nuclear as a technology.
To introduce this, you can add it to your command line arguments

```sh
pixi run calliope run "model-gbr-irl/model.yaml" --save_netcdf "model-gbr-irl-with-nuclear.nc" --save_csv "model-gbr-irl-nuclear-outputs" --scenario add_nuclear
```

## Visualise

Once you have results, you can use [calligraph](https://github.com/calliope-project/calligraph), a tool for visualising Calliope model results that we are currently developing (i.e., expect some rough edges).
Once you have run a model, you can call calligraph with the name of the NetCDF file:

```sh
pixi run calligraph "model-gbr-irl.nc"
```

This will open a tab in your browser with a data dashboard.

## Go further

This pre-built model is a very simple example.
You can extend / amend it directly by modifying the YAML files.
You can also find more complete pre-built models [on Zenodo](https://zenodo.org/record/3949553).
You may find that even that needs customisation to fit your purpose.
Once you've managed to run them, it's a good point in time to learn about [model customisation options in Euro-Calliope](https://euro-calliope.readthedocs.io/en/latest/model/customisation/).

## More information

For more information on Euro-Calliope and how to use and modify the models, see [Euro-Calliope's documentation](https://euro-calliope.readthedocs.io).

## License and attribution

Euro-Calliope is developed and maintained within the [Calliope project](https://www.callio.pe).

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

Contains modified Copernicus Atmosphere Monitoring Service information 2020.
Neither the European Commission nor ECMWF is responsible for any use that may be made of the Copernicus information or data it contains.

Contains modified data from [Renewables.ninja](https://www.renewables.ninja/).

Contains modified data from [Open Power System Data](https://open-power-system-data.org).

For more details on sources, see the [Euro-Calliope GitHub repository](https://github.com/calliope-project/euro-calliope/tree/e9cb908a5b4c6274148a16b59e5dd0b412aaf560).

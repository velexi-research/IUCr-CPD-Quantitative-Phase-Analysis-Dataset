IUCr CPD Quantitative Phase Analysis Dataset
===============================================================================

This repository contains the dataset assembled by the International Union of
Crystallography (IUCr) in the late 1990s to research the use of powder X-ray
diffraction (PXRD) data for quantitative phase analysis.

-------------------------------------------------------------------------------

Contents
--------

1. [Overview][#1]

    1.1. [Dataset Contents][#1.1]

    1.2. [Data Format][#1.2]

    1.3. [License][#1.3]

    1.4. [Known Issues][#1.4]

2. [Using the Dataset][#2]

    2.1. [Importing the Dataset][#2.1]

    2.2. [Updating the Dataset][#2.2]

3. [Maintaining the Dataset][#3]

    3.1. [Managing Data][#3.1]

    3.2. [Releasing an Official Dataset Version][#3.2]

    3.3. [Dataset Conventions][#3.3]

4. [References][#4]

-------------------------------------------------------------------------------

## 1. Overview

This repository contains the dataset assembled by the International Union of
Crystallography (IUCr) by the Commission on Powder Diffraction (CPD) for the
Round Robin on Quantitative Phase Analysis. The original dataset and details
about the CPD project are available directly from the project website (which
is no longer actively maintained):

  https://www.iucr.org/resources/commissions/powder-diffraction/projects/qarr

### 1.1. Dataset Contents

```
├── README.md            <- this file
├── RELEASE-NOTES.md     <- dataset release notes
├── DATASET-LICENSE      <- license for data components of the dataset
├── DATASET-NOTICE       <- copyright notices for third-party data included in
│                           the dataset
├── SOFTWARE-LICENSE     <- license for software components of the dataset
├── SOFTWARE-NOTICE      <- copyright notice for the software components of the
│                           dataset
├── Makefile             <- Makefile containing useful shortcuts (`make` rules).
│                           Use `make help` to show the list of available rules.
├── pyproject.toml       <- Python project metadata file
├── poetry.lock          <- Poetry lockfile
├── bin/                 <- scripts and programs for managing the dataset
├── data/                <- directory containing data for dataset
│   ├── mixtures/        <- PXRD data for mixtures
│   ├── pure-compounds/  <- PXRD data for pure compounds
│   └── VERSION          <- version of the latest official release of the dataset
├── docs/                <- dataset documentation
└── extras/              <- additional files and references that may be useful
                            for dataset maintenance
```

### 1.2. Data Format

* Data files with the `prn` extension contain diffractogram data stored in
  column format (without headers): 2-theta, counts.

* The `pure-compounds/structures.txt` file contains structure information for
  pure compounds (except for the pharmaceutical compounds `pharm1gr` and
  `pharm2gr`).

* The `mixtures/compositions.txt` file contains weight percentages for the
  samples associated with the diffractograms in the `mixtures` directory.

### 1.3. License

The data contained in this dataset is covered under the Creative Commons
Attribution 4.0 International Public License (included in the
[`DATASET-LICENSE`](DATASET-LICENSE) file). Licenses for third-party data
included with this dataset are contained in the [`DATASET-NOTICE`](DATASET-NOTICE)
file.

The software contained in this repository is covered under the Apache License
2.0 (included in the [`SOFTWARE-LICENSE`](SOFTWARE-LICENSE) file). The copyright
for the software is contained in the [`SOFTWARE-NOTICE`](SOFTWARE-NOTICE) file.

### 1.4. Known Issues

* Structure information is unavailable for the pharmaceutical compounds
  (i.e., PXRD data files `pharm1gr.prn` and `pharm2gr.prn`).

-------------------------------------------------------------------------------

## 2. Using the Dataset

The instructions provided below require [DVC][dvc-docs] and [FastDS][fastds] to
be installed.

### 2.1. Importing the Dataset

To use the dataset a "read-only" manner (i.e., without maintenance code),
import the dataset after initializing DVC in the working directory.

1. Initialize DVC.

   ```
   $ cd /PATH/TO/PROJECT
   $ fds init
   $ fds commit "Initialize DVC"
   ```

   In the example commands above, `/PATH/TO/PROJECT` should be replaced
   by the path to the directory that where the dataset will be used.

2. ___Recommended___. Enable auto staging for DVC-managed data.

   ```
   $ dvc config core.autostage true
   $ fds commit "Enable DVC auto staging"
   ```

3. Import the dataset.

    ```
    $ dvc import https://github.com/velexi-research/IUCr-CPD-Quantitative-Phase-Analysis-Dataset.git data -o LOCAL_PATH
    $ fds commit "Import 'IUCr CPD Quantitative Phase Analysis Dataset'"
    ```

    In the example command above, the following substitutions should be made:

     * `LOCAL_PATH` should be replaced by the local path relative to
       `/PATH/TO/PROJECT` where the dataset should be placed. __Note__: the
       parent directory of `LOCAL_PATH` should be created before running
       `dvc import`.

     * `DATASET_NAME` should be replaced by the name of the imported dataset.

    For example, if the dataset repository is located at

    `https://github.com/account/cool-dataset`

    and we would like to to place the dataset into a directory named
    `data/cool-dataset`, we would use the following command:

    ```
    $ mkdir data
    $ dvc import https://github.com/account/cool-dataset data -o data/cool-dataset
    $ fds commit "Import cool-dataset"
    ```

### 2.2. Updating the Dataset

If a previously imported dataset has been updated, the local copy of the
dataset can be updated (to the latest version on the default branch of the
dataset Git repository) by using the `dvc update` command.

```
$ dvc update DATASET.dvc
$ fds commit "Update 'IUCr CPD Quantitative Phase Analysis Dataset'"
```

or

```
$ dvc update DATASET
$ fds commit "Update 'IUCr CPD Quantitative Phase Analysis Dataset'"
```

In the example commands above, the following substitutions should be made:

* `DATASET.dvc` should be replaced by the `.dvc` file that was generated when
  the dataset was imported (or, equivalently, `DATASET` should be replaced by
  name of the directory that the dataset was imported into).

To specify the particular revision of the dataset to retreive, use the
`--rev REVISION` option, where `REVISION` is a Git tag, branch, or commit SHA/hash.

```
$ dvc update DATASET.dvc --rev REVISION
$ fds commit "Update 'IUCr CPD Quantitative Phase Analysis Dataset'"
```

-------------------------------------------------------------------------------

## 3. Maintaining the Dataset

### 3.1. Managing Data

#### 3.1.1. Adding Data

1. Add the data files to the `data` directory.

2. Add the contents of `data` to the data tracked by DVC.

   ```
   $ fds add data
   ```

3. Commit the dataset changes to the local Git repository.

   ```
   $ fds commit "Add initial version of data"
   ```

4. Push the dataset changes to the remote Git repository and DVC remote
   storage.

   ```
   $ fds push
   ```

#### 3.1.2. Updating Data

1. Update the data files in the `data` directory.

2. Update the data tracked by DVC with the new content of the `data` directory.

   ```
   $ fds add data
   ```

3. Commit the dataset changes to the local Git repository.

   ```
   $ fds commit "Update dataset"
   ```

4. Push the dataset changes to the remote Git repository and DVC remote
   storage.

   ```
   $ fds push
   ```

#### 3.1.3. Removing Data

1. Remove the data files from the `data` directory.

2. Update the data tracked by DVC with the new content of the `data` directory.

   ```
   $ fds add data
   ```

3. Commit the dataset changes to the local Git repository.

   ```
   $ fds commit "Update dataset"
   ```

4. Push the dataset changes to the remote Git repository and DVC remote
   storage.

   ```
   $ fds push
   ```

### 3.2. Releasing an Official Dataset Version

1. Make sure that the dataset has been updated ([Section 3.1.2][#3.1.2])

2. Update the `README.md` file.

3. Increment the version number in `pyproject.toml`.

4. Update `data/VERSION`.

   ```
   $ cd data
   $ poetry version -s > VERSION
   ```

5. ___Recommended___. Update the release notes for the dataset to include any
   major changes from the previous released version of the dataset.

7. Create a tag for the release in git.

    ```
    $ git tag `poetry version -s`
    $ git push --tags
    ```

6. _Optional_. If the Git repository for the dataset is hosted on GitHub (or
   analogous service), create a release associated with the git tag created
   in Step #4.

### 3.3. Dataset Conventions

#### Data

* `data` directory. All data files that should be imported when using the
  `dvc import URL data -o LOCAL_PATH` command should be placed in the `data`
  directory.

  * Depending on the nature of the dataset, it may be useful to organize the
  data files into sub-directories (e.g., by type of data).

* `data/VERSION` file. The `data/VERSION` file contains the version number of
  the latest official release of the dataset. It is generated automatically
  and _should not be manually edited_.

#### Documentation

* `README.md` file. The `README.md` file should contain

  * a high-level description of the dataset and

  * instructions for software tools used to create and maintain the dataset.

* `docs` directory. The `docs` directory should be used for detailed
  documentation for the dataset (i.e., data and supporting software tools).

#### Supporting Software Tools

* `bin` directory. The `bin` directory should be used for supporting software
  tools (e.g., data capture and processing scripts) developed to help maintain
  the dataset.

* `pyproject.toml` file. Python dependencies for supporting tools should be
  maintained in the `pyproject.toml` file. Most of the time, `poetry` utility
  will appropriately update `pyproject.toml` as dependencies are added or
  removed.

* `extras` directory. The `extras` directory should be used for ancillary files
  (e.g., `direnv` configuration template, general reference documents for tools
  that are not dataset-specific).

-------------------------------------------------------------------------------

## 4. References

### Software Tools

* [DVC Documentation][dvc-docs]

* [FastDS][fastds]

* [FastDS Quick Reference][fastds-quick-reference]

* [Poetry Quick Reference][poetry-quick-reference]

### IUCr References

* [IUCr CPD Round Robin on Quantitative Phase Analysis: Standard Powder X-ray Diffraction
  Data Sets - Data Analysis Kit][iucr-qarr-data]

* [IUCr CPD Round Robin on Quantitative Phase Analysis: Weighed and Measured Values of Distributed Samples and Data][iucr-qarr-values]

------------------------------------------------------------------------------

[-----------------------------INTERNAL LINKS-----------------------------]: #

[#1]: #1-overview
[#1.1]: #11-dataset-contents
[#1.2]: #12-data-format
[#1.3]: #13-license
[#1.4]: #14-known-issues

[#2]: #2-using-the-dataset
[#2.1]: #21-importing-the-dataset
[#2.2]: #22-updating-the-dataset

[#3]: #3-maintaining-the-dataset
[#3.1]: #31-managing-data
[#3.1.2]: #312-updating-data
[#3.2]: #32-releasing-an-official-dataset-version
[#3.3]: #33-dataset-conventions

[#4]: #4-references

[---------------------------- REPOSITORY LINKS ----------------------------]: #

[fastds-quick-reference]: extras/quick-references/FastDS-Quick-Reference.md

[poetry-quick-reference]: extras/quick-references/Poetry-Quick-Reference.md

[-----------------------------EXTERNAL LINKS-----------------------------]: #

[dvc-docs]: https://dvc.org/doc

[fastds]: https://github.com/DAGsHub/fds

[poetry]: https://python-poetry.org/

[python]: https://www.python.org/

[iucr-qarr]: https://www.iucr.org/resources/commissions/powder-diffraction/projects/qarr

[iucr-qarr-data]: https://www.iucr.org/resources/commissions/powder-diffraction/projects/qarr/data

[iucr-qarr-values]: https://www.iucr.org/resources/commissions/powder-diffraction/projects/qarr/values

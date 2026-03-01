# Computational Astrophysics: GPU-Accelerated Stellar Structure Identification in the Solar Neighbourhood using GAIA Data

The study addresses the computational challenges of unsupervised stellar clustering within the local solar neighbourhood by implementing high-performance GPU algorithms.

We present a comparative analysis of parallel configurations, evaluating the performance trade-offs between sequential CPU executions and optimised GPU strategies. The focus is placed on the efficient translation of density-based methodologies—specifically DBSCAN and Friends-of-Friends (FoF)—into massive parallel architectures using the CUDA programming model.

## Repository Structure

The project is structured to separate the main analytical notebook from the underlying C++/CUDA utility files and data directories. 

The root directory contains the primary Jupyter Notebook (`PDPProject.ipynb`), which orchestrates the data loading, the compilation of the CUDA kernels, the execution of the benchmarks, and the generation of the performance plots. The root also houses the official project paper (`PADP_project_Toma_Ferrara_Bonsignore.pdf`), detailing the theoretical background, the algorithmic parallelisation strategies, and the comprehensive results analysis.

The `PDPProject` directory contains all auxiliary files. The `DATASET` folder is the designated location for the astronomical CSV files. The `OUTPUT` folder serves as the destination for all generated benchmarking data. The `UTILS` folder contains the custom CUDA headers (`common.cuh` and `Output_Utils.h`) required. Finally, the `SLINK` folder contains the baseline sequential C++ implementation of the Single Linkage algorithm used for performance comparisons.

## Dataset Acquisition

To ensure the repository remains lightweight and complies with version control file size limits, the astronomical datasets are not included directly. Researchers and reviewers wishing to replicate the benchmarks must procure the data via the official European Space Agency Gaia Archive.

The raw data can be extracted using the following ADQL query, which selects the highest-precision astrometry for stars within the local solar neighbourhood (approximately 200 parsecs):

```sql
SELECT TOP 200000000
    source_id,
    ra, dec, parallax
FROM gaiadr3.gaia_source
WHERE parallax > 5
  AND parallax_over_error > 10
ORDER BY parallax DESC
```

Following the extraction, the spherical astronomical coordinates must be transformed into a three-dimensional heliocentric Cartesian system expressed in parsecs. Furthermore, discrete subsets must be generated based on the Euclidean distance from the Sun to facilitate scalability testing.

The execution of the main notebook strictly expects the resulting files to be placed in the ```PDPProject/DATASET/``` directory with the following exact nomenclature:
- `GAIA_DR3_Cartesian_Heliocentric.csv`    (The complete dataset of 2,234,316 stars)

- `GAIA_nearest_1000.csv`
- `GAIA_nearest_10000.csv`
- `GAIA_nearest_50000.csv`
- `GAIA_nearest_100000.csv`
- `GAIA_nearest_200000.csv`
- `GAIA_nearest_1000000.csv`

## Usage Instructions

The project is designed to be executed within a Google Colaboratory environment, or any similarly configured system equipped with an NVIDIA GPU and the CUDA Toolkit.

To run the experiments, clone this repository and ensure the datasets have been correctly generated and placed in the respective folder. Open the `PDPProject.ipynb` notebook and execute the cells sequentially. The notebook will automatically compile the necessary CUDA source files, run the sequential and parallel algorithms across the various dataset dimensions, and output the comparative execution times and speedup metrics.

## Authors

- Toma Elia
- Ferrara Ludovico
- Bonsignore Christian

## Acknowledgements

The sequential implementation of the Single Linkage (SLINK) algorithm, utilised within this project as a sequential benchmark to evaluate the performance of our parallel Euclidean Minimum Spanning Tree strategies, is derived from the open-source repository authored by Battuzz. The source files located in the `PDPProject/SLINK/` directory are used in accordance with their original licensing terms.

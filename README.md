# BoostFL-IoT: A Novel Boosting-Inspired Federated Learning Approach for IoT Intrusion Detection.

BoostFL-IoT integrates a boosting mechanism into the federated learning communication pipeline for IoT intrusion detection. Each client trains a shallow weak learner on the residuals of the current global ensemble, and the server performs a dynamic, boosting-weighted aggregation in which each client's contribution is scaled by a weight derived from its residual-loss reduction.

## Repository layout

```
BoostFL-IoT/
├── data/                       Extracted dataset subsets used in the paper
│   ├── Bot-IoT.csv
│   ├── UNSW-NB15.csv
│   ├── CICIoT2023_extracted.csv
│   └── ToN-IoT_extracted.csv
├── notebooks/
│   ├── iid/                    IID experiments (BoT-IoT, UNSW-NB15)
│   ├── noniid/                 Non-IID comparison (BoostFL-IoT vs CIDIoT)
│   ├── ablation/               Component ablation study
│   ├── robustness/             Label-flipping robustness (IID and Non-IID)
│   └── scalability/            Accuracy vs number of edge nodes
├── requirements.txt
└── README.md
```

## Setup

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```



## Experiment-to-notebook mapping

The paper reports IID results for BoT-IoT and UNSW-NB15, and Non-IID results for CICIoT2023 and ToN-IoT.

### IID experiments

| Notebook | Produces |
| --- | --- |
| `notebooks/iid/BoostFL_BoT-IoT_IID.ipynb` | BoT-IoT binary and multiclass metrics, convergence curve |
| `notebooks/iid/BoostFL_UNSW-NB15_IID.ipynb` | UNSW-NB15 binary and multiclass metrics, convergence curve |

### Non-IID comparison

| Notebook | Produces |
| --- | --- |
| `notebooks/noniid/BoostFL_CICIoT2023.ipynb` | BoostFL-IoT on CICIoT2023, multi-seed mean/std, convergence , |
| `notebooks/noniid/BoostFL_ToN-IoT.ipynb` | BoostFL-IoT on ToN-IoT, multi-seed mean/std, convergence ,  |
| `notebooks/noniid/CIDIoT_CICIoT2023.ipynb` | Independent CIDIoT re-implementation on CICIoT2023  |
| `notebooks/noniid/CIDIoT_ToN-IoT.ipynb` | Independent CIDIoT re-implementation on ToN-IoT  |

The two `CIDIoT_*` notebooks are the independent CIDIoT implementation. they reproduce the CIDIoT baseline under the same
data subsets and Non-IID protocol.

### Detection metrics 

Detection Rate (DR) and False Alarm Rate (FAR), averaged over three seeds. The BoostFL-IoT and CIDIoT DR/FAR values are also produced by the `noniid` notebooks above.


### Ablation study

| Notebook | Produces |
| --- | --- |
| `notebooks/ablation/BoostFL_Ablation_BoT-IoT.ipynb` | Full model vs no-residual-learning vs FedAvg-aggregation, under a severe Non-IID split (Dirichlet alpha = 0.08) on BoT-IoT |

### Robustness to label-flipping

Each notebook sweeps the fraction of malicious (label-flipping) clients for one method.


- `Binary = True` reproduces the binary robustness results.

| Notebook | Method |
| --- | --- |
| `notebooks/robustness/iid/BoostFL_LabelFlipping_BoT-IoT.ipynb` | BoostFL-IoT |
| `notebooks/robustness/iid/FedAvg_LabelFlipping_BoT-IoT.ipynb` | FedAvg |
| `notebooks/robustness/iid/FedProx_LabelFlipping_BoT-IoT.ipynb` | FedProx |
| `notebooks/robustness/iid/FedMut_LabelFlipping_BoT-IoT.ipynb` | FedMut |

| Notebook | Method |
| --- | --- |
| `notebooks/robustness/noniid/BoostFL_BoT-IoT03.ipynb` | BoostFL-IoT |
| `notebooks/robustness/noniid/BoostFL_BoT-IoT05.ipynb` | BoostFL-IoT |
| `notebooks/robustness/noniid/FedAvg_BoT-IoT03.ipynb` | FedAvg |
| `notebooks/robustness/noniid/FedAvg_BoT-IoT05.ipynb` | FedAvg |
| `notebooks/robustness/noniid/FedProx_BoT-IoT03.ipynb` | FedProx |
| `notebooks/robustness/noniid/FedProx_BoT-IoT05.ipynb` | FedProx |
| `notebooks/robustness/noniid/FedMut_BoT-IoT03.ipynb` | FedMut |
| `notebooks/robustness/noniid/FedMut_BoT-IoT05.ipynb` | FedMut |

### Scalability

| Notebook | Produces |
| --- | --- |
| `notebooks/scalability/BoostFL_Scalability_CICIoT2023.ipynb` | Accuracy vs number of edge nodes (K = 5, 7, 9, 11) on CICIoT2023 |
| `notebooks/scalability/BoostFL_Scalability_ToN-IoT.ipynb` | Accuracy vs number of edge nodes (K = 5, 7, 9, 11) on ToN-IoT |

## Reproducibility

All experiments fix the random seeds for data partitioning, model initialization, and training. Non-IID comparison, detection-metric, and scalability notebooks run over three seeds and report the mean and standard deviation. The IID and Non-IID settings follow the hyperparameters described in the paper: Adam optimizer, learning rate 0.001, batch size 32, 5 local epochs per round, 15 communication rounds for the IID configuration and 30 for the Non-IID configuration. The Non-IID partitioning uses the shard-based scheme described in the paper (100 label-sorted shards distributed unevenly across K = 5 edge nodes).

## Datasets

The `data/` directory contains the extracted dataset subsets used in the experiments. Original sources: BoT-IoT, UNSW-NB15, CICIoT2023, and ToN-IoT.

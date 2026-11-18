# Transfer Learning for ECG Classification

Research codebase for **Patiño et al. (2026)**, *"Transfer Learning for ECG Classification: Effects of Shared Labels, Granularity, and Fine-Tuning in Small Clinical Datasets,"* presented at the **39th IEEE International Symposium on Computer-Based Medical Systems (CBMS 2026)**.

> **Upstream Attribution:** This project builds upon the [UTU-Health-Research/dl-ecg-classifier](https://github.com/UTU-Health-Research/dl-ecg-classifier) framework, extending it with transfer learning, fine-tuning pipelines, and experimental evaluation tools for small clinical ECG datasets.

---

## Core Capabilities

1. **Pretraining:** Train deep ECG encoders on large source datasets and tasks.
2. **Transfer & Fine-Tuning:** Load pretrained representations and fine-tune models on low-resource downstream clinical datasets.
3. **Evaluation:** Assess model performance across label granularities using standardized metrics and experiment tracking.
  
# Usage

To get started, clone the repository and install the required Python packages from `requirements.txt`:

```
pip install -r requirements.txt
```

# Recommended folder repository structure
```
.
├── create_data_csvs.py
├── create_yaml_files.py
├── train_model.py
├── finetune_model.py
├── run_model.py
├── label_mapping.py
├── utils.py
├── data/
│   ├── preprocessed_data/                  # ECG files grouped by database
│   ├── split_csvs/
│   │   └── stratified_data/                # generated split CSVs
│   └── AHA_SNOMED_mapping.csv              # mapping file for SPH conversion
├── configs/                                # YAML configuration files
│   ├── training/
│   ├── predicting/
│   └── finetuning/
├── experiments/                            # training/prediction outputs
├── model_source/
│   └── source_multi/                       # source checkpoints for transfer learning
│   └── source_bin/
│   └── source_cat/
└── src/
    ├── modeling/
    │   ├── train_utils.py
    │   ├── finetune_utils.py
    │   ├── predict_utils.py
    │   ├── metrics.py
    │   └── models/
    │       └── seresnet18.py
    └── dataloader/
        └── dataset.py
        └── dataset_utils.py
        └── transforms.py                      
```

# Fine-tuning (Transfer Learning)

Script: **finetune_model.py**

This script fine-tunes from a pretrained (source) model checkpoint stored under:

```
model_source/<source_model>/<args.model>
```
Example:

* model_source/source_9_multi/split_1_1.pth

To run fine-tuning, the following code can be run in terminal. The first argument can either be a folder or a single YAML file. The second argument is the directory name of the source model.
```
python finetune_model.py <finetuning_yaml_or_dir> <source_model_dir_name>
```

## Freezing / Unfreezing layers

Edit the script **finetune_utils.py** to specify the layers to be frozen and unfrozen during the fine-tuning process.

Example snipped for freezing all the layers:
```
# Freeze model parameters
        for param in self.model.parameters():
            param.requires_grad = False
```
            
Example snippet for unfreezing the last two layers of the pretrained ResNet model:
```
# Unfreeze the last two layers
        for name, child in self.model.named_children():
            if "layer4" in name or "fc" in name or "fc1" in name:
                for param in child.parameters():
                    param.requires_grad = True
```

# Citation

```
@article{patino2026transferlearning,
  title={Transfer Learning for ECG Classification: Effects of Shared Labels, Granularity, and Fine-Tuning in Small Clinical Datasets},
  author={Patiño, Chito and Kaisti, Matti and Pahikkala, Tapio and Airola, Antti},
  journal={Proceedings of the 39th IEEE International Symposium on Computer-Based Medical Systems},
  year={2026},
  address={Limassol, Cyprus},
  year={2026},
  publisher={IEEE},
  month={June},
  doi={10.1109/CBMS69103.2026.00279}
}
```

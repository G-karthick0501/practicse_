# Cloud Classification Datasets - Comprehensive Report

## Executive Summary

This document summarizes all cloud classification datasets explored for the ground-based cloud classification project, including datasets that have been individually analyzed with standardized metrics and additional freely available datasets identified for potential exploration.

---

## Part 1: Datasets Fully Analyzed (Individual EDA Completed)

### Overview Table

| Dataset | Images | Classes | Resolution | Type | Access |
|---------|--------|---------|------------|------|--------|
| **CCSN** | 2,543 | 11 | Mixed (400×400, 256×256) | Classification | Harvard Dataverse (Free) |
| **TJNU-GCD** | 19,000 | 7 | 512×512 | Classification | GitHub/Google Drive (Free) |
| **MGCD** | 8,000 | 7 | 1024×1024 (fisheye) | Classification + Multimodal | GitHub/Google Drive (Free) |

---

### 1. CCSN (Cirrus Cumulus Stratus Nimbus)

**Source:** Harvard Dataverse  
**Download:** https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/CADDPD  
**GitHub Mirror:** https://github.com/Callmewuxin/CCSN_dataset

#### Dataset Details
- **Total Images:** 2,543
- **Classes:** 11 (Ac, As, Cb, Cc, Ci, Cs, Ct, Cu, Ns, Sc, St)
- **Resolution:** Mixed (400×400 and 256×256)
- **Format:** JPEG
- **Train/Test Split:** Not provided

#### Standardized Quality Metrics (Computed)

| Metric | Value | Score |
|--------|-------|-------|
| Total Images | 2,543 | - |
| Number of Classes | 11 | - |
| Imbalance Ratio | 2.45x | - |
| Duplicate Rate | 0.67% | - |
| Mean Class Similarity | 0.94 | - |
| **Health Score** | 90.2/100 | ✅ Good |
| **Balance Score** | 76.3/100 | ⚠️ Moderate |
| **Separability Score** | 29.8/100 | ❌ Low |
| **Overall Score** | 67.9/100 | ⚠️ Moderate |

#### Key Findings
- ✅ Good data health (minimal corrupted files, few duplicates)
- ⚠️ Moderate class imbalance (2.45x between min/max classes)
- ❌ Low separability (classes look visually similar, mean similarity 0.94)
- ⚠️ Small dataset size limits deep learning potential

#### Training Recommendations (from EDA)
```
Model:        EfficientNet-B2 or higher (due to low separability)
Loss:         Weighted CrossEntropyLoss (due to 2.45x imbalance)
Expected Acc: 78-88% (11-class), 92-96% (merged 4-class)
```

#### Citation
```
Zhang, J. L., Liu, P., Zhang, F., & Song, Q. Q. (2018). 
CloudNet: Ground-based Cloud Classification with Deep Convolutional Neural Network. 
Geophysical Research Letters, 45, 8665-8672.
```

---

### 2. TJNU-GCD (TJNU Ground-based Cloud Dataset)

**Source:** Tianjin Normal University  
**Download:** https://github.com/shuangliutjnu/TJNU-Ground-based-Cloud-Dataset  
**Direct Link:** https://drive.google.com/file/d/1dsgoEQLqR3YrOMBC_hOsVEUQC7HuV2fN/view

#### Dataset Details
- **Total Images:** 19,000
- **Classes:** 7 (Cumulus, Altocumulus+Cirrocumulus, Cirrus+Cirrostratus, Clear sky, Stratocumulus+Stratus+Altostratus, Cumulonimbus+Nimbostratus, Mixed cloud)
- **Resolution:** 512×512 (uniform)
- **Format:** JPEG
- **Train/Test Split:** 10,000 / 9,000 (predefined)
- **Geographic Coverage:** 9 provinces in China
- **Time Period:** 2019-2020
- **Annotation:** Meteorologists + cloud researchers

#### Standardized Quality Metrics (Estimated)

| Metric | Value | Score |
|--------|-------|-------|
| Total Images | 19,000 | - |
| Number of Classes | 7 | - |
| Imbalance Ratio | ~8.63x | - |
| Data Leakage | 1.7% (~156 files) | - |
| **Health Score** | ~85/100 | ✅ Good |
| **Balance Score** | ~70/100 | ⚠️ Moderate |
| **Separability Score** | ~50-60/100 | ⚠️ Moderate |
| **Overall Score** | ~75/100 | ✅ Good |

#### Key Findings
- ✅ Largest ground-based cloud dataset (19K images)
- ✅ Uniform resolution (512×512)
- ✅ Expert annotations (meteorologists)
- ✅ Geographic diversity (9 provinces)
- ✅ Pre-defined train/test split
- ⚠️ Higher class imbalance (8.63x)
- ⚠️ Data leakage detected (1.7% of files appear in both train/test)

#### Training Recommendations (from EDA)
```
Model:        MobileNetV3 or EfficientNet-Lite (for Jetson deployment)
Loss:         Weighted CrossEntropyLoss + possible oversampling
Preprocessing: Remove 156 leaked files from test set
Expected Acc: 88-94%
```

#### Citation
```
Liu, S., Duan, L., Zhang, Z., Cao, X., & Durrani, T. S. (2022). 
Ground-based Remote Sensing Cloud Classification via Context Graph Attention Network. 
IEEE Transactions on Geoscience and Remote Sensing, 60, 1-11.
```

---

### 3. MGCD (Multimodal Ground-based Cloud Database)

**Source:** Tianjin Normal University  
**Download:** https://github.com/shuangliutjnu/Multimodal-Ground-based-Cloud-Database

#### Dataset Details
- **Total Images:** 8,000
- **Classes:** 7 (same as TJNU-GCD)
- **Resolution:** 1024×1024 (fisheye lens)
- **Format:** JPEG + Excel (weather data)
- **Train/Test Split:** 4,000 / 4,000 (balanced)
- **Location:** Tianjin, China
- **Time Period:** 2017-2018
- **Multimodal Features:** Temperature, Humidity, Pressure, Wind Speed

#### Standardized Quality Metrics (Estimated)

| Metric | Value | Score |
|--------|-------|-------|
| Total Images | 8,000 | - |
| Number of Classes | 7 | - |
| Imbalance Ratio | 1.73x | - |
| Multimodal Features | 4 weather variables | - |
| **Health Score** | ~92/100 | ✅ Excellent |
| **Balance Score** | ~85/100 | ✅ Good |
| **Separability Score** | ~55/100 | ⚠️ Moderate |
| **Overall Score** | ~78/100 | ✅ Good |

#### Key Findings
- ✅ Best class balance (1.73x)
- ✅ Highest resolution (1024×1024)
- ✅ Multimodal data (weather features boost accuracy 2-3%)
- ✅ Fisheye format matches deployment camera
- ✅ Balanced train/test split (4K/4K)
- ⚠️ Smaller than TJNU-GCD (8K vs 19K)
- ⚠️ Single location (Tianjin only)

#### Weather Feature Statistics
| Feature | Mean | Std | Discriminative Power |
|---------|------|-----|---------------------|
| Temperature (°C) | 30.0 | 3.5 | High (Cumulus: 34.6°C vs Stratocumulus: 26.8°C) |
| Humidity (%RH) | 53.0 | 12.0 | Very High (Cumulonimbus: 64.5% vs Clear: 47.8%) |
| Pressure (hPa) | 1008.5 | 3.0 | High (Clear: 1012.8 vs Cumulus: 1005.9) |
| Wind Speed (m/s) | 0.95 | 0.8 | Moderate |

#### Training Recommendations (from EDA)
```
Model:        CNN + Weather MLP fusion (recommended)
Preprocessing: CenterCrop(900) to remove fisheye black corners
Normalization: Weather standardization required
Expected Acc: 90-95% (with multimodal fusion)
```

#### Citation
```
Liu, S., Li, M., Zhang, Z., Cao, X., & Durrani, T. S. (2020). 
Ground-Based Cloud Classification Using Task-Based Graph Convolutional Network. 
Geophysical Research Letters, 47(5), e2020GL087338.
```

---

## Part 2: Comparison of Analyzed Datasets

### Side-by-Side Comparison

| Feature | CCSN | TJNU-GCD | MGCD |
|---------|------|----------|------|
| **Total Images** | 2,543 | 19,000 ✅ | 8,000 |
| **Classes** | 11 | 7 | 7 |
| **Resolution** | Mixed | 512×512 | 1024×1024 ✅ |
| **Uniform Resolution** | ❌ No | ✅ Yes | ✅ Yes |
| **Pre-split** | ❌ No | ✅ Yes | ✅ Yes |
| **Imbalance Ratio** | 2.45x | 8.63x | 1.73x ✅ |
| **Multimodal** | ❌ No | ❌ No | ✅ Yes |
| **Expert Annotation** | Unknown | ✅ Yes | ✅ Yes |
| **Geographic Diversity** | Unknown | ✅ 9 provinces | ❌ Single city |
| **Camera Type** | Standard | Standard | Fisheye |
| **Overall Score** | 67.9 | ~75 | ~78 ✅ |

### Relationship Between TJNU-GCD and MGCD

```
┌─────────────────────────────────────────────────────────────────┐
│  COMPANION DATASETS FROM SAME RESEARCH GROUP (TJNU)            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  MGCD (2017-2018)              TJNU-GCD (2019-2020)            │
│  ↓                             ↓                                │
│  8,000 images                  19,000 images                    │
│  + weather data                images only                      │
│  1024×1024 fisheye             512×512 standard                 │
│  Tianjin only                  9 provinces                      │
│                                                                 │
│  ✅ SAME: Class definitions, annotation team, labeling criteria │
│  ❌ DON'T MERGE: Different camera optics = domain shift         │
└─────────────────────────────────────────────────────────────────┘
```

### EDA → Model Selection Framework

| Score Range | Health Action | Balance Action | Separability Action |
|-------------|---------------|----------------|---------------------|
| **> 90** | Clean, ready to use | No special handling | Simple model (ResNet-18) |
| **70-90** | Minor cleaning needed | Weighted loss | Medium model (EfficientNet-B2) |
| **50-70** | Heavy cleaning | Weighted loss + oversampling | Complex model (EfficientNet-B5) |
| **< 50** | Consider discarding | Strong oversampling | Very complex model (ViT) |

---

## Part 3: Additional Free Datasets (Not Yet Analyzed)

### Ground-Based Classification Datasets

#### 1. SWIMCAT (Singapore Whole-sky IMaging CATegories)
- **Images:** 784
- **Classes:** 5 (Clear sky, Patterned clouds, Thick dark clouds, Thick white clouds, Veil clouds)
- **Resolution:** 125×125
- **Source:** Singapore (WAHRSIS camera)
- **Period:** January 2013 - May 2014
- **Access:** Free (form required)
- **Link:** http://vintage.winklerbros.net/swimcat.html
- **Registration Required:** Yes (simple form)

#### 2. Zenithal Dataset (Infrared)
- **Images:** 500
- **Classes:** 5 (Stratiform, Cumuliform, Waveform, Cirriform, Clear sky)
- **Resolution:** 320×240
- **Type:** Infrared (8-14μm)
- **Source:** National University of Defense Technology, China
- **Access:** Contact authors
- **Note:** Grayscale infrared images, different modality

#### 3. FabraClouds (Observatori Fabra)
- **Classes:** 10 cloud genera
- **Source:** Observatori Fabra, Barcelona
- **Access:** GitHub
- **Link:** https://github.com/marcosPlaza/Ground-based-Cloud-Classification-with-Deep-Learning
- **Registration Required:** No

#### 4. FabraSwimcat
- **Images:** Various
- **Classes:** 5 (Clear Sky, Patterned Clouds, Thick Dark Clouds, Thick White Clouds, Veil Clouds)
- **Source:** Observatori Fabra, Barcelona
- **Access:** GitHub (same as above)
- **Registration Required:** No

### Ground-Based Segmentation Datasets

#### 5. SWIMSEG (Singapore Whole-sky IMaging SEGmentation)
- **Images:** 1,013
- **Classes:** 2 (Sky/Cloud binary segmentation)
- **Resolution:** 600×600
- **Task:** Binary segmentation (pixel-level masks)
- **Source:** Singapore
- **Period:** October 2013 - July 2015
- **Access:** Free (form required)
- **Link:** http://vintage.winklerbros.net/swimseg.html
- **Registration Required:** Yes (simple form)

#### 6. SWINSEG (Singapore Whole-sky Imaging NIghttime SEGmentation)
- **Images:** 690
- **Classes:** 2 (Sky/Cloud binary)
- **Resolution:** 600×600
- **Task:** Nighttime cloud segmentation
- **Access:** Free (form required)
- **Link:** http://vintage.winklerbros.net/swinseg.html
- **Registration Required:** Yes (simple form)

#### 7. SWINySEG (Combined Day + Night)
- **Images:** 6,768 (6,078 day + 690 night)
- **Classes:** 2 (Sky/Cloud binary)
- **Task:** All-time (day + night) segmentation
- **Access:** Free (form required)
- **Link:** http://vintage.winklerbros.net/swinyseg.html
- **Registration Required:** Yes (simple form)

### Kaggle Datasets (Free, No Registration for Download)

#### 8. Cloud Image Classification Dataset
- **Link:** https://www.kaggle.com/datasets/nakendraprasathk/cloud-image-classification-dataset
- **Type:** Ground-based cloud images
- **Access:** Free (Kaggle account)

#### 9. Weather Image Recognition
- **Images:** 6,862
- **Classes:** Multiple weather types
- **Link:** https://www.kaggle.com/datasets/jehanbhathena/weather-dataset
- **Access:** Free (Kaggle account)

#### 10. Multi-class Weather Dataset
- **Images:** 1,530
- **Classes:** 5 weather conditions
- **Link:** https://www.kaggle.com/datasets/vijaygiitk/multiclass-weather-dataset
- **Access:** Free (Kaggle account)

#### 11. Sky-Image Dataset
- **Link:** https://www.kaggle.com/datasets/antigs/skyimage-dataset
- **Type:** Sky images
- **Access:** Free (Kaggle account)

### Satellite Cloud Datasets (For Reference)

#### 12. 38-Cloud
- **Images:** Landsat 8 scenes (384×384 patches)
- **Task:** Cloud segmentation
- **Source:** Satellite
- **Access:** Free
- **Links:** 
  - GitHub: https://github.com/SorourMo/38-Cloud-A-Cloud-Segmentation-Dataset
  - Kaggle: https://www.kaggle.com/datasets/sorour/38cloud-cloud-segmentation-in-satellite-images

#### 13. CloudSEN12 (Sentinel-2)
- **Type:** Satellite imagery
- **Task:** Cloud detection/segmentation
- **Access:** Free
- **Note:** High quality, newer dataset

#### 14. 95-Cloud
- **Type:** Satellite
- **Task:** Benchmark for cloud detection
- **Access:** Free

---

## Part 4: Dataset Selection Recommendations

### For Your Project (Jetson Nano Deployment)

| Scenario | Recommended Dataset | Rationale |
|----------|---------------------|-----------|
| **Standard camera** | TJNU-GCD | Largest dataset (19K), standard lens, best for training |
| **Fisheye camera** | MGCD | Matches deployment camera, multimodal option |
| **Quick prototyping** | CCSN | Small, fast to download, good for initial testing |
| **Segmentation task** | SWIMSEG | 1,013 images with pixel-level masks |
| **Nighttime support** | SWINySEG | Includes both day and night images |

### Dataset Priority for Exploration

```
Priority 1 (Recommended for full analysis):
├── SWIMCAT (784 images, 5 classes) - Small but high quality
├── Kaggle Cloud Classification - Easy access, no registration
└── FabraClouds - 10 genera, different location

Priority 2 (If more data needed):
├── Weather Image Recognition (6,862 images)
├── SWIMSEG (for segmentation tasks)
└── Zenithal (if infrared camera deployment)

Priority 3 (For awareness/comparison):
├── 38-Cloud (satellite benchmark)
├── CloudSEN12 (satellite, high quality)
└── SWINySEG (day + night)
```

---

## Part 5: Standardized Metrics Framework

### Reusable Quality Scoring System

Use this framework to evaluate any new dataset:

```python
def compute_dataset_quality_report(base_path):
    """
    Computes standardized dataset quality metrics.
    Returns dict with scores from 0-100.
    """
    # Health Score (0-100)
    # - Based on: corrupted files, duplicates, outliers
    # - >90: Clean, ready to use
    # - 70-90: Minor cleaning needed
    # - <70: Consider discarding
    
    # Balance Score (0-100)  
    # - Based on: class distribution CV, imbalance ratio
    # - >80: No special handling
    # - 50-80: Use weighted loss
    # - <50: Oversampling required
    
    # Separability Score (0-100)
    # - Based on: inter-class similarity (histogram comparison)
    # - >60: Simple models work
    # - 30-60: Medium complexity models
    # - <30: Complex models required
    
    # Overall Score
    # = Health × 0.4 + Balance × 0.3 + Separability × 0.3
```

### Key Metrics to Compute for Each Dataset

| Metric | Description | Good Value |
|--------|-------------|------------|
| Total Images | Dataset size | >5,000 for deep learning |
| Num Classes | Classification complexity | 5-10 optimal |
| Resolution | Image quality | >256×256 |
| Uniform Resolution | Preprocessing simplicity | Yes |
| Imbalance Ratio | Max/min class ratio | <3x |
| Duplicate Rate | Data integrity | <1% |
| Mean Class Similarity | Visual distinctiveness | <0.8 |
| Train/Test Split | Benchmark readiness | Provided |
| Expert Annotation | Label quality | Yes |

---

## Appendix: Quick Access Links

### Datasets Analyzed
| Dataset | Direct Download |
|---------|-----------------|
| CCSN | https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/CADDPD |
| TJNU-GCD | https://drive.google.com/file/d/1dsgoEQLqR3YrOMBC_hOsVEUQC7HuV2fN/view |
| MGCD | Contact via GitHub: https://github.com/shuangliutjnu/Multimodal-Ground-based-Cloud-Database |

### Additional Free Datasets
| Dataset | Link | Registration |
|---------|------|--------------|
| SWIMCAT | http://vintage.winklerbros.net/swimcat.html | Form |
| SWIMSEG | http://vintage.winklerbros.net/swimseg.html | Form |
| SWINySEG | http://vintage.winklerbros.net/swinyseg.html | Form |
| FabraClouds | https://github.com/marcosPlaza/Ground-based-Cloud-Classification-with-Deep-Learning | None |
| Kaggle Cloud | https://www.kaggle.com/datasets/nakendraprasathk/cloud-image-classification-dataset | Kaggle account |
| 38-Cloud | https://github.com/SorourMo/38-Cloud-A-Cloud-Segmentation-Dataset | None |

---

*Document generated from conversation history analysis*  
*Last updated: January 2026*

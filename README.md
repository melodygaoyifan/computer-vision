# Nutrition5K Project

## Overview
This project implements and extends the Nutrition5K paper, which aims to predict nutritional content from food images. The model takes both RGB images and depth information as input to predict calories, mass, and macronutrient content (fat, carbohydrates, protein).

## Table of Contents
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Model Architecture](#model-architecture)
- [Training](#training)
- [Evaluation](#evaluation)
- [Web Interface](#web-interface)
- [Results](#results)

## Installation

### Requirements
```bash
# Python packages
pip install torch torchvision
pip install numpy pandas
pip install flask flask-cors
pip install ngrok-api pyngrok
pip install Pillow tqdm
pip install matplotlib seaborn
```

### Setup
1. Clone the repository:
```bash
git clone [your-repo-url]
cd nutrition5k-project
```

2. Prepare the data:
- Place training images in `data/images/`
- Place depth frames in `data/frames/`
- Ensure metadata CSV is available

## Project Structure
```
nutrition5k-project/
│
├── data/
│   ├── images/          # RGB images
│   ├── frames/          # Depth frames
│   └── metadata.csv     # Nutritional annotations
│
├── models/
│   ├── model.py         # Model architecture
│   └── normalizer.py    # Data normalization
│
├── training/
│   ├── dataset.py       # Dataset class
│   └── train.py         # Training script
│
├── evaluation/
│   ├── evaluate.py      # Evaluation metrics
│   └── results/         # Evaluation results
│
├── web_interface/
│   ├── app.py          # Flask application
│   └── templates/      # HTML templates
│
└── outputs/
    ├── best_model.pth   # Saved model
    └── results/         # Training results
```

## Model Architecture
The model uses a modified InceptionV3 architecture with:
- 6-channel input (RGB + depth)
- Multi-task prediction heads
- Shared feature extractor

```python
class NutritionPredictor(nn.Module):
    def __init__(self, input_channels=6):
        # Architecture details...
```

## Training

### Data Preparation
```python
# Load and preprocess data
train_dataset = Nutrition5kDataset(x_train, y_train, mode='train', normalizer=normalizer)
test_dataset = Nutrition5kDataset(x_test, y_test, mode='test', normalizer=normalizer)
```

### Training Process
```python
# Train model
model = NutritionPredictor(input_channels=6)
results, history = train_model(train_loader, test_loader, model, 
                             num_epochs=2000, device='cuda')
```

### Training Parameters
- Learning rate: 1e-4
- Optimizer: RMSprop
- Batch size: 32
- Early stopping patience: 200
- Minimum epochs: 100

## Evaluation
Evaluation metrics include:
- Mean Absolute Error (MAE)
- MAE Percentage
- RMSE
- Correlation

```python
# Run evaluation
results = evaluate_model()
```

### Results Files
The evaluation generates several CSV files:
1. `[metric]_predictions.csv`: Detailed predictions
2. `summary_metrics.csv`: Overall performance metrics
3. `error_distribution.csv`: Error analysis
4. `percentage_error_distribution.csv`: Relative errors
5. `paper_comparison.csv`: Comparison with original paper

## Web Interface

### Setup
1. Get ngrok authentication token
2. Run the Flask application:
```python
start_server()
```

### Features
- Image upload
- Real-time prediction
- Visualization of results
- Error handling
- Debug information

## Results

### Performance Metrics
| Metric    | MAE    | MAE%   | Paper MAE% |
|-----------|--------|--------|------------|
| Calories  | [Value]| [Value]| 26.1%      |
| Mass      | [Value]| [Value]| 18.8%      |
| Fat       | [Value]| [Value]| 34.2%      |
| Carbs     | [Value]| [Value]| 31.9%      |
| Protein   | [Value]| [Value]| 29.5%      |

### Model Comparisons
- Comparison with paper results
- Ablation studies
- Performance analysis

## Citation
```bibtex
@article{nutrition5k2021,
  title={Nutrition5k: Towards Automatic Nutritional Understanding of Generic Food},
  author={Thames, Quin and Karpur, Arjun and Norris, Wade and Xia, Fangting and Panait, Liviu and Weyand, Tobias and Sim, Jack},
  journal={arXiv preprint arXiv:2103.03375},
  year={2021}
}
```



## Contact
[gao2@uchicago.edu]

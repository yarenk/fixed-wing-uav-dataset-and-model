# ***Fixed wing UAV dataset and model***

This repository contains a dataset of labeled images for fixed-wing UAVs (Unmanned Aerial Vehicles), which was trained using YOLOv5. The dataset is suitable for object detection tasks and can be utilized to train, validate, and test deep learning models for UAV recognition. I created this for Teknofest Fighting UAV competition. For Turkish visit [TURKISH](https://github.com/yarenk/fixed-wing-uav-dataset-and-model/blob/main/README_TR.md)

## Dataset Overview
The dataset includes labeled images of fixed-wing UAVs with bounding boxes and corresponding class labels. It is structured to be compatible with YOLOv5's training pipeline.

### Features:
- **Image Format**: JPEG/PNG
- **Annotation Format**: YOLO format (TXT files for each image with class and bounding box coordinates)
- **Classes**: iha
- **Split**: Training, validation, and test sets are included.

## Directory Structure
Below is the directory structure of the dataset:

```
┌── dataset/
│   ├── images/
│   │   ├── train/        # Training images
│   │   └── val/          # Validation images
│   ├── labels/
│   │   ├── train/        # YOLO format annotations for training
│   │   └── val/          # YOLO format annotations for validation
└── README.md  # Dataset description
```

## Dataset Preparation and Training

1. **Data Annotation**:
   The images were manually annotated using [Roboflow](https://app.roboflow.com) to create bounding boxes around the fixed-wing UAVs. The annotations were saved in YOLO format, compatible with YOLOv5 training.

2. **Dataset Splitting**:
   The dataset was divided into training and validation sets to ensure proper evaluation during the training process. Approximately 90% of the data was used for training, and 5% was reserved for validation.

3. **Model Training**:
   The YOLOv5 model was trained using the prepared dataset. The training configuration included:
   - **Model**: YOLOv5s
   - **Image Size**: 640x640
   - **Batch Size**: 16
   - **Epochs**: 100
   - **Optimizer**: SGD with a learning rate scheduler

   The training process was conducted on a GPU for faster computation. The model achieved good performance metrics, making it suitable for UAV detection tasks.

4. **Evaluation**:
   The trained model was evaluated on the validation set. Metrics such as Precision, Recall, and mAP (Mean Average Precision) were calculated to measure the model's performance. The results are as follows:
   - **Precision**: 99.2%
   - **Recall**: 98.6%
   - **mAP**: 98.4%

## How to Use the Dataset

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yarenk/fixed-wing-uav-dataset-and-model.git
   ```

2. **Use the Dataset for Training**:
   You can use this dataset to train your custom models.

## Example Images
![Example Image](path/to/example_image.png)


## Acknowledgements
Special thanks to the contributors and the tools that made this dataset possible:
- [YOLOv5](https://github.com/ultralytics/yolov5)
- Labeling tools like [Roboflow](https://app.roboflow.com/)

## Contact
If you have any questions or suggestions regarding this dataset, feel free to open an issue or contact me at [ykoc7070@gmail.com](mailto:ykoc7070@gmail.com).

---

Thank you for your interest in the fixed-wing UAV dataset!



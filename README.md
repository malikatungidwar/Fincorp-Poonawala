# Road Damage Detection & Severity Assessment

A computer-vision decision-support project for detecting road damage, assessing severity, and prioritizing maintenance action from road images.

## Project Story

**Detect → Assess → Prioritize → Explain**

The project uses object detection to identify road defects and then translates model output into an interpretable maintenance-priority signal.

## Key Features

- Road damage detection using YOLO11n
- Bounding-box localization
- Four target defect classes
- Confidence threshold control
- Project-defined severity assessment
- Maintenance Priority Score
- Explainable priority rationale
- Management insight and recommended action
- Interactive Gradio dashboard

## Detected Classes

1. Longitudinal Crack
2. Transverse Crack
3. Alligator Crack
4. Pothole

The source RDD2022 labels were mapped as follows:

| Original label | Project class |
|---|---|
| D00 | Longitudinal Crack |
| D10 | Transverse Crack |
| D20 | Alligator Crack |
| D40 | Pothole |

## Dataset

The project uses a Roboflow-hosted India subset/mirror of the RDD2022 road-damage dataset rather than downloading the complete original multi-country archive.

The cleaned four-class dataset contained:

- Train: 2,738 images
- Validation: 338 images
- Test: 147 images
- Total: 3,223 images
- Total annotations: 6,831

The dataset itself is not included in this GitHub repository because of its size.

## Model

- Model: YOLO11n
- Image size: 640 × 640
- Training epochs: 50
- Batch size: 16
- Pretrained weights: Yes
- Training hardware: NVIDIA Tesla T4
- Ultralytics: 8.4.165

## Test Performance

Overall test performance:

| Metric | Result |
|---|---:|
| Precision | 40.6% |
| Recall | 43.3% |
| mAP@50 | 37.9% |
| mAP@50-95 | 15.1% |

### Per-Class Results

| Class | Precision | Recall | mAP@50 | mAP@50-95 |
|---|---:|---:|---:|---:|
| Longitudinal Crack | 42.6% | 40.3% | 34.2% | 12.6% |
| Transverse Crack | 18.2% | 20.0% | 7.67% | 1.22% |
| Alligator Crack | 58.1% | 65.0% | 66.6% | 31.1% |
| Pothole | 43.4% | 47.9% | 43.0% | 15.4% |

The Transverse Crack class has weaker performance because it had very limited representation in the source annotations.

## Severity Assessment

RDD2022 does not provide Low/Medium/High severity labels. Therefore, severity in this project is a **project-defined post-detection heuristic**, not a supervised severity model.

The heuristic considers:

- Defect type
- Bounding-box area relative to image area
- Number of detected defects

Type weights:

- Longitudinal Crack: 0.50
- Transverse Crack: 0.55
- Alligator Crack: 0.80
- Pothole: 1.00

Severity thresholds:

- Below 35: Low
- 35–64.9: Medium
- 65 and above: High

## Maintenance Priority

The Maintenance Priority Score is also a project-defined decision-support heuristic:

- 60% average severity
- 25% defect count
- 15% proportion of high-severity defects

Priority interpretation:

- 65 and above: High
- 35–64.9: Medium
- Below 35: Low

The score is intended to demonstrate how computer-vision outputs can be translated into an interpretable resource-prioritization signal. It is not an engineering standard.

## Business / Management Value

The project moves beyond simple image classification by connecting technical detection with a management decision:

**Image → Defect Detection → Severity → Maintenance Priority → Recommended Action**

This can support road-maintenance teams by providing a structured way to review visual road-condition information and prioritize follow-up inspection.

## Dashboard

The interactive dashboard allows a user to:

1. Upload a road image
2. Adjust the confidence threshold
3. Analyze the image
4. View bounding-box detections
5. Review defect KPIs
6. See the Maintenance Priority Score
7. Understand why the priority was assigned
8. Review management insight and recommended action

## Project Structure

```text
Road_Damage_Detection/
├── Road_Damage_Detection.ipynb
├── app.py
├── requirements.txt
├── data.yaml
├── README.md
└── screenshots/
    └── dashboard.png
```

## Running the Dashboard

Install dependencies:

```bash
pip install -r requirements.txt
```

Place the trained `best.pt` model in the project directory and run:

```bash
python app.py
```

The trained model weights are not included in this repository because of file-size constraints.

## Limitations

- The dataset is limited to the India subset used for this project.
- The four-class dataset is imbalanced, particularly for Transverse Crack.
- The severity and maintenance-priority scores are project-defined heuristics.
- The model should not be treated as an engineering-grade road-safety assessment system.
- External validation on broader road conditions would be required for operational deployment.

## Academic Project

Road Damage Detection & Severity Assessment  
MBA in AI & Data Science

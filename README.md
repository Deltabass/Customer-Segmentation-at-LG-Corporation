
# LG Customer Segmentation with K-Means

This project performs customer segmentation using K-Means clustering and deploys the model using Amazon SageMaker.

## 📊 Overview
Using a dataset of LG customers, we:
- Preprocessed data (encoded categorical, scaled numerical)
- Applied K-Means clustering
- Visualized the segments using PCA
- Exported the model and deployment-ready scripts

## 📁 Project Structure
```
customer_segmentation_deployable/
│
├── customer_segmentation_model.pkl   # Trained KMeans model + scaler + encoders
├── inference.py                      # SageMaker inference script
```

## 🚀 Deployment with SageMaker

1. **Upload Files to S3**

Upload `customer_segmentation_model.pkl` and `inference.py` to your S3 bucket.

2. **Deploy in SageMaker**

Use the following sample code to deploy:

```python
from sagemaker.sklearn.model import SKLearnModel
from sagemaker import get_execution_role

role = get_execution_role()
model_uri = 's3://your-bucket/customer_segmentation_model.pkl'

sk_model = SKLearnModel(
    model_data=model_uri,
    role=role,
    entry_point='inference.py',
    framework_version='1.2-1'
)

predictor = sk_model.deploy(
    instance_type='ml.m5.large',
    initial_instance_count=1
)
```

3. **Call the Endpoint**

```python
input_features = [[35, 1, 2, 1, 7, 7, 4, 5, 700]]  # Example input
predictor.predict(input_features)
```

## 🛠 Requirements
- AWS Account with S3 and SageMaker permissions
- Python 3.7+
- Boto3, scikit-learn, joblib

---

Created with ❤️ using scikit-learn and Amazon SageMaker.

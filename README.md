# Skeleton-Guided-Text-Generation

---
## 💾 Dataset Setup

This project uses the **KTH Dataset Complete** from Kaggle. Follow the steps below to set up your Kaggle API credentials and download the dataset automatically using the provided script.

### Prerequisites: Kaggle API Token

1. Go to your [Kaggle Account Settings](https://www.kaggle.com/settings).
2. Scroll down to the **API** section and click **Create New API Token**. This will download a `kaggle.json` file.
3. Upload or place `kaggle.json` in your current working directory.

### Downloading the Dataset

Run the following commands in your terminal or Google Colab notebook:

```bash
# Set up Kaggle credentials
mkdir -p ~/.kaggle
cp kaggle.json ~/.kaggle/
chmod 600 ~/.kaggle/kaggle.json

# Execute the automated download script
./download_data.sh

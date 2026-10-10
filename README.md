# Chest X-Ray AI — Streamlit Community Cloud (free-tier demo)

A Streamlit web app for the user's existing trained checkpoints:

- `densenet121_opacity_best.pth` — classifies the dataset's calculated lung-opacity label.
- `fasterrcnn_opacity_best.pth` — predicts bounding boxes for annotated lung-opacity regions.

This app runs on CPU. Model weights are downloaded from a **public Hugging Face Model repository** (not a Hugging Face Space) using `huggingface_hub`.

## 1. Create a Hugging Face Model repository

1. Go to https://huggingface.co/new
2. Create a repository with type **Model**, e.g. `chest-xray-ai-models`.
3. Upload these exact files to the root of the Model repository:
   - `densenet121_opacity_best.pth`
   - `fasterrcnn_opacity_best.pth`
4. Keep the Model repository public so the free Streamlit app can download the weights without a token.

The repository ID will look like `YOUR_USERNAME/chest-xray-ai-models`.

## 2. Create a GitHub repository for the app

Create a new GitHub repository and upload:

- `streamlit_app.py`
- `requirements.txt`

Do **not** upload the `.pth` files to GitHub; the detector checkpoint may exceed GitHub's regular file-size limit. Do not upload dataset images or any patient data.

## 3. Deploy on Streamlit Community Cloud

1. Sign in at https://share.streamlit.io/ using GitHub.
2. Choose **Create app**.
3. Select your GitHub repository and `streamlit_app.py` as the entrypoint.
4. Open **Advanced settings → Secrets** and set:

```toml
HF_MODEL_REPO_ID = "YOUR_USERNAME/chest-xray-ai-models"
```

Replace the example repository ID with the actual public Model repository ID, then deploy.

## Inputs

PNG, JPG, JPEG and single-frame DICOM images. Some compressed DICOM transfer syntaxes might require an extra pixel decoder and may not work in the default free-tier environment.

## Limitations

The models were trained on dataset labels for lung opacity. They have not been validated to diagnose pneumonia, tuberculosis, lung cancer, or other diseases. The classifier output is an uncalibrated sigmoid score; detector rectangles can be inaccurate. This application is for research/demo use only and must not be used for medical decisions. Do not upload identifiable patient images to a public demo.

## Resource notes

Streamlit Community Cloud is free but resource-limited; apps may sleep after inactivity, and CPU inference can be slow. This app loads the detector only if localization is selected to reduce memory use.
# chest-xray-ai-demo

# 🧠 LinkedIn Profile Photo Generator

This project focuses on converting casual user images into professional-looking profile pictures suitable for LinkedIn. Using the **OstrisAI Toolkit**, we enhanced visual quality while preserving the individual’s identity. A key part of our workflow involved captioning images using **Florence-2** to extract meaningful descriptions. These captions were used to guide **Stable Diffusion** —a powerful image generation model—to enhance pose, background, lighting, and attire. Additionally, we applied **LoRA fine-tuning** to personalize outputs and maintain identity consistency. The project demonstrated the integration of vision-language models and generative AI for creating polished, career-ready profile images.

---
## About Ostris AI Toolkit and why we used it?
The Ostris AI Toolkit is a versatile framework designed for building advanced AI applications with ease. It provides ready-to-use templates and pipelines for tasks such as face-swapping, text-to-image generation, and LoRA-based fine-tuning. In this project, OstrisAI was used in conjunction with Stable Diffusion to transform basic user photos into high-quality, professional-looking LinkedIn profile images. The toolkit supports integration with Florence-2 model, allowing for the generation of detailed image captions that guide the AI to enhance relevant attributes such as attire, background, and lighting. Crucially, it also supports the application of custom LoRA weights, enabling personalized fine-tuning that preserves the subject’s identity while improving overall presentation. To sum it up, Ostris AI-Toolkit is a personalization-first, lightweight, and Cloud GPU -friendly setup to build exactly what we needed without having to start from scratch, and thats why we used it for our project.

---
## 📁 Project Structure

### 1. Collect Input Images
- <ins>Folder</ins> : input images/your_image_dataset
- Place user-uploaded or sample photos here.
- <ins>Recommended format</ins> : .jpg or .png.

### 2. Generate Captions with ComfyUI
- <ins>Tool</ins> : ComfyUI
- <ins>Files</ins> :
  - [Refer this link for captioning images](https://github.com/TheLocalLab/fluxgym-Colab?tab=readme-ov-file) (Florence-2 was working better for us)
  - Scripts/Simple Flux LoRA Captioning Workflow.json
- <ins>Purpose</ins> : Generate rich, descriptive captions using models like BLIP-2, CLIP, or Florence-2. 
- <ins>Output</ins> : Captions stored as .txt files or JSON in the captions/ folder.

### 3. Prepare Prompts using Captions
- <ins>Script</ins> : inside caption_images.py or separate script like prepare_prompts.py
- Combine caption data into prompts for guiding the image generation process.
- Optionally include instructions like "professional lighting", "business attire", or "clean background".

### 4. Run Flux LoRA Model with Stable Diffusion
- <ins>Tool</ins> : Ostris AI Toolkit
- <ins>Script</ins> : scripts/generate_photos.py
- <ins>Steps</ins> :
  - Load a base Stable Diffusion model (e.g., SD 1.5 or SDXL).
  - Apply Flux LoRA weights (fine-tuned for LinkedIn-style outputs).
  - Generate images using the prepared prompts and input photos as conditioning.
- <ins>Output Folder</ins> : outputs/

### 5. Output images at different steps
- Save polished images back to outputs/.

## 📄 Setup Instructions
1. Environment Setup (RunPod or Local with GPU)
Clone the repository and set up the Python environment:

bash
Copy
Edit
git clone https://github.com/ostris/ai-toolkit.git
cd ai-toolkit
git submodule update --init --recursive
python3 -m venv venv
source venv/bin/activate
Install PyTorch (tested version with CUDA 12.6):

bash
Copy
Edit
pip install --no-cache-dir torch==2.6.0 torchvision==0.21.0 --index-url https://download.pytorch.org/whl/cu126
Install the remaining dependencies:

bash
Copy
Edit
pip install -r requirements.txt
If you run into compatibility issues, run the following optional upgrade:

bash
Copy
Edit
pip install --upgrade accelerate transformers diffusers huggingface_hub
2. Upload Your Dataset
Create a new folder at the root level and name it something like your-dataset:

bash
Copy
Edit
/ai-toolkit/
├── your-dataset/
│   ├── image1.jpg
│   ├── image2.png
│   ├── description1.txt
│   └── ...
Your dataset should contain .jpg, .jpeg, or .png images along with .txt files describing them.

3. Hugging Face Authentication
You’ll need to authenticate with Hugging Face to access pre-trained models:

Generate a read-only token.

Request access to the Flux.1-dev model.

Log in via the CLI:

bash
Copy
Edit
huggingface-cli login
# Paste your access token when prompted
4. Training the Model
Copy an example config file from config/examples to config/ and rename it:

bash
Copy
Edit
cp config/examples/train_lora_flux_schnell_24gb.yaml config/your_config.yaml
Edit the configuration file:

Set your dataset path:

yaml
Copy
Edit
folder_path: "/workspace/ai-toolkit/your-dataset"
Review and update other parameters based on your training goals.

Start training:

bash
Copy
Edit
python run.py config/your_config.yaml
💡 Note:

A new folder will be created based on your config’s output path.

It will contain all checkpoints and sample outputs.

You can safely interrupt training with Ctrl+C. Resume training anytime and it will continue from the last saved checkpoint.



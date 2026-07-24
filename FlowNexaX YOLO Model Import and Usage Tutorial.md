# YOLO Model Import and Usage Tutorial

## 1. YOLO Model Training

### How do I create a YOLO object detection model?

- YOLO is an open-source object detection model. Many tools can be used to train custom models. It is recommended to use a training tool that supports YOLO11 to create your object detection model. Recommended input size: `640 × 640`.
- After training the model with YOLO11, you need to convert the model to the `ncnn` format so it can run on a mobile device. The converted model contains two files:
  - a `.bin` file
  - a `.param` file
- Rename the exported model files to:
  - `model.bin`
  - `model.param`
- Create a `config.txt` file on your computer. The file content should use the following format:

```txt
yoloDetection-v11n This is related to the YOLO11 recognition demo
```

- The content above is separated into three parts by spaces. The first part is a fixed model type name. Currently, `v11s` and `v11n` are supported, so you can use:
  - `yoloDetection-v11s`
  - `yoloDetection-v11n`
- The model name in `config.txt` must match the YOLO11 model version you used during training. Currently supported versions:
  - `yolo11n`
  - `yolo11s`
- `YOLO11n` and `YOLO11s` represent model versions of different sizes. Fill in the corresponding content according to the version selected during model training.
- Create a `labels.txt` file on your computer. Write one category name per line. The order must be the same as the category order used in the training data.
- Package the following four files into a `.zip` file, and then import it into FlowNexaX:
  - `model.bin`
  - `model.param`
  - `config.txt`
  - `labels.txt`

After import, the model will run locally on the device through the `ncnn` inference engine and will be used to recognize targets required by user-created automation flows.

## 2. Import the YOLO Model into FlowNexaX

Package the required YOLO model files into a compressed archive.

Then send the compressed file to yourself through Telegram, a browser, or another file transfer method. Make sure FlowNexaX is already installed on your phone, then choose to open the compressed file with FlowNexaX.

FlowNexaX will automatically import the model.

YOLO plugin and model download address:

https://drive.google.com/drive/folders/1YI4IADhPqw0T01bccNXSCBkr6pgr0q69

Telegram contact/link:

https://t.me/flownexax

## 3. Import the YOLO Plugin

The YOLO plugin and model files can be downloaded from the following Google Drive folder:

https://drive.google.com/drive/folders/1YI4IADhPqw0T01bccNXSCBkr6pgr0q69

After downloading the compressed package of the YOLO plugin, the import method is exactly the same as importing the model above: choose to open it with FlowNexaX, and it will be imported automatically.

Telegram contact/link:

https://t.me/flownexax

## Notes

After importing both the YOLO model and YOLO plugin, you can use YOLO-related APIs in FlowNexaX scripts, such as `yoloRunModel`, according to the JS tutorial documentation.

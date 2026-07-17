# YOLO Model Import and Usage Tutorial

## 1. YOLO Model Training

At the moment, there is no especially good recommended option. You can find a Windows computer to train a YOLO model yourself, and use a tool that can convert the YOLO model trained on the computer into an NCNN mobile model.

Recommended tool: Renqi Cat

[Renqi Cat Yolov5 / Yolov8 / Yolov10 / Yolov11 Auto-Annotation Tool + GPU One-Click Training Package Without Python Environment + NCNN Module Detailed Tutorial, Completely Free](https://www.bilibili.com/video/BV1imUPYTEt4/?share_source=copy_web&vd_source=60f09731f4b0983c0d36ccf9574c0e07)

This tool makes model training very convenient, and it also supports secondary processing of models.

If the training and conversion to an NCNN model are successful, you will get two files:

- a `.bin` file
- a `.param` file

Rename these two files to:

- `model.bin`
- `model.param`

Then create a `config.txt` file on your computer. Fill in the file content according to the model configuration requirements.

After filling in the content, you must choose **Save As**, and then select the character encoding as **UTF-8** in the lower-right corner.

This is very important!

Very important!

Then create a `labels.txt` file on your computer. Fill in the content according to the actual labels of your model. For example, write one label per line according to the categories used by your model.

## 2. Import the YOLO Model into FlowNexaX

Package the required YOLO model files into a compressed archive.

Then send the compressed file to yourself through Telegram, a browser, or another file transfer method. Make sure FlowNexaX is already installed on your phone, then choose to open the compressed file with FlowNexaX.

FlowNexaX will automatically import the model.

Telegram contact/link:

https://t.me/flownexax

## 3. Import the YOLO Plugin

The YOLO plugin should be downloaded from the Telegram link below:

https://t.me/flownexax

After downloading the compressed package of the YOLO plugin, the import method is exactly the same as importing the model above: choose to open it with FlowNexaX, and it will be imported automatically.

## Notes

After importing both the YOLO model and YOLO plugin, you can use YOLO-related APIs in FlowNexaX scripts, such as `yoloRunModel`, according to the JS tutorial documentation.
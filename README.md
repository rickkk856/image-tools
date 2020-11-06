![PyInstaller Mac](https://github.com/lobe/image-tools/workflows/PyInstaller%20Mac/badge.svg) ![PyInstaller Windows](https://github.com/lobe/image-tools/workflows/PyInstaller%20Windows/badge.svg)
# Image Tools: creating image datasets
Image Tools helps you form machine learning datasets for image classification.

## Download the desktop application
We use GitHub Actions to build the desktop version of this app. If you would like to download it for
Windows or Mac, please click on [Actions](https://github.com/lobe/image-tools/actions) and then 
you will see [PyInstaller Windows](https://github.com/lobe/image-tools/actions?query=workflow%3A%22PyInstaller+Windows%22)
and [PyInstaller Mac](https://github.com/lobe/image-tools/actions?query=workflow%3A%22PyInstaller+Mac%22)
on the left under 'All Workflows'. Once you click into the Windows or Mac workflow, you will see a list of the builds
in the center of the screen. Click on the topmost item in the results list for the latest version. Once you click
on the latest build, you should see a section titled 'Artifacts' with an item called 'Image Tools Windows' or 
'Image Tools Mac'. When you click on this artifact, it should download the zip containing the app for you!

## Code Setup
These tools were developed with python 3.7.7

Install the required packages.
```bash
pip install -r requirements.txt
```
If you are on Windows, you will also need to install the latest PyInstaller from GitHub:
```bash
pip install git+https://github.com/pyinstaller/pyinstaller.git
```

## CLI Usage
### CSV, XLSX, or TXT files
#### Downloading an image dataset from the urls in a csv or txt file:
```bash
python -m dataset.download_from_file your_file.csv --url UrlHeader --label LabelHeader
```
This downloader script takes either a csv, xlsx, or txt file and will format an image dataset for you. The resulting images 
will be downloaded to a folder with the same name as your input file. If you supplied labels, the images will be 
grouped into sub-folders with the label name.

* csv or xlsx file
  * specify the column header for the image urls with the --url flag
  * you can optionally give the column header for labels to assign the images if this is a pre-labeled dataset
  
* txt file
  * separate each image url by a newline

#### Predicting labels and confidences for images in a csv, xlsx, or txt file:
```bash
python -m model.predict_from_file your_file.csv path/to/lobe/savedmodel --url UrlHeader
```
This prediction script will take a csv or txt file with urls to images and a Lobe TensorFlow SavedModel export directory, 
and create and output csv with the label and confidence as the last two columns.

* csv or xlsx file
  * specify the column header for the image urls with the --url flag
  
* txt file
  * separate each image url by a newline
  
  
### Flickr downloader
Download images from Flickr by latitude and longitude bounding box location and any desired search terms.
```bash
python -m dataset.download_from_flickr api_key dest_folder --bbox min_lat,min_long,max_lat,max_long --search searchTerm
```
This will create an `images.csv` file in your destination folder that includes the EXIF data for the downloaded photos.
  
  
### Export Lobe dataset
Export your project's dataset by giving the project name and desired export directory:
```bash
python dataset/export_from_lobe.py 'Project Name' destination/export/folder
```
Your images will be copied to the destination folder, and their labels will be the subfolder name. You can take this
exported folder and drag it directly to a new project in Lobe.

  
## Build Desktop Application
You can create a desktop GUI application using PyInstaller:

```bash
pyinstaller --onefile app/app.spec
```

This will create a `dist/` folder that will contain the application file `Image Tools.exe` or `Image Tools.app`
depending on your OS.

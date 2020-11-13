# Create WebP images
WebP is an image file format from google that provides lossy comperssion as well as tranparency bu can provide better compression thatn either JPEG or PNG. Losssy WebP images are supported in Android 4.0 and higehr, and lossless and transparent WebP imags are supporte in Android 4.3 and higher. 

## Convert images to WebP
Android studio can convert PNG, JPG, BMP or static GIF images to WebP format. you can convert individual images or folders of images. To convert an image or folder of images, proceed as follows
1. Right click on an image file or a folder containing a number of images files, and then click Convert to WebP
2. The converting images to WebP dialog opens. 
3. Select either lossy or lossless encoding. Lossless endocing is onlly available if your minSdkVersion is set to 18 or hgiehr. If you select lossy encoding, set the encoding quality, and choose whether or not to view a preview of each converted image before saving. 

You can also choose to skip convertin any files where the encoded version would be larger than the orginal, or any files with tranparency or an alpha channel. Becuase Android Studio only allows you to create tranparent WebP images if you minSdkVersion is set to 18 or higher, the skip images with tranparency/alpha channel checkbos is automatically selected if your minSdkVersion is lower than 18.

4. Click to begin the conversion. If you are converitn more than oneimage, the conversion is a single step and canb be undone to revert all the images you converted at once. 

If you selected lossless conversion above, the conversino happens immediately. Your images are converted in place in their orinal location. If you selected lossy conversion, continue on to the next step.

5. If you selected lossy conversion, and you chose to view a preview of each converted image before saving, Android Studio shows you each image druing the conversion so you can inspect the conversion result. (If you did not choose to view a preview, Android Studio skips this tep, and converts your images immediately.) During the preview step, you can sjust the quality setting for each imageindvidually, as described below. 

6. Finsih


## Conver Webp Images to PNG.
If you want to use a WebP image from your project for another purpose (for example, in a web page that needs to correctly display images in a browser without WebP support), you can use Android Studio to convert WebP images to PNG format. To convert a WebP Image to PNG, proceed as follows. 
1. Right click on a Webp image in the AndroidStudio, then click convert to PNG
2. A dialog appears, askign if you would like to conver the image to PNG ,deleting the orignal WebP file, or keep the orignial WebP file as well as the new pNG file. Click Yes to delte the orignial WebP file, or No to retain teh WebP file in addition to the PNG file. Your image is converted immediatley. 


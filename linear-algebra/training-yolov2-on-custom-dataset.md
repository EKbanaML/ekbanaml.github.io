## Training YoloV2 in Custom Dataset

Without sounding too smart as if to describe everything of YOLO artitecture here, in this article I would rather show you a lame approach of plugging the custom data set and training a new model in the Google open image datasets. If you're however to curious to understand it, you could follow the author's [webpage](https://pjreddie.com/darknet/yolo/) and the [articles](https://pjreddie.com/publications/). 


### Data Sets: 
I’m using the dataset provided by [google open image](https://storage.googleapis.com/openimages/web/index.html) , the data looks quite like this: 

You can also find the sample file in the project repo. 

```
ImageID,Source,LabelName,Confidence,XMin,XMax,YMin,YMax,IsOccluded,IsTruncated,IsGroupOf,IsDepiction,IsInside,Class,x,y,width,height
078821d86db99fc9.jpg,freeform,/m/014j1m,1,0.244277,0.542485,0.09314,0.539359,1,0,0,0,0,Apple,0.393381,0.3162495,0.29820800000000003,0.44621900000000003
078821d86db99fc9.jpg,freeform,/m/014j1m,1,0.383319,0.6979920000000001,0.474829,0.9430149999999999,1,0,0,0,0,Apple,0.5406555000000001,0.7089219999999998,0.31467300000000004,0.46818599999999994
09a06df60e4bdd62.jpg,freeform,/m/014j1m,1,0.0,0.999692,0.0,0.9931639999999999,0,0,1,0,0,Apple,0.499846,0.49658199999999997,0.999692,0.9931639999999999
0fdea8a716155a8e.jpg,freeform,/m/014j1m,1,0.013999999999999999,0.991891,0.0,0.985716,0,0,1,0,0,Apple,0.5029454999999999,0.492858,0.977891,0.985716
105433f9d808d18e.jpg,freeform,/m/014j1m,1,0.12879300000000002,0.8589479999999999,0.102363,0.854827,0,0,0,0,0,Apple,0.4938705,0.478595,0.7301549999999999,0.752464


```


We downloaded the whole image dataset worth around 5.x gb using https://github.com/cvdfoundation/open-images-dataset#download-full-dataset-with-google-storage-transfer 

We have the two main csv files, one for training and validation. Our purpose is just to train a model which can detect 12 different fruits available in the google open image and create a bounding box on them. So, we will filter out our desired class from the main csv files and create a smaller csv which contains only those categories in which we’re interested.  


```
import pandas as pd
df = pd.read_csv('train_data_description.csv')
val_df = pd.read_csv('validation_all.csv')


"""This is the list of class we're intrested in"""
list_of_class = ["Apple", "Banana", "Orange", "Pear", "Mango","Pineapple", "Lemon", "Watermelon", "Strawberry",
                "Grapefruit", "Peach", "Pomegranate"]


def prepareData(dataframe:pd.DataFrame(), list_of_class:list) -> pd.DataFrame() : 
    
    '''Accepts a dataframe of either train or validate data, and split our desired classes from the original
    dataset, and create a new dataset for our pre-processing purposes.'''
    
    dff= dataframe[dataframe['Class'].isin(list_of_class)]
    data = dff.drop(dff.index)
    
    for cls in list_of_class:
        count = dff[dff["Class"]==cls]
        print("for class "+ cls ,count.shape)
        
        #We're only taking the 1000 annotation for each category to maintain the categorical dispersity in dataset. 
        data = data.append(dff[dff["Class"]==cls][:1000])
    
    data['ImageID'] = data['ImageID'].apply(lambda a: a+'.jpg')
    if "Unnamed: 0" in data:
        data.drop(columns=["Unnamed: 0"], inplace=True)
    return data


train = prepareData(df, list_of_class)
val = prepareData(val_df, list_of_class)
train.to_csv('train_fruits_1.csv', index=False)
val.to_csv('val_fruits_1.csv', index=False)

```

<!-- The above process is only necessary if you have the the training and validation csv downloaded from the google open image. If however you haven't I've uploaded the csv files concerning the data in the project repo. The two csv file contains the 1000 data point of each fruit category. -->


# Converting Annotation Bbox to Yolo Format

So with the train and validation csv generated from the above code, we shall now move on to making the data suitable for the yolo. Yolo doesn’t use the same annotation box as in object detection model like Faster-RCNN provided in tensorflow model zoo. Rather yolo needs `centerX`, `centerY`, `width` and `height`. 

So we have to convert the annotation, which basically is `Xmin`, `Xmax`, `Ymin`, `Ymax` from our new csvs to something like:

```
<class_number> (<absolute_x> / <image_width>) (<absolute_y> / <image_height>) (<absolute_width> / <image_width>) (<absolute_height> / <image_height>)

```

The following codes does the calculation and gives us the required values:

```
# The following code is the modified version of codes available here: 
# https://blog.goodaudience.com/part-1-preparing-data-before-training-yolo-v2-and-v3-deepfashion-dataset-3122cd7dd884

def convert_labels(path, x1, y1, x2, y2):
    """
    Definition: Parses label files to extract label and bounding box
        coordinates.  Converts (Xmin, Ymin, Xmax, Ymax) annotation format to
        (x, y, width, height) normalized YOLO format.
    """
    def sorting(l1, l2):
        if l1 > l2:
            lmax, lmin = l1, l2
            return lmax, lmin
        else:
            lmax, lmin = l2, l1
            return lmax, lmin
    
    size = get_img_shape(path)
    if(size[1]==None):
        print(path, "not found")
        return '', '', '', ''
    xmax, xmin = sorting(x1, x2)
    ymax, ymin = sorting(y1, y2)
    dw = 1./size[1]
    dh = 1./size[0]
    x = (x1 + x2)/2.0
    y = (y1 + y2)/2.0
    w = x2 - x1
    h = y2 - y1
    x = x*dw
    w = w*dw
    y = y*dh
    h = h*dh
    return (x/dw,y/dh,w/dw,h/dh)


def get_img_shape(path):
    path = path
    img = cv2.imread(path)
    try:
        return img.shape
    except AttributeError:
        print('error! ', path)
        return (None, None, None)

```
Let's append the new columns in the existing training and validation files which consists the annotation. 

```
train['x'], train['y'], train['width'], train['height'] = \
    zip(*train.progress_apply(lambda row: convert_labels("test_yolo/eval/"+row['ImageID'], row['XMin'], row['YMin'], row['XMax'], row['YMax']), axis=1)) # Like python for one lone code.

train.to_csv('test_yolo/fruits_1.csv', index=False)

val['x'], val['y'], val['width'], val['height'] = \
    zip(*val.progress_apply(lambda row: convert_labels("test_yolo/eval/"+row['ImageID'], row['XMin'], row['YMin'], row['XMax'], row['YMax']), axis=1)) # Like python for one lone code.

val.to_csv('test_yolo/fruits_val_1.csv', index=False)

```
### Let Us Now Test The Conversion: 

```
import matplotlib.pyplot as plt
def from_yolo_to_cor(box, shape):
    img_h, img_w, _ = shape
    im_w = 1./img_w
    im_h = 1./img_h
    #box = [b/1000 for b in box]
    # x1, y1 = ((x + witdth)/2)*img_width, ((y + height)/2)*img_height
    # x2, y2 = ((x - witdth)/2)*img_width, ((y - height)/2)*img_height
    x1, y1 = int((box[0] + box[2]/2)*img_w), int((box[1] + box[3]/2)*img_h)
    x2, y2 = int((box[0] - box[2]/2)*img_w), int((box[1] - box[3]/2)*img_h)
    return abs(x1), abs(y1), abs(x2), abs(y2)
    
def draw_boxes(img, boxes):
    shape = np.array([768, 1024, 3])
    for box in boxes:
        x1, y1, x2, y2 = from_yolo_to_cor(box, shape)
        print(x1, y1, x2, y2)
        cv2.rectangle(img, (x2, y2), (x1, y1), (0,255,0), 3)
        #cv2.rectangle(img, (int(0.311875*768), int(0.591597*768) ), (int(0.461875*1024),int(0.768908*1024)) ,(0,255,0), 3)
    plt.imshow(img)
    plt.plot()
    plt.show()
imgs = train[train["ImageID"]=="000d9c59687b509b.jpg"]
#This is the list of values associated with that image id. Here the code can be improved. 
boxes = [[0.189062,0.189584,0.378125,0.379167],[0.57625,0.622084,0.5925,0.485833],[0.36625,0.509583,0.03875,0.0525]]
#boxes = [[0.73, 0.507380073800738, 0.395, 0.5940959409594095]]
draw_boxes(cv2.imread("test_yolo/train/000d9c59687b509b.jpg"), boxes)

```
The output should look like this: 

The converted annotation box:
![Converted Image]({{'static/images/appletest.png' | absolute_url}})



The original annotation: 
```
img = train[train["ImageID"]=="000d9c59687b509b.jpg"]

original = [[0.000000,0.378125,0.000000,0.379167],[0.280000,0.872500,0.379167,0.865000],[0.346875,0.385625,0.483333,0.535833]]
draw_boxes(cv2.imread("test_yolo/train/000d9c59687b509b.jpg"), original)
```

![Original Annotation]({{'/static/images/apple-test-original.png' | abosolute_url}})


Sorry, I was lazy to automate the values placed while testing the conversion, you can select the any values from the dataframe and put that to visualize the result. For me, I just copied one row and paste that there as a list of list. 



### Class to Integer Lablelling, An example of bad code : Now let us convert the class label to the interger, since Yolo requires the class to be represented as the interger. 

```
def class_text_to_int(row_label):
    if row_label == 'Apple':
        return 1
    elif row_label == 'Banana':
        return 2
    elif row_label == 'Orange':
        return 3
    elif row_label == 'Pear':
        return 4
    elif row_label == 'Mango':
        return 5
    elif row_label == 'Pineapple':
        return 6
    elif row_label == 'Lemon':
        return 7
    elif row_label == 'Watermelon':
        return 8
    elif row_label == 'Strawberry':
        return 9
    elif row_label == 'Grapefruit':
        return 10
    elif row_label == 'Peach':
        return 11
    elif row_label == 'Pomegranate':
        return 12


train["ClassInt"] = train["Class"].progress_apply(lambda row: class_text_to_int(row))
val["ClassInt"] = val["Class"].progress_apply(lambda row: class_text_to_int(row))

```

### Create txt file for each image containing ClassId,centerX, centerY, Width, Height

```
#loop through the dataframe and create the related txt file for each imageset.
path = "PATH_TO_YOUR_FOLDER"

def createTxt(path:str, data:pd.DataFrame()):
    for row in data.iterrows():
        row = row[1]
        towrite = str(row["ClassInt"]) + " " + str(row["x"]) + " "+ str(row["y"]) + " "+ str(row["width"]) + " " + str(row["height"])
        with open(path+row["ImageID"][:-4]+".txt", "w") as wr:
            wr.write(towrite)


createTxt('train_data_path', train)
createTxt('val_data_path', val)

```

### Getting back to the Yolo

Clone the darknet repo in your local machine. 

```git clone https://github.com/AlexeyAB/darknet.git```

Build the darknet, following the instruction:\

```
cd darknet

# Open up the Makefile and made the necessary edits, Change the GPU 0 to 1 if you're building darknet for GPU, also if you have installed openCV set OPENCV 0 to 1.) Mind that for the images to be displayed you need to install the OpenCV in your machine and build darknet.

make 

# ./darknet should let you access your darknet installation.

```

### Download the Yolo Weight File 

```wget https://pjreddie.com/media/files/yolo.weights```

### Test the installation by running the detector: 

`./darknet detect cfg/yolo.cfg yolo.weights data/dog.jpg`

Aslo please mind the path of files and the installation. At this point, if everything is working fine, let us create the new configuration files for our new training. 


Lets create a fruits.cfg file inside the `cfg` folder of darknet installation. The sample `.cfg` file for our purpose is: 

```
[net]
# Testing
#batch=1
#subdivisions=1
# Training
batch=64
subdivisions=8
width=416
height=416
channels=3
momentum=0.9
decay=0.0005
angle=0
saturation = 1.5
exposure = 1.5
hue=.1

learning_rate=0.001
burn_in=1000
max_batches = 500200
policy=steps
steps=400000,450000
scales=.1,.1

[convolutional]
batch_normalize=1
filters=32
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=64
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=128
size=3
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=64
size=1
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=128
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=256
size=3
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=128
size=1
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=256
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=512
size=3
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=256
size=1
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=512
size=3
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=256
size=1
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=512
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=1024
size=3
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=512
size=1
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=1024
size=3
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=512
size=1
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=1024
size=3
stride=1
pad=1
activation=leaky


#######

[convolutional]
batch_normalize=1
size=3
stride=1
pad=1
filters=1024
activation=leaky

[convolutional]
batch_normalize=1
size=3
stride=1
pad=1
filters=1024
activation=leaky

[route]
layers=-9

[convolutional]
batch_normalize=1
size=1
stride=1
pad=1
filters=64
activation=leaky

[reorg]
stride=2

[route]
layers=-1,-4

[convolutional]
batch_normalize=1
size=3
stride=1
pad=1
filters=1024
activation=leaky

[convolutional]
size=1
stride=1
pad=1
filters=85
activation=linear


[region]
anchors =  0.57273, 0.677385, 1.87446, 2.06253, 3.33843, 5.47434, 7.88282, 3.52778, 9.77052, 9.16828
bias_match=1
classes=12
coords=4
num=5
softmax=1
jitter=.3
rescore=1

object_scale=5
noobject_scale=1
class_scale=1
coord_scale=1

absolute=1
thresh = .6
random=1



```

We changed the number of classes, and the value of filters in the last convolution layer. They are the necessary steps. 

`filters=(classes + 5)*5 in our case filters= (12+5)*5`

These parameters are described in the [referenced url](https://medium.com/@manivannan_data/how-to-train-yolov2-to-detect-custom-objects-9010df784f36) 

Now let us have the classes names in a seperate file called `fruits.names` in the same `cfg` directory. The following classes represents our new data: 

```
Apple
Banana
Orange
Pear
Mango
Pineapple
Lemon
Watermelon
Strawberry
Grapefruit
Peach
Pomegranate


```

Also let's create the `fruits.data` to point the configuration files, 


```
classes=12  
train=/home/vaghawan/test_yolo/train.txt  
valid=/home/vaghawan/test_yolo/test.txt  
names=/var/www/darknet/cfg/fruits.names  
backup=backup/


```
For train and valid, the path should point to the correct directory where we saved the .txt file for each image with the correct image path.

### Finally, it's time to run the training: 

 `./darknet detector train cfg/fruits.data cfg/yolo-obj.cfg darknet19_448.conv.23` 

 Here, you need to make sure you're giving the correct path of the weight file. If everything went Ok, you will see the new weight file saved in the `backup` directory. You can test the prediction with the backup.weight file: 

 
 `./darknet detector test cfg/fruits.data cfg/fruits.cfg backup/backup.weights data/mango.jpg`



### The output should look like this: 

![Output Image]({{'/static/images/predictions.jpg' | abosolute_url}})


The main purpose of this article was to show how you can convert the annotated data available in google open image to the yolo format and use them to train the yolo model. Let me know if you encountered any error throughout the process.


### References: 

1. https://pjreddie.com/darknet/yolo/
2. https://medium.com/@manivannan_data/how-to-train-yolov2-to-detect-custom-objects-9010df784f36
3. https://blog.goodaudience.com/part-1-preparing-data-before-training-yolo-v2-and-v3-deepfashion-dataset-3122cd7dd884
4. https://github.com/pjreddie/darknet


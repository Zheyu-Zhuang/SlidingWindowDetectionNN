# SlidingWindowDetectionNN

## Intro
The neural network has receives an image and returns a coarse heat map indicating where the animals are.
This NN converts a classification convolutional neural netowork that infers class lables from small image patches to a detection module using 'sliding window' technique. 
The network is *fast to train*. Using the provided small dataset, the network is able to finish training under 10 mins with CPU.

If you want to see the network working, run '''python evaluate.py <image path>''' in the network subfolder. For example, 
python evaluate.py ./dataset_tools/example_raw_data/252.png
The output of the network is a heatmap, and you need to convert this into a bearing (a vector from image centre to animal location), and triangulate with multiple bearings to locate the animal as accurately as possible.

## Improving network outcomes
As you test the network, you may notice that the network accuracy is limited. This is mainly due to insufficient training data.  You can greatly improve the performance by increasing the  training  data size.
Retraining with images captured in the workshop room
Capture Training and Evaluation Data
Please change to the ./network sub-directory to run the following script
Capture data with the image_collection() function in the camera_calibration.py script
Use https://labelbox.com to annotate the data
Parse the data using ./dataset_tools/label_parser.py
Save data to folder ./nn_dataset/train and ./nn_dataset/eval
Detailed labelling instructions are available at the bottom of this page
Train the network
All the netwok hyperparameters are specified in nn_configs.py, and you can change them as you wish. Run python train.py to train your network with the new parameters you have chosen.
To visualise the newly trained network, python evaluate.py <image path> . 
Think of a clever way of converting from heatmap to a bearing that handles outliers
  
## Instructions of Labelling your animals
Please pull the latest code from the github repo
Suggested labelling website: https://labelbox.com
Steps:
Click 'New Project'.
 Upload the collected images. 
 Configure the label editor:
 3.1 Select Image editor (NOT the legacy option),
 3.2 Add objects: 
       - llama
       - crocodile
       - elephant
       - snake
       - background
Start Labelling.
Export the label file as .csv, rename it to labels.csv, and move the file to the same folder where all the images are saved.
Switch to the /network/dataset_tools,  change the paths of image folder and destination folder specified in main function of 'label_parser.py'.
In order to avoid file name conflicts when sharing, I encourage different groups add different prefixes to output image names, you can achieve this by adding a prefix directly to the img_name variable declared in the save_img function.
def save_img(cv2_img, dataset_root, label):
   	dir_path = os.path.join(dataset_root, label)
   	if not os.path.exists(dir_path):
       	os.makedirs(dir_path)
   	file_count = len(glob.glob1(dir_path, '*.png'))
   	img_name = os.path.join(dir_path, <prefix> + f'{file_count:06}' + '.png')
   	cv2.imwrite(img_name, cv2_img)
 Share your dataset via: https://drive.google.com/drive/folders/12gvfk1-So7IGUxe4o-PGizUWacOA7xcQ?usp=sharing. 
To train the network, download all the images from the shared directory and split each category into evaluation and train with the ratio of 1:4, and save them into the nn_dataset folder accordingly. 
Enjoy
  

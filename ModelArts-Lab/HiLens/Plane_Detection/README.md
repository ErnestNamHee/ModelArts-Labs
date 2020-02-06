# Apron aircraft identification

## ModelArts & HiLens side cloud collaboration case

## I. Application Value

Different from road traffic, it is difficult for the aircraft to separate the taxiing path of the flight through overhead, tunnels and other methods when the aircraft is on the ground. Due to the increase in the number of aircraft takeoffs and landings and the impact of bad weather, runway incursions have become the ground safety of aircraft in the civil aviation field First-class problems in operation. Runway safety accidents account for a large proportion of civil aviation accidents. Therefore, the identity, location, and dynamic control of targets such as aircraft and vehicles operating on airport runways and taxiways, largely determine the degree of control. The coefficient of runway safety.

Aircraft and other vehicles operating on the ground of the airport are generally managed and managed by the tower. The core responsibility of the tower is to ensure the "safe, orderly, and efficient" operation of the flight. A decision-making process uniformly commands a large number of aircraft and ground vehicles, and the two processes of "seeing" and "understanding" often play a decisive role in regulatory decisions.

As the scale of the airport increases, the layout becomes more and more complicated. For the towers of large airports, the single-point field of vision is physically limited, and the degree of digitization has increased. At the same time, it has objectively caused more screens and more information. "And" understanding "pose a greater challenge. People's subjective initiative is very strong, but it is often difficult to cover everything.

Based on the above background, we have designed a runway intrusion prevention system to strengthen the prevention of runway incursions. The “optical surveillance subsystem” is an important real-time core of the system. This subsystem has a relatively complete “video stream-based aircraft target recognition and tracking” capability. Its main job is to stream real-time video collected at various key points in the airport plane. Enter the "Aircraft Recognition Model" based on the Huawei Cloud ModelArts framework for processing. After recognizing the primary parameters such as the pixel coordinates of the aircraft object in the screen, the space position conversion is performed, and the secondary parameters including speed and direction of operation are calculated. Calculate and find the matching flight information in the system operation data to mark the target information and present it on the monitor in the form of augmented AR information, so that the controller can intuitively understand enough in a single screen in the form of "head-up display" Comprehensive dynamic information. At the same time, the system will monitor and calculate the trajectory and vector dynamic data of all targets in the background, allowing the computer to replace or assist the control personnel in real-time marking of each flight and each crossing on a global scale, predicting potential operational risks in advance, thereby Reduce the probability of accidents.

## 2. Project Overview

This project is the "Aircraft Identification Model" subsystem of the system. Based on the YoloV3 and ResNet18 models, it can achieve more accurate target recognition and coordinate output. Relying on [ModelArts] (https://www.huaweicloud.com/product/modelarts.html) training operations, the "Aircraft Identification Model" service can be realized , A key step from "seeing" to "understanding".

Use the Huawei Cloud Cloud Collaborative AI Application Development Platform [HiLens] (https://www.huaweicloud.com/product/hilens.html) to send the model trained by ModelArts to the HiLens Kit, an end-side device, to achieve real-time end-side Detection.

This project provides a "ModelArts-Lab Treasure Hunt" project based on Pomelo Data. Project address: [youzidata-Aerodrome Runway Aircraft Identification] (https://github.com/huaweicloud/ModelArts-Lab/tree/master/contrib/0.%E6%8C%96%E5%AE%9D%E8%A1%8C%E5%8A%A8/youzidata-%E6%9C%BA%E5%9D%AA%E8%B7%91%E9%81%93%E8%88%AA%E7%A9%BA%E5%99%A8%E8%AF%86%E5%88%AB).

## Third, the specific process

* This practice needs to enable the yolov3_resnet18 model whitelist. If you need to use it, please use the work order to contact the staff. *

### First, please complete the [pre-work] (https://github.com/huaweicloud/ModelArts-Lab/tree/master/HiLens/%E5%89%8D%E7%BD%AE%E5%B7%A5 % E4% BD% 9C).

#### Step 1 is completed in [Object Storage Service OBS] (https://www.huaweicloud.com/product/obs.html);

#### Steps 2-4 are completed in [ModelArts] (https://www.huaweicloud.com/product/modelarts.html);

#### Steps 5-7 are completed in [HiLens] (https://www.huaweicloud.com/product/hilens.html).

### 1. Prepare the data

First use [Object Storage Service OBS] (https://www.huaweicloud.com/product/obs.html) to upload the obtained data to the OBS bucket.

The data includes the tarmac image and the label file. ** The folder format and label file must strictly follow this practice note **.

The folder directory is as follows:


![](./ img / files.png)

Among them, the txt file is a label file, and the content format of the file is:

`
data / 00003.jpg 1044,963,1140,1030,0 1293,951,1411,1014,0 1411,946,1496,1014,0
`

`
data / 00024.jpg 1039,957,1152,1036,0 1293,940,1417,1014,0
`

Explanation:

(1) The data / 00003.jpg content here and the relative path of the txt file can be modified according to the user's actual data set, but make sure that `input / data / 00003.jpg` is the absolute path of the bucket,` 1044,963 , 1140,1030,0` means x_min, y_min, x_max, y_max, class_id;

(2) class_id: It is the content filled in the parameter class_names, which is defined as "plane" in this project, that is, the label `0` represents plane, and the user can adjust it according to the actual situation;

(3) The training set train.txt and the validation set validate.txt are required, and the test set test.txt is not required.

Image file example:

! [] (./ img / plane.jpg)

### 2. ModelArts Model Training

Training AI models with ModelArts training assignments

(1) Enter the ModelArts management console, click on the left toolbar "Training Management" → "Training Assignment" to enter the "Training Assignment" page.

(2) Click "Create" to enter the "Create Training Job" page.

(3) On the "Create Training Job" page, fill in the training job related parameters, and then click "Next".

a. In the basic information area, "Billing Mode" and "Version" are automatically generated by the system and do not need to be modified. Please fill in "Name" and "Description" as prompted by the interface.

! [] (./ img / 1.png)

b. In the parameter configuration area, select "Data source" and set "Algorithm source", "Run parameter", "Training output location" and "Job log path".

-Data source: Click "Data storage location", and then click "Select" on the right side of the text box, select the OBS path where the data set is located, that is, the path created in the previous step, use "/ youzi / input" in this example .


-Algorithm Source: Click "Select", and from the "Preset Algorithm" list, select the "yolov3_resnet18" algorithm.


- Operating parameters:
  -finetuning = False
  -load_imagenet_weights = True
  -class_names = plane
  -max_epochs = 200
  -batch_size = 32
  
  
-Training output location: Select the model and prediction file storage path from the existing OBS bucket. Use the "output" folder you created in the preparation. If no folder is available, you can click Select to create a new folder in the pop-up dialog box.


-Job log path: Select the log storage path from the existing OBS bucket. Use the "log" folder you created in the preparation. If no folder is available, you can click Select to create a new folder in the pop-up dialog box.

! [] (./ img / 2.png)

c. In the resource setting area, click "Select" on the right side of the resource pool text box, and select "Public Resource Pool" and "GPU". The specifications are divided into p100 and v100. Good, but the two prices are different, please choose as required; "Number of compute nodes" is set to "1".

! [] (./ img / 3.png)

d. Complete the information and click Next.

(4) On the "Specification Confirmation" page, after confirming that the information is correct, click "Create Now".

(5) On the "Training Assignment" management page, you can view the status of the newly created training assignment. It takes some time to create and run the training job. If you use p100, max_epoches = 200, it takes about 1 hour to complete the training. When the status changes to "Run successfully", the training job creation is complete. You can click the name of the training job to enter the job details page to learn about the configuration information, log, resource usage, and evaluation results of the training job. In the OBS path where the “training output location” is located, that is, the “/ youzi / output /” path, you can obtain the generated model file.

### 3. Save the model to the required PB format file:

The ckpt format model generated in step 2 is converted into a pb format model file to meet the model conversion requirements.

** The operation of this step is the same as that of step 2 except for the setting of operation parameters. **

(1) Enter the ModelArts management console, click on the left toolbar "Training Management" → "Training Assignment" to enter the "Training Assignment" page.

(2) Click "Create" to enter the "Create Training Job" page.

(3) On the "Create Training Job" page, fill in the training job related parameters, and then click "Next".

a. In the basic information area, "Billing Mode" and "Version" are automatically generated by the system and do not need to be modified. Please fill in "Name" and "Description" as prompted by the interface.

! [] (./ img / 1.png)

b. In the parameter configuration area, select "Data source" and set "Algorithm source", "Run parameter", "Training output location" and "Job log path".

-Data source: same as step 2.


-Algorithm source: same as step 2.


- Operating parameters:
  -finetuning = True
  -load_imagenet_weights = False
  -class_names = plane
  -max_epochs = 200
  -batch_size = 32
  -run_mode = save_pb
  -inference_device = 310D
  
-Training output position: same as step 2.


-Job log path: same as step 2.

! [] (./ img / 4.png)

c. Resource settings: same as step 2.

! [] (./ img / 3.png)

d. Complete the information and click Next.

(4) On the "Specification Confirmation" page, after confirming that the information is correct, click "Create Now".

(5) On the "Training Assignment" management page, you can view the status of the newly created training assignment. When the status changes to "Run successfully", the job is complete. At this time, a "models" folder is generated under the "/ youzi / output /" path, which contains the model file `yolo3_resnet18.pb` and the aipp parameter file` aipp.cfg`, which are used for the next model conversion.


### 4. Model Conversion

The model issued to HiLens requires the om format, so you need to use the model conversion function of ModelArts to convert the pb format model file to the om format model file.

(1) Enter the ModelArts management console, select "Model Management" → "Compression / Conversion" in the left navigation bar to enter the model conversion list page.

(2) Click "Create Task" in the upper left corner to enter the task creation task page.

(3) On the Create Task page, fill in the relevant information.

-Name: Custom name, "convert-youzi" in this practice.


-Description: Add a description yourself.


-Conversion template: Select "** TensorFlow frozen graph to Ascend **".


-Conversion input directory: Set the "/ youzi / output / models" folder in the previous step as the conversion input path.


-Conversion output directory: Create a new folder "om_model" in the OBS path, and select the conversion output directory as "/ youzi / output / om_model".


-Advanced options: Set "Learning Frame Type" to "** 3 **" and "Input Tensor Shape" to "** images: 1,224,224,3 **".

! [] (./ img / 5.png)

(4) After completing the task information, click "Create Now" in the lower right corner.

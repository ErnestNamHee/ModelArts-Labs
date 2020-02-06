 # Apron aircraft identification
## ModelArts & HiLens side cloud collaboration case

## Application value

Different from road traffic, it is difficult for the aircraft to separate the taxiing path of the flight through overhead, tunnels and other methods when the aircraft is on the ground. Due to the increase in the number of aircraft takeoffs and landings and the impact of bad weather, runway incursions have become the ground safety of aircraft in the civil aviation field First-class problems in operation. Runway safety accidents account for a large proportion of civil aviation accidents. Therefore, the identity, location, and dynamic control of targets such as aircraft and vehicles operating on airport runways and taxiways, largely determine the degree of control. The coefficient of runway safety.

Aircraft and other vehicles operating on the ground of the airport are generally managed and managed by the tower. The core responsibility of the tower is to ensure the "safe, orderly, and efficient" operation of the flight. A decision-making process uniformly commands a large number of aircraft and ground vehicles, and the two processes of "seeing" and "understanding" often play a decisive role in regulatory decisions.

As the scale of the airport increases, the layout becomes more and more complicated. For the towers of large airports, the single-point field of vision is physically limited, and the degree of digitization has increased. At the same time, it has objectively caused more screens and more information. "And" understanding "pose a greater challenge. People's subjective initiative is very strong, but it is often difficult to cover everything.

Based on the above background, we have designed a runway intrusion prevention system to strengthen the prevention of runway incursions. The “optical surveillance subsystem” is an important real-time core of the system. This subsystem has a relatively complete “video stream-based aircraft target recognition and tracking” capability. Its main job is to stream real-time video collected at various key points in the airport plane. Enter the "Aircraft Recognition Model" based on the Huawei Cloud ModelArts framework for processing. After recognizing the primary parameters such as the pixel coordinates of the aircraft object in the screen, the space position conversion is performed, and the secondary parameters including speed and direction of operation are calculated. Calculate and find the matching flight information in the system operation data to mark the target information and present it on the monitor in the form of augmented AR information, so that the controller can intuitively understand enough in a single screen in the form of "head-up display" Comprehensive dynamic information. At the same time, the system will monitor and calculate the trajectory and vector dynamic data of all targets in the background, allowing the computer to replace or assist the control personnel in real-time marking of each flight and each crossing on a global scale, predicting potential operational risks in advance, thereby Reduce the probability of accidents.
Project Overview

This project is the "Aircraft Identification Model" subsystem of the system. Based on the YoloV3 and ResNet18 models, it can achieve more accurate target recognition and coordinate output. Based on ModelArts training operations, it can realize the "aircraft recognition model" service, which is a key step from "seeing" to "understanding.

Using Huawei Cloud Cloud AI application development platform HiLens, the model trained by ModelArts is delivered to the HiLens Kit, an end-side device, to achieve real-time end-to-end detection.

This project provides a "ModelArts-Lab Treasure Hunt" project based on Pomelo Data, project address: youzidata-airport runway aircraft identification.
Third, the specific process

This practice needs to open the yolov3_resnet18 model whitelist. If you need to use it, please use the work order to contact the staff.
First complete the preparatory work.
Step 1 is completed in the object storage service OBS;
Steps 2-4 are completed in ModelArts;
Steps 5-7 are done in HiLens.

Project Overview

This project is the "Aircraft Identification Model" subsystem of the system. Based on the YoloV3 and ResNet18 models, it can achieve more accurate target recognition and coordinate output. Based on ModelArts training operations, the "aircraft recognition model" service can be realized, and a key step from "seeing" to "understanding" is realized.

Using Huawei Cloud Cloud AI application development platform HiLens, the model trained by ModelArts is delivered to the HiLens Kit, an end-side device, to achieve real-time end-to-end detection.

This project provides a "ModelArts-Lab Treasure Hunt" project based on Pomelo Data, project address: youzidata-airport runway aircraft identification.

Third, the specific process

This practice needs to open the yolov3_resnet18 model whitelist. If you need to use it, please use the work order to contact the staff.
First complete the preparatory work.
Step 1 is completed in the object storage service OBS;
Steps 2-4 are completed in ModelArts;
Steps 5-7 are done in HiLens.
Prepare data

First use the object storage service OBS to upload the obtained data to the OBS bucket.

The data includes tarmac images and label files. The folder format and label files must strictly follow this practice description.

The folder directory is as follows:

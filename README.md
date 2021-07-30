# CSC-212-Final-Project-Description

Project Description: This project anaylzes varies types of medical information for each of the patients written in the CSV file included in the project directory. The CSV file stores the name, sex, age, medical condition, blood pressure, pulse, and DNA sequence of 40 patients (number of patients can be easily changed by edittin the CSV file). The purpose of this program is to store various medical and identification records of patients to allow medical professionals to treat and analyze the patients better in the hospital. Once the program runs, it will output a chart comparing each DNA sequence to each other using an algorithm described later in this file and the patient in the most need of assistance based on another algorithm also described in this writeup.

# Topics Used:
* CSV file and anaylsis
* Priority Queues
* Recursion

# SETUP
This program requires minimal setup and input from the user. It is ready to run through the code completely, but will not have the accurrate data, the user needs to successfully use it. In order to change this, the user must input or delete lines ine the CSV file. Each entry must be formatted like the default ones already in the file. This program is only programmed to assess the following conditions: heart attack, stroke, unknown, head injury, broken bone, and stomache pain. Any ages under 1 year must be entered in as 0. The sex can only be "Male" or "Female" for medical purposes. The blood pressure is entered as a fraction "x/y" and the pulse is a number. The code is set up to read the data of any number of patients as long as their data is completely and correct in the CSV file.There is data for 40 different patients already in the file by default. This can be changed by editting the CSV file to hold the data the user gathered. Each line must hold all of the data for only one patient and must be formatted as:                        
full name, sex, age (as an integer), medical condition, blood pressure, heart rate, DNA sequence.

# USE
Once the program is set to run, it will print out a chart with the initials of every patient in the first row and column. It will then, print out the score of each comparison based on each character of the strings (if the chart is at the same patient on the row and column, it will print out "--" instead of a number). This program follows a scoring system of: 
* mismatch (ex. A != C) causes the score to be decreased by 1 
* match (ex. A == A) causes the score to be increased by 1
* gap (_) causes no changes to the score                                                                                    				                                                                                                

If the two DNA sequences are not of the same length, the difference between the two lengths are subtracted from the score.

After the table prints out, the information of the first patient that should be treated will print out. The user must type "Treat" to treat and remove the patient from queue. After the patient is treated, the information of the next patient gets printed.        
Sample patient print out:                                                         
Name: John Doe                                                                                                                                                 
Sex: Male                                                                                           
Age: 65                                                                                            
Condition: Stroke                                                                                                                                                                                               
Heart Rate: 89                                                                                            
This process is repeated until there is no more patients to treat. When this happens, the program will print out "You have treated everyone! There is no more patients waiting!" and end.

# File Descriptions
* Data.h - Collects and manipulates all data from the CSV file so that it can be used to give the user the accurant output
* Data.cpp - Implementation for Data.h
* P_Queue.h - Inserts patients into a priority queue in a order that allows the ones with the most severe conditions to be at the beggining of the queue and removes patients once they are treated
* P_Queue.cpp - Implementation for P_Queue.h
* Node.h - Creates a data structure called "Node" that makes up a priority queue
* Node.cpp - Implementation for Node.h
* patients.csv - Holds all of the data from all patients
* main.cpp - Connects all necessary files together and runs the program

# Code Implementation
## Counting patients - int data::countNumOfPatients(std::string fileName)
This function counts and returns the number of patients whose data is in the csv file. Since each patient has their data on one seperate line of the file, the function does this by opening the file and increasing a counter, initialized as 0 as the function loops through each line of the file with a while loop. When the function exits the while loop, the counter gets returned representing the number of patients within the file.

## Reading CSV file - void data::readCSV(std::string fileName, int numOfPatients, std::string names[], std::string sexes[], std::string ages[], std::string conditions[], std::string bloodPressures[], std::string heartRates[], std::string DNASequences[])
This function is used to read the data in the file and to put each entry in the correct array listed in the parameters of the function. This will allow the program to manipulate and anayzle the data easily later on in the program. This function uses the implementation from the csv file data type. It saves every piece of data in a specific array based on what column the data is in. After it is completed, the file is closed to aviod it being accidently modified while the program is running and only the arrays are used in the program. 

## Calculating the priority of a patient - void data::prioritySetUp(std::string sexes[], std::string ages[], std::string conditions[], std::string bloodPressures[], std::string heartRates[], int numOfPatients, unsigned int priority[])
This function is used to calculate each of the patients who are listed in the file's priority. The patients with a higher priority will show up first while the program is being ran. The priority of each patient starts as 0 before their information is analyzed. Each condition, heart rate, and blood pressure has a priority value. The priority value of the heart rate is also dependent on the sex and age of the patient.                                                                   
Medical Concern:                                                                                                                                                                                                                                                                                                  
* Heart Attack or Stroke: Adds 5 to the priority                                                                                                      
* Head Injury or Unknown: Adds 4 to the priority                                                                                                     
* Bleeding: Adds 3 to the priority                                                                                                                   
* Broken Bone: Adds 2 to the priority                                                                                                                                                 
* Stomach Pain: Adds 1 to the priority                                                                           


Blood Pressure:                                                                                                                                                  
Systolic pressure is the first number in the blood pressure reading and the diastolic pressure is the second.

![Blood Pressure Chart](body_bpchart.png)                                                                                                                                                 
* Hypertensive Critical: Adds 5 to the priority                                                                                                        
* High Blood Pressure Stage 2: Adds 4 to the priority                                                                                                       
* Low Blood Pressure: Adds 3 to the priority                                                                                                                        
* High Blood Pressure Stage 1: Adds 2 to the priority                                                                                                         
* Prehypertension: Adds 1 to the priority                                                                                                                  


Heart Rate:                                                                                                                                        
Uses a private helper function to calculate this for organizational purposes - heartRatesRange(int heartRate, int fstLow, int lstLow, int normLow, int normHigh, int fstHigh, int secHigh, int currentPriority)
![Heart Rate Chart](Unknown.jpg)
* Child 
  * 3 Years Old and Under:                                                                                                                                                  
    * Less than or equal to 30 beats or greater than 150 (0-30, 150+, including 30): Adds 5 to priority
    * Less than or equal to 150 beats and more than 130 beats (130-150, including 150): Adds 4 to priority
    * Less than or equal to 50 beats and more than 30 beats (30-50, including 50): Adds 3 to priority
    * Less than or equal to 70 beats and more than 50 beats (50-70, including 70): Adds 2 to priority
    * Less than or equal to 130 beats and more than 110 beats (110-130, including 130): Adds 1 to priority
    * Less than or equal to 110 beats and more than 70 beats (70-110, including 110): No change
  * 4 to 6 Years Old:                                                                                                                                                  
    * Less than or equal to 25 beats or greater than 150 (0-25, 150+, including 25): Adds 5 to priority
    * Less than or equal to 150 beats and more than 130 beats (130-150, including 150): Adds 4 to priority
    * Less than or equal to 45 beats and more than 25 beats (25-45, including 45): Adds 3 to priority
    * Less than or equal to 65 beats and more than 45 beats (45-65, including 65): Adds 2 to priority
    * Less than or equal to 130 beats and more than 110 beats (110-130, including 130): Adds 1 to priority
    * Less than or equal to 110 beats and more than 65 beats (65-110, including 110): No change
  * 7 to 12 Years Old:                                                                                                                                                  
    * Less than or equal to 30 beats or greater than 125 (0-30, 125+, including 30): Adds 5 to priority
    * Less than or equal to 125 beats and more than 105 beats (105-125, including 125): Adds 4 to priority
    * Less than or equal to 50 beats and more than 30 beats (30-50, including 50): Adds 3 to priority
    * Less than or equal to 60 beats and more than 50 beats (50-60, including 60): Adds 2 to priority
    * Less than or equal to 105 beats and more than 95 beats (95-105, including 105): Adds 1 to priority
    * Less than or equal to 95 beats and more than 60 beats (60-95, including 95): No change
  * 13 to 18 Years Old:                                                                                                                                                  
    * Less than or equal to 15 beats or greater than 115 (0-15, 115+, including 15): Adds 5 to priority
    * Less than or equal to 115 beats and more than 105 beats (105-115, including 115): Adds 4 to priority
    * Less than or equal to 35 beats and more than 15 beats (15-35, including 35): Adds 3 to priority
    * Less than or equal to 55 beats and more than 35 beats (35-55, including 55): Adds 2 to priority
    * Less than or equal to 105 beats and more than 85 beats (85-105, including 105): Adds 1 to priority
    * Less than or equal to 85 beats and more than 55 beats (55-85, including 85): No change
* Men
  * 35 Years and Under:                                                                                                                                                  
    * Less than or equal to 20 beats or greater than 105 (0-20, 105+, including 20): Adds 5 to priority
    * Less than or equal to 105 beats and more than 85 beats (85-105, including 105): Adds 4 to priority
    * Less than or equal to 30 beats and more than 20 beats (20-30, including 30): Adds 3 to priority
    * Less than or equal to 50 beats and more than 30 beats (30-50, including 50): Adds 2 to priority
    * Less than or equal to 85 beats and more than 75 beats (75-85, including 85): Adds 1 to priority
    * Less than or equal to 75 beats and more than 50 beats (50-75, including 75): No change
  * 36 to 45 Years Old:                                                         
    * Less than or equal to 25 beats or greater than 95 (0-25, 95+, including 25): Adds 5 to priority
    * Less than or equal to 95 beats and more than 85 beats (85-95, including 95): Adds 4 to priority
    * Less than or equal to 35 beats and more than 25 beats (25-35, including 35): Adds 3 to priority
    * Less than or equal to 55 beats and more than 35 beats (35-55, including 55): Adds 2 to priority
    * Less than or equal to 85 beats and more than 75 beats (75-85, including 85): Adds 1 to priority
    * Less than or equal to 75 beats and more than 55 beats (55-75, including 75): No change
  * 46 to 55 Years Old:                                                         
    * Less than or equal to 30 beats or greater than 105 (0-30, 105+, including 30): Adds 5 to priority
    * Less than or equal to 105 beats and more than 85 beats (85-105, including 105): Adds 4 to priority
    * Less than or equal to 40 beats and more than 30 beats (30-40, including 40): Adds 3 to priority
    * Less than or equal to 60 beats and more than 40 beats (40-60, including 60): Adds 2 to priority
    * Less than or equal to 85 beats and more than 75 beats (75-85, including 85): Adds 1 to priority
    * Less than or equal to 75 beats and more than 60 beats (60-75, including 75): No change
  * 56 to 65 Years Old:                                                         
    * Less than or equal to 35 beats or greater than 100 (0-35, 100+, including 35): Adds 5 to priority
    * Less than or equal to 100 beats and more than 90 beats (90-100, including 100): Adds 4 to priority
    * Less than or equal to 45 beats and more than 35 beats (35-45, including 45): Adds 3 to priority
    * Less than or equal to 65 beats and more than 45 beats (45-65, including 65): Adds 2 to priority
    * Less than or equal to 90 beats and more than 80 beats (80-90, including 90): Adds 1 to priority
    * Less than or equal to 80 beats and more than 60 beats (65-80, including 80): No change
  * Over 65 Years Old:                                                         
    * Less than or equal to 40 beats or greater than 95 (0-40, 95+, including 40): Adds 5 to priority
    * Less than or equal to 95 beats and more than 85 beats (85-95, including 95): Adds 4 to priority
    * Less than or equal to 50 beats and more than 40 beats (40-50, including 50): Adds 3 to priority
    * Less than or equal to 70 beats and more than 50 beats (50-70, including 70): Adds 2 to priority
    * Less than or equal to 85 beats and more than 75 beats (75-85, including 85): Adds 1 to priority
    * Less than or equal to 75 beats and more than 70 beats (70-75, including 75): No change
* Women
  * 35 Years and Under:                                                                                                                                                  
    * Less than or equal to 35 beats or greater than 105 (0-35, 105+, including 35): Adds 5 to priority
    * Less than or equal to 105 beats and more than 90 beats (90-105, including 105): Adds 4 to priority
    * Less than or equal to 45 beats and more than 35 beats (35-45, including 45): Adds 3 to priority
    * Less than or equal to 65 beats and more than 45 beats (45-65, including 65): Adds 2 to priority
    * Less than or equal to 90 beats and more than 80 beats (80-90, including 90): Adds 1 to priority
    * Less than or equal to 80 beats and more than 65 beats (65-80, including 80): No change
  * 36 to 45 Years Old:                                                         
    * Less than or equal to 35 beats or greater than 105 (0-35, 105+, including 35): Adds 5 to priority
    * Less than or equal to 105 beats and more than 85 beats (85-105, including 105): Adds 4 to priority
    * Less than or equal to 45 beats and more than 35 beats (35-45, including 45): Adds 3 to priority
    * Less than or equal to 65 beats and more than 45 beats (45-65, including 65): Adds 2 to priority
    * Less than or equal to 85 beats and more than 75 beats (75-85, including 85): Adds 1 to priority
    * Less than or equal to 75 beats and more than 65 beats (65-75, including 75): No change
  * 46 to 55 Years Old:                                                         
    * Less than or equal to 30 beats or greater than 90 (0-30, 90+, including 30): Adds 5 to priority
    * Less than or equal to 90 beats and more than 80 beats (80-90, including 90): Adds 4 to priority
    * Less than or equal to 40 beats and more than 30 beats (30-40, including 40): Adds 3 to priority
    * Less than or equal to 60 beats and more than 40 beats (40-60, including 60): Adds 2 to priority
    * Less than or equal to 80 beats and more than 75 beats (75-80, including 80): Adds 1 to priority
    * Less than or equal to 75 beats and more than 60 beats (60-75, including 75): No change
  * 56 to 65 Years Old:                                                         
    * Less than or equal to 40 beats or greater than 100 (0-40, 100 up, including 40): Adds 5 to priority
    * Less than or equal to 100 beats and more than 90 beats (90-100, including 100): Adds 4 to priority
    * Less than or equal to 50 beats and more than 40 beats (40-50, including 50): Adds 3 to priority
    * Less than or equal to 70 beats and more than 50 beats (50-70, including 70): Adds 2 to priority
    * Less than or equal to 90 beats and more than 80 beats (80-90, including 90): Adds 1 to priority
    * Less than or equal to 80 beats and more than 70 beats (70-80, including 80): No change
  * Over 65 Years Old:                                                         
    * Less than or equal to 45 beats or greater than 95 (0-45, 95 up, including 45): Adds 5 to priority
    * Less than or equal to 95 beats and more than 85 beats (85-95, including 95): Adds 4 to priority
    * Less than or equal to 55 beats and more than 45 beats (45-55, including 50): Adds 3 to priority
    * Less than or equal to 70 beats and more than 55 beats (55-70, including 70): Adds 2 to priority
    * Less than or equal to 85 beats and more than 75 beats (75-85, including 85): Adds 1 to priority
    * Less than or equal to 75 beats and more than 70 beats (70-75, including 75): No change

## Comparing DNA sequences - void dataMod::DNASequenceAnaylsis(std::string DNASequences[], std::string names[], int numOfPatients)
This function uses a nested for loop that allows it to compare each DNA sequence to each other. It uses the numOfPatients to transverse through the list of DNA sequences and the names to label the columns and rows of the outputted table. It calculates the score (intialized by 0) of each DNASequence anaylsis using the scoring system described in the "Use" section. It uses std::cout and std::width to format the data so that it is easy to read by the user.

## Insert all patient names into the queue - void P_Queue::insertPatients(std::string names[], unsigned int priority[], int numOfPatients)
This functions uses a for loop to call its helper function, P_Queue::enqueue(std::string data, unsigned int priority) for every patient. In this function, the name of the patient gets added into the queue and is relocated based on how the unsigned, priority interger compares to the priority intergers of the nodes already in the queue. This will cause the patients with the highest priority to be inserted in the front.

## Extract data and treat all patients one by one - void P_Queue::treat(std::string names[], std::string sexes[], std::string ages[], std::string conditions[], std::string bloodPressures[], std::string heartRates[], std::string DNASequences[])
This function is a recursive function that outputs all of the patient's, whom is at the head of the priority queue information. It waits for the user to input "Treat" and then removes the patient from the queue using the helper function, dequeue which deletes the head and reassigns it to the node that was next in queue. Because this function is recursive, it will call itself with its updated queue until the base case. The base case of this function occurs when the head of the queue is equivalent to nullptr because this means all the patients were removed from queue and there's no more patients to treat. When this occurs, the program will print out, "You have treated everyone! There is no more patients waiting!" and return to the main function. The main function will then end because the program has been completed. 

# Potiental Bugs
* The data must be organized in the order described in the "Use" section of this documentation. There must only be commas located between each section therefore, names need to be entered as "(first name) (last name)". Adding additional commas will add more columns into the row which would cause data to go into the incorrect array. This could cause the program to anayzle data in an incorrect way which skews the output or in an impossible way leading to a segfault error.
* The DNA anaylsis function is programmed to understand that only the "_" character represents a gap. If any other character is used for this representation, it would be compared as a nucleotide which can only be A, T, C, or G. This would lead to an output of inaccurate DNA scores.
* The conditions of the patients must be one of the ones listed in the "Use" section. If not, it will automatically add 1 (the lowest amount) to the priority. This may cause the patient to have a smaller priority value than they should. This can be very dangerous because it increases the wait time of a patient with a serious medical emergency.

# Easter Eggs
I implemented the numOfPatients function as a last minute abstraction. The previous version of my program asked the user to input the number of patients they had in the CSV file. This required the user to complete an addition step and could cause errors or missing data making the program unuseful. This version is able to get all the information it needs to automatically start outputting the desired information to the user. The user is now only required to notify the program when they are ready for the next patient.

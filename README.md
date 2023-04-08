# Mapping of Failed RDP in Azure Sentinel

Outline: Here, we are going to map the failed remote desktop login attempts using Azure sentinel by writing the script in the powershell and connecting with API to get the latitude and longitude of the location. This API will give the location from where the live failed login attempts occuring. In the end, you will get better picture.

**Below are the section we will see:**


![alt text](https://github.com/R0seSecurity/SIEM_LabProject/blob/main/Images/1.jpg)

## Setup the Azure account.

Link : Azure Trial: [https://azure.microsoft.com/en-us/free/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGpjNlgtYThTNTNHejFYT3BXX2o5Z0ZSNGxyd3xBQ3Jtc0tuWXF5ZnBxaWM4UlVDVEtyWFNxRXZGSHg5Y3MtUEoyYXJQcEV6OWVCNlEtZ0Z2R3pHSC15ZGo5TG9Fd2NOOFJicFNlMnRGSFdWSFBaNmVudmx0M2V6UDFWUEVSc2RzRzFGTWxHVXdLU3MzdV9ydV9WWQ&q=https%3A%2F%2Fazure.microsoft.com%2Fen-us%2Ffree%2F&v=RoZeVbbZ0o0)

There will be 200$ free azure credit which can be used to complete this project. In the end of the project you can delete the resources and use remaining credit for other work.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a3db643-513b-4eb8-9138-9efbdbf93e05/Untitled.png)

Now we will create a virtual machine. Go to search bar and write virtual machine. This machine will be exposed to the intenet and other people will start attacking on it.

# **Create a Virtual machine**

## **Steps to create VM:**

1. Resources Group: Create new resource and give it a name( You can name it any).
2. Virtual machine name : Azuremap ( You can name it any)
3. Region: Select the region where the data center exit.
4. Image: Select the Windows 10 Pro version.
5. Size: Default

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7066a452-8f7b-4d01-b31b-b0e627f3d14e/Untitled.png)

1. Administrator Account

Username: Shivani_root

Password: ***********

This will be the username and password of the virtual machine. Remember it for the login purpose.

Select inbound port: Default

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c16cc3da-4fbe-45d0-8b94-9081e849918e/Untitled.png)

1. Now, go to Networking tab and configure the network security group.
2. Change the NIC network security group to Advanced.

![Screenshot 2023-04-07 201410.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab9916bb-ca31-4eea-8ae8-936b91b5f8a6/Screenshot_2023-04-07_201410.jpg)

1. Remove the all inbound rule and Create default RDP Allow Rule to all the traffic from any source. Basically, allowing all the traffic of internet.

![Screenshot 2023-04-07 201909.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5e7170e-0b7a-4eda-8d55-dffbb7bc3d2a/Screenshot_2023-04-07_201909.jpg)

1. Then finally, click on the review and create. It will take sometime to deploy the machine.

# **Log Analytics Workspace**

Lets create a log analytics workspace by searching in the search bar.

Purpose:  Link the virtual machince logs to the azure logs analytics. Also, create a custom logs and find out from where it is coming.

### **Steps for log analytics:**

1. Create log  analytics workspcae by clicking on create.
2. Resource Group: Same as created earlier during virtual machine
3. Name: lab-Azuremaps (You can keep it any)
4. Region: Default ( US )

![Screenshot 2023-04-07 202346.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/875fbeed-eb79-4cdd-83c7-eb80ea5aa58b/Screenshot_2023-04-07_202346.jpg)

1. Now click on review and create.

# **Microsoft Defender for Cloud**

Here, we will enable the logs from the virtual machine into the log analytics.

So navigate to search bar and search for Microsoft Defender for Cloud. On the left coloum click on the Environment setting. 

## **Azure Defender plan change**

Select the group and turn on the servers and turn off the sql servers on machine as we will not need. Click on the save.

![chrome-capture-2023-3-7 (1).gif](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c9a244e-e2ad-4ab9-a45b-8179f29a2a54/chrome-capture-2023-3-7_(1).gif)

## **Data Collection**

Data Collection from all the events.

On the left coloum click on data collection and select all event.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48bcba73-fc49-4767-9814-4b1f9cbd1c68/Untitled.png)

Now lets open the **log analytics workspace** from search bar. On the left there wil lbe virtual machine select that and click on connect.

Open another tab  with same url (portal.azure.com)

# **Microsoft Sentinel**

Go on the top in search bar and search **microsoft sentinel**. This will be used as a SIEM which will help to visualize the log data.

First step is to click on the create the sentinel workspace. Then select the workspace which we wanted to be connected to our log analytics. By the time it is getting connecting. 

# Virtual Machine

## Open virtual machine:

1. Agian go to search bar and search virtual machine.

![chrome-capture-2023-3-7 (2).gif](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa100128-28ae-4aee-b240-080211215f7f/chrome-capture-2023-3-7_(2).gif)

1. Open the virtual machine by double click on the virtual machine name.

![Screenshot 2023-04-06 095241.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d7511d5-5237-419d-9621-23ee0b5329a5/Screenshot_2023-04-06_095241.png)

1. Now go to main computer and search Remote Desktop Connection.

Well now paste the Public IP Address. And use the same username and password which was created during virtual machine setup.

1. Now click on the connect and you will be able to access the virtual machine using RDP. Enter the username and connect to the machine.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/57e3b58b-2e53-4cee-b13d-f051851168c1/Untitled.png)

### **Windows Defender Firewall and advanced security**

Before moving toward the Event viewer to view the logs. First we will change the firewall configuration. 

Search for **Windows Defender Firewall and advanced security**

Will turn off the Firewall state for Domain Profile, Private Profile and Public Profile. This will help others to discover new machine which is being now exposed in outside world.

![Screenshot 2023-04-07 232500.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a804de44-e779-4641-86fb-d1f9d7312bd4/Screenshot_2023-04-07_232500.jpg)

Now search for the event viewer in the virtual machine. Here there will be Custom logs, Windows logs, Application and Services logs.

In the windows Log 

- Application
- Security
- Setup
- System
- Forwarded Events

Check the security event logs there will be first of all login attempt logs, failed login, successful login and other logs.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa222363-0935-45dc-a74d-975645197ee7/Untitled.png)

Main foucs will be on the Event ID 4645 which is a failed login attempt. You can filter the logs and check the failed attempts. As our machine is exposed to outside world now when people able to discover they will start to login or brute force.

We will try to login again using RDP from main computer. And enter the wrong username and wrong password just for receiving the failed attempt log in the event viewer.

Now go to event viewer —- >Security log , in that filter the log using eventid 

This picture shows the EventID 4625 (Failed Logon) and whoe description like which account username person tried to login. However, there is no location mentioned only the IP address we know.

Further process we will see how we can map using Geolocation.

Steps:

1. Go to search bar on the VM and search Windows Powershell ISE.
2. Writing a Windows PowerShell ISE. Copy and paste it in new file and save it!. Here, is the Github link.

In the script there are few sample of failed login attempts. This script gather the geo location of the failed login and create a custom failed logs.

[GitHub - R0seSecurity/SIEM_LabProject](https://github.com/R0seSecurity/SIEM_LabProject)

1. API : Will be different for you dont copy paste same as mine as it wont work. 
2. For API  go to  [https://ipgeolocation.io/signup](https://ipgeolocation.io/signup) and generate your API.
3. Now Run the script and wait for a few minutes to get an Output.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aee564fc-4c1c-4656-9a91-b79cb8d4b682/Untitled.png)

1. Now go to C:\ProgramData\failed_rdp.log.
    
    So the initial will be the sample data which will help to train the analytics workspace and followed by the real ones.
    

# Log Analytics

## Create the Custom Logs

Steps:

1. Go to main computer and search the log analytics workspace.
2. On the left column search custom logs.
3. Click on add custom logs. It will ask the location of the log file. So here we will copy paste the failed_rdp.log file in the notebook and save it in Main computer. 
4. Once selected. Click on Next.
5. Details
    
    Custome Log Name: FAILED_RDP_WITH_GEO
    
6. Click on Next

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e4081e1-9659-4aa5-aaee-deb765f94d48/Untitled.png)

## General Logs Query

Logs

1. Search logs on the left column.
2. Write a Query: 
    
    SecurityEvent
    
    SecurityEvent | where EventID == 4625
    
3. This queries will give the data from the windows log.
    
    

## Failed RDP Logs Query

Write a Query.

FAILED_RDP_WITH_GEO_CL

Output: All the failed login attempts.

## Create a Custom field and extract from the raw column

Steps:

1. Write a Query.
    
    FAILED_RDP_WITH_GEO_CL
    
    Output: All the failed login attempts.
    
    Now in the result section, select any output row
    
2. Now, right click on the first row and extract the field.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7635b08c-5ff7-4919-8e96-860d0958da2f/Untitled.png)
    
3. In the raw data section, select the latitude value. Mention the Field title: latitude Field Type: Numeric. Then clcik on save.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fbaa1a4-2c81-426a-ba35-fe8ef2f8f9f6/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5f9a82bd-8e29-4ea6-8587-af7665d72841/Untitled.png)

Repeat same process for all other field. like longitude, destinationhost, username, sourcehost, state, coutnry, label, timestamp. If the right field is not selected on the right side their is a option to modify one by one.

1. Try to login RDP with the wrong password then again run the same query. It will take time to get a result so have paitence!
2. Result:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2706fca4-0c0a-416d-ab37-a4b5a2a24ba4/Untitled.png)

---

# Maps in Azure

## Create Workbook

In this section, we will see the custom query by removing the samples added initial while creating the logs in powershell.

Open the new tab of azure portal and search sentinel.

In the overview section, there is a dashboard with a events, alerts and, failed geo rpd.

Steps:

1. Search workbook in the left column and click on the add workbook.(PS. Remove the previous graph one which is shown in the new workboook with the help of edit tag.)
2. Now clcik on the Add button and select the add query.
3. In the query section add.
    
    FAILED_RDP_WITH_GEO_CL | summarize event_count=count() by sourcehost_CF, latitude_CF, longitude_CF, country_CF, label_CF, destinationhost_CF
    | where destinationhost_CF != "Trial"
    | where sourcehost_CF != ""
    

1. Now run the query.This is the output.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aec364a7-fb5d-454e-97bb-c90d3148b3ba/Untitled.png)
    

---

## Setup Maps

Once the outup start appearing then change the vizualization by selecting maps.

There will be a map settings:

Layout setting

Location: Country of Region

Country/Region Field: Country_region

Size_by:latitude_CL 

Aggregation for location: Default

Color Setting

Color by: Event_count

Aggregation for Color: sum of values

Color palette: 32-color-palette

Metric Setting

Metric value: event_count

Then clcik on the Apply.

Result:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/89b9aeb9-0ad4-49bd-a6a8-79c3e232df78/Untitled.png)

After waiting for an hour check it again that small circles will get bigger as many people able to discover your ID and try to give an attempt.

After 9 hours Result.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fc73dfa4-40f5-4991-8e2e-8521f28845c4/Untitled.png)

**Hurray You Did It!!!!!**

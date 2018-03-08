# Big data hands-on lab (HOL) #
<a name="Overview"></a>
## Overview ##
In this hands-on lab (HOL), you will explore U-SQL, a big data language designed to query both structured and unstructured data at any scale. 

In the lab, you are a developer helping to create an apartment rental app called ContosoBNB.  By querying a large dataset made available by [Airbnb](https://www.airbnb.com/), you hope to provide the following insights to eventual users of the ContosoBNB app:

+ Current rental rate ranges based on their property location and features.
+ Forecasted rental rates based upon occupancy rates.
+ Potential monthly and yearly revenue for renting a room.

### Objectives
In this hands-on lab, you will:
+ Learn how Azure Data Lake and U-SQL is an alternative to Hadoop/HDFS and Map Reduce.
+ Work with data on your local system and in an Azure Data Lake Store.
+ Execute U-SQL queries on your local system and in Azure Data Lake.
+ Learn how to use existing data to calculate new values in your datasets.
+ Modify queries to address data inconsistency issues.

### Prerequisites

The following are required to complete this hands-on lab:

- An Azure subscription, to perform Azure Data Lake data-transfer exercises

### Resources

This lab makes use of an existing dataset (released under public domain) to model real-world property listings with their associated details. The complete dataset can be found at [http://insideairbnb.com/get-the-data.html](http://insideairbnb.com/get-the-data.html). For the purposes of this lab, the data has been converted to a standard Windows text file format to emulate how it might be provided if requested directly from an Azure Data Lake Store.

## Exercises

This hands-on lab includes the following exercises:

-   [Exercise 1: Create a DSVM](#Exercise1)
-   [Exercise 2: Prepare the environment](#Exercise2)
-   [Exercise 3: Create U-SQL queries to view rental listings and review information](#Exercise3)
-   [Exercise 4: Use U-SQL to calculate occupancy rate](#Exercise4)
-   [Exercise 5: Combine queries to determine potential rental income](#Exercise5)


<a name="Exercise1"></a>
## Exercise 1: Create a DSVM


In this exercise, you will create an instance of the DSVM for Windows in Azure. The DSVM for Windows is a VM image in Azure that includes many preinstalled and configured data-science and development tools. You can read a longer description about the many tools and features available in the DSVM [here](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).


### Step 1: Creating the DSVM in Azure

1.  In a web browser, open the [Azure portal](https://portal.azure.com/) at [https://portal.azure.com](https://portal.azure.com/), and then sign in with your Microsoft account.

2.  From the left-side menu, click the **+** sign to add a new resource.
![CreateResource](img/CreateResource.jpg)
3.  In the **Search** field, type **data science**. From the list of matching results, click **Data Science Virtual Machine - Windows 2012**.
![FindDSVM-win](img/FindDSVM-win.jpg)
4.  Take a few moments to read the description of the DSVM, and then click **Create**.
![CreateDSVM](img/CreateDSVM.jpg)
5.  In the **Name** field, enter a name for your VM; for example, **WinDSVM**.
![CreateDSVM2](img/CreateDSVM2.jpg)
7.  In the **User Name** field, type a user name of your choice. Save this information, because you will use it to sign in to the VM later.

9.  In the **Password** field, enter a password of your choice. Save this information, because you will use it to sign in to the VM later.

10.  In the **Subscription** drop-down menu, select your subscription.

11.  In the **Resource Group** section, leave **Create New** selected, and then enter a name of your choice for the resource group in the field below; for example, **DataScienceGroup1**.

12.  In the **Location** drop-down menu, ensure that a geographically close location is chosen.

13.  Click **OK**.
At this stage, the Choose a Size page appears. Proceed to the next step.

### Step 2: Sizing the new VM and reviewing settings

1.  On the **Choose A Size** page, click **View All**.
![ChooseSize](img/ChooseSize.jpg)
2.  In the list of available VM types, select **DS4_V2 Standard**.
![ChooseSize2](img/ChooseSize2.jpg)
3.  Click **Select**.

4.  On the **Settings** page that appears, review the default settings, and then click **OK**. 

The Create page appears, displaying offer details and summary information.

5.  Click **Create**.

**Important: Make sure you return to the Azure portal and shut down this VM after you complete this lab.**

6.  Wait a few minutes while the DSVM is deployed. After it is deployed, you will see a dashboard for your new VM. At the top of the dashboard, you will see controls.

![StartStop](img/StartStop.jpg)

7.  The **Start** button is not available, indicating that the new VM has already started.

### Step 3: Connecting to the new VM
1. If you are on a Mac, download and install [Microsoft Remote Desktop 10](https://itunes.apple.com/us/app/microsoft-remote-desktop-10/id1295203466?mt=12) from the [Mac App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop-10/id1295203466?mt=12) . 

2. On the control bar for your new VM, click **Connect**.
![Connect](img/Connect.jpg)

This step downloads an RDP file through your browser.  

3. Open the RDP file and connect to the DSVM.

- If you are on a Windows computer, you can open the RDP file through the browser. Click **Connect** when prompted, then supply the credentials you specified in step 1 of this exercise when prompted, and finally click Yes to accept the certificate.

- If you are on a Mac, save the RDP file to a convenient location, and then open the file.  If you are prompted to verify a certificate, click **Continue**. Enter the credentials you specified in the step 1 when prompted.

4.  When you see the DSVM desktop, proceed to the next exercise.
![DSVMdesktop](img/DSVMdesktop.jpg)

<a name="Exercise2"></a>
## Exercise 2: Preparing the environment
The DSVM contains the tools we need, but these tools are missing some components by default.  Before you can perform U-SQL queries in our source code editor Visual Studio Code, you will need to download and install an additional Visual Studio component, as well as the Azure Data Lake Tools.
   
### Step 1: Installing the Visual Studio component 
1. In the DSVM, right-click the Start button and select **Search** from the shortcut menu. 
![search](img/search.jpg)
2. In the search box that opens on the right side of the screen, type **Visual Studio Installer**, and then click to open that program when its tile appears below the search box.
![SearchInstaller](img/SearchInstaller.jpg)

3. If you are prompted to update the Visual Studio Installer before proceeding, click Update to perform the update. 
4. In the Visual Studio Installer page that opens, expand the More menu associated with Visual Studio Community 2017, and then click Modify on the menu.
![modify](img/modify.jpg)

5. On the Modifying-Visual Studio Community 2017 page that opens, click **Individual components**.
![ic2](img/ic2.jpg)
6. In the list of individual components, within the **Compilers, build tools, and runtimes category**, select the checkbox associated with **VC++ 2015.3 v140 toolset for desktop (x86,x64)**.
![ic3](img/ic3.jpg)
7. Click Modify.
![ic4](img/ic4.jpg)
The installation will require a few minutes to complete. 
8. After the installation has completed, close the Visual Studio Installer window.
### Step 2: Installing Azure Data Lake Tools in Visual Studio Code
We will be using Visual Studio Code as our source code editor of choice. To make Visual Studio Code compatible with U-SQL, we need to install the Azure Data Lake Tools extension. 
1.  On the desktop the DSVM, locate and double-click the Visual Studio Code icon to open this application.
2. Visual Studio Code opens, along with a web page. 
3. asdfasd
4. asdfasdf

<a name="Exercise3"></a>
## Exercise 3: xxxxx




<a name="Exercise4"></a>
## Exercise 4: xxxxx




<a name="Exercise5"></a>
## Exercise 5: xxxxx
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ5NDg5NTYwOF19
-->
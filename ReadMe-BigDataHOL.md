# Big data hands-on lab (HOL) #
<a name="Overview"></a>
## Overview ##
This hands-on-lab (HOL) is based on a web application called ContosoBNB, which itself is modeled on a short-term vacation rental platform like [Airbnb](https://www.airbnb.com/)
xxx or [HomeAway](https://www.homeaway.com/), and the users are seen as the property owners who are renting out rooms to customers. 
xxxx
In this lab, you will create various Big Data queries that are designed to provide the following insights to these Airbnb users:

+ Current rental rate ranges based on their property location and features.
+ Forecasted rental rates based upon occupancy rates.
+ Potential monthly and yearly revenue for renting a room.

As the scraped dataset does not contain rental information, this last figure will be calculated from listing and review data. This determined occupancy rate will be used in calculations/queries.

### Objectives
In this hands-on lab, you will:
+ Learn how Azure Data Lake and U-SQL is an alternative to Hadoop/HDFS and Map Reduce.
+ Work with data on your local system and in an Azure Data Lake Store.
+ Execute U-SQL queries on your local system and in Azure Data Lake.
+ Learn how to use existing data to calculate new values in your datasets.
+ Modify queries to address data inconsistency issues.

### Prerequisites

The following are required to complete this hands-on lab:

- Microsoft’s free, open-source, Visual Studio Code with the Azure Data Lake extension
- An Azure subscription, to perform Azure Data Lake data-transfer exercises
- A Windows operating system


Note: You can also perform the lab in a Mac or Linux environment, but the specific steps have been written for a Windows environment.

### Resources

This lab makes use of an existing dataset (released under public domain) to model real-world property listings with their associated details. The complete dataset can be found at [http://insideairbnb.com/get-the-data.html](http://insideairbnb.com/get-the-data.html). For the purposes of this lab, the data has been converted to a standard Windows text file format to emulate how it might be provided if requested directly from an Azure Data Lake Store.

## Exercises

This hands-on lab includes the following exercises:

-   [Exercise 1: Create a DSVM](https://github.com/ProwessInfo/AzureUniversityRedShirt/tree/master/Challenges/MachineLearningHOL#Exercise1)
-   [Exercise 2: Install MS Code with Azure Data Lake Local Run Service & Git](https://github.com/ProwessInfo/AzureUniversityRedShirt/tree/master/Challenges/BigDataHOL#Exercise1)
-   [Exercise 3: Create U-SQL queries to view Rental Listings and Review information](https://github.com/ProwessInfo/AzureUniversityRedShirt/tree/master/Challenges/BigDataHOL#Exercise2)
-   [Exercise 4: Use U-SQL to calculate occupancy rate](https://github.com/ProwessInfo/AzureUniversityRedShirt/tree/master/Challenges/BigDataHOL#Exercise3)
-   [Exercise 5: Combine queries to determine potential rental income](https://github.com/ProwessInfo/AzureUniversityRedShirt/tree/master/Challenges/BigDataHOL#Exercise4)



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

4.  On the **Settings** page that appears, review the default settings, and then click **OK**. The Create page appears, displaying offer details and summary information.

5.  Click **Create**.

6.  Wait a few minutes while the DSVM is deployed. After it is deployed, you will see a dashboard for your new VM. At the top of the dashboard, you will see controls.

7.  The **Start** button is not available, indicating that the new VM has already started.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzYzMjQ3NzddfQ==
-->
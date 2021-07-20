![Microsoft Cloud Workshop](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshop')

<div class="MCWHeader1">
Big data and visualization
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
March 2021
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2021 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Big data and visualization hands-on lab step-by-step](#big-data-and-visualization-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Exercise 1: Retrieve lab environment information and create Databricks cluster](#exercise-1-retrieve-lab-environment-information-and-create-databricks-cluster)
    - [Task 1: Retrieve Azure Storage account information and Subscription Id](#task-1-retrieve-azure-storage-account-information-and-subscription-id)
    - [Task 2: Create an Azure Databricks cluster](#task-2-create-an-azure-databricks-cluster)
  - [Exercise 2: Load Sample Data and Databricks Notebooks](#exercise-2-load-sample-data-and-databricks-notebooks)
    - [Task 1: Upload the Sample Datasets](#task-1-upload-the-sample-datasets)
    - [Task 2: Install Azure ML library on the cluster](#task-2-install-azure-ml-library-on-the-cluster)
    - [Task 3: Open Azure Databricks and complete lab notebooks](#task-3-open-azure-databricks-and-complete-lab-notebooks)
  - [Exercise 3: Setup Azure Data Factory](#exercise-3-setup-azure-data-factory)
    - [Task 1: Download and stage data to be processed](#task-1-download-and-stage-data-to-be-processed)
    - [Task 2: Install and configure Azure Data Factory Integration Runtime on your machine](#task-2-install-and-configure-azure-data-factory-integration-runtime-on-your-machine)
    - [Task 3: Configure Azure Data Factory](#task-3-configure-azure-data-factory)
  - [Exercise 4: Develop a data factory pipeline for data movement](#exercise-4-develop-a-data-factory-pipeline-for-data-movement)
    - [Task 1: Create copy pipeline using the Copy Data Wizard](#task-1-create-copy-pipeline-using-the-copy-data-wizard)
  - [Exercise 5: Operationalize ML scoring with Azure Databricks and Data Factory](#exercise-5-operationalize-ml-scoring-with-azure-databricks-and-data-factory)
    - [Task 1: Create Azure Databricks Linked Service](#task-1-create-azure-databricks-linked-service)
    - [Task 2: Trigger workflow](#task-2-trigger-workflow)
  - [Exercise 6: Summarize data using Azure Databricks](#exercise-6-summarize-data-using-azure-databricks)
    - [Task 1: Summarize delays by airport](#task-1-summarize-delays-by-airport)
  - [Exercise 7: Visualizing in Power BI Desktop](#exercise-7-visualizing-in-power-bi-desktop)
    - [Task 1: Obtain the JDBC connection string to your Azure Databricks cluster](#task-1-obtain-the-jdbc-connection-string-to-your-azure-databricks-cluster)
    - [Task 2: Connect to Azure Databricks using Power BI Desktop](#task-2-connect-to-azure-databricks-using-power-bi-desktop)
    - [Task 3: Create Power BI report](#task-3-create-power-bi-report)
  - [Exercise 8: Deploy intelligent web app (Optional Lab)](#exercise-8-deploy-intelligent-web-app-optional-lab)
    - [Task 1: Register for an OpenWeather account](#task-1-register-for-an-openweather-account)
    - [Task 2: Deploy web app from GitHub](#task-2-deploy-web-app-from-github)
    - [Task 3: Manual deployment (optional)](#task-3-manual-deployment-optional)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete resource group](#task-1-delete-resource-group)

<!-- /TOC -->

# Big data and visualization hands-on lab step-by-step

## Abstract and learning objectives

This hands-on lab is designed to provide exposure to many of Microsoft's transformative line of business applications built using Microsoft big data and advanced analytics.

By the end of the lab, you will be able to show an end-to-end solution, leveraging many of these technologies but not necessarily doing work in every component possible.

## Overview

Margie's Travel (MT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves and provide added value to their corporate customers.

They are looking to pilot a web app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to whether the departing flight will encounter a 15-minute or longer delay, considering the weather forecast for the departure hour.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![The high-level overview diagram of the end-to-end solution is displayed. Flight delay data and historical airport weather data are provided to Azure Data Factory. Azure Data Factory provides this data to both blob storage and Azure Databricks. Azure Databricks scores the data and saves the results to an Azure SQL Database. Azure Databricks also creates, trains, and exports a machine learning model to the Azure Machine Learning Service. Azure Machine Learning service provides a containerized services that is consumed by the web portal. The web portal also consumes 3rd party API data for forecasted weather. Map data visualization is provided by Power BI using web portal information and the Azure SQL database.](../Whiteboard%20design%20session/media/high-level-overview.png 'High-level overview diagram')

## Requirements

1. Microsoft Azure subscription must be pay-as-you-go or MSDN.

   - Trial subscriptions will not work.

2. If you are not a Service Administrator or Co-administrator for the Azure subscription, or if you are running the lab in a hosted environment, you will need to install [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** and **Azure development** workloads.

3. Follow all the steps provided in [Before the Hands-on Lab](Before%20the%20HOL%20-%20Big%20data%20and%20visualization.md).

## Exercise 1: Retrieve lab environment information and create Databricks cluster

Duration: 10 minutes

In this exercise, you will retrieve your Azure Storage account name and access key and your Azure Subscription Id and record the values to use later within the lab. You will also create a new Azure Databricks cluster.

### Task 1: Retrieve Azure Storage account information and Subscription Id

You will need to have the Azure Storage account name and access key when you create your Azure Databricks cluster during the lab. You will also need to create storage containers in which you will store your flight and weather data files.

1. From the side menu in the Azure portal, choose **Resource groups**, then enter your resource group name into the filter box, and select it from the list.

2. Next, select your lab Azure Storage account from the list.

   ![The lab Azure Storage account is selected from within your lab resource group.](media/select-azure-storage-account.png 'Azure Storage Account')

3. On the left menu, select **Overview (1)**, locate and copy your Azure **Subscription ID (2)** and save to a text editor such as Notepad for later use.

   ![On the left menu, Overview is selected and the Subscription ID is highlighted.](media/azure-storage-subscription-id.png 'Subscription ID')

4. Select **Access keys (1)** from the menu and select **Show keys (2)**. Copy the **storage account name (3)** and the **key1 (3)** to a text editor such as Notepad for later use.

   ![On the left menu, located in the Settings section, Access keys is selected. The copy button next to the Storage account name textbox is highlighted, as well as the copy button next to the key 1 key textbox.](media/azure-storage-access-keys.png 'Storage Access Keys')

### Task 2: Create an Azure Databricks cluster

You have provisioned an Azure Databricks workspace, and now you need to create a new cluster within the workspace. Part of the cluster configuration includes setting up an account access key to your Azure Storage account, using the Spark Config within the new cluster form. This will allow your cluster to access the lab files.

1. From the side menu in the Azure portal, select **Resource groups**, then enter your resource group name into the filter box, and select it from the list.

2. Next, select your Azure Databricks service from the list.

   ![The Azure Databricks Service is selected from within your lab resource group.](media/select-azure-databricks-service.png 'Azure Databricks Service')

3. In the Overview pane of the Azure Databricks service, select **Launch Workspace**.

   ![The Launch Workspace button is selected within the Azure Databricks service overview pane.](media/azure-databricks-launch-workspace.png 'Launch Workspace')

   Azure Databricks will automatically log you in using Azure Active Directory Single Sign On.

   ![The Azure Databricks Azure Active Directory Single Sign On dialog.](media/azure-databricks-aad.png 'Databricks Sign In')

4. Select **Compute (1)** from the menu, then select **+ Create Cluster (2)** .

   ![From the left menu, Clusters is selected. The + Create Cluster button is selected.](media/azure-databricks-create-cluster-button.png 'Databricks Clusters')

5. On the New Cluster form, provide the following:

   - **Cluster Name (1)**: `lab`

   - **Cluster Mode (2)**: **Standard**

   - **Pool**: Select **None**

   - **Databricks Runtime Version (3)**: **Runtime: 6.4 Extended Support (Scala 2.11, Spark 2.4.5)** (**Note**: the runtime version CANNOT be > 6.6, due to compatibility issues with the supplied notebooks.)

   - **Enable Autoscaling (4)**: **Uncheck** this option.

   - **Terminate after**: **Check** the box and enter `120`

   - **Worker Type (5)**: **Standard_F4s**

   - **Driver Type**: **Same as worker**

   - **Workers (6)**: `1`

   - **Spark Config**: Expand **Advanced Options (7)** and edit the Spark Config by entering the connection information for your Azure Storage account that you copied above in Task 1. This will allow your cluster to access the lab files. Enter the following:

     `spark.hadoop.fs.azure.account.key.<STORAGE_ACCOUNT_NAME>.blob.core.windows.net <ACCESS_KEY>`, where <STORAGE_ACCOUNT_NAME> is your Azure Storage account name, and <ACCESS_KEY> is your storage access key.

   **Example: (8)** `spark.hadoop.fs.azure.account.key.bigdatalabstore.blob.core.windows.net HD+91Y77b+TezEu1lh9QXXU2Va6Cjg9bu0RRpb/KtBj8lWQa6jwyA0OGTDmSNVFr8iSlkytIFONEHLdl67Fgxg==`

   ![The New Cluster form is populated with the values as outlined above.](media/azure-databricks-create-cluster-form.png 'Create Cluster')

6. Select **Create Cluster (9)**.

## Exercise 2: Load Sample Data and Databricks Notebooks

Duration: 60 minutes

In this exercise, you will implement a classification experiment. You will load the training data from your local machine into a dataset. Then, you will explore the data to identify the primary components you should use for prediction and use two different algorithms for predicting the classification. You will then evaluate the performance of both algorithms and choose the algorithm that performs best. The model selected will be exposed as a web service integrated with the optional sample web app at the end.

### Task 1: Upload the Sample Datasets

1. Before you begin working with machine learning services, there are three datasets you need to load.

2. Download the three CSV sample datasets from here: <http://bit.ly/2wGAqrl> (If you get an error, or the page won't open, try pasting the URL into a new browser window and verify the case sensitive URL is exactly as shown). If you are still having trouble, a zip file called AdventureWorksTravelDatasets.zip is included in the lab-files folders.

3. Extract the ZIP and verify you have the following files:

   - FlightDelaysWithAirportCodes.csv
   - FlightWeatherWithAirportCode.csv
   - AirportCodeLocationLookupClean.csv

4. Open your Azure Databricks workspace. Before continuing to the next step, verify that your new cluster is running. Do this by navigating to **Compute (1)** on the left-hand menu and ensuring that the state of your cluster is **Running (2)**.

   ![The Clusters menu item is selected and the cluster is shown indicating that it is in the Running state.](media/azure-databricks-clusters-running.png 'Clusters')

5. Select **Data (1)** from the menu. Next, select **default (2)** under Databases (if this does not appear, start your cluster). Finally, select **Create Table (3)** above the Tables header.

   ![From the Azure Databricks workspace, Data is selected from the menu, default database is selected from a list of available databases, the Create Table button is selected.](media/azure-databricks-create-tables.png 'Create new table')

6. Select **Upload File (1)** under Create New Table, and then select either select or drag-and-drop the FlightDelaysWithAirportCodes.csv file into the file area. Select **Create Table with UI (3)**.

   ![In the Create New Table form, the Upload File button is highlighted and the FlightDelaysWithAirportCodes.csv shows as uploaded. The Create Table with UI button is shown at the bottom of the form.](media/create-flight-delays-table-ui.png 'Create new table')

7. Select your cluster **(1)** to preview the table, then select **Preview Table (2)**.

8. Change the Table Name to `flight_delays_with_airport_codes` **(3)** and select the checkmark for **First row is header (4)**. Select **Create Table (5)**.

   ![The Specify Table Attributes form is displayed, flight_delays_with_airport_codes is highlighted in the Table Name field and the First row is header checkbox is checked. The Table Preview displays the Column Names and types along with a sampling of data.](media/flight-delays-attributes.png 'Rename table')

9. Repeat steps 5 through 8 for the FlightWeatherWithAirportCode.csv and AirportCodeLocationLookupClean.csv files, setting the name for each dataset in a similar fashion:

   - flightweatherwithairportcode_csv renamed to **flight_weather_with_airport_code**
   - airportcodelocationlookupclean_csv renamed to **airport_code_location_lookup_clean**

   ![In the Data section, the default database is selected and the list of tables shows the three tables that were created based on the spreadsheet data.](media/uploaded-data-files.png 'Uploaded data files')

### Task 2: Install Azure ML library on the cluster

1. Select **Compute (1)** on the left-hand menu, then select your lab cluster **(2)** to open it.

   ![From the left menu, the Clusters item is selected, and the lab cluster is highlighted.](media/azure-databricks-cluster.png 'Lab cluster')

2. Select the **Libraries** tab. If you **do not** see the Azure ML library already installed on the cluster, continue to the next step. Otherwise, continue to Task 3.

3. Select **Install New**.

   ![The Libraries tab is selected and Install New button is highlighted.](media/azure-databricks-cluster-install-new.png 'Libraries')

4. In the Install Library dialog, select **PyPI (1)** for the Library Source, then enter the following in the Package field: `azureml-sdk[databricks]` **(2)**. Select **Install (3)**.

   ![The Install Library dialog is displayed with PyPI selected as the Library Source, and the azureml-sdk package name is entered into the Package field.](media/azure-databricks-cluster-install-library-pypi.png 'Install Library')

5. **Wait** until the library's status shows as **Installed** before continuing.

   ![The azureml-sdk databricks library is shown in the list of packages with a status of Installed.](media/azure-databricks-cluster-libraries-installed.png 'Libraries')

### Task 3: Open Azure Databricks and complete lab notebooks

1. Within Azure Databricks, select **Workspace (1)** on the menu, then **Users (2)**, then select the down arrow next to your user name **(3)**. Select **Import (4)**.

   ![In the left menu, the Workspace item is selected. Beneath the Workspaces pane, the Users item is selected. Beneath the Users pane, the current user is selected. The menu carat next to the username of the user is expanded with the Import item selected.](media/select-import-in-user-workspace.png 'Import')

2. Within the Import Notebooks dialog, select Import from: **URL (1)**, then paste the following into the URL textbox **(2)**: `https://github.com/microsoft/MCW-Big-data-and-visualization/blob/master/BigDataVis.dbc?raw=true`. Select **Import (3)** to continue.

   ![The Import Notebooks dialog is shown that will allow the user to import notebooks via a file upload or URL.](media/import-notebooks.png 'Import from file')

3. After importing, expand the new **BigDataVis** folder.

   ![Workspace is open. The current user is selected. BigDataVis folder is highlighted.](media/adf-selecting-bigdatavis.png 'BigDataVis')

   > **WARNING:** When you open a notebook, make sure you attach your cluster to the notebook using the **Attach to cluster** dropdown. You will need to do this for each notebook you open.
   >
   >![In the taskbar for a notebook, the cluster that is currently attached is highlighted.](media/attach-cluster-to-notebook.png 'Attach cluster to notebook')

4. Run each cell (except `Clean up` section in Notebook 3) of the notebooks located in the **Exercise 2** folder (01, 02 and 03) individually by selecting within the cell, then entering **Ctrl+Enter** on your keyboard. Pay close attention to the instructions within the notebook, so you understand each step of the data preparation process.

   > **WARNING:** If you plan on running the web app in the optional exercise: **Exercise 8: Deploy intelligent web app (Optional Lab)**, then do **NOT** run the cells in the `Clean up` section of Notebook `03 Deploy as Web Service`.

   ![In the Workspace screen, beneath BigDataVis the Exercise 2 folder is selected. Beneath Exercise 2, three notebooks are displayed 01 Data Preparation, 02 Train and Evaluate Models, and 03 Deploy as Web Service.](media/azure-databricks-exercise-2.png 'Exercise 2 folder')

5. Make sure you take note of the web service URL to be used later in **Exercise 8: Deploy intelligent web app (Optional Lab)**. Copy the URL to a text editor such as Notepad for later use.

   ![Web Service URL Retrieval section of Notebook 3 runs. The resulting URL is highlighted.](media/adf-notebook3-web-service-url.png 'Web Service URL')

6. Do NOT run any notebooks within the Exercise 5 or 6 folders. They will be discussed later in the lab.

## Exercise 3: Setup Azure Data Factory

Duration: 20 minutes

In this exercise, you will create a baseline environment for Azure Data Factory development to further operationalize data movement and processing. You will create a Data Factory service and then install the Data Management Gateway, which is the agent that facilitates data movement from on-premises to Microsoft Azure.

### Task 1: Download and stage data to be processed

1. Open a web browser.

2. Download the AdventureWorks sample data from <http://bit.ly/2zi4Sqa>. If you are having trouble downloading the file, a zip file called FlightsAndWeather.zip is included in the lab-files folders.

   >**Note**: If you are using the optional VM provisioned in the Before the HOL document, ensure that you download and extract the data on the VM.

3. Extract it to a new folder called **C:\\Data**.

### Task 2: Install and configure Azure Data Factory Integration Runtime on your machine

1. To download the latest version of Azure Data Factory Integration Runtime, go to <https://www.microsoft.com/en-us/download/details.aspx?id=39717>.

   >**Note**: If you are using the optional VM provisioned in the Before the HOL document, ensure that you install the IR on the VM.

2. Select Download, then choose the download you want from the next screen.

   ![Under Choose the download you want, the latest version MSI file is selected.](media/download-ir.png 'Choose the download you want section')

3. Run the installer once downloaded.

4. When you see the following screen, select Next.

   ![The Welcome page in the Microsoft Integration Runtime Setup Wizard displays. A language drop-down list is shown, and the Next button is selected.](media/image114.png 'Microsoft Integration Runtime Setup Wizard')

5. Check the box to accept the terms and select Next.

   ![On the End-User License Agreement page, the check box to accept the license agreement is selected, as is the Next button.](media/image115.png 'End-User License Agreement page')

6. Accept the default Destination Folder, and select Next.

   ![On the Destination folder page, the destination folder is set to C:\Program Files\Microsoft Integration Runtime\ and the Next button is selected.](media/image116.png 'Destination folder page')

7. Choose Install to complete the installation.

   ![On the Ready to install Microsoft Integration Runtime page, the Install button is selected.](media/image117.png 'Ready to install page')

8. Select Finish once the installation has been completed.

   ![On the Completed the Microsoft Integration Runtime Setup Wizard page, the Finish button is selected.](media/image118.png 'Completed the Wizard page')

9. After selecting Finish, the following screen will appear. Keep it open for now. You will come back to this screen once the Data Factory in Azure has been provisioned and obtain the gateway key to connect Data Factory to this "on-premises" server.

   ![The Microsoft Integration Runtime Configuration Manager, Register Integration Runtime dialog displays.](media/ir-self-hosted-registration-screen.png 'Register Integration Runtime page')

### Task 3: Configure Azure Data Factory

1. Launch a new browser window, and navigate to the Azure portal (<https://portal.azure.com>). Once prompted, log in with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or a Microsoft account. This will be based on which account was used to provision your Azure subscription used for this lab.

2. From the side menu in the Azure portal, choose **Resource groups**, then enter your resource group name into the filter box, and select it from the list.

3. Next, select your Azure Data Factory service from the list.

   ![Azure Portal Resource Listing page is shown. Azure Data Factory resource is highlighted.](media/select-azure-datafactory.png 'Azure Data Factory')

4. On the Data Factory Overview screen, select **Open Azure Data Factory Studio**.

   ![In the Azure Data Factory resource screen, Overview is selected from the left menu. The Open Azure Data Factory Studio tile is selected.](media/adf-author-monitor.png 'Open Azure Data Factory Studio')

5. A new page will open in another tab or new window. Within the Azure Data Factory site, select **Manage** on the menu.

   ![In the left menu, the Manage icon is selected.](media/adf-home-manage-link.png 'Manage link on ADF home page')

6. Now, select **Integration runtimes (1)** in the menu, then select **+ New (2)**.

   ![The Integration runtimes menu item is selected, and the + New button is selected.](media/adf-new-ir.png 'Steps to create a new Integration Runtime connection')

7. In the Integration Runtime Setup blade that appears, select **Azure, Self-Hosted (1)**, then select **Continue (2)**.

   ![In the Integration runtime setup options, select Azure, Self-Hosted. Perform data flows, data movement, and dispatch activities to external compute.The Continue button is selected.](media/adf-ir-setup-1.png 'Integration Runtime Setup step 1')

8. Select **Self-Hosted (1)** then select **Continue (2)**.

   ![In the Network environment selected, Self-Hosted is selected, and the Continue button is highlighted.](media/adf-ir-setup-2.png 'Integration Runtime Setup step 2')

9. Enter a **Name (1)**, such as bigdatagateway-\[initials\], and select **Create (2)**.

   ![In the Integration runtime setup form, the Name textbox is populated with the value defined above.](media/adf-ir-setup-3.png 'Integration Runtime Setup step 3')

10. Under Option 2: Manual setup, copy the Key1 authentication key value by selecting the Copy button, then select **Close**.

    ![Beneath Option 2: Manual setup, the Key 1 textbox, and copy button are highlighted.](media/adf-ir-setup-4.png 'Integration Runtime Setup step 4')

    > **WARNING**: Don't close the current screen or browser session.

11. Paste the **Key1** value into the box in the middle of the Microsoft Integration Runtime Configuration Manager screen.

    ![The Microsoft Integration Runtime Configuration Manager Register Integration Runtime page displays with the Key1 value pasted into the textbox. A green checkmark next to the textbox denotes it's a valid key.](media/adf-ir-vm-register.png 'Microsoft Integration Runtime Configuration Manager')

12. Select **Register**.

13. It can take up to a minute or two to register. If it takes more than a couple of minutes, and the screen does not respond or returns an error message, close the screen by selecting the **Cancel** button.

14. The following screen will be New Integration Runtime (Self-hosted) Node. Select **Finish**.

    ![The Microsoft Integration Runtime Configuration Manager New Integration Runtime (Self-hosted) Node page displays with a Finish button.](media/adf-ir-self-hosted-node.png 'Microsoft Integration Runtime Configuration Manager')

15. Depending on your version of the Integration Runtime, you will be asked to specify a backup file. Select **Skip** and **OK**.

   ![The Integration Runtime Configuration Manager requests a backup credentials. Configuring backup settings is unncessary for this lab.](media/skip_ir_backup.png 'Skipping Integration Runtime backup settings')

16. You will then get a screen with a confirmation message. Select the **Launch Configuration Manager** button to view the connection details.

    ![The Microsoft Integration Runtime Configuration Manager Indicates the Integration Runtime (Self-hosted) node has been successfully registered. The Launch Configuration Manager button is highlighted.](media/adf-ir-launch-config-manager.png 'Microsoft Integration Runtime Configuration Manager')

    ![The Microsoft Integration Runtime Configuration Manager indicates the Self-hosted note is connected to the cloud service.](media/adf-ir-config-manager.png 'Microsoft Integration Runtime Configuration Manager')

17. You can now return to the Azure Data Factory page and view the Integration Runtime you just configured. You may need to select **Refresh** to view the Running status for the IR.

    ![In the Connections tab in Azure Data Factory, the Integration runtimes tab is selected, and the integration runtime bigdatagateway-initials is shown in the list.](media/adf-ir-running.png 'Integration Runtime in running state')

18. Select the Azure Data Factory Overview button on the menu. Leave this open for the next exercise.

    ![The Azure Data Factory Overview button is selected from the left menu.](media/adf-overview.png 'ADF Overview')

## Exercise 4: Develop a data factory pipeline for data movement

Duration: 20 minutes

In this exercise, you will create an Azure Data Factory pipeline to copy data (.CSV files) from an on-premises server (your machine) to Azure Blob Storage. The goal of the exercise is to demonstrate data movement from an on-premises location to Azure Storage (via the Integration Runtime).

### Task 1: Create copy pipeline using the Copy Data Wizard

1. Within the Azure Data Factory overview page, select **Ingest**.

   ![The Ingest item is from the Azure Data Factory overview page.](media/adf-copy-data-link.png 'Ingest')

2. Enter the **Properties** page
  
   - Select **Built-in copy task** (1)
   - Select **Schedule** below **Task cadence or task schedule** (2)
   - Set the **Start Date (UTC)** to **01/01/2021 12:00 AM** (3) 
   - Set the **Recurrence** to **Every 1 month** (4)
   - Below **Advanced recurrence options**, set **Hours** and **Minutes** to 0 (5)

   ![The Properties form for the copy data task is shown populated with the values outlined above.](media/adf-copy-data-properties.png 'Properties dialog box')

3. Select **Next (6)**.

4. On the Source data store screen, select **+ Create new connection**.

   ![The source form for the copy data task is shown. + Create new connection link is highlighted.](media/adf-copy-data-new-connection.png 'Data Source Selection')

5. Scroll through the options and select **File System (1)**, then select **Continue (2)**.

   ![In the New linked service list, File System is selected. The Continue button is selected.](media/adf-copy-data-new-linked-service.png 'Select File System')

6. In the New Linked Service form, enter the following:

   - **Name (1)**: `OnPremServer`

   - **Connect via integration runtime (2)**: Select the Integration runtime created previously in this exercise.

   - **Host (3)**: **C:\\Data**

   - **User name (4)**: Use your machine's login username.

   - **Password (5)**: Use your machine's login password.

   ![In the New linked service form, fields are populated with the values specified in Step 6. The Test connection button is highlighted.](media/adf-copy-data-linked-service-settings.png 'New Linked Service settings')

7. Select **Test connection (6)** to verify you correctly entered the values. Finally, select **Create (7)**.

8. On the Source data store page, select **Next**.

   ![On the Source data store page, OnPremServer is selected, and the Next button is highlighted.](media/adf-copy-data-source-next.png 'Select Next')

9. On the **Source data store** screen, select **Browse (1)**, then select the **FlightsAndWeather (2)** folder. Next, select **Load all files** under file loading behavior, check **Recursively**, then select **Next**.

   ![In the Source data store screen, the Browse button is highlighted. The File or Folder is set to FlightsAndWeather, the File loading behavior is set to Load all files, and the checkbox for Recursively is checked.](media/adf-copy-data-source-choose-input.png 'Choose the input file or folder page')

10. On the File format settings page, select the following options:

    - **File format (1)**: **Text format**

    - **Column delimiter**: **Comma (,)**

    - **Row delimiter**: **Default (\r, \n, or \r\n)**

    - **First row as header (2)**: **Checked**

    ![The File format settings form is displayed populated with the previously defined values.](media/adf-copy-data-file-format.png 'File format settings')

11. Select **Next (3)**.

12. On the Destination data store screen, select **+ New connection**.

13. Select **Azure Blob Storage (1)** within the New Linked Service blade, then select **Continue (2)**.

    ![In the New linked service list, Azure Blob Storage is selected, and the Continue button is highlighted.](media/adf-copy-data-blob-storage.png 'Select Blob Storage')

14. On the New Linked Service (Azure Blob Storage) account screen, enter the following, test your connection **(4)**, and then select **Create (5)**.

    - **Name (1)**: `BlobStorageOutput`

    - **Connect via integration runtime (2)**: Select your Integration Runtime.

    - **Authentication method**: Select **Account key**

    - **Account selection method (3)**: **From Azure subscription**

    - **Storage account name (3)**: Select the blob storage account you provisioned in the before-the-lab section.

    ![On the New linked service (Azure Blob storage) page, the fields are set to the previously defined values.](media/adf-copy-data-blob-storage-linked.png 'New Linked Service Blob Storage')

15. On the Destination data store page, configure the Blob Storage output path.

    - **Folder path (1)**: `sparkcontainer/FlightsAndWeather/{Year}/{Month}/`

    - **Filename (2)**: `FlightsAndWeather.csv`

    - **Year (3)**: **yyyy**

    - **Month (3)**: **MM**

    - **Copy behavior (4)**: **Merge files**

    - Select **Next (5)**.

      ![On the Destination data store form, fields are set to the previously defined values.](media/adf-copy-data-output-file-folder.png 'Destination data store page')

16. On the File format settings screen, select the **Text format (1)** file format, and check the **Add header to file (2)** checkbox, then select **Next (3)**. If present, leave **Max rows per file** and **File name prefix** at their defaults.

    ![On the File format settings page, the File Format is set to Text format, and the check box for Add header to file is selected. The Next button is selected.](media/adf-copy-data-file-format-settings.png 'File format settings page')

17. On the **Settings** screen, select **Skip incompatible rows (1)** under Fault tolerance, and uncheck **Enable logging (2)**. If present, keep **Data consistency verification** unchecked. Expand Advanced Settings and set Degree of copy parallelism to `10` **(3)**, then select **Next (4)**.
   
    ![In the Fault tolerance drop-down Skip incompatible rows is selected, and the Degree of copy parallelism is set to 10. The Next button is selected.](media/adf-copy-data-settings.png 'Settings page')

18. Review settings on the **Summary** tab, but **DO NOT choose Next**.

    ![The Summary page is displayed.](media/adf-copy-data-summary.png 'Summary page')

19. Scroll down on the summary page until you see the **Copy Settings (1)** section. Select **Edit (2)** next to **Copy Settings**.

    ![The Edit link is selected next to the Copy settings header.](media/adf-copy-data-review-page.png 'Summary page')

20. Change the following Copy setting:

    - **Retry (1)**: `3`

    - Select **Save (2)**.

      ![In the Copy settings form, the Retry textbox is set to 3, and the Save link is highlighted.](media/adf-copy-data-copy-settings.png 'Copy settings')

21. After saving the Copy settings, select **Next (3)** on the Summary tab.

22. On the **Deployment** screen, you will see a message that the deployment is in progress, and after a minute or two, the deployment is completed. Select **Edit Pipeline** to close out of the wizard and navigate to the pipeline editing blade.

    ![The Deployment screen indicates the deployment is complete.](media/adf-copy-data-deployment.png 'Deployment page')

## Exercise 5: Operationalize ML scoring with Azure Databricks and Data Factory

Duration: 20 minutes

In this exercise, you will extend the Data Factory to operationalize data scoring using the previously created machine learning model within an Azure Databricks notebook.

### Task 1: Create Azure Databricks Linked Service

1. Return to, or reopen the Author & Monitor page for your Azure Data Factory in a web browser, navigate to the Author view **(1)**, and select the `CopyOnPrem2AzurePipeline` pipeline **(2)**.

   ![Under Factory Resources, the CopyOnPrem2AzurePipeline pipeline is selected.](media/adf-ml-select-pipeline.png 'Select the ADF pipeline')

   >**Note**: You may need to rename your pipeline if you followed the steps above. Simply press the three dots next to the pipeline and select **Rename**. Use `CopyOnPrem2AzurePipeline` as the new pipeline name.

   ![Renaming the new Azure Data Factory Pipeline.](media/adf-pipeline-rename.png "Pipeline rename in ADF")

2. Once there, expand Databricks under Activities.

   ![Beneath Activities, the Databricks item is expanded.](media/adf-ml-expand-databricks-activity.png 'Expand Databricks Activity')

3. Drag the Notebook activity onto the design surface to the side of the Copy activity.

   ![The Notebook activity is dragged onto the design surface.](media/adf-ml-drag-notebook-activity.png 'Notebook on design surface')

4. Select the Notebook activity **(1)** on the design surface to display tabs containing its properties and settings at the bottom of the screen. On the **General (2)** tab, enter `BatchScore` into the Name **(3)** field.

   ![BatchScore is entered into the Name textbox under the General tab.](media/adf-ml-notebook-general.png 'Databricks Notebook General Tab')

5. Select the **Azure Databricks (1)** tab, and select **+ New (2)** next to the Databricks Linked service drop-down. Here, you will configure a new linked service that will serve as the connection to your Databricks cluster.

   ![In the Azure Databricks tab, the + New button is selected next to the Databricks Linked Service textbox.](media/adf-ml-settings-new-link.png 'Databricks Notebook Settings Tab')

6. On the New Linked Service dialog, enter the following:

   - **Name (1)**: `AzureDatabricks`
  
   - **Connect via integration runtime**: Leave set to Default.
  
   - **Account selection method**: **From Azure subscription**
  
   - **Azure subscription (2)**: Choose your Azure Subscription.
  
   - **Databricks workspace (3)**: Pick your Databricks workspace to populate the Domain automatically.
  
   - **Select cluster (4)**: **Existing interactive cluster**

   ![The New linked service form is shown populated with the previously listed values.](media/adf-ml-databricks-service-settings.png 'Databricks Linked Service settings')

7. Leave the form open and open your Azure Databricks workspace in another browser tab. You will generate and retrieve the Access token here.

8. In Azure Databricks, select the **BigDataLab (1)** in the top corner of the window, then select **User Settings (2)**.

   ![The BigDataLab button is selected in Azure Databricks. User Settings is selected from the list of Account options.](media/databricks-select-user-settings.png 'Azure Databricks user account settings')

9. Select **Generate New Token (1)** under the Access Tokens tab. Enter **ADF access (2)** for the comment and leave the lifetime at 90 days. Select **Generate (3)**.

   ![The Generate New Token modal is shown with the previously specified values.](media/databricks-generate-new-token.png 'Generate New Token')

10. **Copy** the generated token and **paste it into a text editor** such as Notepad for a later step. Select **Done**.

    ![The generated token is shown. The done button is highlighted.](media/databricks-copy-token.png 'Copy generated token')

11. Switch back to your Azure Data Factory screen and paste the generated token into the **Access token (1)** field within the form. After a moment, select your cluster underneath **Choose from existing clusters (2)**. Select **Create (3)**.

    ![In the New linked service form, the access token is pasted into the Access Token field, and the Azure Databricks Cluster (lab) is selected.](media/adf-ml-access-token.png 'Paste access token')

12. Switch back to Azure Databricks. Select **Workspace > Users > BigDataVis** in the menu. Select the **Exercise 5 (2)** folder, then open notebook **01 Deploy for Batch Scoring (3)**. Examine the content, but _don't run any of the cells yet_.

    ![In the Azure Databricks workspaces, beneath BigDataVis, the Exercise 5 folder is selected. Beneath Exercise 5, the 01 Deploy for Batch Score notebook is selected.](media/databricks-workspace-create-folder.png 'Create folder')

13. Replace **`STORAGE-ACCOUNT-NAME`** with the name of the blob storage account you copied in Exercise 1 into Cmd 4.

    ![Cmd 4 is shown. Storage account name placeholder is replaced with bigdatalabstore10.](media/databricks-storage-name.png 'Storage Account Name')

14. Switch back to your Azure Data Factory screen. Select the **Settings (1)** tab, then browse **(2)** to your **Exercise 5/01 Deploy for Batch Score** notebook **(3)** into the Notebook path field.

    ![In the Azure Data Factory pipeline designer, with the Notebook activity selected, the Settings tab is the active tab. The Browse button is selected next to the Notebook path.](media/adf-ml-notebook-path.png 'Notebook path')

15. The final step is to connect the **Copy data** activity with the **Notebook** activity. Select the small green box on the side of the copy activity, and drag the arrow onto the Notebook activity on the design surface. What this means is that the copy activity has to complete processing and generate its files in your storage account before the Notebook activity runs, ensuring the files required by the BatchScore notebook are in place at the time of execution. Select **Publish All (1)**, then **Publish** the **CopyOnPrem2AzurePipeline**, after making the connection.

    ![In the Azure Data Factory pipeline designer. The Copy Data activity is attached to the Notebook activity.](media/adf-ml-connect-copy-to-notebook.png 'Attach the copy activity to the notebook')

### Task 2: Trigger workflow

1. Switch back to Azure Data Factory. Select your pipeline if it is not already opened.

2. Select **Trigger**, then **Trigger Now** located above the pipeline design surface.

   ![In the taskbar for the Azure Data Factory pipeline designer, Trigger is selected, and Trigger Now is selected from the drop-down options.](media/adf-ml-trigger-now.png 'Trigger Now')

3. Enter `3/1/2017` into the **windowStart** parameter, then select **OK**.

   ![The Pipeline Run form is displayed with the windowStart parameter set to 3/1/2017.](media/adf-ml-pipeline-run.png 'Pipeline Run')

4. Select **Monitor (1)** in the menu. You will be able to see your pipeline activity in progress **(4)** as well as the status of past runs.

   ![From the left menu in Azure Data Factory, Monitor is selected. The current status of the pipeline run is displayed in the table.](media/adf-ml-monitor.png 'Monitor')

   > **Note**: You may need to restart your Azure Databricks cluster if it has automatically terminated due to inactivity.

## Exercise 6: Summarize data using Azure Databricks

Duration: 10 minutes

In this exercise, you will prepare a summary of flight delay data using Spark SQL.

### Task 1: Summarize delays by airport

1. Open your Azure Databricks workspace, expand the **Exercise 6** **(2)** folder, and open the final notebook called **01 Explore Data** **(03)**.

   ![Beneath the BigDataVis workspace, the Exercise 6 folder is selected. Beneath the Exercise 6 folder, the 01 Explore Data notebook highlighted.](media/azure-databricks-explore-data.png 'Databricks workspace')

2. Execute each cell and follow the instructions in the notebook that explains each step.

## Exercise 7: Visualizing in Power BI Desktop

Duration: 20 minutes

In this exercise, you will create visualizations in Power BI Desktop.

### Task 1: Obtain the JDBC connection string to your Azure Databricks cluster

Before you begin, you must first obtain the JDBC connection string to your Azure Databricks cluster.

1. In Azure Databricks, go to Clusters and select your cluster.

2. On the cluster edit page, in the **Configuration** tab, scroll down to the bottom of the page, expand **Advanced Options**, then select the **JDBC/ODBC** tab.

3. On the **JDBC/ODBC** tab, copy and save the **Server Hostname (1)** and **HTTP Path (2)** to be used during the next task. You can use a text editor such as Notepad to keep the values for later use.

   ![An image of the JDBC URL with the necessary values for the new Power BI connection string selected.](media/databricks-power-bi-spark-information.png 'Construct Power BI connection string')

### Task 2: Connect to Azure Databricks using Power BI Desktop

1. If you did not already do so during the before the hands-on lab setup, download Power BI Desktop from <https://powerbi.microsoft.com/en-us/desktop/>.

2. When Power BI Desktop starts, you will need to enter your personal information or Sign in if you already have an account.

   ![The Power BI Desktop Welcome page displays prompting the user for personal details.](media/image177.png 'Power BI Desktop Welcome page')

3. Select Get data on the screen that is displayed next.

   ![On the Power BI Desktop Sign-in page, the Get data item is selected.](media/powerbi-getdata.png 'Power BI Desktop Sign in page')

4. Select **Azure Databricks** from the list of available data sources. You may enter `databricks` into the search field **(1)** to find it faster.

   ![In the Get Data screen, Spark is selected from the list of available sources.](media/pbi-desktop-get-data.png 'Get Data page')

5. Select **Connect**.

6. On the next screen, you will be prompted for your Azure Databricks cluster information.

7. On the Azure Databricks connection information dialog, enter the following:

   - **Server Hostname (1)**: Paste the JDBC **Server Hostname** value you copied in the previous task.
  
   - **HTTP Path**: Paste the JDBC **HTTP Path** value you copied in the previous task.
  
   - **Data Connectivity mode**: Select **DirectQuery (2)** for the Data Connectivity mode. This option will offload query tasks to the Azure Databricks Spark cluster, providing near-real-time querying.
  
   ![The Spark form is populated with the Server, Protocol, and Data Connectivity mode specified in the previous steps.](media/pbi-desktop-connect-databricks.png 'Spark form')

8. Select **OK (3)**.

9. Enter your credentials on the next screen as follows:

    - **User name**: `token`

    - **Password**: Remember that ADF Access token we generated for the Azure Data Factory notebook activity? Paste the same value here for the password.

    ![The Generate New Token form from when we generated the access token.](media/databricks-copy-token.png 'Copy generated token')

    !["token" is entered for the username, and the access token is pasted into the password field.](media/pbi-desktop-login.png 'Enter credentials')

10. Select **Connect**.

11. In the Navigator dialog, check the box next to **flight_delays_summary (1)**, and select **Load (2)**.

    ![In the Navigator dialog box, in the pane under Display Options, the check box for flight_delays_summary is selected. In the pane, the table of flight delays summary information displays.](media/pbi-desktop-select-table-navigator.png 'Navigator dialog box')

### Task 3: Create Power BI report

1. Once the data finishes loading, you will see the fields appear on the far side of the Power BI Desktop client window.

   ![The Power BI Desktop Fields pane displays the fields from the flight_delays_summary table.](media/pbi-desktop-fields.png 'Power BI Desktop Fields')

2. From the Visualizations area, next to Fields, select the Globe icon to add a Map visualization to the report design surface.

   ![On the Power BI Desktop Visualizations palette, the globe icon is selected.](media/pbi-vis-map.png 'Power BI Desktop Visualizations palette')

3. With the Map visualization still selected, drag the **OriginLatLong** field to the **Location** field under Visualizations. Then Next, drag the **NumDelays** field to the **Size** field under Visualizations.

   ![In the Fields column, the checkboxes for NumDelays and OriginLatLong are selected. An arrow points from OriginLatLong in the Fields column to OriginLatLong in the Visualization's Location field. A second arrow points from NumDelays in the Fields column to NumDelays in the Visualization's Size field.](media/pbi-desktop-configure-map-vis.png 'Visualizations and Fields columns')

4. You should now see a map that looks similar to the following (resize and zoom on your map if necessary):

   ![On the Report design surface, a Map of the United States displays with varying-sized dots over different cities.](media/pbi-desktop-map-vis.png 'Report design surface')

5. Unselect the Map visualization by selecting the white space next to the map in the report area.

6. From the Visualizations area, select the **Stacked Column Chart** icon to add a bar chart visual to the report's design surface.

   ![The stacked column chart icon is selected on the Visualizations palette.](media/pbi-vis-stacked.png 'Visualizations palette')

7. With the Stacked Column Chart still selected, drag the **DayofMonth** field and drop it into the **Axis** field located under Visualizations.

8. Next, drag the **NumDelays** field over, and drop it into the **Value** field.

   ![In the Fields column, the checkboxes for NumDelays and DayofMonth are selected. An arrow points from NumDelays in the Fields column to NumDelays in the Visualization's Axis field. A second arrow points from DayofMonth in the Fields column to DayofMonth in the Visualization's Value field.](media/pbi-desktop-configure-stacked-vis.png 'Visualizations and Fields columns')

9. Grab the corner of the new Stacked Column Chart visual on the report design surface, and drag it out to make it as wide as the bottom of your report design surface. It should look something like the following.

   ![On the Report Design Surface, under the United States map with dots, a stacked bar chart displays.](media/pbi-desktop-stacked-vis.png 'Report Design Surface')

10. Unselect the Stacked Column Chart visual by selecting the white space next to the map on the design surface.

11. From the Visualizations area, select the Treemap icon to add this visualization to the report.

    ![On the Visualizations palette, the Treemap icon is selected.](media/pbi-vis-treemap.png 'Visualizations palette')

12. With the Treemap visualization selected, drag the **OriginAirportCode** field into the **Group** field under Visualizations.

13. Next, drag the **NumDelays** field over, and drop it into the **Values** field.

    ![In the Fields column, the checkboxes for NumDelays and OriginAirportcode are selected. An arrow points from NumDelays in the Fields column to NumDelays in the Visualization's Values field. A second arrow points from OriginAirportcode in the Fields column to OriginAirportcode in the Visualization's Group field.](media/pbi-desktop-config-treemap-vis.png 'Visualizations and Fields columns')

14. Grab the corner of the Treemap visual on the report design surface, and expand it to fill the area between the map and the side edge of the design surface. The report should now look similar to the following.

    ![The Report design surface now displays the map of the United States with dots, a stacked bar chart, and a Treeview.](media/pbi-desktop-full-report.png 'Report design surface')

15. You can cross filter any of the visualizations on the report by selecting one of the other visuals within the report, as shown below (This may take a few seconds to change as the data is loaded).

    ![The map on the Report design surface is now zoomed in on the northeast section of the United States, and the only dot on the map is on Chicago. In the Treeview, all cities except ORD are grayed out. In the stacked bar graph, each bar is now divided into a darker and a lighter color, with the darker color representing the airport.](media/pbi-desktop-full-report-filter.png 'Report design surface')

16. You can save the report by choosing Save from the File menu and entering a name and location for the file.

    ![The Power BI Save as window displays.](media/image197.png 'Power BI Save as window')

## Exercise 8: Deploy intelligent web app (Optional Lab)

Duration: 20 minutes

In this exercise, you will deploy an intelligent web application to Azure from GitHub. This application leverages the operationalized machine learning model deployed in Exercise 1 to bring action-oriented insight into an already existing business process.

> **Please note**: If you are running your lab in a hosted Azure environment and do not have permissions to create a new Azure resource group, the automated deployment task (#2 below) may fail, even if you choose an existing resource group. The automated deployment will also fail if the user you are logged into the portal with is **not** a Service Administrator or a Co-Administrator. If this happens, we recommend that you install [Visual Studio 2017/2019 Community](https://visualstudio.microsoft.com/downloads/) or greater, then use the [Publish feature](https://docs.microsoft.com/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019) to publish to a new Azure web app. You will then need to create and populate two new Application Settings as outlined in the tasks that follow: `mlUrl` and `weatherApiKey`. **Skip ahead to Task 3 for further instructions.**

### Task 1: Register for an OpenWeather account

To retrieve the 5-day hourly weather forecast, you will use an API from OpenWeather. There is a free version that provides you access to the API you need for this hands-on lab.

1. Navigate to <https://openweathermap.org/home/sign_up>.

2. Complete the registration form by providing your desired username, email address, and a password. Verify you are 16 years old and over, and agree to the privacy policy and terms of conditions. Select **Create Account**. If you already have an account, select **Sign in** at the top of the page instead.

   ![The registration form is displayed.](media/openweather-register.png "Create New Account")

3. Check your email account you used for registration. You should have a confirmation email from OpenWeather. Open the email and follow the email verification link within to complete the registration process. When the welcome page loads, log in with your new account.

   ![The OpenWeather confirmation and account login page is displayed.](media/openweather-confirmation-login.png "Sign In To Your Account")

4. After logging in, select the **API keys** tab. Take note of your Default **Key** and copy it to a text editor such as Notepad for later. You will need this key to make API calls later in the lab.

   ![The OpenWeather API keys page is displayed.](media/openweather-api-keys.png "API keys")

5. To verify that your API Key is working, replace **{YOUR API KEY}** in the following URL and paste the updated path to your browser's navigation bar: `https://api.openweathermap.org/data/2.5/onecall?lat=37.8267&lon=-122.4233&appid={YOUR API KEY}`. You should see a JSON result that looks similar to the following:

   >**Note**: If you send this request immediately after key creation, you may encounter a 401 response code. If so, wait for a couple of minutes.

   ![The OpenWeather API call displays JSON data for an API call for Los Angeles weather.](media/openweather-api-results.png "OpenWeather API results")

### Task 2: Deploy web app from GitHub

1. Navigate to <https://github.com/Microsoft/MCW-Big-data-and-visualization/blob/master/Hands-on%20lab/lab-files/BigDataTravel/README.md> in your browser of choice, but where you are already authenticated to the Azure portal.

2. Read through the README information on the GitHub page.

3. Select **Deploy to Azure** button in the Readme.md file.

   ![Screenshot of the Deploy to Azure button.](media/deploy-to-azure-button.png 'Deploy to Azure button')

4. On the following page, ensure the fields are populated correctly.

   - Ensure the correct Directory and Subscription are selected.

   - Select the Resource Group **(1)** that you have been using throughout this lab.

   - Either keep the default Site name, or provide one that is globally unique, and then choose a Site Location.

   - Enter the **OpenWeather API Key (3)**.

   - Finally, enter the **ML URL (3)**. We got this from Azure databricks Notebook #3 in the Exercise 2 folder. If you cleaned your resources at the end of this Notebook #3, you will need to re-run it and keep the web service running to get its associated URL.

      ![The web service URL is output from a cell within the Databricks notebook.](media/azure-databricks-web-url.png 'Web service URL')

   ![Fields on the Deploy to Azure page are populated with the previously copied information.](media/azure-deployment-form.png 'Deploy to Azure page')

5. Select **Review + create (4)**, and on the following screen, select **Create**.

6. The page should begin deploying your application while showing you a status of what is currently happening.

   > **Note**: If you run into errors during the deployment that indicate a bad request or unauthorized, verify that the user you are logged into the portal with an account that is either a Service Administrator or a Co-Administrator. You won't have permissions to deploy the website otherwise.

7. After a short time, the deployment will complete, and you will be able to access the web site.

8. Try a few different combinations of origin, destination, date, and time in the application. The information you are shown is the result of both the ML API you published, as well as information retrieved from the OpenWeather API.

   ![The Margie's Travel web app is displayed.](media/webapp.png "Web App")

9. Congratulations! You have built and deployed an intelligent system to Azure.

### Task 3: Manual deployment (optional)

**If the automated deployment from GitHub in the previous task failed**, follow these instructions to manually deploy.

1. Install [Visual Studio 2017/2019 Community](https://visualstudio.microsoft.com/downloads/) or greater. Make sure you select the **ASP.NET and web development** and **Azure development** workloads.

   ![The Visual Studio workloads are displayed with ASP.NET and web development and Azure development workloads selected.](media/vs-workloads.png 'Visual Studio workloads')

   > **Note**: If you are prompted to sign in to Visual Studio for the first time, enter the Azure account credentials you are using for this lab.

2. In a web browser, navigate to the [Big data and visualization MCW repo](https://github.com/microsoft/MCW-Big-data-and-visualization).

3. On the repo page, select **Clone or download**, then select **Download ZIP**.

   ![The Github repository page is displayed with the Clone or Download item expanded and the Download Zip button highlighted.](media/github-download-repo.png 'Download ZIP')

4. Unzip the contents to your root hard drive (i.e., `C:\`). This will create a folder on your root drive named `C:\MCW-Big-data-and-visualization-master`.

5. Open Windows Explorer and navigate to `C:\MCW-Big-data-and-visualization-master\Hands-on lab\lab-files\BigDataTravel\`, then open **BigDataTravel.sln**.

6. In the Visual Studio Solution Explorer, right-click on the BigDataTravel project, then select **Publish...**.

   ![In Solution Explorer, the BigDataTravel project context menu is expanded with the Publish menu item selected.](media/vs-publish-link.png 'Publish')

7. In the Publish dialog, select the **Azure (1)** as your publish target, select **Next (2)** to continue.

   ![In the Pick a publish target window, Azure is selected. The Next button is highlighted.](media/vs-publish-target.png 'Pick a publish target')

8. In the Specific target step, select the **Azure App Service (Windows) (1)** as your publish target, select **Next (2)** to continue.

   ![In the Pick a publish target window, the App Service item is selected. In the Azure App Service form, Create New is selected, and the Publish button is highlighted.](media/vs-publish-target-app-service.png 'Pick a publish target')

9. In the App Service selection step, select the **+** **(1)** plus button to create a new App Service instance.

   > **WARNING**: When asked login with your Azure Subscription credentials.

   ![App Service selection screen is open. Plus button to create a new app service instance is highlighted.](media/vs-publish-new-app-service.png 'Create new app service instance.')

10. Enter the following into the App Service form that follows, then select **Create**:

    - **Name (1)**: Enter a unique value.

    - **Subscription (2)**: Choose the Azure subscription you are using for the lab.

    - **Resource group (3)**: Select the Azure resource group you are using for the lab.

    - **Hosting Plan (4)**: Select **New**, then create a new Hosting Plan in the same location and the **Free** size.

    ![The App Service Create new form is displayed populated with the values listed above.](media/vs-app-service.png 'App Service dialog')

11. Select the newly created App Service instance **(1)** and select **Finish (2)** to go back to Visual Studio.

    ![The newly create App Service Instance is selected. Finish button is highlighted.](media/vs-publish-app-service-select.png 'App Service Selection')

12. Once you are back in Visual Studio select **Publish** to start the publishing process.

    ![Visual Studio publish screen is visible. Publish button is highlighted.](media/vs-publish-start.png 'Visual Studio Publish')

    > **Note**: If you receive an error concerning the Roslyn compiler, open the NuGet package manager interface (**Tools --> NuGet Package Manager --> Package Manager Console**) and run the command below to update the package. Then, publish the application again.
    >
    > ```PowerShell
    > Update-Package Microsoft.CodeDom.Providers.DotNetCompilerPlatform -r
    > ```

13. After publishing is completed, open the new App Service located in your resource group in the [Azure portal](https://portal.azure.com).

14. Select **Configuration** in the left-hand menu.

    ![In the App Service resource screen, the Configuration menu item is highlighted.](media/app-service-configuration-link.png 'App Service')

15. Select **Configuration (1)** in the left-hand menu. Create the two following **Application settings (3)**, then select **Save (4)**:

    - **mlUrl**: Enter the Machine Learning URL. We got this from Azure databricks Notebook #3 in the Exercise 2 folder. If you cleaned your resources at the end of this Notebook #3, you will need to re-run it and keep the web service running to get its associated URL.
  
    - **weatherApiKey**: Enter the OpenWeather API key.

    ![In the Application settings section, the two new application settings described above are highlighted.](media/app-service-configuration.png 'Application settings')

You will now be able to successfully navigate the web app.

## After the hands-on lab

Duration: 10 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab.

### Task 1: Delete resource group

1. Using the Azure portal, navigate to the Resource group you used throughout this hands-on lab by selecting **Resource groups** in the menu.

2. Search for the name of your research group and select it from the list.

3. Select **Delete** in the command bar and confirm the deletion by re-typing the Resource group name and selecting **Delete**.

You should follow all steps provided _after_ attending the Hands-on lab.

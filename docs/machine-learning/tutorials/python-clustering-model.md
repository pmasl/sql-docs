---
title: "Python tutorial: Categorize customers"
titleSuffix: SQL machine learning
description: In this four-part tutorial series, you'll  cluster customers, using K-Means, in a database using Python with SQL machine learning.
ms.service: sql
ms.subservice: machine-learning
ms.devlang: python
ms.date: 05/26/2020
ms.topic: tutorial
author: WilliamDAssafMSFT
ms.author: wiassaf

ms.custom: seo-lt-2019
monikerRange: ">=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
---
# Python tutorial: Categorizing customers using k-means clustering with SQL machine learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In this four-part tutorial series, you'll use Python to develop and deploy a K-Means clustering model in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) or on [Big Data Clusters](../../big-data-cluster/machine-learning-services.md) to categorize customer data.
::: moniker-end
::: moniker range="=sql-server-2017"
In this four-part tutorial series, you'll use Python to develop and deploy a K-Means clustering model in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) to cluster customer data.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In this four-part tutorial series, you'll use Python to develop and deploy a K-Means clustering model in [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) to cluster customer data.
::: moniker-end

In part one of this series, you'll set up the prerequisites for the tutorial and then restore a sample dataset to a database. Later in this series, you'll use this data to train and deploy a clustering model in Python with SQL machine learning.

In parts two and three of this series, you'll develop some Python scripts in an Azure Data Studio notebook to analyze and prepare your data and train a machine learning model. Then, in part four, you'll run those Python scripts inside a database using stored procedures.

*Clustering* can be explained as organizing data into groups where members of a group are similar in some way. For this tutorial series, imagine you own a retail business. You'll use the **K-Means** algorithm to perform the clustering of customers in a dataset of product purchases and returns. By clustering customers, you can focus your marketing efforts more effectively by targeting specific groups. K-Means clustering is an *unsupervised learning* algorithm that looks for patterns in data based on similarities.

In this article, you'll learn how to:

> [!div class="checklist"]
> * Restore a sample database

In [part two](python-clustering-model-prepare-data.md), you'll learn how to prepare the data from a database to perform clustering.

In [part three](python-clustering-model-build.md), you'll learn how to create and train a K-Means clustering model in Python.

In [part four](python-clustering-model-deploy.md), you'll learn how to create a stored procedure in a database that can perform clustering in Python based on new data.

## Prerequisites

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) with the Python language option - Follow the installation instructions in the [Windows installation guide](../install/sql-machine-learning-services-windows-install.md) or the [Linux installation guide](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true). You can also [enable Machine Learning Services on SQL Server Big Data Clusters](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) with the Python language option - Follow the installation instructions in the [Windows installation guide](../install/sql-machine-learning-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Azure SQL Managed Instance Machine Learning Services. For information, see the [Azure SQL Managed Instance Machine Learning Services overview](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) for restoring the sample database to Azure SQL Managed Instance.
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). You'll use a notebook in Azure Data Studio for both Python and SQL. For more information about notebooks, see [How to use notebooks in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* Additional Python packages - The examples in this tutorial series use Python packages that you may or may not have installed.

  Open a **Command Prompt** and change to the installation path for the version of Python you use in Azure Data Studio. For example, `cd %LocalAppData%\Programs\Python\Python37-32`. Then run the following commands to install any of these packages that are not already installed.

  ```console
  pip install matplotlib
  pip install pandas
  pip install pyodbc
  pip install scipy
  pip install sklearn
  ```

## Restore the sample database

The sample dataset used in this tutorial has been saved to a **.bak** database backup file for you to download and use. This dataset is derived from the [tpcx-bb](http://www.tpc.org/tpcx-bb/default5.asp) dataset provided by the [Transaction Processing Performance Council (TPC)](http://www.tpc.org/).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> If you are using Machine Learning Services on Big Data Clusters, see how to [Restore a database into the SQL Server big data cluster master instance](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Download the file [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Follow the directions in [Restore a database from a backup file](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) in Azure Data Studio, using these details:

   * Import from the **tpcxbb_1gb.bak** file you downloaded
   * Name the target database "tpcxbb_1gb"

1. You can verify that the dataset exists after you have restored the database by querying the **dbo.customer** table:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Download the file [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Follow the directions in [Restore a database to a Managed Instance](/azure/sql-database/sql-database-managed-instance-get-started-restore) in SQL Server Management Studio, using these details:

   * Import from the **tpcxbb_1gb.bak** file you downloaded
   * Name the target database "tpcxbb_1gb"

1. You can verify that the dataset exists after you have restored the database by querying the **dbo.customer** table:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## Clean up resources

If you're not going to continue with this tutorial, delete the tpcxbb_1gb database.

## Next steps

In part one of this tutorial series, you completed these steps:

* Restore a sample database

To prepare the data for the machine learning model, follow part two of this tutorial series:

> [!div class="nextstepaction"]
> [Python tutorial: Prepare data to perform clustering](python-clustering-model-prepare-data.md)
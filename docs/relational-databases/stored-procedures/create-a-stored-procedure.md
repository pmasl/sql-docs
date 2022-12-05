---
title: "Create a Stored Procedure"
description: Learn how to create a Transact-SQL stored procedure by using SQL Server Management Studio and by using the Transact-SQL CREATE PROCEDURE statement.
ms.custom: intro-quickstart, FY22Q2Fresh
ms.date: 10/25/2021
ms.service: sql
ms.reviewer: ""
ms.subservice: stored-procedures
ms.topic: quickstart
helpviewer_keywords:
  - "new stored procedures"
  - "stored procedures [SQL Server], creating"
  - "creating stored procedures"
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: ">=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"
---
# Create a stored procedure

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


This topic describes how to create a [!INCLUDE[tsql](../../includes/tsql-md.md)] stored procedure by using [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] and by using the [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE PROCEDURE statement.  
  
## Permissions  
 Requires CREATE PROCEDURE permission in the database and ALTER permission on the schema in which the procedure is being created.  
  
## How to create a stored procedure  
 You can use one of the following:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="SSMSProcedure"></a> Using SQL Server Management Studio  
 **To create a procedure in Object Explorer**  
  
1.  In **Object Explorer**, connect to an instance of [!INCLUDE[ssDE](../../includes/ssde-md.md)] and then expand that instance.  
  
2.  Expand **Databases**, expand the [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] database, and then expand **Programmability**.  
  
3.  Right-click **Stored Procedures**, and then click **New Stored Procedure**.  
  
4.  On the **Query** menu, click **Specify Values for Template Parameters**.  
  
5.  In the **Specify Values for Template Parameters** dialog box, enter the following values for the parameters shown.  
  
    |Parameter|Value|  
    |---------------|-----------|  
    |Author|*Your name*|  
    |Create Date|*Today's date*|  
    |Description|Returns employee data.|  
    |Procedure_name|HumanResources.uspGetEmployeesTest|  
    |@Param1|@LastName|  
    |@Datatype_For_Param1|**nvarchar**(50)|  
    |Default_Value_For_Param1|NULL|  
    |@Param2|@FirstName|  
    |@Datatype_For_Param2|**nvarchar**(50)|  
    |Default_Value_For_Param2|NULL|  
  
6.  Click **OK**.  
  
7.  In the **Query Editor**, replace the SELECT statement with the following statement:  
  
    ```sql  
    SELECT FirstName, LastName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory  
    WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    ```  
  
8.  To test the syntax, on the **Query** menu, click **Parse**. If an error message is returned, compare the statements with the information above and correct as needed.  
  
9. To create the procedure, from  the **Query** menu, click **Execute**. The procedure is created as an object in the database.  
  
10. To see the procedure listed in Object Explorer, right-click **Stored Procedures** and select **Refresh**.  
  
11. To run the procedure, in Object Explorer, right-click the stored procedure name **HumanResources.uspGetEmployeesTest** and select **Execute Stored Procedure**.  
  
12. In the **Execute Procedure** window, enter Margheim as the value for the parameter @LastName and enter the value Diane as the value for the parameter @FirstName.  
  
> [!WARNING]  
>  Validate all user input. Do not concatenate user input before you validate it. Never execute a command constructed from unvalidated user input.  
  
###  <a name="TsqlProcedure"></a> Using Transact-SQL  
 **To create a procedure in Query Editor**  
  
1.  In **Object Explorer**, connect to an instance of [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  From the **File** menu, click **New Query**.  
  
3.  Copy and paste the following example into the query window and click **Execute**. This example creates the same stored procedure as above using a different procedure name.  
  
    ```sql 
    USE AdventureWorks2012;  
    GO  
    CREATE PROCEDURE HumanResources.uspGetEmployeesTest2   
        @LastName nvarchar(50),   
        @FirstName nvarchar(50)   
    AS   
  
        SET NOCOUNT ON;  
        SELECT FirstName, LastName, Department  
        FROM HumanResources.vEmployeeDepartmentHistory  
        WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    GO  
  
    ```  
  
4.  To run the procedure, copy and paste the following example into a new query window and click **Execute**. Notice that different methods of specifying the parameter values are shown.  
  
    ```sql  
    EXECUTE HumanResources.uspGetEmployeesTest2 N'Ackerman', N'Pilar';  
    -- Or  
    EXEC HumanResources.uspGetEmployeesTest2 @LastName = N'Ackerman', @FirstName = N'Pilar';  
    GO  
    -- Or  
    EXECUTE HumanResources.uspGetEmployeesTest2 @FirstName = N'Pilar', @LastName = N'Ackerman';  
    GO  
  
    ```  
  
## Next steps  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
  

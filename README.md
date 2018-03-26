# A method for creating a simple images database application using Microsoft Access 2016 and SQL Server 2008R2
## Overview
This article describes a way to create an image management database and application using Microsoft Access 2016 and Microsoft SQL Server 2008R2.  The method relies on SQL Server's ability to store binary data in a varbinary(max) data type.  The trick is to stream the 1s and 0s in the image file into a database table's varbinary(max) column and then describe a method to convert the data back into an image for presentation in an application. Let's start by creating the database table.  I used SQL Server 2008R2 but any recent version will be similar.
## Create the database table
Create a SQL Server database to store the image files, then execute the following CREATE TABLE script.  The result will be a database table with three columns; ImageID, SurveyImage and Filename.  The SurveyImage column is the varbinary(max) column that will store the images.
```
CREATE TABLE [dbo].[Images](
	[ImageID] [int] IDENTITY(1,1) NOT NULL,
	[SurveyImage] [varbinary](max) NULL,
	[Filename] [varchar](255) NULL,
 CONSTRAINT [PK_SurveyImages] PRIMARY KEY CLUSTERED 
(
	[ImageID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```
## Create an ODBC data source  
In order to use Access as a front end to your database you will have to create an ODBC data source on your computer that links to your database. The ODBC data source is the intermediate software that Access will use to communicate with your database. Use Microsoft's documentation to accomplish this task.  
## Create an Access application
Create a new Access application. Using the 'External Data' tab at the top of the application, select 'ODBC Database' and navigate to the ODBC data source you created above. Use Microsoft's documentation on linking Access to SQL Server table. Make sure you 'Link' to the data source rather than 'Import'. Your Images table will show up in Access when you have successfully linked to your images database.


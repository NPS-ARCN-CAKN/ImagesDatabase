# A method for creating a simple images database application using Microsoft Access 2016 and SQL Server 2008R2
## Overview
This article describes a way to create an image management database and application using Microsoft Access 2016 and Microsoft SQL Server 2008R2.  The method relies on SQL Server's ability to store binary data in a VarBinary(Max) data type. Images are stored in the database tables, rather than as individual files on a file system. The trick is to stream the binary data (1s and 0s) in an image file into a database table's VarBinary(Max) column. The advantages are numerous:
- Images and metadata can stored in the same record.
- All images and associated data travel together in a single database package.  
-  No need to worry about synchronizing image file paths with database records the way you would if you stored image metadata in a database but images in a file system.
There are some drawbacks, however
- The method described here requires a good deal of technical skill.  Converting images to and from bit streams for everyday use requires a good deal of knowledge.
- SQL Server has no 'knowledge' of what data is in a VarBinary(Max) column type.  The data could just as easily be a PDF, a spreadsheet or an image file.
Hopefully the method described here is useful.   From there I describe a method to convert the data back into an image for presentation in an application. Let's start by creating the database table.  I used SQL Server 2008R2 but any recent version will be similar.
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


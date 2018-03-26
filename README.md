# A method for creating a simple images database application using Microsoft Access 2016 and SQL Server 2008R2
## Overview
This repository describes a way to create an image management database and application using Microsoft Access 2016 and Microsoft SQL Server 2008R2.  The method relies on SQL Server's ability to store binary data in a VarBinary(Max) data type. Images are stored in the database tables, rather than as individual files on a file system. The trick is to stream the binary data (1s and 0s) in an image file into a database table's VarBinary(Max) column. The advantages are numerous:
- Images and metadata can stored in the same record.
- All images and associated data travel together in a single database package.  
-  No need to worry about synchronizing image file paths with database records the way you would if you stored image metadata in a database but images in a file system.

There are some drawbacks, however

- The method described here requires a good deal of technical skill.  Converting images to and from bit streams for everyday use requires a good deal of knowledge.
- SQL Server has no 'understanding' of what data is in a VarBinary(Max) column type.  The data could just as easily be a PDF, a spreadsheet or an image file. To SQL Server your data is simply a long string of ones and zeroes.

If you think this method is for you please find in this repository a working example Access/VBA application, a create table query to generate the Images table used in the example and a [Word document](Access_SQLServer_ImagesDatabase.docx) describing the method in detail.

To create a table in stagging db each time  i need pause the adg to convert into snapshot mode so in this case its not doable best way to check is one shot exporting


Possibilities from DBA

First way:

Filter based Export into dumpfiles and then import into Schema R (to speed up export we need indexes on filtered columns)  

important: 
1.need to check Indexes in the queries before running
2.before exporting need to create indexes for quicker way
3.If its Big tables and selecting less size of data definitely need to check with indexes and create first 

Steps for this way 

need to list out the indexes first and create indexes and then start exporting.
start export then import 


Second way:

Creating stagging tables in stagging database and then exporting and importing into schema R  (additionally we need to export indexes and constrains separately beacuese while we are creating tables it will create only table structure and table data)

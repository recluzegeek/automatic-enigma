---
title: Questions (Q?A)
updated: 2023-01-08 11:52:27Z
created: 2023-01-08 10:42:42Z
latitude: 51.50735090
longitude: -0.12775830
altitude: 0.0000
---



## 1.1. What is data independence? Explain the difference between physical and logical data independence.
**Database Management Systems:**
While a database is an organized collection of data, a database management system (DBMS) is the software that allows users to create, access, use, and modify a database. Other software programs usually access a database through a DBMS. A database schema is a logical structure that defines how the data in the database is organized and how its relationships are defined. Database schemas have different levels, with physical or internal schemas at the lowest level, then logical or conceptual schemas, and finally external schemas at the highest level.
**Answer and Explanation:**
<p style="font-family:dubai; color:#9f7f9f; font-size:18px">Data independence is the property of a database management system that allows changes to be made in the DBMS schemas without requiring any changes in higher schemas of the DBMS or in any software that uses that DBMS.</p>

### Types of Database Independance
   **1. Physical Data Independence**: changes made to the physical schema do not require any changes to be made to the logical or external schemas. The physical schema describes how data is stored. For example, reorganizing the file structure of a database does not affect the logical or external schemas.
    **2. Logical Data Independence**: changes made to the logical schema do not require any changes to be made to external schemas or the way external software programs use the DBMS. In this case, adding or deleting data items in a data table changes the logical schema but does not affect the external schema.


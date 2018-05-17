# Triumviate Environmental Waste Collection PaaS Application

## Purpose of this application

This app was built to demonstrate how PaaS mobile technology can be used to allow employees of Triumvirate Environmental to record usage of supplies, materials and labor when collecting hospital waste.

The application was built with a responsive design as it's assumed that usage would mainly be with mobile devices.

The application is fully self-contained and does not require any external integration to SaaS Cloud application API's or third-party API's. A future enhancement may be to integrate the data collected via this app to Cloud Projects but thus far that has not been requested.

## How to install

The application can be imported into any Applications Express workspace by navigating to the Application Builder tab and importing the trimvirate.sql file. There will be an option to import supporting objects (i.e. database objects such as tables, sequences, plsql packages etc.) and this option should be checked 'yes'.

## Application Components

### Database Tables

This application uses the following tables:

#### TRI_JOBS

This table holds top-level information for each job. Each job can and will have multiple associated visits.
```
CREATE TABLE  "TRI_JOBS" 
   (	
  "JOB_ID" NUMBER, 
	"JOB_NUMBER" VARCHAR2(15 CHAR), 
	"JOB_NAME" VARCHAR2(30 CHAR)
   )
/
```

#### TRI_JOB_VISITS

This table holds header-level informatuon for individual job visits. Job visits will have header info and records in ```TRI_JOB_LABOR```, ```TRI_JOB_SUPPLIES``` and ```TRI_WASTE_INFORMATION```.

```
CREATE TABLE  "TRI_JOB_VISITS" 
   (	
  "JOB_ID" NUMBER, 
	"VISIT_ID" NUMBER, 
	"VISIT_DATE" DATE, 
	"SITE_SUPERVISOR" VARCHAR2(60 CHAR), 
	"DIRECT_SHIP_FLAG" VARCHAR2(1 CHAR), 
	"LESS_THAN_FULL_FLAG" VARCHAR2(1 CHAR), 
	"LAB_PACK_FLAG" VARCHAR2(1 CHAR), 
	"DOWNTIME_FLAG" VARCHAR2(1 CHAR), 
	"DOWNTIME_REASON" VARCHAR2(240 CHAR), 
	"VERIFIED" VARCHAR2(1 CHAR)
   )
/
```
#### TRI_JOB_LABOR

This table holds individual employee's time associated with each job visit.

```
CREATE TABLE  "TRI_JOB_LABOR" 
   (	
  "JOB_ID" NUMBER, 
	"VISIT_ID" NUMBER, 
	"LABOR_LINE_ID" NUMBER, 
	"EMPLOYEE" VARCHAR2(60 CHAR), 
	"VISIT_DATE" DATE, 
	"POSITION" VARCHAR2(30), 
	"VEHICLE_NUMBER" VARCHAR2(30), 
	"LOAD_TRAVEL" NUMBER, 
	"ONSITE" NUMBER, 
	"LUNCH" NUMBER, 
	"UNLOAD" NUMBER, 
	"TRAVEL" NUMBER, 
	"ONSITE_TSDF" NUMBER, 
	"OFFICE_COMPLIANCE" NUMBER
   )
/
```

#### TRI_JOB_SUPPLIES

This table holds supplies usage data for each item/job visit.

```
CREATE TABLE  "TRI_JOB_SUPPLIES" 
   (	
  "JOB_ID" NUMBER, 
	"ITEM_ID" NUMBER, 
	"QUANTITY" NUMBER, 
	"TYPE" VARCHAR2(15), 
	"VISIT_ID" NUMBER
   )
/
```
#### TRI_WASTE_INFORMATION

This table holds waste collection data for each job visit. One job visit can and will have multiple records in this table.

```
CREATE TABLE  "TRI_WASTE_INFORMATION" 
   (	"JOB_ID" NUMBER, 
	"WASTE_LINE_ID" NUMBER, 
	"DOC_NUMBER" VARCHAR2(15), 
	"APPROVAL_CODE" VARCHAR2(15), 
	"FIVE_G_QTY" NUMBER, 
	"SIXTEEN_G_QTY" NUMBER, 
	"TWENTY_G_QTY" NUMBER, 
	"THIRTY_G_QTY" NUMBER, 
	"FORTY_G_QTY" NUMBER, 
	"FIFTYFIVE_G_QTY" NUMBER, 
	"EIGHTYFIVE_G_QTY" NUMBER, 
	"SQ_YD" NUMBER, 
	"VISIT_ID" NUMBER
   )
/
```
#### TRI_SUPPLIES_MASTER

This table holds setup data needed to run the application. Each supply item that could be used on a job visit will have a record in this table and will be classified by one of the following three 'types': Supplies, Bio & Pharmacy Suppliers, Personal Protective Equipment. These values are hardcoded in application item ```P9_TYPE``` on page 9 of the application 

```
CREATE TABLE  "TRI_SUPPLIES_MASTER" 
   (	
  "ITEM_ID" NUMBER, 
	"ITEM_DESCRIPTION" VARCHAR2(60), 
	"PRICE" NUMBER, 
	"TYPE" VARCHAR2(15)
   )
/
```
### Database Sequences

There are five sequences used by this application:

- ```TRI_ITEMS_SEQ``` Used for the ```ITEM_ID``` column in the ```TRI_SUPPLIES_MASTER``` table.
- ```TRI_JOBS_SEQ``` Used when creating new jobs in ```TRI_JOBS```.
- ```TRI_LABOR_LINE_SEQ``` Used when adding labor to jobs in ```TRI_JOB_LABOR```
- ```TRI_VISITS_SEQ``` Used when adding a new job visit in ```TRI_JOB_VISITS```
- ```TRI_WASTE_LINE_SEQ``` Used when adding waste lines in ```TRI_WASTE_INFORMATION```

### Application Pages:

| Page Number   | Page Name     |Page Purpose|
| ------------- | ------------- |-------------
|3  |Visits|This page is a job visit maintenance page for the SC to clean up/change/delete data as necessary before the demo. |4  |Visit Entry|The user is brought to this page when clicking the ```Create New``` button on the main page. Here they can enter visit header information before proceeding to the visit details page.
|6  | Analytics  |This page displays all of the analytics charts for the application. There are four pie charts on this page.
|8  |Supply Items|This is a report of all supply items that are used in the application.
|9  |Supply Items Entry|This is a data entry screen for entering/editing supply items.
|10 |Job Report|This page displays all jobs currently in the system
|11 |Jobs Entry|This is a data entry screen for adding and editing job information
|12 |Job Cards |This is the main application page that shows jobs, job visits and provides links for creating new job visits

Main Page:
<img width="1247" alt="main page" src="https://user-images.githubusercontent.com/21246211/40200845-2dbe2c68-59d2-11e8-97c6-a4b529dd8678.png">

Analytics Page:
<img width="1158" alt="charts" src="https://user-images.githubusercontent.com/21246211/40200796-12276ae6-59d2-11e8-9a1c-50e19c724f28.png">

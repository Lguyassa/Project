# Project
# Temple University

College of Liberal Arts, Department of Geography and Urban Studies
PSM GIS, Spatial Database Design Course
Term Project on creating spatial database design for Philadelphia City Facilities

By: Lamessa K Guyassa
**Introduction** 

The role of geographic information systems (GIS) in urban planning and management has become increasingly crucial. The term project focuses on creating a spatial database design for the City of Philadelphia Facilities. It encompasses a broad spectrum of structures and fixed assets under the ownership, lease, or operation of the City of Philadelphia. This encompassing category includes administrative buildings, parks, fire stations, libraries, and various other facilities. Commonly referred to as the Master Facilities Database, this repository typically comprises comprehensive lists of buildings and fixed assets managed by the city. It spans across government offices, public facilities, and infrastructure. Importantly, it delineates sites that have received funding from the Capital Program to sustain or enhance the use of these facilities. This database is integral to urban management, acting as a linchpin in decision-making processes related to maintenance, strategic planning, and the allocation of resources. 

Far from being a static inventory, the City Facilities database emerges as a dynamic tool crucial for effective city governance. It goes beyond being a mere compilation of assets; instead, it serves as a dynamic and responsive resource. Its multifaceted role facilitates the tracking and management of diverse assets, empowering the city to make well-informed decisions pertaining to the upkeep, strategic planning, and utilization of its properties and infrastructure. The database emerges as an indispensable asset, essential for steering urban management practices in a direction that ensures both efficiency and efficacy. It underlines the interconnectedness of data-driven decision-making with the successful administration of urban resources. 

These Facilities include, but are not limited to: - administrative or multi-purpose buildings, athletic fields, airfields and airport buildings, bridges, ball courts, fire stations, health centers, libraries, museums, plazas, parks and park buildings, playground equipment, piers, Police stations, pools, recreation centers and buildings, radio towers, Water and wastewater facilities Metadata Catalog | phila.gov 

**Project Objectives** 

The primary goal of the project is to work with the Philadelphia City Facilities database, sourced from OpenDataPhilly, covering the years 2000 to 2016. The objectives include: 


**Normalization Plan:** Develop a normalization plan to organize the data efficiently for the Philadelphia City Facilities database. Normalization is the process of organizing data in a database to reduce data redundancy and improve data integrity. In this context, it involves structuring the data to eliminate duplication and ensure efficient storage and retrieval of information. 

**Normalization Scripts:** This step aims to reduce data redundancy and enhance data integrity. Creating scripts for its implementation. These scripts will be used to transform the existing data into the normalized format. This step may involve restructuring tables, defining relationships between tables, and ensuring data consistency. 

 

**Data Analysis and Optimization:** Perform a comprehensive analysis of the database, optimizing its structure for improved performance. This involves database optimization techniques to enhance data retrieval and query performance, specifically focusing on the 2000 to 2016 time. 

**Data Sources and Catalogs** 

The primary data sources for this project are the City Facilities Dataset in .CSV or SHP format from OpenDataPhilly. Additionally, the City Facilities Data Dictionary from the Metadata Catalog provided by phila.gov is indispensable for understanding the attributes and structure of the data. This metadata is created by the Philadelphia City’s Office of Information Technology, providing critical insights into the dataset. 

Integration of Census Tracts: The Census_Tract.shp or is identified as a valuable resource for understanding the spatial relationships between city facilities and the geographic characteristics of different areas within Philadelphia. This integration supports evidence-based decision-making and enhances the overall robustness of the analysis. 

**Potential Questions and Analytical query goals** 

1. What are the top 10 most recent revision details, including asset names, revision notes and edit dates for assets with revision notes created on or after January 1, 2015? 

This query retrieves information about the most recent revisions for assets, including the asset name, revision note, edit date, and editor, for assets with revision notes created on or after January 1, 2015. The results are sorted in descending order based on the edit date, and only the top 10 records are returned. 

2, To calculate the distance between two assets in the asset table. The goal is to find the pair of assets with the minimum spatial distance. 

The query aims to address the problem of finding the two assets with the smallest spatial distance between geographical points (assets in this case). 

3. How many assets are currently occupied within each subgroup, categorized by asset subgroup? 

This query is counting the number of assets that are currently in use (occupied) and grouping them based on their categories (subgroups). 

**Data Structure and Normalization Process** 
The ERD involve creating tables, inserting data, and establishing relationships between the tables.  The structure of the data and the normalization process are based on the information provided. The provided SQL script outlines the creation of several tables (opa, site, revision, asset_subtype, asset_subgroup, asset, and bldg) along with the insertion of data into these tables. Below is a summary of the structure and relationships among these tables:

opa Table:

Primary Key: opa_id
Columns: opa_id, opa_owner, opa_address
site Table:

Primary Key: site_name
Columns: site_code, site_acres, primary_site, secondary_site, site_name
revision Table:

Primary Key: note
Columns: note, edit_date, edit_source, editor
asset_subtype Table:

Primary Key: subtype
Columns: subtype, description
asset_subgroup Table:

Primary Key: subgroup
Columns: subgroup, description
asset Table:

Primary Key: gid
Foreign Keys: note (References revision), opa_id (References opa), subtype (References asset_subtype), site_name (References site), subgroup (References asset_subgroup)
Columns: gid, asset_id, asset_name, asset_type, asset_address, status, geom, city_owned, not_public, cityown_desc, serving_type, occupant, opa_id, site_name, subgroup, subtype, note
bldg Table:

Primary Key: bldg_id
Foreign Key: asset_id (References asset)
Columns: bldg_id, sharebldg, asset_id
Normalization Process:

Normalization involves organizing data to eliminate redundancy and dependency issues.
The script creates separate tables for different entities such as opa, site, revision, asset_subtype, asset_subgroup, asset, and bldg.
Each table has a primary key to uniquely identify records, and foreign keys establish relationships between tables.

ERD (Entity-Relationship Diagram):

![ERD_Project_final](https://github.com/Lguyassa/Project/assets/89851403/713fb6de-b109-4dc6-99ec-b2a00125d1e4)

Figure.1.2     ERD (Entity-Relationship Diagram) 

This ERD illustrates the relationships between different tables, including primary and foreign keys. Arrows indicate the direction of the relationship, and cardinality notation represents how many records in one table relate to another. 

 The normalization process aims to reduce data redundancy, improve data integrity, and make the database more efficient.

**Normalization Process:** 
Primary Keys: 
Each table has a primary key that uniquely identifies each record. Primary keys are used to establish relationships between tables. 
Foreign Keys: 
Foreign keys are used to establish relationships between the asset table and lookup tables (opa, site, note, asset_subtype, asset_subgroup). 

Lookup Tables: 
Lookup tables (opa, site, revision, asset_subtype, asset_subgroup) help maintain data integrity and avoid data duplication by storing unique values that are referenced by other tables. 

 
Query 1: Top 10 Most Recent Revision Details 

Question: What are the top 10 most recent revision details, including asset names, revision notes, and edit dates for assets with revision notes created on or after January 1, 2015? 

Optimized Query: 

    SELECT asset.asset_name, revision.note, revision.edit_date, revision.editor 
    FROM asset 
    JOIN revision ON asset.note = revision.note 
    WHERE revision.edit_date >= '2015-01-01' 
    ORDER BY revision.edit_date DESC 
    LIMIT 10; 
     

Explanation: 

The query uses a JOIN operation to connect the asset and revision tables based on the common column `asset.revision_note = revision.note.` 

The WHERE clause filters the results to include only revisions created on or after January 1, 2015 `(revision.edit_date >= '2015-01-01')`. 

The ORDER BY clause sorts the results in descending order based on the edit date `(revision.edit_date DESC)`, ensuring that the most recent revisions appear first. 

The LIMIT clause restricts the output to the top 10 records, improving query efficiency. 

Potential Optimization: 

Creating indexes on `asset.note and revision.edit_date` can enhance the performance of the query, especially when dealing with large datasets. 



    CREATE INDEX idx_asset_note ON asset (revision_note); 
    CREATE INDEX idx_revision_edit_date ON revision (edit_date); 

 

Query 2: Calculate Distance Between Two Assets 

Question: To calculate the distance between two assets in the asset table, find the pair of assets with the minimum spatial distance. 

Optimized Query: 

    SELECT a1.asset_name AS asset1_name, a2.asset_name AS asset2_name, ST_Distance(a1.geom, a2.geom) AS distance_in_meters 
    FROM asset a1 
    JOIN asset a2 ON a1.gid <> a2.gid 
    ORDER BY distance_in_meters 
    LIMIT 1; 

 

Explanation: 

The query performs a self-join on the asset table `(a1 joined with a2)` to compare different assets. 

The WHERE clause `(a1.gid <> a2.gid)` ensures that assets are not compared to themselves. 

The ORDER BY clause sorts the result set by the calculated distance in ascending order. 

The LIMIT clause restricts the result set to only return the first row, representing the pair of assets with the minimum distance. 

Potential Optimization: 
Creating a spatial index on the `geom` column can significantly speed up spatial search operations. 
`CREATE INDEX idx_asset_geom ON asset USING GIST(geom); 
 

Query 3: Count Occupied Assets by Subgroup 
Question: How many assets are currently occupied within each subgroup, categorized by asset subgroup? 

Optimized Query: 

    SELECT asg.subgroup, COUNT(*) AS occupied_assets 
    FROM asset a 
    JOIN asset_subgroup asg ON a.subgroup = asg.subgroup 
    WHERE a.occupant IS NOT NULL 
    GROUP BY asg.subgroup;

Explanation: 

The query joins the asset table with the `asset_subgroup` table based on the common column `asset.asset_subgroup = asset_subgroup.subgroup.` 

The WHERE clause filters records to include only those where the occupant column in the a`sset table is not NULL (a.occupant IS NOT NULL).` 

The GROUP BY clause groups the results by the subgroup column from the `asset_subgroup` table. 

Potential Optimization: 

Creating indexes on `asset.asset_subgroup and asset.occupant` can enhance the efficiency of the JOIN and WHERE operations. 


    CREATE INDEX idx_asset_subgroup ON asset (asset_subgroup); 
    CREATE INDEX idx_occupant ON asset (occupant); 
    CREATE INDEX idx_subgroup ON asset_subgroup (subgroup); 

These optimizations aim to improve the performance of the queries by creating indexes on relevant columns and supporting efficient data retrieval and filtering. 


Appendix - I 

Appendix 1.1 Data Extraction and Loading in QGIS  

Overview 
This appendix outlines the step-by-step process of obtaining and loading the City Facilities Dataset using QGIS  
                                          Steps: 
a) Download City Facilities Dataset: 
Access the City Facilities Dataset from OpenDataPhilly in .CSV or SHP format. The relevant link City Facilities (Master Facilities Database) - OpenDataPhilly for downloading the dataset. 
b) Understand Data Structure with Data Dictionary: 
Refer to the City Facilities Data Dictionary from the Metadata Catalog. 
Metadata Catalog | phila.gov  which Highlight key attributes, data types, and any special considerations. 
c) Install QGIS: Ensure QGIS is installed on your system. Visit the QGIS Download page.  Additional Notes: Ensure your system meets the system requirements for QGIS. And also refer to the official QGIS documentation for more detailed instructions and troubleshooting. 
d) Prepare Environment: Set up the necessary directories or folders for storing data. 
e) Load Data in QGIS: Open QGIS, create schema Postgres and navigate to the Data Source Manager. Use the ogr2ogr tool within QGIS to load the City Facilities Dataset. Specify the source format (CSV or SHP) and target format if needed. 
f) Review Loaded Data: Verify that the data is loaded successfully in QGIS. Check for any errors or warnings during the loading process. 

 
![image](https://github.com/Lguyassa/Project/assets/89851403/75e81905-947f-40d8-aa55-07cda9b6ea12)

 

 

 

 

 

 



 

 
 

 

 

 

 

 

 

 

 

 

 

 

Appendix – 2 
Normalization Scripts 
----the original file SRID 4326 , WGS84 (lat,lng) 
-- new SRID 2272, NAD83 / Pennsylvania South (ftUS) 
---the data is uploaded	in a public schema as 'City_Facilities_pub' and renamed to City_facilities 

    set search_path to public; 

--Create a lookup table for the opa column 
CREATE TABLE  opa  (
   opa_id  text ,
   opa_owner  text,
   opa_address  text,
   PRIMARY key ( opa_id )
);

-- Insert into opa_lookup
INSERT INTO opa (
  opa_id, 
  opa_owner, 
  opa_address
)
select 
  cf.opa_id,
  cf.opa_owner,
  cf.opa_addr 
FROM (
    select cf.opa_id, cf.opa_owner, cf.opa_addr,  
    row_NUMBER() over (partition by cf.opa_id order by cf.opa_id) as rn 
  from city_facilities cf
 where cf.opa_id is NOT NULL
) cf
where cf.rn = 1;

----Create a lookup table for the site_lookup column
CREATE TABLE  site  (
   site_code  text,
   site_acres  int,
   primary_site  text,
   secondary_site  text,
   site_name  text,
  PRIMARY KEY ( site_name )
);
-- Insert into site_lookup

INSERT INTO site (site_code, site_acres, primary_site, secondary_site, site_name)
select cf.site_code, cf.site_acres, cf.primarysit, cf.secondarys, cf.site_name
FROM (
    select cf.site_code, cf.site_acres, cf.primarysit, cf.secondarys, cf.site_name,  
    row_number () over (partition by cf.site_name order by cf.site_name) as rn 
from city_facilities cf
where cf.site_name  is NOT NULL
) as cf
where cf.rn = 1;

---- Create a lookup table for the revision_lookup column----

 CREATE TABLE  revision  (
   note  text,
   edit_date  text,
   edit_source  text,
   editor  text,
  PRIMARY KEY ( note )
);

-- Insert into revision_lookup

INSERT INTO revision (note, edit_date, edit_source, editor)
SELECT cf.editnote1,cf.editdate, cf.editsource, cf.editor
FROM (
     select cf.editnote1,cf.editdate, cf.editsource, cf.editor,
      row_number () over (partition by cf.editnote1 order by cf.editnote1) as rn
FROM City_Facilities cf
where cf.editnote1 is not null 
) as cf
where cf.rn = 1;


CREATE TABLE  asset_subtype  (
   subtype  text,
   description  text,
  PRIMARY KEY ( subtype )
);

----- Insert into asset_subtype_lookup

INSERT into asset_subtype (subtype, description)
VALUES  ('C1', 'Airport Airfield'),
  ('C2', 'Airport Terminal\Hanger'),
  ('C4', 'Basketball Court1'), 
  ('C5', 'Barn\Stables'),
  ('C3', 'Athletic Field\Soccer Field'),
  ('C6', 'Bocci Court'),
  ('C7', 'Bridge'),
  ('C8', 'City-Owned Land'),
  ('C9', 'Compost\Recycling Center'),
  ('C10', 'Concessions\Retail\Cafe'),
  ('C11', 'Detention Center Adult'),
  ('C12', 'Detention Center Youth'),  
  ('C13', 'Golf Driving Range'),
  ('C14', 'Fire Station'),
  ('C15', 'Fire Station Marine'),
  ('C16', 'Fountain'),
  ('C17', 'Garage\Maintenance Building'),
  ('C18', 'Golf Course'),
  ('C19', 'Greenhouse\Nursery'),
  ('C20', 'Health Center'),
  ('C21', 'Historic House\Site'),
  ('C22', 'Housing\Group Quarters'),
  ('C23', 'Ice Rink'),
  ('C24', 'Laboratory'),
  ('C25', 'Library Branch'),
  ('C26', 'Library Central'),
  ('C27', 'Library Operations'),
  ('C28', 'Library Regional'),
  ('C29', 'Library Specialized'),
  ('C30', 'Linear Park\Parkway'),
  ('C31', 'Breezeway\Island\Managed Area'),
  ('C32', 'Kennel'),
  ('C33', 'Materials Yard'),
  ('C34', 'Multi-Use\Office Building'),
  ('C35', 'Museum'),
  ('C36', 'Neighborhood Park'),
  ('C37', 'Nursing Home'),
  ('C38', 'Older Adult Center'),
  ('C39', 'Parking Lot'),
  ('C40', 'Pavilion\Shelter'),
  ('C41', 'Pier'),
  ('C42', 'Playground'),
  ('C43', 'Police Station'),
  ('C44', 'Police Sub-Station'),
  ('C45', 'Police Operations\Unit'),
  ('C46', 'Pool'),
  ('C47', 'Radio\Cell Tower'),
  ('C48', 'Public Safety Training Center'),
  ('C49', 'Recreation Building'),
  ('C50', 'Recreation Center'),
  ('C51', 'Recreation Other'),
  ('C54', 'Regional\Metro Park'),
  ('C55', 'Restrooms'),
  ('C56', 'Fish Ladder'),
  ('C57', 'Salt Shed'),
  ('C58', 'Skateboard Park'),
  ('C60', 'Spray Ground'),
  ('C61', 'Square\Plaza'),
  ('C63', 'Stage\Stands'),
  ('C64', 'Statue\Monument'),
  ('C65', 'Shed'),
  ('C67', 'Tennis Court'),
  ('C68', 'Athletic Track'),
  ('C69', 'Trailers'),
  ('C70', 'Transfer Station'),
  ('C71', 'Transit Station'),
  ('C73', 'Warehouse'),
  ('C74', 'Waste Water Storage'),
  ('C75', 'Waste Water Processing Facility'),
  ('C76', 'Water Pumping Station'),
  ('C77', 'Water Filter\Processing Facility'),
  ('C78', 'Water Storage'),
  ('C79', 'Watershed\Conservation Park'),
  ('C80', 'Zoo\Animal Habitat'),
  ('C81', 'Other'),
  ('C82', 'Fuel Pump'),
  ('C83', 'Helipad'),
  ('C84', 'Dog Run'),
  ('C85', 'Volleyball Court'),
  ('C86', 'Judicial Court Building'),
  ('C87', 'Baseball Field'),
  ('C88', 'Street Hockey Court\Rink'),
  ('C89', 'Batting Cage'),
  ('C90', 'Handball Court'),
  ('C91', 'Recreation/Bike Trail'),
  ('C92', 'Generator'),
  ('C93', 'Dam'),
  ('D1', 'Warehouse'),
  ('D2', 'Automotive Shop'),
  ('D3', 'Operations Center'),
  ('D4', 'Training Center'),
  ('D5', 'Marine Unit'),
  ('D6', 'Gymnasium'),
  ('D8', 'Indoor'),
  ('D9', 'Maintenance Shop'),
  ('D10', 'Multi-Use\Office Building'),
  ('D11', 'Non-Programmed Recreation'),
  ('D12', 'Office Building'),
  ('D13', 'Park Land'),
  ('D14', 'Judicial Court'),
  ('D15', 'Outdoor'),
  ('D16', 'Pier'),
  ('D17', 'Playground Equipment'),
  ('D18', 'Programmed Recreation'),
  ('D20', 'Vacant'),
  ('D22', 'Recreation Land'),
  ('D23', 'Laboratory'),
  ('D24', 'Police Station'),
  ('D25', 'Police Unit'),
  ('D26', 'Fire Station'),
  ('D27', 'Managed Open Space'),
  ('D28', 'Transfer Station'),
  ('D29', 'Health Center'),
  ('D30', 'Fresh Water Pumping Station'),
  ('D31', 'Waste Water Pumping Station'),
  ('D32', 'Historic House\Site');
 
---CREATE TABLE asset_subgroup

 CREATE TABLE  asset_subgroup  (
   subgroup  text,
   description  text,
  PRIMARY KEY ( subgroup )
);

--- Insert into asset_subgroup_lookup table

INSERT into asset_subgroup (subgroup, description)
VALUES  ('A1', 'Airports'),
  ('A2', 'City Operations'),
  ('A3', 'Judiciary Courts'),
  ('A7', 'Health and Human Services All'),
  ('A8', 'Libraries'),
  ('A10', 'Other'),
  ('A11', 'Parks and Recreation All'),
  ('A13', 'Public Safety All'),
  ('A15', 'Public Works and Utilities'),
  ('A17', 'Transit and Transportation'),
  ('A18', 'Museums and Zoo'),
  ('A19', 'Telcom'),
  ('A2.1', 'Quadplex (MSB, OPB, City Hall, CJC)'),
  ('A7.1', 'Health Centers (Public) (Dept of Public Health Centers)'),
  ('A11.2', 'Park Land and Open Space (park land holdings)'),
  ('A11.3', 'Recreational Resources (rec centers, playgrounds, athletic fields and courts)'),
  ('A13.1', 'Fire Dept'),
  ('A13.2', 'Police Dept'),
  ('A13.3', 'Prisons All (Prison facilities)');
 
 ------CREATE  asset TABLE-----------------
 
  CREATE TABLE   asset   (
    gid   int primary key,
    asset_id int,
    asset_name   text,
    asset_type   text,
    asset_address   text,
    status   text,
    geom GEOMETRY(point, 2272),
    city_owned   text,
    not_public   text,
    cityown_desc   text,
    serving_type   text,
    occupant   text,
    opa_id   text,
    site_name   text,
    subgroup   text,
    subtype   text,
    note   text,
  FOREIGN KEY (  note  ) REFERENCES   revision  (  note  ),
  FOREIGN KEY (  opa_id  ) REFERENCES opa ( opa_id  ),
  FOREIGN KEY (subtype) REFERENCES asset_subtype  (  subtype  ),
  FOREIGN KEY (  site_name  ) REFERENCES   site  (  site_name  ),
  FOREIGN KEY (  subgroup  ) REFERENCES   asset_subgroup  (  subgroup  )
);

-- Insert into asset
 
INSERT INTO asset (
	gid, asset_name, asset_type, asset_address, status, geom, site_name, asset_id, city_owned,
	not_public, cityown_desc, serving_type, occupant, opa_id, 
	subgroup, subtype, note
	)
SELECT 
	 cf.gid, cf.asset_name, cf.asset_type, cf.asset_addr, cf.status, cf.geom, cf.site_name, cf.asset_id, 
	 cf.cityowned, cf.not_public, cf.cityuse_de, cf.serving_ty, cf.occupant, cf.opa_id, 
	 cf.asset_grou, cf.asset_subt, cf.editnote1
FROM City_Facilities as cf
INNER JOIN asset ON asset.opa_id = cf.opa_id
INNER JOIN asset_subtype ON asset.subtype = asset_subtype.subtype
INNER JOIN asset_subgroup ON asset.subgroup = asset_subgroup.subgroup
INNER JOIN revision ON asset.note = revision.note
INNER JOIN site ON asset.site_name = site.site_name;

---CREATE TABLE bldg

CREATE TABLE  bldg  (
   bldg_id  int PRIMARY KEY,
   asset_id  int,
   sharebldg  text, 
   FOREIGN KEY ( asset_id ) REFERENCES  asset ( asset_id )
);

-- Insert into bldg

INSERT INTO bldg (bldg_id, sharebldg, asset_id)
SELECT cf.buildingid, cf.sharebldg_ , cf.asset_id
from (
    select cf.buildingid, cf.sharebldg_,cf.asset_id,
    ROW_NUMBER() over (partition by cf.buildingid order by cf.buildingid) as rn
FROM city_facilities cf
where cf.buildingid is not null 
) as cf
where cf.rn = 1;





   Appendix – 4 
Data Dictionary 

Field Name ,     Description ,   Type                                                                          
ASSET_ADDR ,  , OPA address based on parcel where available, otherwise the address is specified by data source. If both OPA and alternate address are available, OPA takes precedence. ,     Text  

asset_subtype (subtype, description) 
    VALUES ('C1', 'Airport Airfield'), 
    ('C2', 'Airport Terminal\Hanger'), 
    ('C4', 'Basketball Court1'),  
    ('C5', 'Barn\Stables'), 
    ('C3', 'Athletic Field\Soccer Field'), 
    ('C6', 'Bocci Court'), 
    ('C7', 'Bridge'), 
    ('C8', 'City-Owned Land'), 
    ('C9', 'Compost\Recycling Center'), 
    ('C10', 'Concessions\Retail\Cafe'), 
    ('C11', 'Detention Center Adult'), 
    ('C12', 'Detention Center Youth'),  
    ('C13', 'Golf Driving Range'), 
    ('C14', 'Fire Station'), 
    ('C15', 'Fire Station Marine'), 
    ('C16', 'Fountain'), 
    ('C17', 'Garage\Maintenance Building'), 
    ('C18', 'Golf Course'), 
    ('C19', 'Greenhouse\Nursery'), 
    ('C20', 'Health Center'), 
    ('C21', 'Historic House\Site'), 
    ('C22', 'Housing\Group Quarters'), 
    ('C23', 'Ice Rink'), 
    ('C24', 'Laboratory'), 
    ('C25', 'Library Branch'), 
    ('C26', 'Library Central'), 
    ('C27', 'Library Operations'), 
    ('C28', 'Library Regional'), 
    ('C29', 'Library Specialized'), 
    ('C30', 'Linear Park\Parkway'), 
    ('C31', 'Breezeway\Island\Managed Area'), 
    ('C32', 'Kennel'), 
    ('C33', 'Materials Yard'), 
    ('C34', 'Multi-Use\Office Building'), 
    ('C35', 'Museum'), 
    ('C36', 'Neighborhood Park'), 
    ('C37', 'Nursing Home'), 
    ('C38', 'Older Adult Center'), 
    ('C39', 'Parking Lot'), 
    ('C40', 'Pavilion\Shelter'), 
    ('C41', 'Pier'), 
    ('C42', 'Playground'), 
    ('C43', 'Police Station'), 
    ('C44', 'Police Sub-Station'), 
    ('C45', 'Police Operations\Unit'), 
    ('C46', 'Pool'), 
    ('C47', 'Radio\Cell Tower'), 
    ('C48', 'Public Safety Training Center'), 
    ('C49', 'Recreation Building'), 
    ('C50', 'Recreation Center'), 
    ('C51', 'Recreation Other'), 
    ('C54', 'Regional\Metro Park'), 
    ('C55', 'Restrooms'), 
    ('C56', 'Fish Ladder'), 
    ('C57', 'Salt Shed'), 
    ('C58', 'Skateboard Park'), 
    ('C60', 'Spray Ground'), 
    ('C61', 'Square\Plaza'), 
    ('C63', 'Stage\Stands'), 
    ('C64', 'Statue\Monument'), 
    ('C65', 'Shed'), 
    ('C67', 'Tennis Court'), 
    ('C68', 'Athletic Track'), 
    ('C69', 'Trailers'), 
    ('C70', 'Transfer Station'), 
    ('C71', 'Transit Station'), 
    ('C73', 'Warehouse'), 
    ('C74', 'Waste Water Storage'), 
    ('C75', 'Waste Water Processing Facility'), 
    ('C76', 'Water Pumping Station'), 
    ('C77', 'Water Filter\Processing Facility'), 
    ('C78', 'Water Storage'), 
    ('C79', 'Watershed\Conservation Park'), 
    ('C80', 'Zoo\Animal Habitat'), 
    ('C81', 'Other'), 
    ('C82', 'Fuel Pump'), 
    ('C83', 'Helipad'), 
    ('C84', 'Dog Run'), 
    ('C85', 'Volleyball Court'), 
    ('C86', 'Judicial Court Building'), 
    ('C87', 'Baseball Field'), 
    ('C88', 'Street Hockey Court\Rink'), 
    ('C89', 'Batting Cage'), 
    ('C90', 'Handball Court'), 
    ('C91', 'Recreation/Bike Trail'), 
    ('C92', 'Generator'), 
    ('C93', 'Dam'), 
    ('D1', 'Warehouse'), 
    ('D2', 'Automotive Shop'), 
    ('D3', 'Operations Center'), 
    ('D4', 'Training Center'), 
    ('D5', 'Marine Unit'), 
    ('D6', 'Gymnasium'), 
    ('D8', 'Indoor'), 
    ('D9', 'Maintenance Shop'), 
    ('D10', 'Multi-Use\Office Building'), 
    ('D11', 'Non-Programmed Recreation'), 
    ('D12', 'Office Building'), 
    ('D13', 'Park Land'), 
    ('D14', 'Judicial Court'), 
    ('D15', 'Outdoor'), 
    ('D16', 'Pier'), 
    ('D17', 'Playground Equipment'), 
    ('D18', 'Programmed Recreation'), 
    ('D20', 'Vacant'), 
    ('D22', 'Recreation Land'), 
    ('D23', 'Laboratory'), 
    ('D24', 'Police Station'), 
    ('D25', 'Police Unit'), 
    ('D26', 'Fire Station'), 
    ('D27', 'Managed Open Space'), 
    ('D28', 'Transfer Station'), 
    ('D29', 'Health Center'), 
    ('D30', 'Fresh Water Pumping Station'), 
    ('D31', 'Waste Water Pumping Station'), 
    ('D32', 'Historic House\Site'); 
    
Asset_Type ,  , Basic categorization of asset as building, land, equipment, etc. Geodatabase coded
 asset_type (Building, Equipment, Land, Structure, Parking Lot, Pier, Other) 
            Values (B1, B2, B3, B4, B5, B6, B7) ,Text  
	    
Bldg_Age ,  , Age of building as of 2015 ,                      Numeric  
Bldg_FL ,  , Number of floors per building (per OPA data where available.) ,            Numeric  
Bldg_SQFT ,  , Building square footage ,                         Numeric  
Bldg_SQFTUSE ,  ,  ,                                                       Numeric  
Bldg_YR ,  , Year building was constructed ,                Numeric   
CityOwned ,  , City-owned building Yes or No , Text  
CityUse_Desc ,  , Description of city owned, leased or used by agreement.  

           Owned - City Owned 
           Lessor - City Owned, Lessor (leased to tenants) 
           Lessee - Other Ownership, City is Lessee 
           UA - Other Ownership, City Use Agreement , Text 

EditNote1, EditDate, EditSource, EditSourceDate ,  , Used to track verification of the entry. , Text  
MaintainedBy ,  , City dept or entity responsible for the building maintenance. , Text  
 Not_Public ,  , Y - Yes     ,    N - No ,  Text  
Occupant ,  , Primary city department or lessee occupying the site or building. , Text 
OPA_ID ,  , OPA parcel ID number , Numeric  
OPA_Owner ,  , OPA parcel owner , Text  
  serving_Type ,  , C - Community <br /> M - Municipal , Text  
 SharedBldg_AssetID ,  , Tracks AssetIDs where two facilities share the same buildings. 
Site_Acres ,  , Acreage of site parcel , Numeric  
Site_Code , User Code , The Capital Program identification number to track expenditures and programming. Also known as the UserCode or Capital Code. , Text  
Site_Name ,  , The Asset_Name unless facility or site comprise more than one building, structure, equipment, etc. All assets are assigned the facility name (e.g., Police Academy has multiple buildings with individual asset names) , Text  
Status ,  , A - Active or currently in use  or  I - Inactive or closed or shuttered , Text  

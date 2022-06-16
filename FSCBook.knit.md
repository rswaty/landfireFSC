--- 
title: "LANDFIRE and Certified Sustainable Forest Management"
author: "Keith Phelps, Stacey Marion, and Randy Swaty"
date: "2022-06-16"
site: bookdown::bookdown_site
output: bookdown::gitbook
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
description: "This is a TNC LANDFIRE GIS and excel tutorial to assist forest managers in FSC® audits"
always_allow_html: true 
---


# Introduction {#Introduction}

## Forest Certification

Forest certification, in which third-parties audit forest operations to defined standards, is used as a tool to ensure forests are well-managed and provide confidence to consumers of forest products. Many of these standards include goals and requirements to manage and restore native ecosystems. One of these systems, the [Forest Stewardship Council®](FSC®) develops and delivers Principles, Criteria and Indicators for sustainable forest management (https://fsc.org/en/document-centre/documents/resource/392) through their national standards. Currently in the United States, FSC® Principles 6 and 9 are focused on Environmental Values and Impact and Maintenance of High Conservation Value Areas respectfully. In order for forest managers to receive FSC® certification and pass audits, many of the criteria and indicators within Principles 6 and 9 are helped by an analysis, often through a GIS assessment, of historical and current vegetation conditions, disturbance regimes, and landscape patterns. The following provides procedures with which forest managers may be able to conduct analysis using LANDFIRE data that could aid in standard conformance and leverage high-quality, open-source data to drive restoration on suitable sites. 

## LANDFIRE 

[LANDFIRE](https://landfire.gov/) is an interagency program within the United States that "provides 20+ national geo-spatial layers (e.g. vegetation, fuel, disturbance, etc.), databases, and ecological models that are available to the public for the US and insular areas." With this free and regularly updated data, it is possible to conduct analysis supporting environmental values and High Conservation Value Areas. Here we provide guidance for:

* Downloading relevant LANDFIRE datasets and models
* Completing the GIS processing of LANDFIRE data
* Developing visuals (e.g., maps and charts) to help illustrate findings 

## GIS Tutorial Goals

In general the analyses developed here will help forest managers:

* Assess “representation” of ecosystems within ownerships and landscapes.
* Map ecosystems and their conditions- past and present.
* Understand natural disturbance regimes and compare them to current landscape disturbance patterns.
* Explore ecosystem conversion both to unnatural (e.g., urban) land uses and different ecosystems.
* Provide context for where ecological restoration or conservation of current ecosystems should be prioritized on a landscape.  
* Assess site conditions that can be completed with existing datasets.
* Assess climate change and future condition considerations where possible with existing data.

In the United States, including the insular areas, [LANDFIRE](https://www.landfire.gov/) provides the datasets and ecological model results to get at these challenges and more. Here we walk you through some of the technical steps needed to start your analysis. We will do our work in a model landscape called the Ataya Forest Tract (hereafter referred to as "Ataya"). Ataya is managed by The Nature Conservancy (TNC) under FSC® standards (FSC®C008922) and is located in the Central Appalachians (highlighted in green in map below).

To help you with the concepts we have worked through pre-processed LANDFIRE data for this example landscape. To prepare the datasets for your own landscape, you may download the relevant LANDFIRE data for your own landscape and follow our steps.

**ZOOM, pan, explore the Ataya Forest Tract area**


![](FSCBook_files/figure-latex/leafletAtaya-1.pdf)<!-- --> 


<!--chapter:end:index.Rmd-->

# Our Example Landscape {#Ataya} 

**Introducing Ataya**

Ataya is an approximately 100,688-acre forested landscape in southeastern Kentucky and northeastern Tennessee. The Ataya is located in the Central Appalachians, and is situated in the historic territories of the S'atsoyaha (Yuchi), Shawandasse Tula (Shawnee), and the Tsalaguwetiyi (Cherokee, East) nations. You can explore these territorial claims here: (https://native-land.ca/). The nearest largest metropolitan area to Ataya is Knoxville, TN (population 186,173). According to LANDFIRE's Biophysical Settings (BpS), the top three (3) historically dominant forest ecosystem types in Ataya were: Southern Appalachian Oak Forest, South-Central Interior Mesophytic Forest, and Southern Appalachian Cove Forest. Historically in Southern Appalachian Oak Forests, low severity surface fires with fire intervals of 7-26 years were the dominant ecosystem disturbance. Other important historic disturbances in the forests of the Ataya included insect outbreaks, ice storms, windthrow, and drought. 

The forests in the Central Appalachians experienced considerable alteration from European colonization and land use changes. Important land use and ecological changes included extensive clear-cut logging in the early 20th century, coal mining, the functional extinction of the American Chestnut (*Castanea dentata*) and fire-suppression policies enacted in the early 20th century. Currently, the Central Appalachians are facing population reductions in fire tolerant/shade intolerant species such as oaks (*Quercus* sp.) and shortleaf pine (*Pinus echinata*). Drastic population declines are also evident in eastern hemlock (*Tsuga canadensis*) due to the introduction of Hemlock Wooly Adelgid. Lastly, various invasive plants (i.e. kudzu, autumn olive, Japanese stilt grass) are also important threats to Central Appalachian forest biodiversity and resiliency.  

The Ataya is of conservation interest due to the relatively high connectivity, large average land parcel size and projected climate change resiliency.


\includegraphics[width=2500pt]{KP_GISmaps/Ataya_studyfig} 

<!--chapter:end:01-AtayaIntro.Rmd-->

# Our Method  {#ourmethods}

## GIS Analysis Requirements 

This tutorial uses ESRI ArcGIS Pro (2.9). If using ArcGIS Pro, ensure your Spatial Analyst Extension is enabled. If you are unsure if this is enabled, you can check by going to Settings, Licensing, and ESRI Extensions. Once in the ESRI Extensions box, scroll down to Spatial Analyst. You will see if the license is active or "grayed out" (indicating you need to activate the Spatial Analyst License). 

Additionally, you will need Microsoft Excel (2022) and a reliable internet connection to download the LANDFIRE datasets pertinent to this analysis. 

Lastly, prior to beginning this analysis, we recommend that you create a new ArcGIS Pro project with a project specific geodatabase (named to your choosing!) If you are following the Ataya Study Area tutorial, please save that Study Area shapefile to your geodatabase. You'll be able to acquire an Ataya shapefile at the end of this section. Alternatively, if you are using your own Study Area shapefile, save it to your project specific geodatabase as you follow along with this tutorial.


## Methods 

Each subsequent chapter outlines steps to complete a different component of the GIS analysis. The general components of the analysis are:

* Downloading and preparing the necessary LANDFIRE datasets for a Study Area.
* Spatially combining these LANDFIRE datasets into one (1) new Study Area LANDFIRE dataset.
* Performing "Join Fields" to update the new Study Area Landfire dataset.
* Assessing acreage of the Study Area LANDFIRE dataset and "masking" it to the Study Area boundary.
* Performing data summary Excel steps to observe acreage of: historic ecosystems, current vegetation types, ecosystem conversion, and succession classes within the Study Area.

## Disclaimers 

There are always many different ways to complete a GIS analysis and because of this, we'd like to share a few disclaimers. 

First, this tutorial is specific to the Ataya Study Area and outlines steps exactly performed to this Study Area. If you are using a different Study Area, you will have to locate the relevant LANDFIRE datasets and Biophysical Settings (BpS) descriptions for your Study Area through the LANDFIRE website. 

Second, LANDFIRE data is best represented at coarse landscape scales (i.e. landscapes in excess of 100,000 acres or more). LANDFIRE data is not as accurate for fine landscape level assessments (i.e. a forest stand of 100 acres or less). Due to the development of LANDFIRE data at large landscape scales, some LANDFIRE ecosystem data, such as Existing Vegetation Type (EVT), may not represent a 1:1 relation of the GIS data to current ground conditions. We like to think of our analysis as a guide for understanding forest conditions past and present. This guide still functions best with human input on the ground to make effective final forest management decisions. If you have smaller acreage of forest land and still wish to perform this analysis, consider using the watershed your forest is in!  

Third, this tutorial GIS analysis was performed using ArcGIS Pro only. Unfortunately for managers who are using older versions of ArcMap or open source GIS/data processing software (i.e. GRASS GIS, QGIS) this tutorial is not designed for these GIS platforms. These platforms can likely recreate the outcomes of this tutorial, however specific geoprocessing tools may look or act differently. If using software other than ArcGIS Pro, make sure to understand the underlying process behind each tool. 

If you are having trouble with this tutorial, here are some people that can assist you!

* Problems with LANDFIRE data? helpdesk@landfire.gov
* Problems with the rMarkdown document? Randy Swaty: rswaty@tnc.org
* Problems with the GIS analysis? Keith Phelps: kpphelp@clemson.edu or Stacey Marion: stacmarion@gmail.com

We are always looking for ways to improve our GIS analysis steps, accessibility of our content, and availability of this analysis to as many forest managers as possible. If you have any suggestions for us, please reach out to Keith Phelps (kpphelp@clemson.edu), Stacey Marion (stacmarion@gmail.com), or Randy Swaty (rswaty@tnc.org). 


## Lastly... Download this!
To work through this guide you will need to download this [zipped folder](https://github.com/rswaty/landfireFSC/blob/main/toDownload.zip) by clicking the hyperlinked text, then the "Download" button that will be on the right side of the screen.  This folder contains:

* An Ataya shapefile
* An Excel file
* One ecosystem description
* An ArcPro map package file in case you want to explore in ArcPro.

## About this document

It was written in R using [R-Studio](https://rstudio.com/), the ["bookdown" package](https://www.bookdown.org/) and is hosted on [GitHub(links to this repository)](https://github.com/rswaty/landfireFSC).  

<!--chapter:end:02-OurMethods.Rmd-->

# LANDFIRE Data {#inputData}

## LANDFIRE for Landscape Ecosystem Assessments  

To support the assessment of a forest's conditions, including attributes such as Environmental Impacts of management and identifying and maintaining High Conservation Value Areas, we will consider:

* Change between historical and current ecosystem condition. 
* The representation of ecosystems within and outside of your landscape of interest.
* Change in Succession Classes (aka seral states) of communities over time.

These steps, while foundational and conceptually simple, can be difficult due to a lack of data, especially when the study area of interest is large scale, occurs across multiple land ownerships, or is subject to land cover change. Using LANDFIRE spatial models overcomes this data barrier by providing a suite of terrestrial data appropriate for landscape scale analysis. 

## LANDFIRE Datasets Used For This Tutorial

* Spatial datasets, clipped to your area(s) of interest
    * [Biophysical Settings (BpS)](https://www.landfire.gov/bps.php). This dataset will be used to look at "community habitat", or where ecosystems could occur based on abiotic factors (e.g., soils, climate and natural disturbance regimes).
    * [Succession classes](https://www.landfire.gov/sclass.php). This dataset characterizes structural classes on the landscape at the time the dataset represents (e.g., 2016 for LF Version 200).
    * [Existing Vegetation Type](https://www.landfire.gov/evt.php). This dataset maps NatureServe's Ecological Systems (see descriptions [here](https://www.landfire.gov/documents/LANDFIRE_Ecological_Systems_Descriptions_CONUS.pdf)).
* Non-spatial products
    * [Biophysical Settings Descriptions](http://landfirereview.org/test/search.php) These descriptions have information on natural disturbance regimes and succession class descriptions (also available [here](https://www.landfire.gov/zip/LANDFIRE_CONUS_SClass_Mapping_Rules_9182020.zip)).  
    * [Reference Condition Table](https://www.landfire.gov/zip/LANDFIRE_CONUS_Reference_Condition_Table_August_2020.zip) supplements the BpS descriptions with the "reference" percentages for each succession class, for each Biophysical Settings.
    
## Guidance on Obtaining LANDFIRE Products

There are multiple ways to get LANDFIRE products depending on whether you are looking to obtain BpS models, their descriptions, or the spatial data:

* For BpS models and descriptions go to: http://landfirereview.org/search.php. Start by clicking on the "View map of LANDFIRE Map Zones". This will help you narrow down your search. Alternatively, you can wait to download BpS descriptions until you do some GIS work and get specific names of BpSs of interest.
* For the spatial datasets you can explore options [here](https://www.landfire.gov/getdata.php) which include:
    * Downloading [Full Extent Mosaics](https://www.landfire.gov/version_comparison.php). When you do use this option you get large files that cover the entire lower 48, AK or HI (depending on your selection).  We use this method when we have several landscapes and/or a large area and no computer storage issues.
    * Using the [LANDFIRE Data Distribution Site](https://www.landfire.gov/viewer/).  With this method you essentially select the datasets you need then draw a rectangle (or you can select state or counties) around your area of interest to start the downloader. We recommend this if your area of interest is not too large and/or you have a low number of landscapes and/or have storage limits.
*We also recommend downloading your files in a TIFF with attributes format.*
    
    
**If you still need more assistance in downloading LANDFIRE products, please see the appendix for some tips and screenshots!** 

<!--chapter:end:03-inputData.Rmd-->

# LANDFIRE Data for Landscape Assessment {#gis}

## If you haven’t already... Download this!

Download this [zipped folder] (https://github.com/rswaty/landfireFSC/blob/main/toDownload.zip) by clicking the hyperlinked text, then the "Download" button that will be on the right side of the screen.  This folder contains:

* An Ataya shapefile
* Folders containing the BpS, EVT, and Sclass rasters 
* An Excel file
* One ecosystem description
* An ArcGIS Pro map package file in case you want to explore in ArcPro. This map package includes the following:
  * Our Ataya shapefile
  * BpS, EVT, and SClass maps for Ataya
  * A combined raster file of BpS, EVT, and Sclass datasets. 

## Load Spatial Data 

Open ArcGIS Pro and create a new project using a blank map template. Move the downloaded Ataya shapefile tutorial data into your project folder.


\includegraphics[width=1000pt]{04_gis_screenshots/1_load_data} 

Note that Pro creates a default geodatabase with the same name as your project. We recommend saving the Ataya shapefile to your geodatabase and hereafter, save intermediate layers to your geodatabase.

**Consider your study area:** Is it appropriate to end your analysis at the shapefile boundary? If you would like information to extend beyond the study boundary, consider creating a buffer. For our tutorial, we will limit our analysis to within the provided Ataya forest tract boundary.

## Download Three (3) LANDFIRE Rasters for the Study Area: BpS, EVT, and sClass
There are multiple ways to get LANDFIRE products.

* For the spatial datasets you can explore options [here](https://www.landfire.gov/getdata.php), including:
* Use the [LANDFIRE Data Distribution Site](https://www.landfire.gov/viewer/).  With this method you essentially select the datasets you need then draw a rectangle (or you can select state or counties) around your area of interest to start the downloader.  We recommend this if your area of interest is not too large and/or you have a low number of landscapes and/or have storage limits. *Recommended for this tutorial.*
* Download [Full Extent Mosaics](https://www.landfire.gov/version_comparison.php) to obtain large files that cover the entire lower 48, AK or HI (depending on your selection).  We use this method when we have several landscapes and/or a large area and no computer storage issues.   

For some LANDFIRE data products, like the BpS model, the model (spatial data) and descriptions are downloaded separately:
* For BpS models and descriptions go to: http://landfirereview.org/search.php. Start by clicking on the "View map of LANDFIRE Map Zones".  This will help you narrow down your search. We recommend downloading descriptions for BpS models relevant to your study area.


\includegraphics[width=1000pt]{04_gis_screenshots/2_view_mapzone} 

Note that for this tutorial, we downloaded rasters by using the rectangle drawing tool and *drew a rectangle that was larger than our Study Area*. This ensures no raster cell data is lost in our calculations. This tutorial allows us to "cut" out that extra area in the end. If you are downloading your own data, always make sure to draw a downloading box that is larger than your Study Area to prevent GIS errors. 

Once downloaded, move the LANDFIRE data product downloads into your Pro project folder. You should have a BpS, EVT, and SCLASS folder. Each folder contains metadata, a corresponding database table, and a raster.


\includegraphics[width=500pt]{04_gis_screenshots/3_fsc_pro_poject_catalog} 

## Project to the Appropriate Coordinate System
The spatial reference of the Ataya tract shapefile is NAD83_StatePlane_VA_South_FIPS_4502_Feet Projected Coordinate System (Geographic Coordinate System NAD 1983).


\includegraphics[width=1000pt]{04_gis_screenshots/4_crs_ataya} 

1. LANDFIRE data (BpS, EVT, sClass rasters) was downloaded as “Best Fit UTM (NAD83 Datum)” so the Projected Coordinate System of the LANDFIRE rasters is NAD83_UTM_Zone_17N (Geographic Coordinate System was GCS_North American_1983).
Given the size and location of our study area, we’ll use the Virginia State Plane (South) as our map coordinate system. *Note: unit is feet.*

2. Use the Project Raster tool to project each raster to the Virginia State Plane (South) coordinate system. Save the projected raster to the project geodatabase. *Since the datum is the same for State Plane and UTM, no “Geographic Transformation” was needed. Resampling technique was “Nearest Neighbor” which is preferred for categorical/discrete data. Output cell size was in feet (98.425ft. = 30m.)*


\includegraphics[width=1000pt]{04_gis_screenshots/5_raster_project} 

3. Project the BpS, EVT, and sClass Rasters *(sClass projection shown above).*
4. These (3) new rasters can now be used to create GIS maps for:
  * Historical ecosystems (Bps) 
  * Existing ecosystem types and land cover (EVT) 
  * Succesion class maps (sClass)

## LANDFIRE Data Stacking 
The three used LANDFIRE data products (BpS, EVT, sClass) provide unique aspects of land cover information. Because each raster maintains the same 30 x 30 meter cell, we can stack the LANDFIRE rasters to gain information about each arbitrary pixel within our study area. 

Before we combine the rasters, take a look at the attribute table for each raster. Details on attributes can be found at landfire.gov
Open the Combine tool. This tool can be found by running a search in geoprocessing or you can find it in the Spatial Analyst Toolbox. Run the combine tool using the (3) project LANDFIRE rasters: us_200bps_proj, us_200evt_proj, is_200sclass_proj. Output raster was labeled combine_landfire and saved within the project geodatabase.


\includegraphics[width=1000pt]{04_gis_screenshots/6_combine_rasters} 

Open the attribute table of the combine_landfire raster. You will notice that only the “Value” field was retained in the combine. Next we will perform joins to append tabular data to the combine_landfire raster.


\includegraphics[width=1000pt]{04_gis_screenshots/7_combine_noattributes} 

## Join BpS Tabular Data
Use the Join Field tool (found with a geoprocessing search) to permanently join BpS attributes to the combine_landfire raster. Follow these parameters:

1. Input Table = combine_landfire
2. Input Join Field = us_200bps_proj
3. Join Table = us_200bps_proj.	
4. Join Table Field = VALUE.
5. Transfer Fields = BPS_CODE, BPS_MODEL, BPS_NAME, GROUPVEG. *Note: selecting additional fields will prevent you from using the prepared excel pivot tables.*


\includegraphics[width=1000pt]{04_gis_screenshots/8_join_bps} 

## Join EVT Tabular Data
This second join is to import EVT attributes. Open the Join Field Tool again and follow these parameters:

1. Input Table = combine_landfire
2. Input Join Field = us_200evt_proj
3. Join Table = us_200evt_proj
4. Join Table Field = VALUE
5. Transfer Fields = EVT_NAME, EVT_PHYS. *Note: selecting additional fields will prevent you from using the prepared excel pivot tables.*


\includegraphics[width=1000pt]{04_gis_screenshots/9_join_evt} 

## Join sClass tabular data
The third and final join is to import the sClass attributes. Open the Join Field tool again and follow these parameters: 

1. Input Table = combine_landfire
2. Input Join Field = us_200sclass_proj
3. Join Table = us_200sclass_proj
4. Join Table Field = VALUE
5. Transfer Fields = LABEL, DESCRIPTION. *Note: selecting additional fields will prevent you from using the prepared excel pivot tables.*


\includegraphics[width=1000pt]{04_gis_screenshots/10_join_sclass} 

## Crop the combined raster to the study area 
To "cut" the combined raster to the boundary of our shapefile, use the Extract by Mask tool. Follow these parameters:

1. Input Raster = combine_landfire 
2. Input Raster or Feature Mask Data = Ataya feature class
3. Output raster = combine_studyArea
*Reminder: if you decided to create a buffer around your study area, use the buffered study area feature class as your feature mask.*


\includegraphics[width=1000pt]{04_gis_screenshots/11_extract} 

The resultant "combine_studyArea raster" will only contain pixels within the Ataya study region: 


\includegraphics[width=1000pt]{04_gis_screenshots/12_combine_studyArea} 

We choose to maintain the shape of the original raster (default setting within Extract by Mask tool), and thus the resultant extracted raster has jagged edges. Note that area calculations using the raster will differ slightly from area calculated from the vector feature. 


\includegraphics[width=1000pt]{04_gis_screenshots/13_jagged_edge} 

## Calculate Area Field
Next, we will calculate area using raster pixel size for "combine_studyArea" raster. Follow these steps:

1. Open the combine_studyArea attribute table.
2. Within the attribute table, create a new field called “Acres”. Select “Float” for data type to allow decimals in the calculation for acres.


\includegraphics[width=1000pt]{04_gis_screenshots/14_add_acres_field} 


\includegraphics[width=1000pt]{04_gis_screenshots/15_fields_view} 

3. In the “Acres” field, right click on the top and scroll to “Calculate Field”.
4. Select “Python 3” as the Expression Type.
5. In “Acres =” banner, type this expression: !Count! *9687.4806249998 / 43560.
6. Click "Apply", then "OK". 


\includegraphics[width=1000pt]{04_gis_screenshots/16_calculate_acres} 

*The sum of the Acres field corresponds to the total Ataya area (~100,653 acres).* 


\includegraphics[width=1000pt]{04_gis_screenshots/17_check_area_calc} 

**Understanding the Field Calculation Expression**

* We use raster cell size to ascertain acreage. (See: https://gis.stackexchange.com/questions/106883/measuring-area-of-raster-classes) . Our general calculation to determine acreage is [Pixel count of xxx unique raster value] x [Area of a pixel (feet)] x [1 acre/43,560 sqft] = xxx acres of a specific unique raster value 
* !Count! is the number of raster pixels classified as a unique raster value. From our attribute table, we see that we have 549 unique raster values. These unique raster values hold unique combinations of our BpS x EVT x sClass data.
* Each original raster downloaded from LANDFIRE had a cell size of 30x30m. When we projected our rasters to Virginia State Plane South FIPS 4502 Feet, we changed the units of our rasters to feet; yielding cell size of  98.425 x 98.425 feet. The area of a single pixel is  98.425 x 98.425, equaling 9,687.4806. Therefore, we multiply  !Count! by 9,687.4806. This yields the square footage associated with each unique raster value for our Study Area. To convert to acres, divide by 43,560 (1 acre = 43,560 sqft).
* *If you are using a different unit (i.e. meters), you must adjust this calculation accordingly.* 

## Export combine_studyArea attribute table into a .csv format
Now we'll take our GIS attribute table and make it usable in Excel. The following steps provide instructions for turning an attribute table into a .csv file. If you want to turn the attribute table into a new excel workbook, please see the appendix.

1. Within the attribute table panel, navigate to the ‘burger’ menu on the top right, and select Export Table. 
2. Select combine_studyArea as the Input Rows. 
3. Next, select a folder location to export your table. Save the table as a logical name, such as ataya_attributes with the .csv appendage. You must manually add the .csv appendage to export as a csv. 


\includegraphics[width=1000pt]{04_gis_screenshots/18_export_table} 

## Open the Tutorial Excel Workbook 
Next, open the Ataya _combineClean.xlsx in the toDownload folder containing tutorial-specific files. This template excel spreadsheet already contains the exported Ataya attribute table in the first tab. The remaining tabs contain templates for using pivot tables to summarize the Ataya attributes.

If you performed the tutorial using your own study area rather than the Ataya study area, copy your csv into the Ataya_combineClean.xslx template spreadsheet in the place of the ataya_attribute tab. Name the spreadsheet to better represent your study area. Next, delete the OID, BpS code, Value, and DESCRIPTION columns to match what's in our workbook.

The following sections discuss analysis of historical conditions, current conditions, land use conversion and succession classes.

<!--chapter:end:04-GIS.Rmd-->

# Historical Ecosystems {#historicalEcosystems}

In this section we will learn which ecosystems and their extent on our landscape historically. Before we begin, let's review our BpS GIS map and draw some quick visual conclusions:


\includegraphics[width=1000pt]{KP_GISmaps/Ataya_bps} 

According to LANDFIRE, most of Ataya was dominated by:

* Southern Appalachian Oak Forest
* Southern and Central Appalachian Cove Forest
* South-Central Interior Mesophytioc Forest 

We can also see that most of the Southern Appalachian Oak Forest and Cove Forest is confined to the southwest portions of the Study Area. In contrast, South-Central Interior Mesophytic Forests are more prevalent in the northern portions of the Study Area. Ok- let's move on! 

## General Methods

In this section, and most others following, we will depend on "Pivot Tables" in Excel. These are powerful (for better or worse!) tools that allow for tasks such as:

* Organization and formatting of data.
* Calculations.
* Filtering data.
* Nesting data.

With the power comes the call for caution.  It is very easy to display values that look illuminating- but may be wrong. You can easily be duped into complacency, especially when working in the "value field settings."

### Some Potential Traps with Pivot Tables

Pivot Tables allow you to calculate, summarize, format and analyze datasets. These are a few traps we run into frequently which are worth mentioning here:

* **Your Pivot Table controls go away**.  This happens when you click outside of the main Pivot Table area (where your values are, usually on the left side of screen).  To fix this click inside one of the Pivot Table columns.  Another way to fix this is to right click inside a Pivot Table cell and select "Show field list". 
* **Miscalculating.**  This typically happens when you are in the "Value Field Settings" pane and select the wrong "Show values as" option.  The key here is to visually inspect the results to make sure you have made the correct selection.
* **You want to remove totals and subtotals without having to delete them all.** To fix this and many formatting issues click anywhere inside of the Pivot Table -> Click "Design" from the top ribbon -> click the "Subtotals" drop down -> click "Do Not Show Subtotals".  Repeat for Grand Totals.  While you are there, explore the Report Layout and Blank Rows drop downs.


## Getting at amounts of historical ecosystems

**Start by opening the "Historical" tab in the Excel workbook ("combineClean").**  

1. In the Pivot Table Fields pane, select "BPS_NAME" then "ACRES".  
2. Right click in the top cell of the "Sum of ACRES" column (not the column header) in the Pivot Table, then "Sort Largest to Smallest".
3. In our example we have some BpSs that have low ACRES values. We can do a little formatting/cleaning before making a chart:
    * To remove BpSs from the table you will click the drop-down menu to the right of "BPS_NAME" in the Pivot Table Fields pane.  You can uncheck BpSs as appropriate.
    * It is also possible to filter by right clicking on the top value in the list of BpSs, then selecting Filter > Top 10....  Once in that menu you can refine the filtering.
4. To get percentages, drag "ACRES" from the top Pivot Table Field pane to the "Values" pane. This will add a second "ACRES" column to the table. Right click in the second instance of "ACRES" (reads "SUM of ACRES2" in our example) to open up a drop down menu, then navigate to Value Field Settings. In this menu select the "Show Values As" tab, click the "Show Values As" drop down then select "% of Grand Total% to get percentages of each BpS (make sure that "BPS_NAME" is selected as the "Base field").  
5.  To get a "running total" of percentages you will add a third instance of "ACRES" to the "Values" pane, then navigate to Value Field Settings.  In this menu select the "Show Values As" tab, click the "Show Values As" drop down then select "% Running Total In" to get running totals of  percentages of each BpS (make sure that "BPS_NAME" is selected as the "Base field"). 
6. Save!


**Example output**

Below is essentially the output from our Pivot Table work with a couple changes:

* Names have been shortened for ease of reading.
* Numbers have been rounded.

![](FSCBook_files/figure-latex/bpsDT-1.pdf)<!-- --> 

<br>
<br>

We see that the top 3 BpSs comprised ~80% of our example landscape historically. We can visually confirm this and other patterns with a quick chart made in R (similar charts available in Excel by highlighting the data, clicking Insert then selecting the chart type in the "Charts" tab):

<br>

![](FSCBook_files/figure-latex/bpsChart-1.pdf)<!-- --> 




<!--chapter:end:05-HistoricalEcosystems.Rmd-->

# Existing Vegetation Types {#evt}

While looking at the historical ecosystems gives context, we also need to get a picture of which ecosystems are on the landscape today. Like the last section, let's first take a look at our EVT GIS Map to draw some conclusions:
'

\includegraphics[width=1000pt]{KP_GISmaps/Ataya_evt} 

As we can see, there are FAR more ecosystem types present in Ataya today. In the map we see that there is far more Allegheny Dry Oak Forest/Woodland, Northern and Central Native Ruderal Forest, and recently logged cover. This map is certainty different from our Bps map! Let's move on... 

## General Methods

We use the same general methods as we did on the "Historical Ecosystems" page, as we will be using another Pivot Table. We rely on [LANDFIRE's Existing Vegetation Type (EVT)](https://www.landfire.gov/evt.php) data for this assessment.


## Getting at Amounts of Current Ecosystems

**Start by opening the "current" tab in the Excel workbook.**

1. In the Pivot Table Fields pane, select "EVT_NAME" then "ACRES".  Make sure that EVT_NAME is in the "Rows" pane, and that "Sum of ACRES" is in the "Values" pane.  
2. Right click in the top cell of the "Sum of ACRES" column (not the column header) in the Pivot Table, then "Sort Largest to Smallest".
3. In our example we have some BpSs that have low ACRES values. We can do a little formatting/cleaning before making a chart:
    * To remove EVTs from the table you will click the drop-down menu to the right of "BPS_NAME" in the Pivot Table Fields pane.  You can uncheck BpSs as appropriate.
    * It is also possible to filter by right clicking on the top value in the list of EVTs within the Pivot Table, then selecting Filter > Top 10....  Once in that menu you can refine the filtering.
    * Additionally, we would like to add commas and remove decimal places. To do this right click the row that contains "Sum of ACRES" in the spreadsheet (likely row "B"), select "Format Cells" > Number > set Decimal places to "0" then check the "Use 1000 Separator box.
4. To get percentages, drag "ACRES" from the top Pivot Table Field pane to the "Values" pane.  This will add a second "ACRES" column to the table. Right click to open up the drop down in the second instance of "ACRES" (reads "SUM of ACRES2" in our example), then navigate to Value Field Settings. In this menu select the "Show Values As" tab, click the "Show Values As" drop down then select "% of Grand Total% to get percentages of each EVT (make sure that "EVT_NAME" is selected as the "Base field").  
5.  To get a "running total" of percentages you will add a third instance of "ACRES" to the "Values" pane, then Value Field Settings.  In this menu select the "Show Values As" tab, click the "Show Values As" drop down then select "% Running Total In" to get running totals of  percentages of each BpS (make sure that "BPS_NAME" is selected as the "Base field"). 
6. Save!




**Example output**

Below is essentially the output from our Pivot Table work with a couple changes:

* Names have been shortened for ease of reading
* Numbers have been rounded
* We have only included a selection of (5) EVTs

![](FSCBook_files/figure-latex/evtDT-1.pdf)<!-- --> 

<br>
<br>

We see that first there are many EVT's. This is partially due to the addition of Developed and other "modern" categories such as "Recently Logged-Tree Cover". Second we see that of the "natural" types there are some important changes from historic ecosystem conditions. In Ataya, there is currently far more acreage associated with Allegheny Dry Oak Forest/Woodland. We can visually confirm this and other patterns with a quick chart made in R (similar charts available in Excel by highlighting the data, clicking Insert then selecting the chart type in the "Charts" tab):

<br>

![](FSCBook_files/figure-latex/evtChart-1.pdf)<!-- --> 



<!--chapter:end:06-evt.Rmd-->

# Exploring Conversion {#conversion}

**The main question we explore on this page: how much of the historic ecosystems have been converted to a different land use or ecosystem type? (e.g., agriculture, forest types).**  

## The Fine Print
While this assessment is illustrative, it is important to note that the methods used to create the BpS and EVT datasets are substantially different, and LANDFIRE datasets are not made for assessing small areas.  Please review.  In the exercise we will focus on the coarsest classifications.  If you need to work at the finer classification levels proceed with caution!

## Pivot Table Work
Working in the "Conversion" worksheet:

1. Check the "GROUPVEG" then the "EVT_PHYS" fields to add them to the Pivot Table.  Make sure that GROUPVEG is the leftmost column.  If not drag that field to the top in the Rows pane.
2. In the cell above the "GROUPVEG" field header type "past"; in the cell above the "EVT_PHYS" type "present".
3. If you fancy click the dropdown in the "GROUPVEG" header.  Here you can remove types.
4. Check the "ACRES" field to add it to the Pivot Table.
5. Explore.  Of what was historically mapped as CONIFER, what's the biggest non-CONIFER type today?
6. Switch the order of the fields by dragging "EVT_PHYS" to the top of the Rows pane. Which GROUPVEG type was most converted to "Developed"?

We can easily make excel charts and bar graphs to summarize these findings and demonstrate change from historic conditions. We can also dive a little bit deeper to generate a table that illustrates percent change from historic ecosystem conditions to current ecosystem conditions. 

## Excel Steps for Preparing an Ecosystem Conversion Table 
Here are steps to create a table summary which will allow us to view percent change of historical ecosystems (Bps) into current ecosystems (EVT):

1. Open the "Bps2evt" worksheet
2. In the Pivot Table, check "BPS_Name", "EVT_Name", and "COUNT"
3. In the Main Ribbon of the worksheet (where File, Home, Insert, etc. are located) navigate all the way to the last Tab on the right labeled "Design". Click on "Design".
4. In the "Subtotals" tab, click on the down arrow and navigate to "Do Not Show Subtotals". Click on this option. 
5. In the "Grand Totals" tab, click on the down arrow and navigate to "Turn Off for Rows and Columns". Click on this option. 
6. In the "Report Layout" tab, click on the down arrow and navigate to "Show In Tabular Form". Click on this option. 
7. In the "Report Layout" tab, click on the down arrow and navigate to "Repeat All Item Labels". Click on this option. 
8. In the top data cell for the "BPS_Name" data column, right click and navigate to "Filter". Click on "Top 10..."
9. In the box that opens, change "10" to 90 and "Items" to "Percent". 
    * This will select the top 90% of BpS settings in the Ataya Study Area. To display all BpS, we would type in 100. 
10. In the top data cell for the "EVT_Name" data column, right click and navigate to "Filter". Click on "Top 10..."
11. In the box that opens, change "10" to 90 and "Items" to "Percent". 
    * This will select the top 90% of EVT settings in the Ataya Study Area. To display all EVTs, we would type in 100. 
12. In the "Sum of Count" data column, right click on the top cell and navigate to "Value Field Settings". 
13. In "Value Field Settings", click on "Show Values As" and navigate to "% of Parent Row total" and click. 
14. Change the name of "Sum of Count" to "Percent". 

Voila! We now have a table which should look similar to what's below. For this tutorial, we have rounded our percents and are only displaying a portion of the table for better website viewing. 

![](FSCBook_files/figure-latex/bps2evtDT-1.pdf)<!-- --> 

## Interpreting the Ecosystem Conversion Table
This table gives us a general idea of the percent change from historic ecosystems into current ecosystems. In our table above, we see that 57% of historic Allegheny-Cumberland Dry Oak Forest/Woodland has remained as this ecosystem type within Ataya. We also see that 14% of Allegheny-Cumberland Dry Oak Forest/Woodland has converted into Northern & Central Ruderal Forest (a forest type marked by "weedy" generalist species). Additionally, 29% of South-Central Interior Mesophytic Forest has converted into Alleghany-Cumberland Dry Oak Forest/Woodland.  

Here is a potential ecological interpretation for the Ecosystem Conversion Table results of our Study Area (partially based on the table and partially based on local knowledge):

* There is far more Allegheny Dry Oak Forest and Northern/Central Native Ruderal Forest mapped today than historically. Additionally, Southern Appalachian Cove and South-Central Interior Mesophytic Forests have greatly decreased over time. This could be due to a legacy of extensive ecosystem disturbance on the landscape. For example, we know that extensive clear-cut logging occurred all throughout the Appalachians in the late 19th and early 20th centuries. After these cut-over events, extensive wildfires erupted across the region due to the residual logging slash. These wildfires also spawned US Forest Service fire suppression policies in the early 20th century which altered ecosystem disturbances further. 
* One hypothesis would be that due to homogeneous logging and wildfire disturbance across large landscape contexts, favorable conditions to shade-intolerant oak species (i.e. high light levels, open canopies, reduced duff layers) resulted in a massive increase in oak recruitment. As the forests grew back, oak became a far more common tree species and influenced the development of forest types away from maples, tulip-poplars, and other more shade-tolerant trees- typical dominants of Southern Appalachian Cover and South-Central Interior Mesophytic Forests. Additionally, as American chestnuts were lost due to chestnut blight in the early 20th century, oaks most likely took their place.  

We can also dive EVEN FURTHER by visualizing these ecosystem changes...

## Visual Exploration with a Sankey Diagram 

Sometimes patterns emerge more explicitly from visuals. Below is a [Sankey Diagram](https://en.wikipedia.org/wiki/Sankey_diagram) generated in R (which can also be created in Excel we hear. See instructions [here](https://mychartguide.com/how-to-draw-sankey-diagram-in-excel/)). We used the data from our Conversion Table above to create this R figure. On the left are the historical ecosystems, on the right current ecosystems and flow bands. The width of the bands is proportional to the flow rate. 

Click [here](https://rswaty.github.io/landfireFSC/sankey.html) to the view diagram in your web browser. 

![](FSCBook_files/figure-latex/Atayasankey-1.pdf)<!-- --> 

<!--chapter:end:07-Conversion.Rmd-->

# Succession Classes {#successionClasses}

## What We Will Do
While looking at conversion of historical ecosystems is valuable, we can perform more analyses, comparing amounts historical succession classes to current. This gives us a broad measure of the changes in vegetation structure for ecosystems.

## What are "Succession Classes?"

In general LANDFIRE succession classes are stages of development defined in the descriptions of each Biophysical Setting. They are characterized by vegetation height, canopy cover and to some degree species composition. Key takeaways:

* To learn what the succession classes are for each Biophysical Setting you look to the corresponding description on http://landfirereview.org/test/search.php
* LANDFIRE used state and transition models to estimate the amount of each succession class that would occur with natural (pre-European colonization) disturbance regimes.  Historical succession classes were not mapped as they were assumed to move around over time.
* The [Succession Class](https://www.landfire.gov/sclass.php) dataset maps the current location of succession classes, in addition to agricultural and developed land-use classes plus uncharacteristic vegetation. Uncharacteristic typically represents exotic vegetation, or vegetation structure that would not have occurred historically.
* By comparing the amounts of historical and current succession classes you can get a sense of which classes are over/under represented. This assumes that the landscape you are assessing is large enough to potentially include the full compliment of succession classes.

## Our Sample Ecosystem 

First, let's take another look at our sClass GIS map for some visual context: 


\includegraphics[width=1000pt]{KP_GISmaps/Ataya_sclass} 

We see that most of the succession classes in Ataya are in Succession Class B, D, and E. There is also a large amount of area devoted to UE: Uncharacteristic Exotic Vegetation. 

To illustrate the concept of comparing reference to current succession classes, we will focus on the South-Central Interior Mesophytic Forest Biophysical Setting. It is a dominant ecosystem in the Ataya, has been heavily managed, has experienced extensive clear cut logging, and faces many threats such as deer browsing (and in isolated pockets hemlock woody adelgid damage).  

## Your Tasks
We will start with our friend the Pivot Table to get current percentages of the relevant succession classes, then we will look to the Biophysical Settings descriptions to get historical percentages.

### Current Succession Classes

1. Open the "successionClasses" worksheet in our "Ataya_combineClean.xlsx" workbook.
2. In the Pivot Table Fields box highlight "BPS_MODEL" -> click the down arrow -> select only "South-Central Interior Mesophytic Forest".  Check the box next to "BPS_NAME".
3. In the Pivot Table Fields check the box next to "LABEL" to get a list of current succession classes.
4. In the Pivot Table Fields check the box next to "ACRES".
5. In the Values pane click the down arrow -> select Value Field Settings. Once the Value field settings are open click Show Values As -> select "% of Grand Total" from the dropdown list.
6. Copy the resultant table -> paste next to the Pivot Table in the same worksheet -> add column names "Succession class" and "Current". 

###  Historical succession classes

1. Open the "SouthMesophytic.docx" document that is in the "toDownload" zipped folder.  
2. Skim through the document to get a sense of the ecosystem, paying particular attention to the "Succession Classes" section.
3. There are be 5 succession classes, A, B, C, D and E with corresponding estimated reference amounts. For example, Succession Class A was modeled to be rare in this ecosystem as fire was infrequent with the average fire return interval for the ecosystem being 50-200 years. LANDFIRE estimated that only 2% of this ecosystem would have been Succession Class A historically.
4. Back in the pivot table, you will create a new column named "Historical", then type the succession class percentages from the document into the corresponding row.  

Now we can start to see some patterns emerge. What do you see and can you draw any conclusions for forest management within the Ataya? We won't spoil your fun by saying what those patterns are!


<!--chapter:end:08-SuccessionClasses.Rmd-->

# Conclusions {#conclusions}

## Bringing it All Together

We've got a lot of useful deliverables from our analysis (GIS maps, tables, charts, interpretations) but let's spend some time bringing all our analyses together. For Ataya, we see that historically our forest was dominated mostly by Southern Appalachian Oak Forest, Southern and Central Appalachian Cove Forest, and South-Central Interior Mesophytic Forests. We also know the current forests have shifted most to Allegeheny Dry Oak Forest/Woodland. Southern and Central Appalachian Cove Forests have experienced a particularly high degree of ecosystem conversion. Additionally, Ataya has large areas of uncharacteristic exotic vegetation and native ruderal forest. 

**Let's draw some potential forest management implications.** 

This is not an exhaustive list by any means- just some ideas to get the creative juices flowing. Feel free to revisit our previous sections to view GIS maps and charts! 

* We see a very high departure from historic conditions for Ataya. Our restoration efforts shouldn't be focused on re-converting everything back to our BpS map as this is not feasible. This is not to say we should avoid preserving and locating unchanged forest. We can use our BpS and EVT maps to potentially find areas where the forests have remained stable for longer periods of time, and use these areas as representatives of historic conditions for teaching purposes, genotype conservation, and seed collection/preservation among other things.  
* Perhaps we need to spend some extra time mapping Cove Forest communities in the field. If this forest type is experiencing particular heavy ecosystem conversion, it may be important we assess, preserve, and more intimately manage the best representations of that forest community type.
* We have a better idea of where we may need to concentrate efforts on invasive plant management. This includes areas in the southern portions of Ataya where concentrated pockets of uncharacteristic exotic vegetation can be observed. A field assessment could confirm these suspicions and influence an Integrated Pest Management plan. 
* We know that oaks require specific shade-reduced conditions to regenerate and recruit into the forest canopy. Based on the level of oak community types present on the landscape now, prescribed fire and timber harvesting to facilitate oak regeneration (i.e. thinning, shelterwood cuts) will be incredibly important for a forest management plan/strategy. 
  * Since most of the oak dominated forests in the south of Ataya are in Succession Class D and E (i.e. more closed canopy conditions), prescribed fire and harvesting activities to facilitate tree canopy gaps and oak recruitment may be very important in this section of the forest. 


## Recap for certified forest management 

After our analysis is complete, we can see that we have provided the following for demonstrating conformance to certified sustainable forest management standards:

* Maps and acreage for historic ecosystems 
* Maps and acreage for current ecosystems/land cover
* Mapss of current Forest Succession Classes
* Tables to assess ecosystem conversion
* Tables to assess acreage for historic and present conditions
* Some strategies for developing management implications with LANDFIRE 

We hope you have enjoyed our tutorial. We wish you all the best in your future LANDFIRE analyses and keep us updated with any unique findings for your forest area! 

<!--chapter:end:09-Conclusions.Rmd-->

# APPENDIX 

## LANDIFRE Data Downloading Help 

**LANDFIRE Data Download Homepage**

1. LANDFIRE 2016 EVT data will be the default display.
2. You can turn off EVT layers and find different data layers on the left hand column. 
3. Regions and bookmarks can be found/made on the right. 


\includegraphics[width=1000pt]{KP_screenshots/LANDFIRE First Load} 

**LANDFIRE Identify Tool**

1. Click where the red arrow is pointing. 
2. This will allow you to ID layers you have selected. Your query will be displayed in the right hand column (under "Feature Info"). 
3. We see that we have clicked on "Southern and Central Appalachian Cove Forest" from our EVT layer.


\includegraphics[width=1000pt]{KP_screenshots/LANDFIRE Identify} 

**LANDFIRE Data Download Tool**

1. In the right-hand column, expand the "Download Tool" (where the red star is located).
2. Click on the Blue Download Tool Icon in the toolbar.
3. Select "Rectangle" where the red arrow is pointing. 
4. Draw a Study Area of your interest. 
5. Select your projection.
6. Select your data product. We highly recommend TIFF with attributes (this will ensure the attribute table comes over with the raster).


\includegraphics[width=1000pt]{KP_screenshots/LANDFIRE Rectangle Data Download} 

**Download EVT Data**

1. Select the drop down menu for "LF 2016" on the left-hand column (first red arrow).
2. Click on us_200 Existing Vegetation Type (second red arrow).


\includegraphics[width=1000pt]{KP_screenshots/LANDFIRE Find EVT} 

**Download BpS Data**

1. Select the drop down menu for "LF 2016" on the left-hand column (first red arrow).
2. Click on us_200 Biophysical Settings (second red arrow).


\includegraphics[width=1000pt]{KP_screenshots/LANDFIRE Find BpS} 

**Download Sclass Data**

1. Select the drop down menu for "LF 2016" on the left-hand column (first yellow arrow).
2. Expand the "Fire Regime" folder beneath Fuel 2020 Capable (second yellow arrow).
3. Click on us_200 Succesion Classes. 


\includegraphics[width=1000pt]{KP_screenshots/LANDFIRE Find sClass} 


## Export "combine_studyArea" attribute table into an Excel Workbook

**Turn your GIS attribute table into an excel workbook and create your own pivot tables**

1. Locate the "Table to Table" tool in a geoprocessing search.
2. In inputs rows, select "combine_studyArea".
3. For the output location, save it to your project database.
4. For output name, type "Ataya_combineClean" (or your preferred name).


\includegraphics[width=500pt]{04_gis_screenshots/19_table_table} 

5. Run the tool. Once complete locate "Table to Excel".
6. For the Input Table, select the table we just created in our project database.
7. Navigate to an output folder location of your choosing. *Make sure to type ".xlsx" at the end of the file pathway. This will ensure your excel file is in the latest excel workbook format.*


\includegraphics[width=500pt]{04_gis_screenshots/20_Table_Excel} 

8. Click "Run" and open your table!
9. You can now clean up the data table by getting rid of unnessecary columns (OID, VALUE, BPS CODE, etc.)
10. To create pivot tables, select all columns and go to Insert -> Pivot Table -> New worksheet. 
11. Repeat step 10 for all the pivot table analyses you need. 

<!--chapter:end:appendix.Rmd-->

---
title: "Randy's Notes on Ataya"
author: "Randy Swaty"
date: '2022-06-13'
output:
  pdf_document: default
  html_document: default
---


# Basic QAQC on Ataya_combineClean.xlsx

Partly a QAQC, but more a way to force me to look more critically

* do acres add up?  Sum of COUNT of "combineClean" sheet = 452589*.222 = 100,474 Acres = Good Enough

## BpS
* does the list of BpSs make sense?  

    *  Southern Appalachian Oak Forest	31.99%
    *  Southern and Central Appalachian Cove Forest	25.16%.  
    *  South-Central Interior Mesophytic Forest	22.56%
    *  Allegheny-Cumberland Dry Oak Forest and Woodland	11.71%
    *  Central Interior and Appalachian Riparian Systems	5.63%
    *  Appalachian (Hemlock-)Northern Hardwood Forest	2.11%
    *  Southern Appalachian Northern Hardwood Forest	0.35%
    *  Open Water	0.23%
    *  Central Interior and Appalachian Floodplain Systems	0.16%
    *  Central Interior and Appalachian Swamp Systems	0.05%
    *  Southern Appalachian Low-Elevation Pine Forest	0.04%
    
*Notes from this list*
* the list of names is OK
* relative amounts raise eyebrows as we'd expect less cove/mesophytic and more oak
* does seem like LANDFIRE mapped too much of the cove forest type, which should be restricted to the "opographically protected areas (e.g. coves, v-shaped valleys, north- and east-facing toe slopes) within highly dissected hills and mountains".  
* the South-Central Interior Mesophytic Forest is not "oaky" either.
* ~42% of the area was mapped as oak types historically

* Does BpS attribute table match when I reprocess? YES
* I have sent remade BPS and EVT maps to Mark Rogers who works on the Ataya for his opinion. 




# Ideas for dealing with suspect LANDFIRE data

* link to Helmbrehct and Blankenship data review paper https://www.conservationgateway.org/ConservationPractices/FireLandscapes/LANDFIRE/Documents/ModifyingLF_DataGuide_V1.pdf
* reclassify if there are credible reasons to
* don't use spatial data.  Can still use BPS models and descriptions to get concepts.
* "lump up" i.e., use group veg or something...can still explore conversion





<!--chapter:end:rs_qaqc.Rmd-->


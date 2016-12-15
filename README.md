## A Summary of Recent Changes

A more detailed description of changes can be found on the [Pentaho Report for Odoo wiki](https://github.com/WilldooIT/Pentaho-Odoo/wiki/Significant-Changes "Pentaho Reports for Odoo Wiki")

* December 2016 - Repository rename to Pentaho-Odoo in preparation for version 10.0 development.  Company references changed to WilldooIT.  Note: Not tested at all yet with version 10!
* February 2016 - Upgrade to support Odoo version 9.0.
* September 2015 - Overcome previous limitation - Now works with the auth_crypt module turned on.
* August 2015 - Remove need for special duplicated "Pentaho user" - a limitation has been introduced for object based reports with password encryption turned on - work-around in pipeline.
* August 2015 - Testing with Pentaho version 5.4 - updated war file on Willow website.
* April 2015 - Testing with Pentaho version 5.3 - updated war file on Willow website.
* October 2014 - Support for Excel 2007 format output (xlsx).
* October 2014 - Testing with Pentaho version 5.2 - updated war file on Willow website.
* June 2014 - Upgrade to support Odoo version 8.0.
* May 2014 - Report selection sets can be nominated as default selections for users or groups.
* April 2014 - Function support for default values in report selection sets, including multi value list selections.
* March 2014 - Report scheduler with ability to run with selection sets.
* March 2014 - Report email / message scheduler module added.
* March 2014 - Selection sets creation and recalling.
* March 2014 - Support for selections with multi value lists, implemented as many2manytags widget.
* December 2013 - Testing and release for Pentaho version 5.0 support.
* December 2013 - Support reserved variable passing to parameter queries.
* November 2013 - Pentaho Report Actions better integrated to standard OpenERP Report Actions.
* January 2013 - Upgrade to support OpenERP version 7.0.

# Pentaho Reports for Odoo

This project provides a system that integrates Odoo with the Pentaho reporting system. End users of Odoo can design reports using Pentaho report designer v5.0 through v5.4, and install/access them from inside the Odoo interface.

This project carries on from the previoust Pentaho Reports for OpenERP.

### Features:
* Support for Odoo 9.0.
* Support for OpenERP 6.1, 7.0, and 8.0.
* Odoo data can be accessed via SQL, objects, custom python procedures, or via "Pentaho Data Integration" tools.
* Report parameters specified in the designer are automatically prompted for when the report is run from Odoo.
* Parameters can be generated dynamically from various data sources. 

_Pentaho Report designer_ is the software separate from this project that is used to design the report templates. You can download the designer [here](http://sourceforge.net/projects/pentaho/files/Report%20Designer/ "Pentaho Report Designer").

We have prepared a number of instructional videos for using this project with Odoo [here](https://www.youtube.com/user/WillowITMedia "Willow on Youtube"). The videos provide instructions for creating reports based upon SQL and the Odoo object data sources, and explain how to install them in Odoo.  Videos were made with a much older version of OpenERP, but are still relevant and useful.

Keep in mind that while these videos are for version 3.9 of the designer, they are still applicable. 

## A Note About Versions

At the time of writing, this project was working with version 3.9.1 of the Pentaho report designer, however this version is no longer supported and may stop working at any time. If you wish to use version 3.9.1, please follow the [instructions](http://pvandermpentaho.blogspot.com.au/2012/05/adding-openerp-datasource-to-pentaho.html "Pentaho 3.9.1 Plugin") to install the required plugin for the report designer. 

Version 5.4 is the latest version of the report designer.  It comes bundled with all required plugins (data sources etc) and needs no special additional installations.

## Overview

This project encompasses two separate components:

### The Java Component

This is a Java web application that can be deployed in a suitable container such as [Apache Tomcat](http://tomcat.apache.org/ "Apache Tomcat"). This component does the actual rendering of the reports based upon the definitions created in the [Pentaho Report Designer](http://sourceforge.net/projects/pentaho/files/Report%20Designer/ "Pentaho Report Designer"), which is separate from this project. The Java Server communicates with Odoo to retrieve the required data, and works with the Odoo module (described below) to prompt the user for any required parameters, and provide selections for these parameters.

### The Odoo Module

The other component in this project is the Odoo Module. This module allows Odoo to communicate with the Java Server to render reports created with the Report Designer. For a more detailed explanation, look at the description of the module in Odoo, or [here](https://github.com/WilldooIT/Pentaho-Odoo/blob/version10/odoo_addon/pentaho_reports/__manifest__.py "__manifest__.py in Pentaho Odoo Module"). 

## Quick Start

Reports can be designed and created using the [Pentaho Report Designer](http://sourceforge.net/projects/pentaho/files/Report%20Designer/ "Pentaho Report Designer") (which is software that is separate from this project).

The report server needs to be installed and running. The quickest and easiest way is to download and use a pre-built .war file from [here](http://cloud1.willowit.com.au/dist/pentaho-reports-for-openerp.war "Pentaho Report Server Packaged for Download"). This file will be rebuilt and updated on a semi-regular basis, but if the absolute latest version is required, you will have to build it yourself following the instructions [below.](#building-and-installing) 

The report server needs an application container such as [Apache Tomcat](http://tomcat.apache.org/ "Apache Tomcat") for it to run in. Installation and deployment on Tomcat or any other application container is beyond the scope of this document, however the Tomcat website has very detailed documentation on how to do so. 

The Odoo module needs to be installed and configured, as explained [here](#the-odoo-module-1). 

Finally, you will need to deploy your reports. Instructions for doing this can be found in the [module description](https://github.com/WilldooIT/Pentaho-Odoo/blob/version10/odoo_addon/pentaho_reports/__manifest__.py "__manifest__.py in Pentaho Odoo Module") under the "Report Actions" heading. 



## Building and Installing

### The Java Server
To build the Java server component, a suitable [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html "Java Development Kit 7") needs to be installed. 

Also needed are [Apache Ant](http://ant.apache.org/ "Apache Ant") and [Apache Ivy](http://ant.apache.org/ivy/ "Apache Ivy"). Ant is the build system, and Ivy downloads all of the dependencies required.

To build the project execute:

	$ cd <extracted_path>/java_server
	$ ant

Two files will be generated in the 'dist' directory. The first file, 'pentaho-reports-for-openerp.war', can be deployed using a servlet engine such as Tomcat. The second file, 'pentaho-reports-for-openerp.jar', is used when running the built in standalone test server.  

The standalone test server running on port 8090 can be started after a successful build using the following command:

	$ ant launch

For production deployment, however, it is recommended that the server be hosted in an application container. Instructions on how to deploy the war file on Tomcat can be found [here](http://tomcat.apache.org/tomcat-6.0-doc/deployer-howto.html#Deploying_using_the_Tomcat_Manager "Deploying Using Tomcat").

### The Odoo Module

This module is installed like any other Odoo module. Briefly:

* Place the 'odoo_addon' folder on the filesystem, somewhere that is accessible to the Odoo server.
* Update 'odoo-server.conf' file, and include the full path to this folder on the 'addons_path' line.
* Restart Odoo and log in as a user with administrator rights.
* Go to Settings -> Update Modules List and click Update.
* Go to Settings -> Installed Modules, remove the 'Installed Modules' filter and search for 'pentaho'.
* Install the 'Pentaho Reports for Odoo' module.

After installation, the module still needs to be configured. Refer to the [module description](https://github.com/WilldooIT/Pentaho-Odoo/blob/version10/odoo_addon/pentaho_reports/__manifest__.py "__manifest__.py in Pentaho Odoo Module") for detailed instructions on how to do this. 


## Appendices

### Integrating and Defining Reports to Odoo

The description of the Odoo module contains an overview of creating report actions, as well as defining and using report parameters.

### Concurrency Issue When Using Email Template

When generating a Pentaho report at the same time as parsing the email template, Odoo might raise the following exception:

    TransactionRollbackError: could not serialize access due to concurrent update

The Odoo module 'willow_pentaho_email_patch' works around this issue. However, it is not a perfect solution to the problem and we are open to suggestions and pull requests.

### Contributors

This project was developed by Willdoo IT (formerly Willow IT), using the libraries and extensions developed by De Bortoli Wines, Australia (Pieter van der Merwe in particular) for the Pentaho reporting system. The Odoo addon also derives from and/or is inspired by the Jasper Reports addon developed by NaN-tic.

Willdoo IT contributions:

* Richard deMeester - frontend and core functions, automated wizard and action implementation, documentation and videos, ongoing maintenance and development
* Deepak Seshadri - OpenERP-Pentaho server connector (Java)
* Douglas Parker - additional integration
* Jon Wilson - inspiration, testing, and whipping
* Thomas Cook - documentation


## Disclaimer

This project has been developed over time to meet specific requirements as we have needed to meet them. If something is wrong, or you have suggestions, please contribute via the git issues tab, or let us know at:

	richardd@willdooit.com



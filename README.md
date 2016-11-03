# Tableau Server Client (Python)

Use the Tableau Server Client (TSC) library to increase your productivity as you interact with the Tableau Server REST API. With the TSC library you can do almost everything that you can do with the REST API, including:

* Publish workbooks and data sources.
* Create users and groups.
* Query projects, sites, and more.

This repository contains Python source code and sample files.

For more information on installing and using TSC, see the documentation:

#### Installing From Source

Download the `.zip` file. Unzip the file and then run the following command:

```text
pip install -e <directory containing setup.py>
```

#### Installing the Development Version from Git

*Only do this if you know you want the development version, no guarantee that we won't break APIs during development*

```text
pip install git+https://github.com/tableau/server-client-python.git@development
```

If you go this route, but want to switch back to the non-development version, you need to run the following command before installing the stable version:

```text
pip uninstall tableauserverclient
```

###Basics
The following example shows the basic syntax for using the Server Client to query a list of all workbooks and the associated pagination information on the default site:

```python
import tableauserverclient

tableau_auth = tableauserverclient.TableauAuth('USERNAME', 'PASSWORD')
server = tableauserverclient.Server('SERVER')

with server.auth.sign_in(tableau_auth):
    all_workbooks, pagination_item = server.workbooks.get()
```

###Server Client Samples
* Can be run using the command prompt or terminal

Demo | Source Code | Description
-------- |  -------- |  --------
Publish Workbook | [publish_workbook.py](./samples/publish_workbook.py) | Shows how to upload a Tableau workbook.
Move Workbook | [move_workbook_projects.py](./samples/move_workbook_projects.py)<br />[move_workbook_sites.py](./samples/move_workbook_sites.py) | Shows how to move a workbook from one project/site to another. Moving across different sites require downloading the workbook. 2 methods of downloading are demonstrated in the sites sample.<br /><br />Moving to another project uses an API call to update workbook.<br />Moving to another site uses in-memory download method.
Set HTTP Options | [set_http_options.py](./samples/set_http_options.py) | Sets HTTP options on server and downloads workbooks.
Explore Datasource | [explore_datasource.py](./samples/explore_datasource.py) | Demonstrates working with Tableau Datasource. Queries all datasources, picks one and populates its connections, then updates the datasource. Has additional flags for publish and download.
Explore Workbook | [explore_workbook.py](./samples/explore_workbook.py) | Demonstrates working with Tableau Workbook. Queries all workbooks, picks one and populates its connections/views, then updates the workbook. Has additional flags for publish, download, and getting the preview image. Note: if you don't have permissions on the workbook the script retrieves from the server, the script will result in a 403033 error. This is expected.
Initialize Server | [initialize_server.py](./samples/initialize_server.py) | Shows how to intialize a Tableau Server with datasources and workbooks from the local file system.



###Adding New Features

1. Create an endpoint class for the new feature, following the structure of the other endpoints. Each endpoint usually has get, post, update, and delete operations that require making the url, creating the xml request if necesssary, sending the request and creating the target item object based on the server response.
2. Create an item class for the new feature, following the structure of the other item classes. Each item has properties that correspond to what attributes are sent to/received from the server (refer to docs amd Postman for attributes). Some items also require constants for user input that are limited to specific strings. After making all the properties, make the parsing method that takes the server response and creates an instances of the target item. If the corresponding endpoint class has an update function, then parsing is broken into multiple parts (refer to another item like workbook or datasource for example).
3. Add testing by getting real xml responses from the server, and asserting that all properties are parsed and set correctly.
4. Add samples to show users how to use the new feature.

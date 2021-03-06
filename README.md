# Introduction

eNMS is a network visualization, inventory and automation web platform.

**You can find a demo of eNMS _[here](http://afourmy.pythonanywhere.com/)_ !**

![eNMS](https://github.com/afourmy/eNMS/blob/master/readme/eNMS.png)

# Features

## Object creation

Nodes and links can be created in two ways: 
- one by one by specifying all properties manually, in the _Object creation_ page.
- by importing an Excel file with one sheet per type of object.
Examples of such Excel files are available in the **_/project folder_**.
Once your objects have been created, you can go to the _Overview_ page.
All objects are displayed in a sortable and searchable table.

![Object creation](https://github.com/afourmy/eNMS/blob/master/readme/object_creation.gif)

## Dashboard

The dashboard displays pie charts for any property. You can select, for both nodes and links,
which properties are displayed as pie charts in the dashboard.

![Dashboard](https://github.com/afourmy/eNMS/blob/master/readme/dashboard.gif)

## Network visualization

Network visualization is of paramount importance for quickly understanding the network topology.
There are two ways of visualizing the network in eNMS:
- **Geographical view**: eNMS uses Open Street Map to draw all network devices at their exact location GPS location.
- **Logical view**: the geographical view only makes sense if we have all GPS coordinates: it is not always the case. The logical view uses a graph-drawing algorithm to display the network in an aesthetically pleasing way.

![Network GIS visualization](https://github.com/afourmy/eNMS/blob/master/readme/views.gif)

## Export to Google Earth

Networks can be exported on Google Earth from the geographical view: the resulting file is stored in the **_/kmz_** folder.

![Export to Google Earth](https://github.com/afourmy/eNMS/blob/master/readme/google_earth.gif)

## Embedded SSH client

eNMS uses PuTTY to automatically establish an SSH connection to any SSH-enabled device from the web interface.
Your credentials are automatically provided to PuTTY for faster login.

![SSH connection](https://github.com/afourmy/eNMS/blob/master/readme/ssh_connection.gif)

## Network automation

There are four types of task in eNMS:
- **Netmiko _configuration_ task**: list of commands to configure the device (plain text or Jinja2 template).
- **Netmiko _show commands_ task**: list of “show commands” which output will be displayed in the task logs.
- **NAPALM _configuration_ task**: partial or full configuration (plain text or Jinja2 template).
- **NAPALM _getters_**: list of getters which output will be displayed in the task logs.

For each task, you can select a list of target devices. All tasks are _multithreaded_.

eNMS also provides some scheduling functions:
- **Start date**: instead of running the task immediately, the task will start at a specific time.
- **Frequency**: the task will be run periodically. This is especially useful for tasks that pull some information from the device, i.e netmiko **_show commands_** / **_NAPALM getters_** tasks.

### Simple configuration script with Netmiko

- Create a script in the _Script creation_ page.
- Set the script parameters (netmiko driver, global delay factor, target devices).

![Simple script with netmiko](https://github.com/afourmy/eNMS/blob/master/readme/netmiko_simple.gif)

### Template-based configuration

For complex script, it is best to use Jinja2 templating language:
- Write a Jinja2 template in the _Script creation_ page.
- Import a YAML file that contains all associated variables.
eNMS will take care of converting the template to a real text-based script.

![Send jinja2 script via SSH with netmiko](https://github.com/afourmy/eNMS/blob/master/readme/netmiko_j2.gif)

### NAPALM configuration

NAPALM is an automation framework that provides a set of functions to interact with different network device Operating Systems using a unified API.
NAPALM can be used to change the configuration (merge or replace), either via a plain text script or a Jinja2-enabled template.

![Use NAPALM to configure static routes](https://github.com/afourmy/eNMS/blob/master/readme/napalm_config.gif)

### Netmiko _show commands_ periodic retrieval

You can schedule a task to retrieve the output of a list of commands (show, ping, traceroute, etc) periodically. The result is stored in the database and displayed in the logs of the task, in the _Task management_ page.

![Netmiko show](https://github.com/afourmy/eNMS/blob/master/readme/netmiko_show.gif)

### NAPALM _getters_ periodic retrieval

You can also schedule a task to retrieve a NAPALM getter periodically.

![Configuration automation with NAPALM and Jinja2 scripting](https://github.com/afourmy/eNMS/blob/master/readme/napalm_getters.gif)

### Comparison

For all periodic tasks, you can compare the results between any two devices, at two different times.

The comparison result is displayed with two methods:
- A **_unified diff_**: show just the lines that have changed plus a few lines of context, in an inline style. (like Git)
- A **_ndiff_**: list every line and highlights interline changes.

![Comparison](https://github.com/afourmy/eNMS/blob/master/readme/comparison.gif)

## Display control

The user can filter the objects available in the GUI by searching for specific value for each property. 
For each property, the select can choose to use a regular expression instead of a hardcoded value:
regexes allows for specific search like a location or an IP subnet.

In the following example, we use the regexes [france|spain] for location to filter all objects that are not in France or in Spain, as well as the regex [Router|Switch] for type to filter all nodes that are neither a router, nor a switch.

![Object filtering](https://github.com/afourmy/eNMS/blob/master/readme/object_filtering.gif)

Note that filters apply to everything in eNMS that uses objects: dashboard, object deletion,
geographical and logical views, task scheduling, etc. You can use them to visualize or send to script
to a specific subset of devices.

## Add a new property

- Open /eNMS/source/objects/models.py and add a Column to the appropriate model (Object, Node or Link)
Example: description = Column(String) in the Object class.
- Open /eNMS/source/objects/properties.py and add the property in the appropriate tuple.
- Open /eNMS/source/base/properties.py and add the property and it's user-friendly name in the "pretty_names" dictionnary.
- (Optional) If you want the new property to be displayed as a diagram in the dashboard, open /eNMS/source/objects/properties.py and add the property in the appropriate diagram tuple.
- Delete the database.db file, and restart the application.

# Getting started

The following modules are used in eNMS:
```
flask (web framework)
flask_wtf (forms)
flask_sqlalchemy (database)
flask_login (login system)
tacacs_plus (used for enabling TACACS+ authentication)
xlrd (used for creating objects from an Excel file)
netmiko, NAPALM (used for network automation)
jinja2, pyyaml (used for sending complex template-based scripts)
simplekml (used for exporting project to Google Earth)
```

In order to use eNMS, you need to:
    
- clone eNMS (or download as a zip archive from github)
```
pip clone https://github.com/afourmy/eNMS.git
```
    
- install the requirements
```
cd eNMS
pip install -r requirements.txt
```

- run **/source/flask_app.py**.
```
cd source
python flask_app.py
```

# Contact

You can contact me at my personal email address:
```
''.join(map(chr, (97, 110, 116, 111, 105, 110, 101, 46, 102, 111, 
117, 114, 109, 121, 64, 103, 109, 97, 105, 108, 46, 99, 111, 109)))
```

or on the [Network to Code slack](http://networktocode.herokuapp.com "Network to Code slack"). (@minto, channel #enms)

# Credits

[Bootstrap](https://getbootstrap.com/ "Bootstrap"): Front-end HTML/CSS framework.

[Bootstrap Date/Time Picker](https://github.com/Eonasdan/bootstrap-datetimepicker "Bootstrap Date/Time Picker"): Date/time picker widget based on twitter bootstrap.

[Datatables](https://datatables.net/ "Datatables"): Advanced interaction controls to any HTML table with jQuery.

[eCharts](https://github.com/ecomfe/echarts "eCharts"): Interactive charting and visualization javascript library.

[Flask](http://flask.pocoo.org/ "Flask"): A microframework based on the Werkzeug toolkit and Jinja2 template engine.

[Flask WTForms](https://github.com/lepture/flask-wtf "Flask WTForms"): Simple integration of Flask and WTForms, including CSRF, file upload, and reCAPTCHA.

[Flask SQLAlchemy](http://flask-sqlalchemy.pocoo.org/ "Flask SQLAlchemy"): Adds support for SQLAlchemy to Flask.

[Flask Login](https://flask-login.readthedocs.io/en/latest/ "Flask Login"): Provides user session management for Flask.

[Font awesome](http://fontawesome.io/ "Font awesome"): Font and CSS toolkit.

[FullCalendar](https://fullcalendar.io/ "FullCalendar"): JavaScript drag-n-drop event calendar.

[Jinja2](https://github.com/pallets/jinja "Jinja2"): A modern and designer-friendly templating language for Python.

[Jquery](https://jquery.com/ "Jquery"): JavaScript library designed to simplify the client-side scripting of HTML.

[Leaflet](http://leafletjs.com/ "Leaflet"): JavaScript library for mobile-friendly interactive maps.

[Moment](https://momentjs.com/ "Moment"): JavaScript library to Parse, validate, manipulate, and display dates and times.

[NAPALM](https://github.com/napalm-automation/napalm "NAPALM"): A library that implements a set of functions to interact with different network device Operating Systems using a unified API.

[Netmiko](https://github.com/ktbyers/netmiko "Netmiko"): A multi-vendor library to simplify Paramiko SSH connections to network devices.

[Nprogress](https://github.com/rstacruz/nprogress "Nprogress"): Slim progress bars in JavaScript.

[OpenStreetMap](https://www.openstreetmap.org/ "OpenStreetMap"): Collaborative project to create a free editable map of the world.

[Parsley](http://parsleyjs.org/ "Parsley"): JavaScript form validation library.

[pyYAML](https://github.com/yaml/pyyaml "pyYAML"): YAML parser and emitter for Python.

[simplekml](http://simplekml.readthedocs.io/en/latest/ "SimpleKML"): Library to generate KML files (Google Earth).

[TACACS+](https://github.com/ansible/tacacs_plus/ "TACACS+"): A TACACS+ client that supports authentication, authorization and accounting.

[Vis](http://visjs.org/ "Vis"): JavaScript visualization library to display dynamic, automatically organised network views.

[xlrd](https://github.com/python-excel/xlrd): Library to extract data from Microsoft Excel (tm) spreadsheet files.

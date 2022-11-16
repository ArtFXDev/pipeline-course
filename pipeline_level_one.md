
   
# "Pipeline by levels"  


## Level 0 - A filesystem with a naming convention  

A minimal pipeline consists of files, following a naming convention, organized in a folder structure.  

It should comprise Asset and Shot hierarchies, including Tasks, and Work and Publish Scene Versions.  
For example:  
- Asset type / Asset name / Task / Version / State (work or publish) / Scene File 
- Sequence / Shot / Task / Version / State (work or publish) / Scene File 


## Level 1 - UI tool to browse and manipulate Entities, from the file system, inside the DCCs.  

We call "Entity" the data that we handle (Assets, Shots, Tasks, Versions, Scene files).  
A UI tool browses existing Entities and implements actions on them.  

For example:  
- list Assets, Shots, Tasks, Versions, Scenes
- open, save, increment a Scene
- publish a Version
- build the first Version of a Task

The Tool is usable from within the different DCCs.

### UI

- We use [Qt.py](https://pypi.org/project/Qt.py) 
- on top of [PySide2](https://pypi.org/project/PySide2)

### Engine / DCC

We implement "Engine" classes, one for each DCC.  
A [factory](https://www.tutorialspoint.com/design_pattern/factory_pattern.htm) function returns the appropriate Engine, depending on the context.  

Each "Engine" class implements actions, that are discoverable by the UI.  


### File system access

We implement a file system access layer, to find the files and resolve to pipeline entities.

- first we use [pathlib](https://docs.python.org/3/library/pathlib.html) 
- with glob ([Path.glob](https://docs.python.org/3/library/pathlib.html#pathlib.Path.glob) or [glob.glob](https://docs.python.org/3/library/glob.html))
- then we use the [Lucidity](https://gitlab.com/4degrees/lucidity) resolver

**Resolver**

A resolver is used to translate a path into a Pipeline Entity, and to format a path from an Entity.

Examples:
- [Shotgrid Toolkit Templates](https://github.com/shotgunsoftware/tk-config-default/blob/master/core/templates.yml)
- [CGWire's kitsu File tree](https://zou.cg-wire.com/file_trees)
- [Lucidity](https://gitlab.com/4degrees/lucidity)


### Environment Handling  

We use [rez](https://github.com/AcademySoftwareFoundation/rez) as the software package and environment manager.

We create rez packages:
- pipeline tool package (could be split into pipeline Core and pipeline UI)
- DCCs package (one per DCC)
- DCC Engine package (one per DCC)

#
## Level 2 - Using a backend to manage Entities  

Besides the File System, we create an API to connect to a Backend, and handle Entities from there.

Possible backends: 
- [Shotgrid](https://www.shotgridsoftware.com) - Market leading CG project manager, for medium to big sized studios
- [CGwire Kitsu](https://www.cg-wire.com) - Open source CG project manager, for small to medium sized studios
- Custom relational database - See [Relational database with SQLite](rdb_sql.md) 

We start with [Shotgrid](https://developer.shotgridsoftware.com/python-api).  
See Admin [getting started](https://help.autodesk.com/view/SGSUB/ENU/?guid=SG_Administrator_ar_get_started_html) and [Schema](https://help.autodesk.com/view/SGSUB/ENU/?guid=SG_Administrator_ar_get_started_ar_shotgun_schema_html)  
*(Tip: use `sg.schema_entity_read()` to get the data schema)*  

### Common API

We create APIs to encapsulate (abstract) the backend(s) and file system, to use them as data sources for Entities.  
On top of these, a common API offers a unified access to the Entities. 
Depending on the type of the Entities, we connect to a different data source.


#
## Level 3 - A flexible Action framework









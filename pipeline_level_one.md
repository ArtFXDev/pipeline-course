
##   
## "Level One Pipeline"

A "level zero" pipeline is just a filesystem with a naming convention.  
A "level one" pipeline adds a UI tool to browse and manipulate Entities, from a file system.   

### UI
- We use [Qt.py](https://pypi.org/project/Qt.py) 
- on top of [PySide2](https://pypi.org/project/PySide2)

### Engine / DCC
We implement "Engine" classes, one for each DCC.  
A factory function returns the appropriate Engine, depending on the context.  

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
 
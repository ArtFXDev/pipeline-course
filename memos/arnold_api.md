# Memo Arnold API

## Useful links

- https://help.autodesk.com/view/ARNOL/ENU/?guid=arnold_dev_guide_home_html
- https://help.autodesk.com/view/ARNOL/ENU/?guid=Arnold_API_REF_arnold_ref_index_html
- https://arnoldsupport.com/2014/12/11/arnold-python-changing-arnold-options-in-python/
- https://arnoldsupport.com/tag/python/
- https://arnoldsupport.com/category/scripting

## Python environment

Using rez and pycharm:  
`rez env arnold pycharm -- pycharm`

Test:  
` 
rez env arnold -- python -c "import arnold;print(arnold)" 
`

Pythonpath needs to know about the API, for example from HTOA or MTOA
- **HTOA**\htoa-XXX\scripts\python
- **MTOA**\mayaXXX\scripts
  

## Usage examples

### Changing OCIO setting in a series of ass files

```python
from pathlib import Path

from arnold import *
import os

path = Path("//path/testpipe/shots/s01/p010/lighting_main/publish/v000/ass/main/renderSetupLayer1__test")

AiBegin()

AiMsgSetConsoleFlags(AI_LOG_ALL)
AiLoadPlugins(os.getenv('ARNOLD_PLUGIN_PATH'))
# universe = AiUniverse()

# getting ass options
options = AiUniverseGetOptions()

for ass_file in path.glob("*.ass"):

    print(f"Handling {ass_file.name}")

    # converting pathlib.Path to string
    ass_file = str(ass_file)

    # loads Ass
    AiASSLoad(ass_file, AI_NODE_ALL)

    # get options
    color_manager_node = AiNodeGetPtr(options, "color_manager")
    print(f"color_manager node name: {AiNodeGetName(color_manager_node)}")

    # this is to get the options explicitly by node name
    # color_manager_node = AiNodeLookUpByName("defaultColorMgtGlobals")

    # updating the node
    AiNodeSetStr(color_manager_node, "config", "//path/aces/1.2/config.ocio")

    # write updated ass file
    AiASSWrite(ass_file, AI_NODE_ALL)

AiEnd()
```

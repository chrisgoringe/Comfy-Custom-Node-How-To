# How to deploy your custom node

Custom nodes are loaded by ComfyUI based on the `NODE_CLASS_MAPPINGS` attribute, which is a dictionary with a globaly unique node name as the key, and the custom node type as the value. Optionally, you can also provide a `NODE_DISPLAY_NAME_MAPPINGS` which maps the unique node name to a display name.

If you have something really simple, you can just put the python file in the custom_nodes directory, and add

```python
NODE_CLASS_MAPPINGS = { "my unique name" : SimpleCustomNode }
NODE_DISPLAY_NAME_MAPPINGS = { "my unique name" : "Image inverter" }
```

But it's better practice for custom nodes to be in their own subdirectories (and if you're going to deploy them through git that's how they'll end up), in which case you need to add an `__init__.py` file, which will look like this:

```python
from .simple_source_file import SIMPLE_CUSTOM_NODE
NODE_CLASS_MAPPINGS = { "my unique name" : SimpleCustomNode }
NODE_DISPLAY_NAME_MAPPINGS = { "my unique name" : "Image inverter" }
__all__ = ['NODE_CLASS_MAPPINGS', 'NODE_DISPLAY_NAME_MAPPINGS']
```

# Hidden Inputs

The dictionary returned by `INPUT_TYPES` has an optional key `'hidden'` which allows the node to receive information that doesn't come from the inputs to the node. There are three bits of information that can be accessed in this way, which are identified by the strings `PROMPT`, `EXTRA_PNGINFO` and `UNIQUE_ID`. To see all of them:

```python
    @classmethod
    def INPUT_TYPES(cls):
        return {
            "required" : {},
            "hidden" : { 
                "prompt": "PROMPT",
                "extra_info": "EXTRA_PNGINFO",
                "id": "UNIQUE_ID",
            }
        }

    def my_function(self, prompt:dict, extra_info:dict, id:int):
        ...code...

```

Notice that the values for `hidden` are just `str`, not `tuple(str,dict)` as used for `required` and `optional`.

`EXTRA_PNGINFO` is the metadata dictionary that will be saved with the image. You could choose to add custom metadata to it.

`PROMPT` is the prompt (the job request sent by the front end). It's complicated, but you can use this to access (or even change) widget values or previous calculated outputs. You'll need to understand the order of execution to make that work. Put a breakpoint in the method and use your debugger to look at the dictionary.

`UNIQUE_ID` the id of the node (determined by the UI and used in the prompt)
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

---

Some more details provided by Lerc:

Hidden Inputs are inputs that do not need to be explicitly provided, They behave as inputs for the purposes of the execution function declared in the FUNCTION field of a node class.

The values for the hidden inputs are automatically filled out.

https://github.com/comfyanonymous/ComfyUI/blob/77c124c5a17534e347bdebbc1ace807d61416147/execution.py#L32
```python
        h = valid_inputs["hidden"]
        for x in h:
            if h[x] == "PROMPT":
                input_data_all[x] = [prompt]
            if h[x] == "EXTRA_PNGINFO":
                if "extra_pnginfo" in extra_data:
                    input_data_all[x] = [extra_data['extra_pnginfo']]
            if h[x] == "UNIQUE_ID":
                input_data_all[x] = [unique_id]
```

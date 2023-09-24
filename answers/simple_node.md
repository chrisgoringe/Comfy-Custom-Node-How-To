# A really simple custom node explained

Here's a custom node which has one (image) input and one (image) output, and simply inverts the image.

```python
class SimpleCustomNode:
    @classmethod
    def INPUT_TYPES(cls):
        return {
            "required": { "image_in" : ("IMAGE", {}) },
        }

    RETURN_TYPES = ("IMAGE",)
    RETURN_NAMES = ("image_out",)
    FUNCTION = "invert"
    CATEGORY = "examples"

    def invert(self, image_in):
        image_out = 1 - image_in
        return (image_out,)
```

The code defines the inputs (`INPUT_TYPES`), the outputs (`RETURN_TYPES` and `RETURN_NAMES`), the actual function (`FUNCTION` and `invert`), and where to find it in the add nodes menu (`CATEGORY`).

`CATEGORY` is pretty obvious, right?

`FUNCTION` is the name of the function that the node will call when it is executed (in this case, `invert`). 

`RETURN_TYPES` is a tuple of strings that specify the [data type](./data_types.md) of the outputs. Notice that trailing comma in `("IMAGE",)`? That's because python would interpret `("IMAGE")` as a string, and then when that gets treated as an interator, it would give `"I"`, then `"M"`, then...

`RETURN_NAMES` is a tuple of string that specify the name of the outputs. It's optional - if you don't provide `RETURN_NAMES`, `RETURN_TYPES` is used.

`INPUT_TYPES` specifies the inputs, and this is the only complicated bit. Notice that it's a `@classmethod`. The method returns a dictionary which must contain the key `required`, and may also include `optional` and `hidden`. Each of these keys has, as it's value, another dictionary. This inner dictionary has one key-value pair for each input - the key are the names of the input nodes, and the values are a tuple (str, dict), where the string is the type of the input node, and the dict provides additional parameters (things like default values, min and max, etc..)

So in the example we specify one required input, named `image_in`, which is an IMAGE (for which there are no additional parameters).

Then there's the function itself: `invert` (as named in the `FUNCTION` class attribute). It will be passed keyword arguments with names that match the input names specified in `INPUT_TYPES` - so it's `invert(self, image_in)` because `image_in` was a key in the dictionary INPUT_TYPES returned. Required inputs will always be provided, optional ones will be provided if the input had something attached to it, hidden inputs will be provided if they are available.

The function returns a tuple matching (in order) the `RETURN_TYPES`. Again, notice the trailing comma!

That's it. Now just [deploy it](./deploying%20a%20custom%20node.md).
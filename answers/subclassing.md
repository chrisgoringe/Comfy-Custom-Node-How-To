# Subclassing Custom Nodes

You can subclass nodes. The simplest example of this is the Preview Node is a subclass of the Save image node.

class PreviewImage(SaveImage):
    def __init__(self):
        self.output_dir = folder_paths.get_temp_directory()
        self.type = "temp"
        self.prefix_append = "_temp_" + ''.join(random.choice("abcdefghijklmnopqrstupvxyz") for x in range(5))

    @classmethod
    def INPUT_TYPES(s):
        return {"required":
                    {"images": ("IMAGE", ), },
                "hidden": {"prompt": "PROMPT", "extra_pnginfo": "EXTRA_PNGINFO"},
                } 
The INPUT_TYPES is overridden so that you no longer have a prefix edit widget, and the init sets up a temp file for the preview image to be saved as.

Subclassing custom nodes might depend on scope visibility. I can't see why it wouldn't work if the subclass were in the same file as the parent. There may be issues if the Parent node class is not visible from the context where you wish to make subclass. That should be easy to test for a given context using globals()["ParentClass"]
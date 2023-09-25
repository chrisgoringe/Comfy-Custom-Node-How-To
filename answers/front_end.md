# Sending data to the front end nodes

On the python side the execution function set in FUNCTION can return information for the user interface
return { "ui": {"name":value}}

Handling of the data returned by the execution function is at
https://github.com/comfyanonymous/ComfyUI/blob/77c124c5a17534e347bdebbc1ace807d61416147/execution.py#L78

On the JavaScript side nodes have an onExecuted event

```JavaScript
node.onExecuted = (output)=> {
    if (output?.name {
        console.log(output.name)
    }      
  }
```
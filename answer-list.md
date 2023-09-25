# The A of Q&A

If you've got a question not answered here, post it as an [issue](https://github.com/chrisgoringe/Comfy-Custom-Node-How-To/issues).

## Starting Points

- ComfyUI uses nodes to do all the work of stable diffusion. It comes with lots of them out of the box, but if you want to do something that can't be done with one that already exists, you can create new nodes.
- [ComfyUI-Manager][https://github.com/ltdrdata/ComfyUI-Manager] is your go-to for finding and installing custom nodes.

## Writing a simple custom node

In the ComfyUI repository, the folder `custom_nodes` you'll find `example_node.py.examples`. That's worth reading. 

- [A simple custom node explained](./answers/simple_node.md) 
- [How to get ComfyUI to load a custom node](./answers/deploying%20a%20custom%20node.md)

## Going further

`nodes.py` in the top level of the ComfyUI directory has the definitions of lots of the built in nodes. Look at the code for a node that does something a bit like you want.

Here are some guides on how to do things that have been contributed:
- [Hidden inputs](./answers/hidden_inputs.md)
- [Communicating with the javascript](./answers/front_end.md)
- [Subclassing custom nodes](./answers/subclassing.md)

## More referency stuff

**What are the different input and output types, as python objects?**

Stick a breakpoint in your debugger and take a look at them! But [here](./answers/data_types.md) are some details

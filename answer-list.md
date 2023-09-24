# The A of Q&A

If you've got a question not answered here, post it as an [issue](https://github.com/chrisgoringe/Comfy-Custom-Node-How-To/issues).

## Starting Points

**What is ComfyUI anyway?**

It's "a powerful and modular stable diffusion GUI and backend", at least according to it's [github page](https://github.com/comfyanonymous/ComfyUI)

**What's a custom node?**

ComfyUI uses nodes to do all the work of stable diffusion. It comes with lots of them out of the box, but if you want to do something that can't be done with one that already exists, you can create new nodes.

**Where can I find custom nodes?**

The (ComfyUI-Manager)[https://github.com/ltdrdata/ComfyUI-Manager] is a great place to start.

## Writing a simple custom node

**How do I get started writing custom nodes?**

In the ComfyUI repository, the folder `custom_nodes` you'll find `example_node.py.examples`. That's worth reading. And [here](./answers/simple_node.md) is a really simple custom node explained.

**How do I get ComfyUI to load my node?**

Here's how you [deploy a node](./answers/deploying%20a%20custom%20node.md)

## Going further

**Where's a good place to look for more information?**

`nodes.py` in the top level of the ComfyUI directory has the definitions of lots of the built in nodes. Look at the code for a node that does something a bit like you want.

**What are hidden inputs?**

[Hidden inputs](./answers/hidden_inputs.md)

## More referency stuff

**What are the different input and output types, as python objects?**

Stick a breakpoint in your debugger and take a look at them! But [here](./answers/data_types.md) are some details
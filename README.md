# Comfy Custom Node How-To

[In the wiki](https://github.com/chrisgoringe/Comfy-Custom-Node-How-To/wiki)

## Assuming Chris will move this to the wiki and then kill this PR?

# Passing Control to Javascript

This example strips down @chrisgoringe's [Image picker](https://github.com/chrisgoringe/cg-image-picker) node to illustrate how to pause the graph execution and wait for something to get processed on the Javascript end.

First, define your custom node in the normal way. In this example, the node will have one input and one output, both of type `INT`, and the front end will just double the input number and feed it to the output.

### Back-end: `custom_nodes/proxy.py`

```py
from server import PromptServer
from aiohttp import web
import time
```
We will use this stuff to handle the incoming replies.
```py
class ClientProxy:
  def __init__(self): pass

  @classmethod
  def INPUT_TYPES(s):
    return {
      "required": {},
      "optional": {
        "input": ("INT", {}),
      },
      "hidden": {
        "id": "UNIQUE_ID",
      }
    }

  CATEGORY = "proxies"
  FUNCTION = "run"
  RETURN_TYPES = ("INT",)
  RETURN_NAMES = ("out",)
```
This is all normal custom node stuff that creates our one-in-one-out node and puts in the 'proxies' category of Comfy's node menu. Don't forget the trailing comma in the `RETURN` attributes, or Python will collapse our 1-tuple into a single value.
```py
  def IS_CHANGED(id):
    return True
```
Defining this function gives you the chance to force the node to execute every time. By default, Comfy will skip your node unless the inputs have changed. By setting `True` here, the client code will always get called.
```py
  def run(self, id, input):
    me = prompt[id]
    PromptServer.instance.send_sync("proxy", {
      "id":    id,
      "input": input,
    })
    outputs = MessageHolder.waitForMessage(id)
    return (outputs['output'],)
```
In our node's main method, we are requesting `id` from the hidden inputs, which we will pack into the message to the front-end.

Same file, new class:
```py
# Message Handling

class MessageHolder:
  messages = {}

  @classmethod
  def addMessage(self, id, message):
    self.messages[str(id)] = message

  @classmethod
  def waitForMessage(self, id, period = 0.1):
    sid = str(id)

    while not (sid in self.messages):
      time.sleep(period)

    message = self.messages.pop(str(id),None)
    return message
```
`waitForMessage` will spin until the api receives a message with a matching id, and adds it via `addMessage`. 

Add a new route to Comfy's API:
```py
routes = PromptServer.instance.routes
@routes.post('/proxy_reply')
async def proxy_handle(request):
  post = await request.json()
  MessageHolder.addMessage(post["node_id"], post["outputs"])
  return web.json_response({"status": "ok"})
```
The request body can be whatever you need, in this example it looks like this:
```
{
  node_id
  outputs: {
    output
  }
}
```
We are extracting just the `outputs` chunk to pass to the `MessageHolder`.

Back in the `run` method, `waitForMessage` will give us the `outputs` chunk, and we can return the values as the output of the node, and execution will continue from there.

### Front End

In your front-end code, set up a connection to the API by using Comfy's API class (`import {api} from '../../scripts/api.js'`). You can now subscribe to the `proxy` event on the websockets stream:

```js
api.addEventListener('proxy', function proxyHandler (event) {
  const data = event.detail

  const reply = {
    node_id: data.id,
    outputs: {
      output: data.input * 2
    }
  }
```
Here we implement the doubling function for our example, build the reply object, and then send it to our custom API route:
```js
  api.fetchApi("/proxy_reply", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(reply),
  })
})
```

That should be all you need to process some data on the front-end as part of the workflow execution. To get more sophisticated, refer to the [Image picker](https://github.com/chrisgoringe/cg-image-picker) node, which implements a way of aborting the while loop in `waitForMessage` to prevent the server getting stuck, resetting the state when a new prompt starts, and various other things you will need to consider to create a seamless experience for your node users.


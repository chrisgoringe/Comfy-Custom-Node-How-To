# Comfy types and python objects

| Comfy type | Python | Notes
|-|-|-|
|CLIP||It's complicated|
|CONDITIONING||It's complicated|
|FLOAT|float|As an input you can specify "min", "max", "default", "step"|
|IMAGE|torch.Tensor with shape (B,H,W,C)| A batch of B images. C=3 (RGB) |
|INT|int|As an input you can specify "min", "max", "default", "step" |
|LATENT|dict|'samples' is the key for the samples as a torch.Tensor|
|MODEL||It's complicated|
|STRING|str|As an input type a "default" *must* be provided |
|VAE|comfy.sd.VAE|It's complicated|

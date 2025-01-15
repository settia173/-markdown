Fn + Alt + PrintScreen：截取光标所在的窗口

Python 3.11.4 (main, Jul  5 2023, 14:15:25) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.

> > > import torch
> > > 
> > > x = torch.rand(5, 3)
> > > print(x)
> > > tensor([[0.5421, 0.5950, 0.3337],
> > >         [0.8443, 0.2287, 0.5316],
> > >         [0.0301, 0.0151, 0.3522],
> > >         [0.3456, 0.5901, 0.5970],
> > >         [0.6271, 0.8065, 0.7645]])

> > > torch.cuda.is_available()
> > > True
> > > torch.cuda.current_device()
> > > 0
> > > torch.cuda.device(0)
> > > <torch.cuda.device object at 0x7f2bb0556700>
> > > torch.cuda.get_device_name(0)
> > > 'NVIDIA GeForce RTX 4070 Ti'

> > > print(torch.__version__)
> > > 1.8.2+cu111
> > > print(torch.version.cuda)
> > > 11.1
> > > ———————————————

# The diagram of fiber_insert_spin_unlocker

The inserted content will be different between platforms because of calling convention. The below is x86 example.
```
hi |             :             |
   |             :             |
   |             :             |
   |             :             |
   +---------------------------+
   |       return address      |
   +---------------------------+ 
   |                           |
   |      stored registers     |
   |                           |
   +---------------------------+
   |    fiber_resume address   |
   +---------------------------+ <---- fiber.stack_top
   |             :             |
   |             :             |
lo |             :             |


                      hi |             :             |
                         |             :             |
                         |             :             |
 insert_spin_unlocker    +---------------------------+
--------------------->   |       return address      |
                         +---------------------------+
                         |                           |
                         |      stored registers     |
                         |                           |
                         +---------------------------+
                         |    fiber_resume address   |
                         +---------------------------+
                         |       spin_lock_addr      |
                         +---------------------------+
                         |       clear_func *[1]     |
                         +---------------------------+
                         |        spin_unlock        |
                         +---------------------------+ <---- fiber.stack_top
                         |             :             |
                         |             :             |
                      lo |             :             |
                      
*[1] : substract stack register in order to
return to correct return address
```

# The diagram of fiber_insert_spin_unlocker

```
hi |             :             |
   |             :             |
   |             :             |
   |             :             |
   +---------------------------+
   |  fiber_switch_return_addr |
   +---------------------------+ 
   |                           |
   |      stored registers     |
   |                           |
   +---------------------------+ <---- fiber.stack_top
   |             :             |
   |             :             |
lo |             :             |


                      hi |             :             |
                         |             :             |
                         |             :             |
 insert_spin_unlocker    +---------------------------+
--------------------->   |  fiber_switch_return_addr |
                         +---------------------------+
                         |       spin_lock_addr      |
                         +---------------------------+
                         |       clear_func *[1]     |
                         +---------------------------+
                         |        spin_unlock        |
                         +---------------------------+
                         |                           |
                         |      stored registers     |
                         |                           |
                         +---------------------------+ <---- fiber.stack_top
                         |             :             |
                         |             :             |
                      lo |             :             |
                      
*[1] : substract stack register in order to
return to correct return address
```

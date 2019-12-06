# Initial Fiber Stack

The initial stack will be different between platforms because of calling convention. The below is x86 example.
```
hi +----------------------+
   | fiber scheduler addr |
   +----------------------+
   |      data pointer    |
   +----------------------+
   |      fiber_exit      |
   +----------------------+
   |     entry_function   |
   +----------------------+ <---- fiber.stack_top
   |          :           |
   |          :           |
lo |          :           |
```

# Initial Fiber Stack

```
hi +----------------------+
   | fiber scheduler addr |
   +----------------------+
   |      data pointer    |
   +----------------------+
   |      fiber_exit      |
   +----------------------+
   |     entry_function   |
   +----------------------+
   |                      |
   |    dummy registers   |
   |                      |
   +----------------------+ <---- fiber.stack_top
   |          :           |
   |          :           |
lo |          :           |
```

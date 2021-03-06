Goals
-----

- design an s-expression syntax to represent rv32i instructions

- implement an assembler for this syntax

My original goal was to do this entirely in r7rs. However this turns out to be
100% impossible without at least srfi-151. I feel disillusioned.

Usage
-----

Given ``fib.scm`` has content:

.. code::

   ((addi x1 x0 0)
    (addi x2 x0 1)
    (addi x31 x0 254)

    loop
    (addi x3 x2 0)
    (add x2 x2 x1)
    (addi x1 x3 0)
    (bge x31 x2 loop)

    forever
    (jal x0 forever))

.. code::

   ./asm fib.scm fib.bin

``fib.bin`` disassembles to:

.. code::

   0x0:   00000093          addi  x1,x0,0
   0x4:   00100113          addi  x2,x0,1
   0x8:   0fe00f93          addi  x31,x0,254
   0xc:   00010193          addi  x3,x2,0
   0x10:  00110133          add   x2,x2,x1
   0x14:  00018093          addi  x1,x3,0
   0x18:  fe2fdae3          bge   x31,x2,-12   # 0xc
   0x1c:  0000006f          jal   x0,0         # 0x1c

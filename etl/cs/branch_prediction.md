# 分类

- Static prediction

  Does not depend on the run-time behavior of the program. It is based on the instruction itself.
- Dynamic prediction

  Based on the previous run-time behavior of the program.

# Some sophisticated techniques

## Branch Target Buffer (BTB)

A cache that stores the target address of a branch instruction.

## Return Stack Buffer (RSB)

A cache that stores the return address of a call instruction. For better prediction on return instructions.

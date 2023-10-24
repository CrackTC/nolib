Basically, the `cmp` instruction is a subtraction that does not store the result, but only sets the flags. The flags are then used by the conditional jump instructions.

There are 6 flags the `cmp` instruction affects:

- `ZF` - Zero Flag
- `SF` - Sign Flag
- `OF` - Overflow Flag
- `CF` - Carry Flag
- `PF` - Parity Flag
- `AF` - Auxiliary Carry Flag

Here is the C equivalent of the `cmp` instruction:

```c
static uint64_t count_bits(uint64_t x) {
  uint64_t res = 0;
  while (x) {
    res += x & 1;
    x >>= 1;
  }
  return res;
}

static void cmpq(core_t *core, uint64_t *src, uint64_t *dst) {
  uint64_t res = *dst - *src;
  core->cflag.CF = *dst < *src;
  core->cflag.ZF = *dst == *src;
  core->cflag.SF = res >> 63;
  core->cflag.OF = (*dst >> 63) != (res >> 63);
  core->cflag.PF = ~(count_bits(res & 0xff) & 1);
  core->cflag.AF = (*dst & 0xf) < (*src & 0xf);
}
```

Note that AT&T syntax is `cmp src, dst`, `dst` is the operand on the left side of subtraction, and `src` is the operand on the right side of subtraction.

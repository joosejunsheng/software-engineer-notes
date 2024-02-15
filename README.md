# software-engineer-notes

## Bit Shifting
```mjs
x << y
```
`x << y` represents a bitwise left shifting operation, integer `x` to be shift to the left by `y` positons where `x` is a constant.

For example, 1 is represented as `0001` in binary, by applying bitwise left shifting operation by 2 positions to the binary `0001`, imagine the binary representation is zero-padded, it will become `0100`. By converting `0100` to hexadecimal, you will get an integer `4`.  We can conclude that the equation will be `2 ^ y` where `y = 2` in this case, `2 ^ 2 = 4`.

The full equation of `x << y` simply equals to `x * (2 ^ y)` where `x` is a constant.

```mjs
int x = 3;
int y = 5;
int res = x << y; // x * (2 ^ y) = 3 * (2 ^ 5) = 3 * 32 = 96
```

An example of bit shifting application is converting IP address into integer.

Merging IP octets into a single integer consumes less memory, more efficient when it comes to comparison operations. Integer representations of IP can be found internally in many network protocols for packet routing.

### Database
## ACID - Database Transaction Properties

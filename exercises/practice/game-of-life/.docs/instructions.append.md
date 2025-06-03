# Instructions append

Each row of the grid is represented as a word.

For example,

```
    0 1 0
    1 0 0
    1 1 0
```

is represented as

```
0x2
0x4
0x6
```

The grid has at most 32 columns.

## Registers

| Register | Usage        | Type    | Description                 |
| -------- | ------------ | ------- | --------------------------- |
| `$a0`    | input        | integer | number of rows              |
| `$a1`    | input        | integer | number of columns           |
| `$a2`    | input        | address | grid for current generation |
| `$a3`    | input/output | address | grid for next generation    |
| `$t0-9`  | temporary    | any     | used for temporary storage  |

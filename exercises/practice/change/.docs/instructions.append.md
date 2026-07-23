# Instructions append

## Registers

| Register | Usage        | Type    | Description                                 |
| -------- | ------------ | ------- | ------------------------------------------- |
| `$a0`    | input        | integer | number of input coins                       |
| `$a1`    | input        | integer | target                                      |
| `$a2`    | input/output | address | overwritable array of coin words            |
| `$v0`    | output       | integer | number of output coins, -1 if input invalid |
| `$t0-9`  | temporary    | any     | used for temporary storage                  |

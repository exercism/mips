# Instructions append

## Registers

| Register | Usage        | Type    | Description                                  |
| -------- | ------------ | ------- | -------------------------------------------- |
| `$a0`    | input        | integer | number of input digits                       |
| `$a1`    | input        | integer | base for input digits                        |
| `$a2`    | input        | integer | base for output digits                       |
| `$a3`    | input/output | address | overwritable array of digit words            |
| `$v0`    | output       | integer | number of output digits, -1 if input invalid |
| `$t0-9`  | temporary    | any     | used for temporary storage                   |

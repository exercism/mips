# Instructions append

## Minefield format

The minefield is represented as a null-terminated string, with a newline character at the end of each row.

An example would be `"   \n * \n   \n"`

## Registers

| Register | Usage        | Type    | Description                   |
| -------- | ------------ | ------- | ----------------------------- |
| `$a0`    | input        | address | null-terminated input string  |
| `$a1`    | input/output | address | null-terminated output string |
| `$t0-9`  | temporary    | any     | for temporary storage         |

# Instructions append

## Output format

Output the diamond as a null-terminated string, with a newline character at the end of each row.

An example output would be `"  A  \n B B \nC   C\n B B \n  A  \n"`

## Registers

| Register | Usage        | Type    | Description                   |
| -------- | ------------ | ------- | ----------------------------- |
| `$a0`    | input        | byte    | letter                        |
| `$a1`    | input/output | address | null-terminated result string |
| `$t0-9`  | temporary    | any     | for temporary storage         |

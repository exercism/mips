# Instructions append

## Output format

Output a null-terminated string, using a comma and a space as separators.

An example output would be `"491, 914, 142"`

If the input is invalid, output an empty string.

## Registers

| Register | Usage        | Type    | Description                    |
| -------- | ------------ | ------- | ------------------------------ |
| `$a0`    | input        | address | null-terminated series string  |
| `$a1`    | input        | integer | slice length                   |
| `$a2`    | input/output | address | null-terminated output string  |
| `$t0-9`  | temporary    | any     | used for temporary storage     |

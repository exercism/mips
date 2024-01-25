# Instructions append

## Output format

Output the sequence of actions as a null-terminated string, using a comma and a space as separators.

## Registers

| Register | Usage        | Type    | Description                   |
| -------- | ------------ | ------- | ----------------------------- |
| `$a0`    | input        | integer | number between 1 and 31       |
| `$a1`    | input/output | address | null-terminated output string |
| `$t0-9`  | temporary    | any     | used for temporary storage    |

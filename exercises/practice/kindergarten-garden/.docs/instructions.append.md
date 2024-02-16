# Instructions append

## Output format

Output the student's plants as a null-terminated string, using a comma and a space as separators.

## Registers

| Register | Usage        | Type    | Description                   |
| -------- | ------------ | ------- | ----------------------------- |
| `$a0`    | input        | address | garden diagram                |
| `$a1`    | input        | address | student's name                |
| `$a2`    | input/output | address | null-terminated output string |
| `$t0-9`  | temporary    | any     | used for temporary storage    |

# Instructions append

## Output format

Output the lyrics as a null-terminated string.

## Registers

| Register | Usage        | Type    | Description                   |
| -------- | ------------ | ------- | ----------------------------- |
| `$a0`    | input        | integer | start verse                   |
| `$a1`    | input        | integer | end verse                     |
| `$a2`    | input/output | address | null-terminated output string |
| `$t0-9`  | temporary    | any     | for temporary storage         |

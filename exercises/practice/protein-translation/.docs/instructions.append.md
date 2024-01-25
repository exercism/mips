# Instructions append

## Output format

Output the protein sequence as a null-terminated string, with a newline character after every protein.

An example output would be `"Methionine\nPhenylalanine\nSerine\n"`

If the input is invalid, output an empty string.

## Registers

| Register | Usage        | Type    | Description                   |
| -------- | ------------ | ------- | ----------------------------- |
| `$a0`    | input        | address | null-terminated input string  |
| `$a1`    | input/output | address | null-terminated output string |
| `$t0-9`  | temporary    | any     | used for temporary storage    |

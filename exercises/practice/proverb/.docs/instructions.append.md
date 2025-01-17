# Instructions append

## Input format

Each list of inputs is represented as a null-terminated string, with a newline character at the end of each input.

An example would be `"nail\nshoe\nhorse\nrider\nmessage\nbattle\nkingdom\n"`

## Registers

| Register | Usage        | Type    | Description                                                    |
| -------- | ------------ | ------- | -------------------------------------------------------------- |
| `$a0`    | input        | address | null-terminated input string with newline after each input     |
| `$a1`    | input/output | address | null-terminated output string                                  |
| `$t0-9`  | temporary    | any     | for temporary storage                                          |

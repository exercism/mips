# Instructions append

## Input/Output format

Each set of words is represented as a null-terminated string, with a newline character at the end of each word.

You must return the anagrams in the same order as they are listed in the candidate words.

An example would be `"tones\nnotes\nSeton\n"`

## Registers

| Register | Usage        | Type    | Description                                                    |
| -------- | ------------ | ------- | -------------------------------------------------------------- |
| `$a0`    | input        | address | null-terminated target string, without newline                 |
| `$a1`    | input        | address | null-terminated candidates string with newline after each word |
| `$a2`    | input/output | address | null-terminated output string with newline after each word     |
| `$t0-9`  | temporary    | any     | for temporary storage                                          |

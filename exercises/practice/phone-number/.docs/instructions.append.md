# Instructions append

Overwrite the supplied phone number string.
If the phone number is invalid, replace it with an empty string.

## Registers

| Register | Usage        | Type    | Description                            |
| -------- | ------------ | ------- | -------------------------------------- |
| `$a0`    | input/output | address | phone number as null-terminated string |
| `$t0-9`  | temporary    | any     | used for temporary storage             |

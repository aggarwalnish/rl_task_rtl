# Specifications: Variable Input & Output Rate FIFO Buffer

Create a module that implements a **Variable input and variable output rate First-in First-out buffer (FIFO)**. This module takes in up to `INPUT_SIZE` (parameter) data entries per cycle and outputs up to `OUTPUT_SIZE` (parameter) data entries per cycle. Each data is `BITWIDTH` bits wide (parameter).

The input is registered in a flop when the data is accompanied by a valid input signal (`req_in`) and the corresponding number of entries that need to be entered into the FIFO (`size_in`), where `0 < size_in <= INPUT_SIZE`. Similarly, the output interface will have an input signal `req_out` indicating a data request at the output of the FIFO, accompanied by a `req_size` signal indicating how many entries are requested at the output interface (`0 < req_size <= OUTPUT_SIZE`).

---

## Module Name
- **vivo_fifo** (to be added in the RTL directory)

## Parameters

1. **INPUT_SIZE**
    - Number of entries that can be added to the FIFO in one cycle.
    - **Default:** 32
    - **Related signals:** `data_in`

2. **OUTPUT_SIZE**
    - Number of entries that can be output from the FIFO in one cycle.
    - **Default:** 16
    - **Related signals:** `data_out`

3. **BITWIDTH**
    - Size of one data entry (in bits)
    - **Default:** 32
    - **Related signals:** `data_in`, `data_out`

## Latency
- Each input and output request is processed with a latency of **1 cycle**.
- After a `req_in` is asserted, the module should check if the buffer can accept `size_in` entries without overflowing, then raise the `accept_in` signal in the same cycle if possible.
    - The data is entered in the next cycle and available at the output starting the following cycle.
- Similarly, after a `req_out` is asserted, the module should check if the buffer has `size_out` number of valid entries. If it does, it raises the `valid_out` signal and exposes the corresponding output entries to `data_out[size_out-1:0]` ports in the next cycle.


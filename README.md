# Migrating from SW to Clinical grade HW: An Open MVDR Beamformer

The MVDR beamforming is the algorithm inside medical ultrasound machines that sharpens focus and suppresses noise from unwanted directions, In this case we use it for picking out human cyst phantoms.

<img width="894" height="555" alt="image" src="https://github.com/user-attachments/assets/5eeadb52-9f08-4a2b-8d8c-a5c8e1c97c33" />

Computing it in real time requires QR matrix decomposition on every time sample, expensive enough that it normally runs on server hardware or proprietary ASICs

This project implements the critiacal part of the architecture (the adaptive weights calculator) on a PYNQ-Z2 board (a FPGA with an opensource framework from AMD made for SWEs to migrate to HW with ease) using only zero-cost tools.

Our Research group came up with an architecture that goes like this. Where the critical part of the weights computation is solved using the hardare (PL-Programmable logic) and the less computationally intensive part is handed off to the software stack (PS-Programmable System)

This is what I built - A systolic array of Givens rotation QR decomposers PEs are implemented in C++ using Vitis HLS (free) and Vivado (free), running on float32 arithmetic. 

A concrete visual result anyone can understand: the same algorithm in 64-bit fixed-point produces a corrupted ultrasound cyst image, however the float32 produces a diagnostically clear reconstruction. 

The benchmarks speak for themselves, the production board (ZCU-104, beat a 24-core Xeon at 50x lower power, moreover the  CPU runs on a clock frequency of 2.20Ghz where as our FPGA just runs on 100MHz clock domain. ) This is just a opensource/ educational version version with the PYNQ-Z2 board ; same algorithm, same precision, but student-accessible hardware

Why this belongs at IndiaFOSS
The PYNQ-Z2 board is an educational board and is easy to use, available in most college labs.

Pre-generated float32 cyst phantom dataset committed to the repo no proprietary software (MATLAB) needed to reproduce hardware results

The tools are free of cost Vitis HLS, Vivado, PYNQ framework, jupyter nb.

This is a educational version of our research project. There are not a lot of people in Hardware/ Deep tech, This might help in that cause


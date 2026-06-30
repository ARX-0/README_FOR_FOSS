# Migrating from SW to Clinical grade HW: An Open MVDR Beamformer

- The MVDR beamforming is the algorithm inside medical ultrasound machines that sharpens focus and suppresses noise from unwanted directions, In this case we use it for picking out human cyst phantoms.

<img width="894" height="555" alt="image" src="https://github.com/user-attachments/assets/5eeadb52-9f08-4a2b-8d8c-a5c8e1c97c33" />

- Computing it in real time requires QR matrix decomposition on every time sample, expensive enough that it normally runs on server hardware or proprietary ASICs

- This project implements the critiacal part of the architecture (the adaptive weights calculator) on a PYNQ-Z2 board (a FPGA with an opensource framework from AMD made for SWEs to migrate to HW with ease) using only zero-cost tools.

- Our Research group came up with an architecture that goes like this. Where the critical part of the weights computation is solved using the hardare (PL-Programmable logic) and the less computationally intensive part is handed off to the software stack (PS-Programmable System)

<img width="1176" height="665" alt="image" src="https://github.com/user-attachments/assets/d92cb83a-95c1-4ec2-8a82-07b1c24b8040" />

<img width="535" height="695" alt="image" src="https://github.com/user-attachments/assets/e7549852-2311-47dc-97fe-1d05ff2485fa" />

- This is what I built - A systolic array of Givens rotation QR decomposers PEs are implemented in C++ using Vitis HLS (free) and Vivado (free), running on float32 arithmetic.

  <img width="1524" height="764" alt="Screenshot 2026-03-23 001111" src="https://github.com/user-attachments/assets/929dfb87-bd2d-4a55-86e6-1c035bb05f2c" />

<img width="940" height="897" alt="Screenshot 2026-03-23 001032" src="https://github.com/user-attachments/assets/d8680a2c-7e5c-4a42-acea-ab7b50aa7ace" />

- A concrete visual result anyone can understand: the same algorithm in 64-bit fixed-point produces a corrupted ultrasound cyst image, however the float32 produces a diagnostically clear reconstruction. 

<img width="859" height="549" alt="image" src="https://github.com/user-attachments/assets/03b5c467-c8ba-4bc7-b164-4b9a7a5e3a56" />

- The benchmarks speak for themselves, the production board (ZCU-104, beat a 24-core Xeon at 50x lower power, moreover the  CPU runs on a clock frequency of 2.20Ghz where as our FPGA just runs on 100MHz clock domain. ) This is just a opensource/ educational version version with the PYNQ-Z2 board ; same algorithm, same precision, but student-accessible hardware

<img width="593" height="219" alt="image" src="https://github.com/user-attachments/assets/c8fe8513-bcaf-43cf-8e77-b417c267d6af" />

<img width="3270" height="2070" alt="image" src="https://github.com/user-attachments/assets/59aa262d-60bf-426a-a029-0b9587b24bb3" />

## Why this belongs at IndiaFOSS
- The PYNQ-Z2 board is an educational board and is easy to use, available in most college labs.

- Pre-generated float32 cyst phantom dataset committed to the repo no proprietary software (MATLAB) needed to reproduce hardware results

- The tools are free of cost Vitis HLS, Vivado, PYNQ framework, jupyter nb.

- This is a educational version of our research project. There are not a lot of people in Hardware/ Deep tech, This might help in that cause

<img width="1793" height="1077" alt="image" src="https://github.com/user-attachments/assets/820b174b-ea0c-4202-86d0-ffeef68a8bb1" />

<img width="695" height="448" alt="image" src="https://github.com/user-attachments/assets/42dcedcd-9ebb-4a1f-b5b2-5d650b49beef" />



<img width="629" height="780" alt="image" src="https://github.com/user-attachments/assets/53800e87-9033-4dc8-b171-f97ca0e30c37" /> (4x6 Systolic Array TPU Design (Weight Stationary)
## Overview
This project implements a hardware accelerator for Matrix Multiplication using a Systolic Array Architecture. The design follows a Weight-Stationary dataflow, optimized for high-throughput convolution and matrix operations commonly found in Deep Learning (Neural Networks).
The core of the design is a 4x6 grid of Processing Elements (PEs) capable of performing FP16 (Floating Point 16-bit) Multiply-Accumulate operations.
## Key Features
Architecture: 2D Systolic Array (4 Rows x 6 Columns).
Dataflow: Weight Stationary (Weights stay in PEs, Inputs flow horizontally, Partial Sums flow vertically).
Precision: 16-bit Floating Point (FP16) 
Synchronization: Implements Data Skew Buffers to align input wavefronts with partial sum propagation.
Pipelining: Deeply pipelined MAC units (Latency = 9 cycles) for high-frequency operation.
 ## System Architecture
-  Top-Level Diagram: The system consists of the Systolic Array core, surrounded by a Data Skew Buffer for input alignment.
 <img width="756" height="593" alt="image" src="https://github.com/user-attachments/assets/97ea4cb3-430d-4fde-b0cf-3c386e97d320" />
 
- Processing Element (PE): Each PE is the fundamental computation unit comprising:
    Weight Register: Stores the stationary weight.
    MAC Unit: Performs (A * B) + C.
    Data Forwarding: Registers input feature maps (ifmap) to pass them to the eastern neighbor in the next clock cycle.
<img width="629" height="780" alt="image" src="https://github.com/user-attachments/assets/53800e87-9033-4dc8-b171-f97ca0e30c37" />

- Data Skew Buffer: To ensure correct timing synchronization, input feature maps are "skewed" before entering the array.
    Row 0: No delay.
    Row 1: 1-cycle delay.
    Row N: N-cycle delay.
  This creates a diagonal "wavefront" of data that meets the partial sums flowing vertically at the exact right moment.
<img width="1257" height="742" alt="image" src="https://github.com/user-attachments/assets/e0620462-3f53-479a-994f-7703ee9f9611" />

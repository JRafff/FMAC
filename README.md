# Parameterized FMAC (Fused Multiply-Accumulate) Generator

## intro
This repository contains a highly configurable **Fused Multiply-Accumulate (FMAC)** hardware generator. Driven by a Python script, this tool automatically generates fully synthesizeable VHDL RTL for an FMAC unit. 

The project is designed with a specific focus on **Design Automation** and **High-Performance Arithmetic**, serving as the foundational Processing Element (PE) for a future high-throughput Systolic Array accelerator.

## Architecture: Why "Fused"?
In standard DSP or CPU arithmetic units, a MAC operation ($Y = Y + A \times B$) is often implemented sequentially: a multiplier computes the product, which is then fed into a separate adder along with the accumulator. This cascaded approach introduces a significant bottleneck in the critical path due to the multiple carry-propagate additions.

This design implements a true **Fused Architecture**:
* The accumulation value ($Y$) is not added at the end of the multiplication. Instead, it is injected directly into the reduction tree as an additional partial product (in Carry-Save format).
* By skipping an entire stage of carry-propagation, the critical path is drastically reduced, allowing for higher clock frequencies and better timing closure in deep pipelines.

## ⚙️ Repository 
To maximize utility and demonstrate immediate verification, this repository includes:
* **Fully Parameterized Generator:** A custom Python script that dynamically generates VHDL code based on user-defined operand and accumulator bit-widths. It automatically handles the complex wiring required for the compression matrix, eliminating human error in Wallace tree routing.
* **Pre-configured 8-bit FMAC Instance:** A ready-to-use 8-bit version of the fused MAC, serving as a concrete hardware implementation example.
* **Fully Verified Testbench:** A robust simulation testbench dedicated to the 8-bit instance, ensuring 100% functional correctness and showcasing a clean verification workflow.

## Project & Future Scope
This FMAC unit is an architectural evolution of my previous work on digital multipliers. It builds upon:
1. **Radix-4 Booth Encoding:** For efficient partial product generation, halving the number of rows in the reduction matrix.
2. **Wallace Tree / Sparse Tree Reduction:** Utilizing a highly optimized network of Carry-Save Adders (CSAs) for column compression.
3. **Prefix Tree Adder (P4):** Using a fast Carry-Lookahead (Sparse Tree) architecture for the final stage.

**Next Steps:** This FMAC is currently being pipelined and optimized to act as the core Processing Element (PE) in a highly scalable **Systolic Array** for matrix multiplication and AI hardware acceleration.

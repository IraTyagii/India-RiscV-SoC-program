# TASK 1
## Lecture Summary: Chip Design Workflow

The lecture explained the **step-by-step process of designing a chip**.

# 1. Specifications

   * First, we decide what the chip should do.
   * At this stage, the design is usually written in **C language**, and a **testbench in C** is also prepared to check if the functionality works correctly.
   * We can use gcc compiler as suggested in the video

# 2. RTL Design (Verilog)

   * Once the specifications are clear, we move to RTL coding in **Verilog**.
   * This is basically the *soft version of the hardware*.
   * Different blocks like processors and peripherals are described here.

# 3. Synthesis

   * The RTL is converted into a **gate-level netlist**.
   * Pre-designed blocks such as **macros** and **analog IPs** are added.

# 4. SoC Integration

   * All blocks are brought together into a **System-on-Chip (SoC)**.
   * Floorplanning, placement, and routing are performed to prepare the full design.
**Floorplanning**

Think of this like designing the blueprint of a house.

You decide where each block (CPU, memory, I/O, etc.) should go on the chip.

The goal is to place them in a way that saves space and makes connections shorter (for speed).
**Placement**

Once the floorplan is ready, individual standard cells (logic gates, flip-flops, etc.) are placed inside their allocated regions.

It’s like arranging furniture inside each room of the house.

The tool ensures that cells are placed efficiently to reduce delays.
**Routing**

After placement, all cells and blocks need to be wired together.

Routing automatically creates metal connections between the cells.

It’s like adding electrical wiring and water pipes in your house so everything works.

**Macros**

These are large pre-designed blocks that can’t be broken into smaller pieces.

Example: SRAM blocks, large memory arrays, PLLs.

They are “dropped” into the chip as ready-made pieces.

**Analog IPs**

IPs (Intellectual Property blocks) that handle analog functions.

Example: ADC (Analog-to-Digital Converter), DAC, PHY for USB or HDMI.

These are carefully designed by analog engineers and reused in chips.

# 5. Final Layout

   * The last step is generating the **chip layout (GDSII file)**.
   * This layout is what goes to the foundry for actual fabrication.

 **Real-world applications:** Smartwatches, Arduino boards, TV panels, AC appliances, and many more.

---

# Microprocessor vs. Microcontroller

* **Microprocessor** → Just the CPU. Needs **external memory** and **peripherals** to work.
* **Microcontroller** → A single chip that already includes **CPU, memory, and peripherals** inside.

---


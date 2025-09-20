# TASK 1
## Lecture Summary: Chip Design Workflow

The lecture explained the **step-by-step process of designing a chip**.

1. **Specifications**

   * First, we decide what the chip should do.
   * At this stage, the design is usually written in **C language**, and a **testbench in C** is also prepared to check if the functionality works correctly.

2. **RTL Design (Verilog)**

   * Once the specifications are clear, we move to RTL coding in **Verilog**.
   * This is basically the *soft version of the hardware*.
   * Different blocks like processors and peripherals are described here.

3. **Synthesis**

   * The RTL is converted into a **gate-level netlist**.
   * Pre-designed blocks such as **macros** and **analog IPs** are added.

4. **SoC Integration**

   * All blocks are brought together into a **System-on-Chip (SoC)**.
   * Floorplanning, placement, and routing are performed to prepare the full design.

5. **Final Layout**

   * The last step is generating the **chip layout (GDSII file)**.
   * This layout is what goes to the foundry for actual fabrication.

 **Real-world applications:** Smartwatches, Arduino boards, TV panels, AC appliances, and many more.

---

# Microprocessor vs. Microcontroller

* **Microprocessor** → Just the CPU. Needs **external memory** and **peripherals** to work.
* **Microcontroller** → A single chip that already includes **CPU, memory, and peripherals** inside.

---


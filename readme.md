# Preparatory Steps

1. Install Vivado 2019.2.1

2. Open Vivado:
   - Create Project
   - Default Part window
   - Boards -> Zynq Ultrascale+ ZCU104 Evaluation board

3. Add design sources:
   - Add Directories -> add `/sources/rtl`

4. Add simulation sources:
   - Add Directories -> add `/sources/tb`

5. To add IPs:
   - **DSP Macro:**
     - IP Catalog -> DSP48 Macro
     - Component Name: `xbip_dsp48_macro_0`
     - Instructions:
       - 0: (A+D)*B
       - 1: (A+D)
     - Pipeline Options: By Tier -> Enable tier 4 registers only
     - Implementation:
       - D -> 27 bits
       - A -> 27 bits
       - B -> 18 bits

   - **BRAM Macro:**
     - IP Catalog -> Block Memory Generator
     - Component Name: `blk_mem_gen_0`
     - Basic:
       - Memory Type -> True Dual Port RAM
       - Port A Options:
         - Write Width -> 128
         - Write Depth -> 256
         - Operating Mode -> Read First
         - Uncheck Primitives Output Register
       - Port B Options:
         - Write Width -> 128
         - Write Depth -> 256
         - Operating Mode -> Read First
         - Uncheck Primitives Output Register
     - Other Options: Check Load Init File -> Add `/sources/random.coe`

# Simulation

Generate switching activity file:
1. Simulation Settings -> Simulator Language = Verilog
2. Simulation tab -> `xsim.simulate.saif`
3. Provide a filename: `*.saif`
4. Check `xsim.simulate.saif_all_signals`
5. Run Behavioral Simulation

# Synthesis & Implementation

1. Add constraints:
   - Add Constraints -> Add `/sources/constraints.xdc`
   
2. Add SAIF file:
   - Add Design Sources -> Add Files -> Add SAIF file generated during simulation

3. Synthesis Settings -> Options -> More Options:
   - Add line: `-mode out_of_context` (to remove I/O power results)

4. Run Implementation

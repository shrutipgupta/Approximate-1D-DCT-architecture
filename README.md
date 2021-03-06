# Approximate-1D-DCT-architecture
# Introduction: 
This is a Verilog implementation of the 1-D 8-point DCT architecture. It implements an approximate design which uses only 12 adders and no multipliers, for the whole computation. The pipeline consists of 8 adder blocks which compute different bit positions of the consecutive operands in the pipeline. The delay involved due to ripple carry generation is utilized to perform other independent tasks, for performance gain.
# Requirements: 
The Xilinx Vivado Design Suite (Vivado 2019.1) was used for HDL sysnthesis and analysis. The installation guide is [here](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/2019-1.html). Simulation waveform can be seen on the Vivado Simulator and the user needs to provide the input text file to the testbench. 
# Customised Input:
Testbench input generation: <br/>The input sequence is provided as a text file. Run the python script gen_in.py to generate the input .txt file. The module takes a csv file as input, whose elements are 8 bit 2's complement binary representation of the elements (8 in each row).<br/> Check out the given input files in the examples folder for more clarification. <br/>Note: This specific pattern guides the order in which the instructions are given to the pipeline.<br/>The trailing 0 and 1 represent addition and subtraction respectively. 
# Order of Execution:
The inputs are provided in a specific sequence to the pipeline and the Null operations mark the clock cycles in which the Stage II computations would be performed. <br/>Refer to the diagram below <br/>![](images/DCT_arch.png)<br/>
# Pipeline:
![](images/DCT.JPG)<br/>The pipeline involves 8 adder modules, where the ith module computes the (8-i)th bit position of the ith input in the sequence. This creates an overlapping structure where the delay for carry generation is utilised in other computations and the throughput increases, once the pipeline is full. <br/>The inputs to the adder blocks are passed through a multiplexer to select between the addition and subtraction operations. A counter is also added to the architecture to keep track of the outputs generated which are to be used for Stage II computations. 
# Simulations:
![](images/Pipeline_sim.JPG)<br/>In the first 8 clock cycles, the pipeline is not filled completely and no final output is generated until the 9th cycle. (This additional cycle results due to the fact that the operation is performed on the negative clock edge while the output is rendered at the positive clock edge). <br/>![](images/Pipeline_fill.JPG)<br/>Once the pipeline is full, the NULL operations are covered by the Stage II operations and a continuous sequence of outputs is generated without wasting any clock cycle. 
# Output: 
Reading the output sequence: <br/>A fair idea of the output sequence can be gained by the input order, shifted by 9 clock cycles (which do not produced output in the beginning). Moreover, for the stage II computations, pertaining to the approximate design, the last bit is ignored in the calculations and can be read from the LSB signal output in the same sequence. 
# Goals:
- [x] Design 1D DCT architecture 
- [ ] Add the transpose module
- [ ] Extend the design to 2D DCT architecture
- [ ] Incorporate the scaling factors to finalize the approximate design
# Conclusion:
This pipelined architecture computes the 8 DCT coefficients of the 8 elements provided into the 2's complement binary form and the output signals can further be used in other circuits using synchronization and extension of the counter. 

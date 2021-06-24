### RTL Design Using Verilog With SKY130 Technology![120062155-4e7a9200-c07e-11eb-9a57-7c6520750a72](https://user-images.githubusercontent.com/86364922/123119864-826c8b80-d461-11eb-86f1-a0842c7c5a55.jpg)
**Day 1 - Introduction to Verilog RTL design and Synthesis**  
- Introduction to open-source simulator iverilog  
- Labs using iverilog and gtkwave  
- Introduction to Yosys and Logic synthesis  
- Labs using Yosys and Sky130 PDKs  

## Day 1 - Introduction to Verilog RTL design and Synthesis  
# Introduction to open-source simulator iverilog 
**RTL** :  Register-transfer level (RTL) is a representation at the abstraction level that expresses a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers along with  the logical operations performed on those signals.Register-transfer-level abstraction is used in hardware description languages (HDLs) like Verilog and VHDL to create high-level representations of a circuit, from where we can derive lower-level representations.
**Simulator** : It is a tool for checking whether our RTL design meets the required specifications or not.
Icarus Verilog is a simulator used for simulation and synthesis of RTL  designs written in verilog which is one of the many hardware description languages.  
**Design** : It is the code or set of verilog codes that has the intended functionality to meet the required specifications.  
**Testbench** : Testbench is a setup which is used to apply stimulus (test_vectors) to the design to check it's functionality.It tests whether the design provides the output that matches the specifications.The RTL Design gets _instantiated_ in the testbench.![Screenshot (668)](https://user-images.githubusercontent.com/86364922/123183399-19ac0000-d4af-11eb-92ac-c41fcb018ec1.png)  
**Iverilog Based Design Flow** :  
1. The iverilog simulator takes RTL design and Testbench as inputs.
2. It produces a VCD file(Value change dump format) as output. Only changes in the input are dumped to changes in the output.
3. We use **Gtkwave** to see these output changes graphically.

# Labs using iverilog and gtkwave

We install the opensource software Virtual box for ruuning the Linux Ubuntu without actually installing it. Then we can download any version of Linux comfortable to us. Once done,We start with the following steps for our environment seup in our virtual terminal.  

1.mkdir vsd  
2.cd vsd  
3.git clone https://github.com/kunalg123/vsdflow.git  
4.mkdir vlsi  
4.cd vlsi  
5.git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git  
6.cd SKY130RTLDesignAndSynthesisWorkshop  
7.cd my_lib  
8.cd lib  
9.cd ..  
10.cd verilog_model  
11.cd ..  
12.cd ..  
13.cd verilog_files  
Below screenshot shows the above directory structure inside the vsd upto my_lib directories that was set up through the terminal.![1 tool setup (2)](https://user-images.githubusercontent.com/86364922/123187775-68aa6300-d4b8-11eb-9fa1-97e42909bcc3.png)
Below screenshot shows the list of verilog files. Each verilog design file has an assosciated test bench file.  

![verilogfilelist](https://user-images.githubusercontent.com/86364922/123188501-c3908a00-d4b9-11eb-93f8-d305bb0d8b16.png)

Since the environment is now set up,we try to simulate a verilog code named good_mux in one of our verilog_files with the help of it's test bench and gtkwave. The steps are mentioned below:
1. We simulate the RTL design and assosciated test bench .
  ```javascript
  iverilog good_mux.v tb_good_mux.v
  ```
 As a result of the above , a. file is created which can be seen in the list of verilog files.  
 ![iverilog1](https://user-images.githubusercontent.com/86364922/123191444-e2454f80-d4be-11eb-9132-2eaeff2480cb.png)  
 
 2. We dump the output into a vcd file.  
   ```javascript
  ./a.out
  ```
 3.The following command invokes gtkwave window where in we can see all our outputs.
   ```javascript
  gtkwave tb_good_mux.vcd
  ```  
  ![iveri2](https://user-images.githubusercontent.com/86364922/123191771-5ed82e00-d4bf-11eb-8a8c-3871160acd51.png)   
  
  In the gtkwave window we drag all the variables and zoom fit to see the transitions for selected variable. 
  
  ![gtkwave](https://user-images.githubusercontent.com/86364922/123191925-96df7100-d4bf-11eb-83b5-32c44f7afa15.png)  
  
4. We can also view our Verilog RTL design and testbench code using
  ```javascript
  gvim tb_good_mux.v -o good_mux.v
  ```
  ![iveri3](https://user-images.githubusercontent.com/86364922/123192819-4701a980-d4c1-11eb-9d99-5b8b5c8d35fa.png)
  Add vcode and testbench
  
  # Introduction to Yosys and Logic synthesis  
  **Synthesizer** :It is a tool used for the conversion of an RTL to a netlist.  
  **Netlist** : It is a representation of the input design to yosys in terms of standard cells present in     the library.
  Yosys is the Synthesizer tool that we will be using. 
  Diiferent levels of abstraction and synthesis  
  ![429600_1_En_2_Fig1_HTML](https://user-images.githubusercontent.com/86364922/123193987-5255d480-d4c3-11eb-8fe5-2dd1b8cd6860.gif)
  **Synthesizer flow**  
  ![yosys flow](https://user-images.githubusercontent.com/86364922/123194307-e4f67380-d4c3-11eb-8bef-0846d251ae0b.png)  
- read_verilog : It is used to read the design.
- lib : It is used to read the library .lib.
- write_verilog : It is used to write out the netlist.

**Veifying Synthesis**
We verify our synthesis to check if any unwanted changes have been made in our RTL design due to above operations.  
![verifysynth](https://user-images.githubusercontent.com/86364922/123195241-7a463780-d4c5-11eb-835f-b3f3ab032387.png)  
The set of primary inputs/outputs will remain same between the RTL design and Synthesized netlist.  Hence,same testbench can be used.Netlist and RTL are essentialy same. RTL is written in _behavioural form_ whereas netlist is written in the form of _standard cells_.  
The waveform in the above figure should be same as observed in RTL simulation Design.

**WHAT HAPPENS DURING SYNTHESIS?**
During Logic Synthesis an RTL code is transformed into a gate level netlist.
- A synthesizer performs gate level translation.  
- Design is converted into gates and connections are made between the gates.  
- The file what we write out finally is a gate-level netlist.  
For example the following code:
```javascript
module sample_code(
input clk, rst,
output result, done);

always@(posedge clk, posedge rst)
if(rst)
...
else
...
endmodule
```
is coverted to the following digital circuit .![Screenshot (690)](https://user-images.githubusercontent.com/86364922/123196661-f2adf800-d4c7-11eb-8b5f-9765cee26c93.png)  

**.lib**  
.lib is the collection of logic modules which includes basic logic gates such as AND, OR, NOT, etc.
It also contains different flavours of the same gate.

Like,
```javascript
  2 - input AND gate
  Versions:
  - slow
  - medium
  - fast
 3 - input AND gate
 Versions:
 - slow
 - medium
 - fast
 4 - input AND gate
 Versions:
 - slow
 - medium
 - fastin
 ```
 **But why do we need different flavours of gate?**
 Consider the below circuit:
 ![Screenshot (690)](https://user-images.githubusercontent.com/86364922/123197514-3d7c3f80-d4c9-11eb-8b26-dbf3b98761fb.png)  

**Setup time Analysis**  
 It should take 1 CLK cycle for the signal to propagate from the Launch DFF-A to the Capture DFF-B through the combimational circuit.  
 While this propagation all delays :
 Propagation delay of DFF A (Tcq-a)+ Propagation delay of combinational circuit(Tcomb)+Setup              time of DFF-B(Tsetup_b).
 Thus ,the constraint becomes:
 ```javascript
 Tclk>Tcq-a+Tcomb+Tsetup_b
 ```
 **Setup** time is the time for which data should arrive before the launch clock edge to be reflected at the output reliably.  
 For high performance we need high speed gates.So, The frequency of a gates should be high or the Tclk  should be less . So all the above mentioned delay should be less. We need cells that work fast to make the combinational small. 
 
 **Hold up time Analysis**   
 After the after the edge of the clock I don't want data to change . So, the fastest change that can happen at the input of we would be after the hold time window so that I can reliably capture the previous clock edge of launch flop DFF-A to ensure there are no hold issues at DFF-B . 
 Hence ,the constraint we have:
 ```javascript
  Thold_b<Tcq-a+Tcomb
  ```
  We need cells to work slowly to ensure that there are no hold time issues at Capture flop DFF-B.  
  
  ![tsuandth](https://user-images.githubusercontent.com/86364922/123199421-7c5fc480-d4cc-11eb-8ebf-663e39cb7b8e.png)

 We need cells that work fast to meet the required performance .We need cells that work slow to meet whold time requirements .  
 This collection of cells forms the .lib
 
 **Comparison of Faster Cells and Slower Cells**  
The load for a cell  in a digital logic circuit usually is taken as a capacitor.  
_Propagation delay_ is the time required for the input to be reflected in the output of the cell it depends on the gate driving capacity which is dependent on the capacitance.
Faster the charging and discharging of a capacitance, lesser will be the cell delay.
In order to charge/discharge the capacitance fast, we need transistors which are capable of sourcing more current which means it is a wide transistor as the current carrying capacity of a transistor is the function of its width.
Wider the transistors, lower will be the delay but more is the area and power.
Narrow Transistors will have more delay but less area and less power.

So fastercells don't come  free. They come at the tradeoffs of area and power.

**How do we select cells?**
- We need to guide the synthesizer to select the flavour of cells for optimum implementation of our logic circuits.
- More use of faster cells results in bad circuit in terms of area and power and potentially may result in hold time   violations.
- More use of slower cells may result in a sluggist circuit and may not meet the performance.
 It is required for us to offer guidance to the synthesizer to pick correct set of cells .This guiding parameters to the synthesizer are called as **CONSTRAINTS**.
 
 # Labs using Yosys and Sky130 PDKs  

 
 

 






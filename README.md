### RTL Design Using Verilog With SKY130 Technology![120062155-4e7a9200-c07e-11eb-9a57-7c6520750a72](https://user-images.githubusercontent.com/86364922/123119864-826c8b80-d461-11eb-86f1-a0842c7c5a55.jpg)
**Day 1 - Introduction to Verilog RTL design and Synthesis**  
- Introduction to open-source simulator iverilog  
- Labs using iverilog and gtkwave  
- Introduction to Yosys and Logic synthesis  
- Labs using Yosys and Sky130 PDKs  
**Day2 - TIMING LIBS, HIERARCHICAL Vs FLAT SYNTHESIS AND EFFICIENT FLOP CODING STYLES**
-Introduction to .lib
-Hierarchial vs Flat Synthesis
-Sub Module level Synthesis and it's necessity
**Day3 - Combinational and Sequential Optimisations


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
 
 # DAY2  
 # TIMING LIBS, HIERARCHICAL Vs FLAT SYNTHESIS AND EFFICIENT FLOP CODING STYLES
**INTRODUCTION TO TIMING .lib**
We take a walk through the library that is said to have a collection of all the standard cells along with their different flavors.
We begin by understanding the name of the library. To look into the  library,we use the gvim command  
```javascript
 gvim ../my_lib/lib/SKY130_fd_sc_hd__tt_025C_1v80.lib
 ```
The following window appears that shows the library file **SKY130_fd_sc_hd__tt_025C_1v80.lib**

![lib](https://user-images.githubusercontent.com/86364922/123222320-73cbb600-d4ed-11eb-836c-2e0f00a84165.png)

First line is the name of the library where TT stands for typical. For a design to work three parameters of the library are important :
- **P- Process**:Process is the variations due to fabrication 
- **V -voltage**: Variations in voltage also impact the behavior of the circuit.  
- **T- temperature** :Semiconductors are very sensitive to temperature variations too.
All this variations determine the performance of a silicon whether it is fast or slow. Thus, our libraries are characterized to model this variations.  
Switching off the syntax color red
```javascript
  Command used : syn off
  ```  
  Enabling the line numbers  
```javascript
  Command used :se nu
```  
We can see that the lib file contains the technology and units of all the parameters.  

![Screenshot (716)](https://user-images.githubusercontent.com/86364922/123445651-bb3d6980-d5f5-11eb-8999-e020cfb874b9.png)

The voltage process and temperature conditions are also specified.  

![Screenshot (717)](https://user-images.githubusercontent.com/86364922/123445735-cee8d000-d5f5-11eb-9bc1-78ed341d9dcf.png)

The lib contains  different flavors of this same as well as different types of cells.  

![Screenshot (718)](https://user-images.githubusercontent.com/86364922/123446050-1ff8c400-d5f6-11eb-8ef1-331b8d401ad7.png)  

![arealib](https://user-images.githubusercontent.com/86364922/123446773-d3fa4f00-d5f6-11eb-94dc-b22030b06a50.png)  

As we see in the above window,  
The library also represents the different features of the cell like its **leakage power**,**the various input's combinations** and the operations between them.  
The name **a21110** signifies that it's  **And OR gate** where in the first two inputs A1 and A2 are And'ed. It's result is OR'ed  with the rest of the three inputs B1,C1 and D1. In order to understand the functionality of the cell we can also look into the  equivalent verilog model. Each of the input can take a high or a low power level.For  5 inputs we have 32 combinations in total. The behaviour model specifies the delay and   power  for each of these inputs. Inside the cell block we have different power combinations and their respective leakage  power values. It also shows the area. It gives the description of various pin in terms of their **capacitance transition** , **internal power**  and **the delay associated with this pins**. 

![pinlib](https://user-images.githubusercontent.com/86364922/123447038-1459cd00-d5f7-11eb-889b-db97b24cdf40.png)

We pick a small gate small gate for better understanding. We see it's behaviour view.  

![Smalland (2)](https://user-images.githubusercontent.com/86364922/123447248-42d7a800-d5f7-11eb-89bc-6178e3397eb9.png)

We can see  in the GVIM window above that there are two input for And gate, and thus four possible combinations the leakage power and the logic levels   of which are specified. We now perform the comparison between the and gates.   

![areacomparison](https://user-images.githubusercontent.com/86364922/123447383-6995de80-d5f7-11eb-8363-f3e354174221.png)

On  comparison we see that the and gate "and2_4" has more area as compared to  the and gate "and2_2" which in turn has more area with the and gate "and2_0". It is thus evident that and2_4 employs wider transistors. These are the different flavours of the same and gate. And and2_4 being the widest also has large leakage power values as well as large area. But it will have small delay values as it is faster .    
 **HIERARCHIAL VS FLAT SYNTHESIS**   
 
 While syntheisizing the RTL design in which multiple modules are present, the synthesis can be done in two forms. We understand it through the following example: 
 
![Screenshot (730)](https://user-images.githubusercontent.com/86364922/123449007-e5dcf180-d5f8-11eb-99f6-117fc51ead78.png)


It has two some moduels. The module 1 is an OR gate ,sub module 2 is AND gate. The sub module called multiple modules  instantiates  sub module 1 as u1 and sub module 2 as u2. It has three inputs a b c and an output y.

![Screenshot (734)](https://user-images.githubusercontent.com/86364922/123449475-584dd180-d5f9-11eb-9cd4-db73a4d46f40.png)  

![Screenshot (735)](https://user-images.githubusercontent.com/86364922/123449587-7287af80-d5f9-11eb-932f-eabab006c877.png)  

The report has inferred submodule1 having  one AND gate ,submodule2 to having one OR gate and multiple module having two cells .
Now we link  this design to the library using abc command. To show the graphical version ,we use the command.  
```javascript
show multiple_modules
```
![Screenshot (736)](https://user-images.githubusercontent.com/86364922/123450058-ea55da00-d5f9-11eb-86e8-ea4c345beb4b.png)

Instead of or and and gates it shows the instances u1 and u2 while preserving the hierarchy. This is called the **hierarchical design.** 

Instead of or, and the circuit is implemented using nand and inverter gates. We always prefer stacked NMOS's(nand gates)to stacked the PMOS's(nor cascaded with inverter for or). Because pmos has a very poor mobility and therefore they have to be made quite wide to obtain a good logical effort .   
We use **flatten** to generate a flat netlist. Here there are no instances of U1 and U2 and hierarchy is not present.    
![flatnetlist](https://user-images.githubusercontent.com/86364922/123451021-efffef80-d5fa-11eb-87c4-1101fdc3acf9.png)  

Even in the design view using show command we see that it simply displays the structure completely without any hierarchy.  
![flatview](https://user-images.githubusercontent.com/86364922/123450797-b8914300-d5fa-11eb-8e81-991d74b0c255.png)

**SUB MODULE LEVEL SYNTHESIS AND ITS NECESSITY**  

**Need for Sub-module synthesis**
- Module level synthesis is preferred when we have multiple instances of the same module.  
 Let's assume a top module having multiple instances of the same unit gates(say, a multiplier) .Rather than synthesizing multiplier multiple times as mult1,mult2,mult3, It's better to synthesise it once and replicate it multiple Times.
- **Divide and conquer approach**  
 Let's assume our RTL design is very very massive and we are giving it to a tool which is not doing a good job. Instead of giving one massive design to the tool, we give portions by portions to the tool so that it provides an optimised netlist and we can stitch all these net lists at at the top level.   
 
Hence we control the model that we are synthesizing using the keywords 
```javascript
  synth-top "sub-module's name"
```
In the  following example,
I am going to  synthesize at sub_module 1 level although I have read the RTL file at higher module level
multi_modules.v
```javascript
read_iverilog multiple_modules.v
synth -top sub_module1
```
![Screenshot (781)](https://user-images.githubusercontent.com/86364922/123455897-dca35300-d5ff-11eb-85d7-6a28dc0d1823.png)

Notice ,In the synthesis report,it  inferring only 1 AND gate.

**GLITCHES**  

Glitches are the unwanted or unexpected transactions that occur due to propagation delays in digital circuits. Glitches occur when an input signal changes state ,provided the signal takes two or more paths for circuit and both paths have unequal delays. The higher delay on one of the parts can cause a glitch when the single pass arrive at the output gate.  
![glitch](https://user-images.githubusercontent.com/86364922/123465838-41fd4100-d60c-11eb-83ec-fd1b6f9d95b3.png)  

**Why Flops?**  

More the combinational circuits more glitchy is the output .We therfore need an element to store the value of the output and that element is called FLOP(storage element).Flop provides resistance to glitches as they transition only at the clock edges .Even though the input of the flop is glitching ,the output will be stable. This avoids glitch propagation in further combinational circuits .  
Also,Initialising the flop is required else the combination circuit will evaluate it to a garbage value. To initialise the flop we have reset and set pins. These two pins can be either synchronous or asynchronous.

**Asynchoronous and Synchronous resets**  
Asynchronous reset: this reset signal does not wait for a clock .The moment asynchronous reset signal  is received output queue becomes 0 irrespective of the clock.  

Asynchronous set: this set signal does not wait for a clock. The moment asynchronous reset is signal received output queue becomes 1 irrespective of the clock.      
 Verilog codes of asynchronous reset and set :  
 
![asyncset reset](https://user-images.githubusercontent.com/86364922/123565541-e634e280-d7da-11eb-81fb-a76841061aa3.png)  

GTKWAVE RTL Simulation and Observations :  

![asyncreset0to1](https://user-images.githubusercontent.com/86364922/123565619-2e540500-d7db-11eb-9cd4-16e8da582598.png) 

- Q follows d only at the posedge of the clock.
- But as and when async_rest=1,Q becomes 0 without waiting for the next edge of the clock.  

![asyncreset1to0](https://user-images.githubusercontent.com/86364922/123565633-37dd6d00-d7db-11eb-893b-cdc44bc696ec.png) 

- But when async_reset goes low(1 to 0),Q doesn't become 1 immediately ,it waits for the next clock edge to follow D.
- Even if asunc_reset=1 and D=1, Q=0 as reset takes high precedence(that is how the code has been written,if condition of reset is checked first).  

Synthesis implementation results :  
asynchoronous reset :  
![async_resetyosys](https://user-images.githubusercontent.com/86364922/123565663-49bf1000-d7db-11eb-8aca-364dfe5fb7e2.png)  
asynchronous set:  
![asyncsetyosys](https://user-images.githubusercontent.com/86364922/123566611-b6d3a500-d7dd-11eb-8ac6-12d222443158.png)  

Synchronous Reset and set :
On application of set or reset signals,the signal does not immediately goes high or low,but it waits for the next edge of the clock cycle. 
RTL code of synchronous reset:
![Screenshot (792)](https://user-images.githubusercontent.com/86364922/123567526-fbf8d680-d7df-11eb-8ffb-074399d84258.png)

Note: The loop is entered only at positive edge of the clock.Upon the posedge of the clock I look for the presence of synchronous reset. If present,Q goes low else D is reflected to output Q.

Synthesis Results:  
![Screenshot (796)](https://user-images.githubusercontent.com/86364922/123567604-2cd90b80-d7e0-11eb-80b3-3d2b3696f0d8.png)

Case wherein both both asynchronous and synchronous resets are applied together :  

RTL CODE:  
![syncres v](https://user-images.githubusercontent.com/86364922/123567672-5abe5000-d7e0-11eb-92b4-e15d3fbf8123.png)  
Synthesis results:  
![Screenshot (797)](https://user-images.githubusercontent.com/86364922/123567810-9f49eb80-d7e0-11eb-8ee6-13f917855169.png) 

**OPTIMISATIONS**
We now observe some Interesting Optimisations For Special Cases :  

**Case 1:**

Let's Consider the following design where the 3 bit input is multiplied by 2 and the output is a 4 bit value.  

```javascript
module mul2 (input [2:0] a , output [3:0] y);
	assign y = a* 2;
endmodule
```

Looking at it's truth table :

| a[2:0] | y[3:0] |
|--------|--------|
| 000    |  0000  |
| 001    |  0010  |
| 010    |  0100  |
| 011    |  0110  |
| 100    |  1000  |
| 101    |  1010  |
| 110    |  1100  |
| 111    |  1110  |  

**Observation** : The output y[3:0] is the input a[2:0] appended with a 0 at the LSB. or, we can say that y = aX2 = {a,0} .   

On synthesizing the netlist and look at its graphical realisation , we will see the same optimisation occuring in the netlist.There is no hardware required fot it.

![Screenshot (757)](https://user-images.githubusercontent.com/86364922/123569226-8ee74000-d7e3-11eb-97bb-c65f1dc16aaa.png)


**Case 2:**

Let's consider the following design where the 3 bit input is multiplied by 9 and the output is a 6 bit value.  

```javascript 
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a* 9;
endmodule
```

Looking at it's truth table :

| a[2:0] | y[5:0] |
|--------|--------|
| 000    |  000000  |
| 001    |  001001  |
| 010    |  010010  |
| 011    |  011011  |
| 100    |  100100  |
| 101    |  101101  |
| 110    |  110110  |
| 111    |  111111  |  

**Observation** : The output y[5:0] is equal to the input a[2:0] appended with itself i.e.
```javascript
y = aX9 = aX(8+1)= aX8+aX1 = {a,0}+a 
y = {a,a}
```

On synthesizing the netlist and look at it's graphical realisation , we will see the same optimisation occuring in the netlist.
 
 ![Screenshot (890)](https://user-images.githubusercontent.com/86364922/123570091-2b5e1200-d7e5-11eb-8502-c70994c3416d.png)  
 
 # DAY 3 : Combinational and Sequential Optimisations  
 
 **Introduction to Logic optimisations**  
 Inorder to produce a  digital circuit design which is optimised interms of area and power, the simulator performs many types of optimisations on the combinational and sequential circuits. 
 
 1.Combinational optimisation  methods:
  - Squezzing the logic to get the most optimised design 
     - Area and Power savings
  - Constant propogation
	   - Direct Optimisation
  - Boolean Logic Optimisation
	   - K-map
	   - Quine-mckluskey Algorithm

2.Sequential optimisation methods:
- Basic
	- Sequential constant Propogation
- Advanced
  - State Optimisation
	- Retiming
	- Sequential Logic Cloning (Floor Plan Aware Synthesis)


**Combinational Logic Optimisations**

We will try to understand each of the above mentioned combinational optimisations through different RTL code examples. For each example, We also check the synthesis implementation through yosys to understand how the optimisations take place.   
All the  optimisation examples are in files opt_check.v, opt_check4.v, and multiple_modules_opt.v. All of these files are present under the verilog_files directory.


Example 1:  
opt_check.v
```javascript
module opt_check (input a, input b , output y);
	assign y = a?b:0;
endmodule
```
Ideally ,the above ternary operator should give us a mux. But the constant 0 propagates further in the logic .Using boolean simplification we obtain y = ab.   

Synthesizing this in yosys :  

Before realising the netlist, we must issue a command to yosys to perform optimisations. It removes all unused cells and wires to prduce optimised digital circuit.This can be done using the opt_clean -purge command as shown below.   

![Screenshot (760)](https://user-images.githubusercontent.com/86364922/123571724-79c0e000-d7e8-11eb-94e7-8b037b21e041.png)

Observation : After executing synth -top opt_check ,we see in the report that 1 AND gate gas been inferred.  

Next,
``` javascript
 abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib  
 write_verilog -noattr opt_check_netlist.v 
 show
 ```
 On viewing the graphical synthesis realisation , we can see the Yosys has synthesized an AND gate as expected.
 
 ![opt_check](https://user-images.githubusercontent.com/86364922/123572300-9ad60080-d7e9-11eb-97a7-4b449ecf3c5d.png)

Example 2:  
opt_check2.v  
```javascript
module opt_check2 (input a, input b , output y);
	assign y = a?1:b;
endmodule
```
After simplification,we expect the output y to be an OR gate, since the output of the mux can be simplified to y = a + b. If we generate the netlist and look at its graphical representation , we get 

![Screenshot (891)](https://user-images.githubusercontent.com/86364922/123573785-323c5300-d7ec-11eb-96f0-9495a850a20f.png)  

Note: The synthesis tool instead of OR gates infers a nand gate with inverted inputs based on Demorgan's Law. It is done to avoid stacked PMOS in CMOS implemantation of OR gate.  

Example 3: opt_check3.v  

```javascript
module opt_check3 (input a, input b , input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
For the RTL  verilog code of opt_check3.v , we expect the output to be a 3 input AND gate based on constant propagation and boolean logic optimisation.The output y can be simplified to y = abc.   
Next we generate the netlist  and observe its graphical representation after synthesis  

![opt_check3](https://user-images.githubusercontent.com/86364922/123574392-59dfeb00-d7ed-11eb-9c6a-ba0f6313530f.png)

Yosys synthesizes a 3 input AND gate as expected because of optimisations.  

Example 4:opt_check4.v 
```javascript
module opt_check4 (input a, input b , input c , output y);
	assign y = a?(b?(a & c):c):(!c);
endmodule
```
In this case,the boolean logic optimisation simplifies  the output to a single xnor gate i.e.  y = a xnor c. 
Next we generate the netlist  and observe its graphical representation after synthesis  

![opt_check4](https://user-images.githubusercontent.com/86364922/123574684-f4402e80-d7ed-11eb-9ef2-e47ef795684a.png)  

Yosys synthesizes a 3 input XNOR gate as expected because of optimisations.  

Example 5:multiple_module_opt.v  

```javascript
module sub_module1(input a , input b , output y);
	assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
	assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1, n2, n3;


sub_module1 U1 (.a(a), .b(1'b1), .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0), .y(n2));
sub_module2 U3 (.a(b),  .b(d), .y(n3));

assign y =c | (b & n1);

endmodule
```  

![flattenb4purgeformultimodule](https://user-images.githubusercontent.com/86364922/123577957-6a469480-d7f2-11eb-9059-8031b4adeb8b.png)

While synthesizing this in yosys we use flatten before opt_clean -purge.
The multiple_module_opt instantiates both submodule1 and 2. 
We must use Flat Synthesis here otherwise the optimisations will not be performed on the sub module level. 
 
![multiple_module_opt v](https://user-images.githubusercontent.com/86364922/123578694-f86f4a80-d7f3-11eb-8b3d-573eae53da43.png) 

Example 5: multiple_module_opt2.v  

![multiple_module_opt2 v code](https://user-images.githubusercontent.com/86364922/123579123-e17d2800-d7f4-11eb-8f1b-ede052914b6a.png)
On boolean optimisation, we obtain y=1 simply.
It's synthesis yields:
![multiple_module_opt2](https://user-images.githubusercontent.com/86364922/123582073-c6adb200-d7fa-11eb-9fa5-47cb3de7b2ac.png)  

**Sequential Logic Optimisations**

We will try to understand each of the sequential optimisations through different RTL code examples. For each example, We also check the synthesis implementation through yosys to understand how the optimisations take place.   
All the  optimisation examples are in files  dff_const2.v,dff_const3.v,dffconst4.v and dff_const5.v. All of these files are under the verilog_files directory.


Example 1: dff_const1.v

```javascript
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

Here, it appears that the output Q should be equal to an inverted reset or Q=!reset. However, as the reset is synchronous,even if the flop has D pinned to logic 1,when reset becomes 0, Q does not immediately goto 1. It waits untill the positive edge of the next clock cycle.  
This is observed by simulating the design in verilog, and viewing the VCD with GTKWave as follows   

![Screenshot (871)](https://user-images.githubusercontent.com/86364922/123584288-bb5c8580-d7fe-11eb-89b2-4b89b5785e57.png)

Observation : In the gtk waveform above , when reset becomes 0, Q becomes 1 at the next clock edge. Since Q can be either 1 or 0,we do not get a sequential constant, and no optimisations should be possible here.
We verify it using Yosys synthesis and optimisation.  
While synthesis,We  use 
```javascript
dfflibmao -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
dfflibmap is a switch that tells the synthesizer about the library to pick sequential circuits( mainly  Dff's  and latches) from.

We then generate the netlist 
```javascript
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib 
write_verilog -noattr dff_const1_netlist.v 
show
```

![Screenshot (878)](https://user-images.githubusercontent.com/86364922/123585110-3d00e300-d800-11eb-9748-ee5bfbddca34.png)

As expected, No optimisation is performed in th yosys implementation during synthesis.

Example 2 : dff_const2.v  

```javascript
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

Here, Regardless of the inputs, the output q always remains constant at 1 .  
This is observed by simulating the design in verilog, and viewing the VCD with GTKWave as follows   

![Screenshot (873)](https://user-images.githubusercontent.com/86364922/123585355-b6003a80-d800-11eb-9c8d-ae2f0e4717fb.png)  

Since the output is always constant ie Q=1, it can easily be optimised during synthesis.  

![Screenshot (880)](https://user-images.githubusercontent.com/86364922/123585526-02e41100-d801-11eb-8b1a-073081326b47.png)


Example 3 : dff_const3.v
```javascript
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin 
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```
When reset goes from 1 to 0,Q1 follows D at the next positive clock edge in an ideal ckt. But in reality, Q1 becomes 1 a little after the  next positive clk edge(once reset has been made 0)due to **Clock-to-Q delay**.   
Thus, q takes the value 0 until the next clock edge when it read an input of 1 from q1. This is confirmed with the simulated waveform below.

![Screenshot (874)](https://user-images.githubusercontent.com/86364922/123586503-733f6200-d802-11eb-82e3-821bab0c1d07.png)

Since Q takes both logic 0 and 1 values in different clock cycles. It is wrong to say that
 Q=!(reset)  or Q=Q1    
Hence, both the flip-flops are retained and no optimisations are performed on this design.  We can confirm this using Yosys as shown below.

![Screenshot (881)](https://user-images.githubusercontent.com/86364922/123588573-961f4580-d805-11eb-8fff-ac831dc2219d.png)

Both the D flip-flops are present in the synthesized netlist.

Example 4: dff_const4.v  

RTL Code: 

![Screenshot (875)](https://user-images.githubusercontent.com/86364922/123589213-86543100-d806-11eb-9d2c-5d970d8ff6fc.png)  

Here, regardless of the  input whether reset or not , Q1 is always going to be constant i.e.  Q1=1 . As q can only be 1 or q1 depending on the reset input , but q1 = 1 .Thus q is also constant at the value 1. We can confirm this with the simulated waveforms as shown below.

![Screenshot (877)](https://user-images.githubusercontent.com/86364922/123589283-a4ba2c80-d806-11eb-9f41-f698d5e9b038.png)

As the output is always constant, it can easily be optimised using Yosys as shown in the graphical representation.  

![Screenshot (882)](https://user-images.githubusercontent.com/86364922/123589434-da5f1580-d806-11eb-81ad-600bab8e0772.png)  

**Unused output optimisations**  

# Day 4: Gate Level Simulations,Blocking vs Non Blocking assignments,Synthesis-Simulation Mismatch  
**Introduction to Gate Level Simulations**  
We validate our RTL design by providing stimulus to the testbench and check whether it meets our specifications earlier we were running the test bench with the RTL code as our design under test .  
But now under  GLS ,we apply netlist to the testbench as desh under test . What We did at the behavioral level in the RTL code got transformed to the net list in terms of the standard cells present in the library. So,net list is logically same as the RTL code. They both have the same inputs and outputs so the netlist should seamlessly fit in the place of the RTL code. We put the netlist in place of the RTL file and run the simulation with the test bench.  
When we do simulation in with the help of RTL code there is no concept of timing analysis such as the hold and setup time which are critical for a circuit. For meeting this setup and hold time criteria there are different flavours of cell in the library.

![Screenshot (800)](https://user-images.githubusercontent.com/86364922/123590696-9bca5a80-d808-11eb-80d1-37bd184e22ff.png)

In GLS  using iverilog flow, the design is a netlist which is given to Iverilog simulator  in terms of standard cells present in the library. The library has different flavours of the same type of cell available.To make the simulator understand the specification of the different annotations of the cell the GATE level verilog models is also given as an input. If the GATE level models are **timing aware** (delay annotated ),then we can use the GLS for timing validation as well.  

Question : If netlist is the true representation of my RTL code then what is the need of functional validation of my net list?  
Answer:  Because they can be simulation and synthesis mismatches.  

**Synthesis Simulation Mismatches**  
- Missing sensitivity list
- Blocking and non blocking statements


**Missing sensitivity list**

Simulator functions on the basis of activity that is it looks for if either of the inputs change. If there is no change in the inputs the simulator won't evaluate the output at all.

Example: 
```javascript
module mux(input i0, input i1, input sel, output reg y);

always @(sel)
begin
	if (sel)
	begin
		y = i1;
	end
	else
	begin
		y = i0;
	end
end
endmodule
```
The problem in this mux code is that simulation happens  only when the select is high so if select is slow and there are changes in i zero aur i1 they get completely missed. So for the simulator this marks is as good as a latch a double h block but the synthesizer does not look at the sensitivity list rather on functionality creates a mux.
Simulation: latch
Synthesis:mux
Hence,Mismatch.

**Blocking and non blocking statements** 
Inside always block
- Blocking
 Executes the statements in the order it is written
 So the first statement is evaluated before the second statement
- Non Blocking
 Executes all the RHS when always block is entered and assigns to LHS.
 Parallel evaluation

Example of Blocking assignments:

```javascript
module shift_register(input clk, input reset, input d, output reg q1);
reg q0;

always @(posedge clk,posedge reset)
begin
	if(reset)
	begin
		q0 = 1'b0;
		q1 = 1'b0;
	end
	else
	begin
		q0 = d;
		q1 = q0;
	end
end
endmodule
```
 In this case, D is assigned to Qo not which is then assigned to Q. Due to optimisation a single latch is formed where Q is equal to D.
 
Example of Non-Blocking assignments:  

In the above RTL code if
```javascript
      begin
		q0 <= d;
		q1 <= q0;
      end
 ```
In the non blocking assignments all the RHS are evaluated and parallel assigned to lhs irrespective of the order in which they appear. So we will always get a two flop shift register.

Therefore we always use non blocking statements for writing sequential circuits.  

Other example:
```javascript
module comblogic(input a, input b, input c, output reg y);
reg q0;

always @(*)
begin
	y = q0 & c;
	q0 = a|b;
end
endmodule
```

We enter into the loop whenever any of the inputs a b or C changes but Y is assigned with old Qo value since it is using the value of the previous Tclk ,the  simulator mimics a delay or a flop. Where as, during synthesis we see the the OR and AND gates as expected.  

Therefore ,while using blocking statements in this case,we should evaluate Q0 first and then Y so that Y takes on the updated values of Qo. Although both the circuits on synthesis give the same digital circuit comprising of AND, OR gates. But on simulation we get different behaviours.  

**Labs on GLS and Synthesis-Simulation Mismatch**
Example 1:
A mux designed with the help of ternary operator
```javascript
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
endmodule
```
The synthesized netlist for the above,using yosys   

![Screenshot (868)](https://user-images.githubusercontent.com/86364922/123598554-47c47380-d812-11eb-8f74-9b9eeebe182d.png)    

To invoke GLS,
- We need to read our netlist file and the test bench file assosciated with it.
- We need to read 2 extra files that contain the description of verilog models in the netlist.
```javascript
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
```
To see the waveform of RTL simulation,we execute the following commands further
```javascript
./a.out
gtkwave tb_ternary_operator_mux.v
```  
![Screenshot (892)](https://user-images.githubusercontent.com/86364922/123599839-a76f4e80-d813-11eb-8aac-92b3e6b3068f.png)  
The generated netlist does behave like a 2X1 multiplexer.  

Example 2: 
```javascript
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @(sel)
begin
	if(sel)
		y <= i1;
	else
		y <= i0;
end 
endmodule
```  
In this example,```javascriptalways``` is evaluated only if ```javascriptselect``` is high.It is insensitive to ```javascriptio or i1``` because ```javascriptselect``` is low.  

This is verified by GTKWAVE of  RTL Simulation below  :  
![Screenshot (893)](https://user-images.githubusercontent.com/86364922/123601772-ad662f00-d815-11eb-8298-028aa12b1b62.png)    

The design simulates  a **latch** rather than a 2x1 mux.  
But the Yosys implementation shows a 2X1 mux .   

![Screenshot (894)](https://user-images.githubusercontent.com/86364922/123602392-51e87100-d816-11eb-9d45-8577f4b82393.png)    

If we now implement it's GATE level netlist through GLS and observe the waveform,it shows the behaviour of a  2X1 mux as shown below: 

![Screenshot (895)](https://user-images.githubusercontent.com/86364922/123603424-7ee95380-d817-11eb-9546-9181191a344f.png)    

Since,the waveforms of stimulated RTL Code : Is of a LATCH
      the waveforms of gate level netlist thruogh GLS after synthesis: Is of 2X1 MUX 
We see a **Synthesis-Simulation Mismatch**.  

Example 3: 
This is an example of synthesis-simulation mismatch due to wrong order of assignment in blocking assignments.  

```javascript
module blocking_caveat (input a , input b , input c , output reg d);
reg x;

always @(*)
begin
	d = x & c;
	x = a | b;
end 
endmodule
```
We enter into the loop whenever any of the inputs a b or C changes but D is assigned with old X value since it is using the value of the previous Tclk ,the  simulator mimics a delay or a flop. Where as, during synthesis we see the the OR and AND gates as expected.






































































 
 

 






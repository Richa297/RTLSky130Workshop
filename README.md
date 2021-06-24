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
We istall the opensource software Virtual box for ruuning the Linux Ubuntu without actually installing it. Then we can download any version of Linux comfortable to us.Once done,We start with the following steps for our environment seup in our virtual terminal.
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

Since the environment is now set up,we try to simulate a verilog code named good_mux in one of our verilog_files with the help of it's test bench and gtkwave.










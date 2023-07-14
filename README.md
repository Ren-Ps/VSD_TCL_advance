# TCL : Advanced Scripting in Design and Synthesis

TCL (Tool Command Language) is a scripting language commonly used in the VLSI (Very Large Scale Integration) industry for various tasks related to electronic design automation (EDA) tools. why TCL is important in the VLSI industry:

- Automation: TCL provides powerful scripting capabilities that allow automation of repetitive tasks in the VLSI design flow. It enables engineers to write scripts to perform complex operations, such as running simulations, generating test vectors, modifying designs, and analyzing results. This automation helps improve productivity and efficiency in the VLSI design process.

- EDA Tool Integration: TCL serves as a bridge between different EDA tools used in the VLSI design flow. It provides a standardized scripting language that can control and interact with various tools, such as synthesis, place and route, timing analysis, and layout tools. TCL scripts can automate the transfer of data between different tools, ensuring smooth integration and efficient collaboration among different stages of the design process.

- Customization and Extension: TCL allows engineers to customize and extend the functionalities of EDA tools. Through TCL scripting, designers can create their own commands, procedures, and functions to tailor the EDA tools according to their specific requirements. This flexibility empowers designers to optimize tool flows, add new features, and adapt tools to unique design methodologies.

- Design Verification and Testing: TCL is extensively used for design verification and testing purposes. Engineers can write TCL scripts to automate testbench generation, run simulations, analyze waveforms, and perform coverage analysis. It enables efficient verification of VLSI designs and helps identify and debug issues early in the design cycle.

- Design Data Manipulation: TCL provides powerful data manipulation capabilities, allowing designers to process and manipulate design data efficiently. It can parse and extract information from design files, perform calculations, generate reports, and automate design-related tasks. TCL's flexible string handling and file processing capabilities make it a valuable tool for data analysis and manipulation in the VLSI industry.

# Agenda

- [Unit 1](#Unit-1) :  Introduction, Toolbox usage scenarios

- [Unit 2](#Unit-2) : Sub-Task Two - From CSV to format[1] and SDC - Variable Creation,  Sub-Task Two - From CSV to format[1] and SDC - Processing constraints, CSV.

- [Unit 3](#Unit-3)  : Sub-Task Two - From CSV to format[1] and SDC - Processing clock constraints,  Sub-Task Two - From CSV to format[1] and SDC - Processing input constraints

- [Unit 4](#Unit-4)  : Full script for download and Conclusion, Introduction to Yosys synthesis tool usage, Hierarchy check and error handling script creation for Yosys

- [Unit 5](#Unit-5) : Synthesis main file scripting and output file editing, World of 'Procs', read_sdc proc - interpret clock generation constraints, read_sdc proc - interpret IO delays and transition constraints, Process bussed ports and configuration file creation, Quality of results (QOR) generation algorithm


# Open Source Tools
- [Yosys](https://yosyshq.net/yosys/)
- [OpenTimer](https://github.com/skyformat99/OpenTimer-1.0.5)
- [TCL development suite](https://www.tcl.tk/)
- Libraries (associated with TCL)

# Operating System
- Ubunut 18.04

## Pre-request

**COWSAY** is a program that generates ASCII art pictures of a cow with a message. It can also generate pictures using pre-made images of other animals, such as Tux the Penguin, the Linux mascot.

*Install COWSAY*
`` apt-get install cowsay -y ``

> CLI Usage Example
``` cowsay -f beefymiracle Hello Linux ```

# Unit 1

### Tasks :
- Make a Bash Script and Execute it.
- By accepting CLI arguments, Invoke TCL Command
- Deal with issues to pass command line argument to TCL Script (Eg. No Argument, Multiple Arguments, Argument doesn't exist)

### Bash Script for TCL Automation :

```
#!/bin/tcsh -f
echo
if ($#argv != 1) then
	cowsay -f eyes "INFO: Please provide only a proper csv file" 
echo 
echo "		TCL Command Created By Pritesh - Ps 	"
echo
exit 1
endif
if (! -f $argv[1] || $argv[1] == "-help") then
	if ($argv[1] != "-help") then
		cowsay -f eyes "Error: Cannot find csv file $argv[1] in Directory. Exiting the Flow..."
		echo 
		echo "		TCL Command Created By Pritesh - Ps 	"
		echo
		exit 1
	else
		echo USAGE: ./psynth \<csv file\>
		echo
		echo \<csv file\> ............. consist of 2 columns, below keywords being in 1st column and its case sensitive.
		echo \<Design Name\> .......... Name of top level module
		echo \<Output Directory\> ..... All generated reports is stored at this path
		echo \<Netlist Directory\> .... RTL netlist is at this path
		echo \<Early Library Path\> ... Path for early library cell, used in STA
		echo \<Late Library Path\> .... Path for Late library cell, used in STA
		echo \<Constraints file\> ..... Path for csv file having constraints, used in STA
		echo
		exit 1
	endif
else
	cowsay -f tux " Processing with $argv[1] ...."
	echo 
	echo "	TCL Command Created By Pritesh - Ps 	"
	echo
	tclsh psynth.tcl $argv[1]   
exit 1 
endif

```

### *Results :*

- Case 1) Normal CLI Argument Passing

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/1-5_csv_accepted.png)

- Case 2) No CLI Argument

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/1-1_without_csv.png)

- Case 3) Multiple CLI Arguments

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/1-3_multiple_csv.png)

- Case 4) CLI Argument Doesn't Exist

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/1-4_csv_missing.png)

- Case 5) CLI Argument Usage Help

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/1-2_usage_help.png)

# Unit 2

### Tasks :
- Auto Variable Creation
- Checking if provided directories exist at respective locations, display an error if not found.....
- Convert the inputs into a format that can be passed to Yosys Synthesis Tool

### *Results :*

- Passing Inputs Path from .csv file to the Scripts :

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/2-1_passing_inputs.png)

- Checking the directories for given input path exists at respective locations or not :

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/2-2_directory_check.png)

- Display an Error if the directory file is not found at given location :

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/2-3_directory_file_missing.png)

- Display Error if directory folder is not found and Make it

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/2-4_directory_folder_missing_nd_created.png)


# Unit 3

### Tasks:
- Read the Clock, Input & Output Constraints from the file and create Synopsys Design Constraints (SDC) Format .sdc file
- Take care of the bus port to create SDC file
- Verify the generated .sdc file

### *Results :*

- Terminal Log for SDC

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/3-1_SDC_terminal.png)

- Generated SDC File

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/3-2_SDC_file.png)

# Unit 4

### Tasks:
- Create a synthesis script that can be passed as an argument to Yosys Synthesis Tool
- Run the hierarchy check to verify if all RTL modules are correctly present or not, raise an error if not.
- Observe the generated netlist and remove the redundant part from it

### *Results :*

- Hierarchy Check PASS

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/4-1_Hierachy_pass.png)

- An Example where Hierarchy Check gets FAIL

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/4-2_Hierachy_fail.png)

- Synthesis Pass log

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/4-3_Synthesis_pass.png)

- Generated Synthesis Script for Yosys Synthesis Tool

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/4-4_Synthesis_gen_scrpt.png)

- Netlist Generation log

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/4-5_netlist_gen.png)

- Snapshot of Netlist generated by Yosys Tool

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/4-6_netlist_gen_file.png)

# Unit 5 

### Tasks:
- Check Procs
- Create a constraint .timing file from the .sdc file which can be applied to OpenTimer tool
- Take care of all bits of the bus while creating the .timing file 
- Create a .conf file input script for the OpenTimer tool
- Quality of Results (QoR) Generation

**Proc is just like a function. We can define it in a file and then just source that proc to our main script and call that proc just like any other function by passing the required options and arguments.**

We have used 5 procs in our script:
1) reopenStdout.proc for redirecting the output generated by puts command from console to .conf file.
2) set_num_threads.proc for setting number of threads/cores used to execute the further script.
3) read_lib.proc for reading the standard cell libraries.
4) read_verilog.proc for reading the final synthesized RTL Netlist in openMSP43.final.synth.v file.
5) read_sdc.proc for reading the sdc constraints.**

An Example of Proc *set_num_threads.proc*  

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/5-6_procs_exmpl.png)

### *Results :*

- .conf File Input Script for OpenTimer tool

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/5-1_config_file.png)

- Snapshot of generated .timing file
   
![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/5-2_timing_file.png)

- Snapshot of generated SPF file

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/5-3_spf_file.png)

- Static Timing Analysis

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/5-4_STA1_cort.png)

### Quality of Results (QoR)

The phrase "Quality of Results" is used in the evaluation of technical processes. Here, several factors such as setup violations, hold violations, worst negative slack, and run-time are used to determine the Quality of Results.

![image](https://github.com/Ren-Ps/VSD_TCL_advance/blob/main/Imgs/5-5_QoR1_cort.png)

# Conclusion 

Basic TCL syntax, variables, control structures, and procedures have been learned.
Also, learned about Linux-based Procs and how to use them in TCL scripting, as well as error handling and debugging techniques.

Using TCL scripting, an automation called "PSYNTH" was successfully constructed.
Which accepts user input in the form of RTL Verilog Code files and library directories, synthesises the corresponding hardware, does static timing analysis (STA), and displays the Quality of Results (QoR).

## Acknowledgments

**Mr. Kunal Ghosh, Co-Founder, VSD Corp. Pvt. Ltd.** and
**Geetima Kachari**

> Note - backup of images* may found in Imgs folder as zip.

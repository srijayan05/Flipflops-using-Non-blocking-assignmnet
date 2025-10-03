# EXPERIMENT 3B: Simulation of All Flip-Flops using Non Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **Non blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **Non blocking assignment (`<=`)** inside the `always` block.  
Non Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Non Blocking)
```verilog
module S_R_FF(s, r, clk, rst, q);
    input s, r, clk, rst;
    output reg q;
    
    always @(posedge clk) begin
        if (rst == 1)
            q <= 0;           
        else begin
            case ({s, r})
                2'b00: q <= q;   
                2'b01: q <= 1'b0; 
                2'b10: q <= 1'b1; 
                2'b11: q <= 1'bx; 
            endcase
        end
    end
endmodule
```
### SR Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_sr_ff;
    reg s,r,clk,rst;
    wire q;
    
S_R_FF uut(s,r,clk,rst,q);
always #5 clk=~clk;
initial 
begin
    clk=0;s=0;r=0;rst=1;
    #10 rst=0;
    #10 s=0;r=0;
    #10 s=0;r=1;
    #10 s=1;r=1;
    #10 s=0;r=0;
    #20
    $finish;
end
endmodule


```
#### SIMULATION OUTPUT

<img width="1919" height="1079" alt="Screenshot 2025-09-24 110249" src="https://github.com/user-attachments/assets/e19a29fd-5794-4663-9270-1f16da34fe2e" />


### JK Flip-Flop (Non Blocking)
```verilog
module jk_ff(j, k, clk, rst, q);
  input j, k, clk, rst;
  output reg q;

  always @(posedge clk or posedge rst) begin
    if (rst) 
      q <= 0;               
    else begin
      case ({j, k})
        2'b00: q <= q;      
        2'b01: q <= 0;      
        2'b10: q <= 1;     
        2'b11: q <= ~q;    
      endcase
    end
  end
endmodule

```
### JK Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_JK_FF;
  reg j, k, clk, rst;
  wire q;

  jk_ff uut (j,k,clk,rst,q);

  always #5 clk = ~clk;

  initial begin

    clk = 0; j = 0; k = 0; rst = 1;

    #10 rst = 0;

    #10 j = 0; k = 0;   
    #10 j = 0; k = 1;   
    #10 j = 1; k = 1;   
    #10 j = 1; k = 0;  
    #20
    $finish;
  end
endmodule


```
#### SIMULATION OUTPUT

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/33f340f1-36aa-4e0d-90a8-09bb30b1c374" />

### D Flip-Flop (Non Blocking)
```verilog
module DFF(clk,rst,d,q);
input clk,rst,d;
output reg q;
always @ (posedge clk)
begin
	if(rst==1)
	q<=0;
	else 
	q<=d;
end
endmodule
```
### D Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_DFF;
    reg clk, rst, d;
    wire q;
    DFF uut(clk, rst, d, q);
    always #5 clk = ~clk;
    initial begin
        clk = 0; d = 0; rst = 1;

        #10 rst = 0;  
        #10 d = 0;
        #10 d = 1;
        #10 d = 0;
        #10 d = 1;

        #20 $finish;
    end
endmodule
```

#### SIMULATION OUTPUT

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/e694bb19-f55a-424d-af03-0e05f5857e02" />

### T Flip-Flop (Non Blocking)
```verilog
module TFF(clk,rst,t,q);
input  clk,rst,t;
output reg q;
always @ (posedge clk)
begin
	if(rst)
	q<=0;
	else if(t==0)
	q<=q;
	else
	q<=~q;
end
endmodule
```
### T Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_TFF;
reg clk,rst,t;
wire q;
TFF uut(clk,rst,t,q);
always #5 clk=~clk;
initial
 begin
	clk=0;t=0;rst=1;
	#10 rst=0;
	t=0;
	#10
	t=1;
end 
endmodule
```

#### SIMULATION OUTPUT

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/041a1b42-c56f-4fc6-8cf8-e84a2075d071" />


### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using Non blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.

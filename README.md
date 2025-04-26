# 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
## Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

## Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.

## Code:
//verilog code
```
module bit_rc(
    input  [3:0] A,     
    input  [3:0] B,      
    input        Cin,    
    output [3:0] Sum,    
    output       reg cout    
);

    reg [3:0] sum_temp;
    reg       c1, c2, c3; // Intermediate carry signals

    // Task for Full Adder
    task full_adder;
        input  a, b, cin;
        output reg sum, cout;
        begin
            sum  = a ^ b ^ cin;
            cout = (a & b) | (b & cin) | (cin & a);
        end
    endtask

    // Ripple carry logic using task
    always @(*) begin
        full_adder(A[0], B[0], Cin, sum_temp[0], c1);
        full_adder(A[1], B[1], c1,  sum_temp[1], c2);
        full_adder(A[2], B[2], c2,  sum_temp[2], c3);
        full_adder(A[3], B[3], c3,  sum_temp[3], cout);
    end

    assign Sum = sum_temp;

endmodule
```


// Test bench for Ripple carry adder
```
module bit_rc;
reg [3:0] A, B;
reg Cin;
wire [3:0] Sum;
wire Cout;

// Instantiate the ripple carry adder
bit_rc uut (
    .A(A),
    .B(B),
    .Cin(Cin),
    .Sum(Sum),
    .Cout(Cout)
);

initial begin
    // Test cases
    A = 4'b0001; B = 4'b0010; Cin = 0;
    #10;
    
    A = 4'b0110; B = 4'b0101; Cin = 0;
    #10;
    
    A = 4'b1111; B = 4'b0001; Cin = 0;
    #10;
    
    A = 4'b1010; B = 4'b1101; Cin = 1;
    #10;
    
    A = 4'b1111; B = 4'b1111; Cin = 1;
    #10;

    $stop;
end

initial begin
    $monitor("Time = %0t | A = %b | B = %b | Cin = %b | Sum = %b | Cout = %b", $time, A, B, Cin, Sum, Cout);
end
```
## output:
![Screenshot 2025-04-19 143919](https://github.com/user-attachments/assets/07d3f674-3f5c-4315-96a8-4c292410b403)

## code:

// Verilog Code ripple counter
```

module rc_function(
input clk,
input rst,
output reg [3:0]Q
);
function [3:0] next_state;
    input [3:0] curr_state;
    begin
        next_state = curr_state + 1;
    end
endfunction
    
always @(posedge clk)
 begin
    if (rst)
        Q<=4'b0000;       
    else
        Q <= next_state(Q);
end
endmodule
```

// TestBench
```

module rc_function;
reg clk;
reg reset;
wire [3:0] Q;

// Instantiate the ripple counter
rc_function uut (
    .clk(clk),
    .reset(reset),
    .Q(Q)
);

// Clock generation (10ns period)
always #5 clk = ~clk;

initial begin
    // Initialize inputs
    clk = 0;
    reset = 1;

    // Hold reset for 20ns
    #20 reset = 0;

    // Run simulation for 200ns
    #200 $stop;
end

initial begin
    $monitor("Time = %0t | Reset = %b | Q = %b", $time, reset, Q);
end
```
## output:
![Screenshot 2025-04-26 132042](https://github.com/user-attachments/assets/1d13b965-e0b5-45a1-bf21-85a4fc188908)


## Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.


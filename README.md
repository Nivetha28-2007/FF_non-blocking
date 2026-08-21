# FF_blocking_non-blocking
# EXPERIMENT 3A: Simulation of All Flip-Flops using Blocking Statement
# AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using blocking statements in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

# APPARATUS REQUIRED
Vivado 2023.1
Computer with HDL Simulator
# DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using behavioral modeling with blocking assignment (=) inside the always block.
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

# PROCEDURE
Open Vivado 2023.1.
Create a New RTL Project (e.g., FlipFlop_Simulation).
Add Verilog source files for each flip-flop (SR, D, JK, T).
Add a testbench file to verify all flip-flops.
Run Behavioral Simulation.
Observe waveforms of inputs and outputs for each flip-flop.
Verify that outputs match the truth table.
Save results and capture simulation screenshots.
# VERILOG CODE
# SR Flip-Flop (Non Blocking)
```
module sr_ff (
    input wire S, R, clk,
    output reg Q
);
    always @(posedge clk) begin
      case ({S, R})
        2'b00: Q <= Q;      // Hold
        2'b01: Q <= 1'b0;  // Reset
        2'b10: Q <= 1'b1;  // Set
        2'b11: Q <= 1'bx;  // Invalid
    endcase
end

assign Qbar = ~Q;
endmodule
```
# SR Flip-Flop Test bench
```
module sr_flipflop_tb;

    reg S, R, clk;
    wire Q, Qbar;

    // Instantiate DUT
    sr_flipflop uut (
        .S(S),
        .R(R),
        .clk(clk),
        .Q(Q),
        .Qbar(Qbar)
    );

    // Clock generation
    initial begin
        clk = 1'b0;
        forever #5 clk = ~clk;
    end

    // Test cases
    initial begin
        S = 0;
        R = 0;

        // Hold
        #10;
        S = 1;
        R = 0;

        // Set
        #10;
        S = 0;
        R = 1;

        // Reset
        #10;
        S = 0;
        R = 0;

        // Hold
        #10;
        S = 1;
        R = 1;

        // Invalid
        #10;
        $finish;
    end

    // Display output
    initial begin
        $monitor("Time=%0t | CLK=%b | S=%b R=%b | Q=%b Qbar=%b",
                 $time, clk, S, R, Q, Qbar);
    end

endmodule
```

# SIMULATION OUTPUT
<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/dd2be112-9bee-4a0e-a0f0-117aee41c9ba" />


# JK Flip-Flop (Non Blocking)
```
module jk_ff (
    input wire J, K, clk,
    output reg Q
);
    always @(posedge clk) begin
        case ({J, K})
        2'b00: Q <= Q;       // Hold
        2'b01: Q <= 1'b0;   // Reset
        2'b10: Q <= 1'b1;   // Set
        2'b11: Q <= ~Q;     // Toggle
    endcase
end
assign Qbar = ~Q;
endmodule
```
# JK Flip-Flop Test bench
```
module jk_flipflop_tb;

    reg J, K, clk;
    wire Q, Qbar;

    // Instantiate DUT
    jk_flipflop uut (
        .J(J),
        .K(K),
        .clk(clk),
        .Q(Q),
        .Qbar(Qbar)
    );

    // Clock generation
    initial begin
        clk = 1'b0;
        forever #5 clk = ~clk;
    end

    // Test cases
    initial begin
        J = 0;
        K = 0;

        // Hold
        #10;
        J = 1;
        K = 0;

        // Set
        #10;
        J = 0;
        K = 1;

        // Reset
        #10;
        J = 1;
        K = 1;

        // Toggle
        #10;
        J = 1;
        K = 1;

        // Toggle again
        #10;
        J = 0;
        K = 0;

        // Hold
        #10;
        $finish;
    end

    // Display output
    initial begin
        $monitor("Time=%0t | CLK=%b | J=%b K=%b | Q=%b Qbar=%b",
                 $time, clk, J, K, Q, Qbar);
    end

endmodule
```

# SIMULATION OUTPUT
<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/bb6ba91f-baba-41e6-966a-8d67e69f7d74" />


# D Flip-Flop (Non Blocking)
```
module d_flipflop (
    input D,
    input clk,
    output reg Q,
    output Qbar
);

always @(posedge clk) begin
    case (D)
        1'b0: Q <= 1'b0;
        1'b1: Q <= 1'b1;
    endcase
end

assign Qbar = ~Q;

endmodule
```
# D Flip-Flop Test bench
```
module d_flipflop_tb;

    reg D, clk;
    wire Q, Qbar;

    // Instantiate DUT
    d_flipflop uut (
        .D(D),
        .clk(clk),
        .Q(Q),
        .Qbar(Qbar)
    );

    // Clock generation
    initial begin
        clk = 1'b0;
        forever #5 clk = ~clk;
    end

    // Test cases
    initial begin
        D = 1'b0;
        #10;

        D = 1'b1;
        #10;

        D = 1'b0;
        #10;

        D = 1'b1;
        #10;

        D = 1'b0;
        #10;

        $finish;
    end

    // Display output
    initial begin
        $monitor("Time=%0t | CLK=%b | D=%b | Q=%b | Qbar=%b",
                 $time, clk, D, Q, Qbar);
    end

endmodule
```

# SIMULATION OUTPUT
<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/6a7971be-bf79-4c34-af43-b07ca804cac4" />

# T Flip-Flop (Non Blocking)
```
module t_flipflop (
    input T,
    input clk,
    output reg Q,
    output Qbar
);

always @(posedge clk) begin
    case (T)
        1'b0: Q <= Q;     // Hold
        1'b1: Q <= ~Q;    // Toggle
    endcase
end

assign Qbar = ~Q;

endmodule
```
# T Flip-Flop Test bench
```
module t_flipflop_tb;

    reg T, clk;
    wire Q, Qbar;

    // Instantiate DUT
    t_flipflop uut (
        .T(T),
        .clk(clk),
        .Q(Q),
        .Qbar(Qbar)
    );

    // Clock generation
    initial begin
        clk = 1'b0;
        forever #5 clk = ~clk;
    end

    // Test cases
    initial begin
        T = 0;

        // Hold
        #10;
        T = 1;

        // Toggle
        #10;
        T = 1;

        // Toggle
        #10;
        T = 0;

        // Hold
        #10;
        T = 1;

        // Toggle
        #10;
        T = 0;

        // Hold
        #10;
        $finish;
    end

    // Display output
    initial begin
        $monitor("Time=%0t | CLK=%b | T=%b | Q=%b | Qbar=%b",
                 $time, clk, T, Q, Qbar);
    end

endmodule
```

# SIMULATION OUTPUT
<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/e2219fcf-f7f1-4f42-8cff-2f2a9fc83bb7" />

# RESULT
All flip-flops (SR, D, JK, T) were successfully simulated using Non blocking statements in Verilog HDL. The outputs matched the expected truth table values, demonstrating correct sequential behavior.

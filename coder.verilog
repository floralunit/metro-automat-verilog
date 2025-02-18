module coder
(input wire [2:0] data,
output wire [6:0] seg);
reg [6:0] code;
assign seg=code;
always @*
case(data)
3'b000: code = 7'b1000000; // 0
3'b001: code = 7'b1111001; // 1
3'b010: code = 7'b0100100; // 2
3'b011: code = 7'b0110000; // 3
3'b100: code = 7'b0011001; // 4
3'b101: code = 7'b0010010; // 5
3'b110: code = 7'b0000010; // 6
3'b111: code = 7'b1111000; // 7
default: code = 7'b1111111;
endcase
endmodule
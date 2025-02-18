module count_div
#(parameter N=29,M=29'd25000000)
//#(parameter N=2,M=2'd2)
(input wire clk,
output reg sync=0);
reg [N-1:0]cnt=0;
wire [N-1:0]cnt_next;
assign cnt_next=cnt+1'b1;
always@(posedge clk)
begin
cnt<=cnt_next;
sync<=sync;
if (cnt==M-1)
begin
cnt<=0;
sync<=~sync;
end
end
endmodule
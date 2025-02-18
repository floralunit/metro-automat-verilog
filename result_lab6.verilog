module result_lab6 (
    input wire clk_h,
    input wire reset,
    input wire card_inserted,
    input wire [2:0] balance,
    output wire [6:0] hex
);
    wire clk_lw;
    wire [2:0] out;
    
    count_div block1(.clk(clk_h), .sync(clk_lw));
    
    metro_work block2(
        .clk(clk_lw),
        .reset(reset),
        .card_inserted(card_inserted),
        .balance(balance),
        .out(out)
    );

    coder block3(.data(out), .seg(hex));
endmodule
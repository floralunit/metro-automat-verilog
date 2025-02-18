module metro_work (
    input wire clk,
    input wire reset,
    input wire card_inserted,
    input wire [2:0] balance,
    output reg [2:0] out
);
    reg [2:0] state;
    reg [2:0] cnt;
   reg [2:0] fare=3'd1;
   reg [2:0] balance1;

    // Определение состояний
    parameter S0 = 3'b000; // Начальное состояние
    parameter S1 = 3'b001; // Ожидание карты
    parameter S2 = 3'b010; // Чтение карты
    parameter S3 = 3'b011; // Индикация состояния карты
    parameter S4 = 3'b100; // Вход разрешен

    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= S0;
            
        end else begin
            case (state)
                S0: begin
                    state <= S1; // Переход в состояние ожидания карты
              

                end
                
                S1: begin
        
                    if (card_inserted) begin
                        state <= S2;  // Переход в состояние чтения карты
                    end
                end
                
                S2: begin
                    if (cnt == 3'd2)  // Держим состояние 2 такта
                        state <= S3; // Переход в состояние индикации состояния карты
                   
                         
                    end
               
                
                S3: begin
                   
                        if (balance >= fare) 
                            state <= S4; // Переход в состояние вход разрешен
                        else 
                            state <= S1; // Переход обратно в ожидание карты, если баланс меньше тарифа
                        end
                   
                
                S4: begin
           if(cnt==3'd4)
                    state <= S1;   // Переход обратно в состояние ожидания карты
          

                end
                
                default: begin
                    state <= S0; // Возврат в начальное состояние по умолчанию
                end
            endcase
        end
    end

    always @(posedge clk) begin
        case (state)
            S0: begin 
        out <= 3'b000;
        cnt <= 2'b00; 
        end  // Начальное состояние
            S1: begin 
        cnt <= 2'b00; 
        out <= 3'b111;
        end // Ожидание карты (индикация "_")
            S2:
          begin
        cnt <= cnt + 1'b1;
        out <= 3'b000;
          if(cnt==3'd2)
          begin
          balance1<=balance;
          end
          end
          
          // Чтение карты (индикация "__")
            //S3:  // Индикация остатка на балансе после вычета тарифа
            S4: begin 
        out <= 3'b001;
        cnt<=cnt+1;
        if (cnt==3'd4);
        out <= balance1 - fare;
        end  // GO - турникет открыт
          
        endcase
    end
endmodule
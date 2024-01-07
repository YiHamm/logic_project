//2024.1.7
//邏設期末project——倉庫番
//僅有計時與地圖切換顯示

//1秒CLK
module divfreg_1(
	input  CLK,
	output reg CLK_div_1
);

	reg [24:0]	count=25'b0;
	always @(posedge CLK)
		begin
			if(count>25000000)
				begin
					count<=25'b0;
					CLK_div_1<=~CLK_div_1;
				end
			else 
				count<=count + 1'b1;
		end
endmodule

//0.001秒CLK
module divfreg_1000(
	input  CLK,
	output reg CLK_div_1000
);

	reg [24:0]	count=25'b0;
	always @(posedge CLK)
		begin
			if(count>25000)
				begin
					count<=25'b0;
					CLK_div_1000<=~CLK_div_1000;
				end
			else 
				count<=count + 1'b1;
		end
endmodule

//時間顯示&計時
module showtime_3(
	input 	  CLK_div_1,CLK_div_1000,
	output reg [3:0] count_01,count_10,count_11,
	output reg [2:0] comm,
	output reg a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,
	output reg [0:6] seg 
);
	/*可嘗試版本二
	always @(posedge CLK_div_1)
		if(count_01 == 4'b0000)	count_01 = 4'b0001;
		else if(count_01 == 4'b0001)	count_01 = 4'b0010;
		else if(count_01 == 4'b0010)	count_01 = 4'b0011;
		else if(count_01 == 4'b0011)	count_01 = 4'b0100;
		else if(count_01 == 4'b0100)	count_01 = 4'b0101;
		else if(count_01 == 4'b0101)	count_01 = 4'b0110;
		else if(count_01 == 4'b0110)	count_01 = 4'b0111;
		else if(count_01 == 4'b0111)	count_01 = 4'b1000;
		else if(count_01 == 4'b1000)	count_01 = 4'b1001;
		else if(count_01 == 4'b1001) begin 
			count_01 = 4'b0000;
			count_10 <= count_10 + 1'b1;
		end*/
	always @(posedge CLK_div_1)
		begin
			if(count_01 == 4'b1001)	begin	
					count_01 <= 4'b0000;
					count_10 <= count_10 + 1'b1;
			end
			else if(count_10 == 4'b0110)	begin	
					count_10 <= 4'b0000;
					count_11 <= count_11 + 1'b1;
			end
			else count_01<=count_01 + 1'b1;
		end	
	always @(count_01)
		case(count_01)
			4'b0000:	{a,b,c,d,e,f,g} = 7'b0000001;
			4'b0001:	{a,b,c,d,e,f,g} = 7'b1001111;
			4'b0010:	{a,b,c,d,e,f,g} = 7'b0010010;
			4'b0011:	{a,b,c,d,e,f,g} = 7'b0000110;
			4'b0100:	{a,b,c,d,e,f,g} = 7'b1001100;
			4'b0101:	{a,b,c,d,e,f,g} = 7'b0100100;
			4'b0110:	{a,b,c,d,e,f,g} = 7'b0100000;
			4'b0111:	{a,b,c,d,e,f,g} = 7'b0001111; 
			4'b1000:	{a,b,c,d,e,f,g} = 7'b0000000; 
			4'b1001:	{a,b,c,d,e,f,g} = 7'b0000100; 
		endcase
	always @(count_10)
		case(count_10)
			4'b0000:	{h,i,j,k,l,m,n} = 7'b0000001;
			4'b0001:	{h,i,j,k,l,m,n} = 7'b1001111;
			4'b0010:	{h,i,j,k,l,m,n} = 7'b0010010;
			4'b0011:	{h,i,j,k,l,m,n} = 7'b0000110;
			4'b0100:	{h,i,j,k,l,m,n} = 7'b1001100;
			4'b0101:	{h,i,j,k,l,m,n} = 7'b0100100;
			4'b0110:	{h,i,j,k,l,m,n} = 7'b0100000;
		endcase
	always @(count_11)
		case(count_11)
			4'b0000:	{o,p,q,r,s,t,u} = 7'b0000001;
			4'b0001:	{o,p,q,r,s,t,u} = 7'b1001111;
			4'b0010:	{o,p,q,r,s,t,u} = 7'b0010010;
			4'b0011:	{o,p,q,r,s,t,u} = 7'b0000110;
			4'b0100:	{o,p,q,r,s,t,u} = 7'b1001100;
			4'b0101:	{o,p,q,r,s,t,u} = 7'b0100100;
			4'b0110:	{o,p,q,r,s,t,u} = 7'b0100000;
			4'b0111:	{o,p,q,r,s,t,u} = 7'b0001111; 
			4'b1000:	{o,p,q,r,s,t,u} = 7'b0000000; 
			4'b1001:	{o,p,q,r,s,t,u} = 7'b0000100; 
		endcase

  //同時顯示3個7seg
	always @(posedge CLK_div_1000)
		if(comm == 3'b011)
			begin
				seg <= {a,b,c,d,e,f,g};
				comm <= 3'b110;
			end
		else if(comm == 3'b110)
			begin
				seg <= {h,i,j,k,l,m,n};
				comm <= 3'b101;
			end
		else
			begin
				seg <= {o,p,q,r,s,t,u};
				comm <= 3'b011;
			end
endmodule 

module final_project(
	output reg [0:7] data_r,data_b,data_g,
	output reg [3:0] comm,
	output reg [2:0] comm2,
	output reg [0:6] seg,
	output reg [3:0] count_01,count_10,count_11,
	output reg a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,
	input  			  CLK,map
);

	wire CLK_div_1,CLK_div_1000;
	divfreg_1    F0(CLK,CLK_div_1);
	divfreg_1000 F1(CLK,CLK_div_1000);
	
	showtime_3 F2(CLK_div_1,CLK_div_1000,
					  count_01,count_10,count_11,
	              comm2,
	              a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,
	              seg
	);
	//地圖
	reg[1:0]	now_map,next_map;
	parameter S0 = 2'b00, S1 = 2'b01, S2 = 2'b10, S3 = 2'b11;
	
	always @(posedge CLK_div_1)
		if(map == S3)	now_map <= S0;
		else				now_map <= next_map;
	
	//按一下地圖換一個，循環
	always @(now_map,map)
		case(now_map)
			S0:	if(map)	next_map = S1; else next_map = S0;
			S1:	if(map)	next_map = S2; else next_map = S1;
			S2:	if(map)  next_map = S3; else next_map = S2;
			S3:	if(map)	next_map = S0; else next_map = S3;
		endcase	
		
  
  //每張地圖的三個顏色
	parameter logic [0:7] map0_r [0:7] =
		'{8'b00000000,
		  8'b00111110,
		  8'b01111110,
		  8'b01000000,
		  8'b01111110,
		  8'b00001000,
		  8'b01111110,
		  8'b00000000};
	parameter logic [0:7] map0_g [0:7] =
		'{8'b00000000,
		  8'b00111100,
		  8'b01111100,
		  8'b01000000,
		  8'b01111100,
		  8'b00001000,
		  8'b01111100,
		  8'b00000000};
	parameter logic [0:7] map0_b [0:7] =
		'{8'b00000000,
		  8'b00111010,
		  8'b01111100,
		  8'b01000000,
		  8'b01111100,
		  8'b00001000,
		  8'b01111100,
		  8'b00000000};
		  
	parameter logic [0:7] map1_r [0:7] =
		'{8'b00000000,
		  8'b01111110,
		  8'b00010110,
		  8'b01010110,
		  8'b01110010,
		  8'b01110110,
		  8'b00011110,
		  8'b00000000};
	parameter logic [0:7] map1_g [0:7] =
		'{8'b00000000,
		  8'b01111100,
		  8'b00010110,
		  8'b00010110,
		  8'b01110010,
		  8'b01110010,
		  8'b01011110,
		  8'b00000000};
	parameter logic [0:7] map1_b [0:7] =
		'{8'b00000000,
		  8'b01111010,
		  8'b00010110,
		  8'b00010110,
		  8'b01110010,
		  8'b01110010,
		  8'b01011110,
		  8'b00000000};
		  
	parameter logic [0:7] map2_r [0:7] =
		'{8'b00000000,
		  8'b01111110,
		  8'b01100000,
		  8'b01111110,
		  8'b01010110,
		  8'b00000010,
		  8'b01111100,
		  8'b00000000};
	parameter logic [0:7] map2_g [0:7] =
		'{8'b00000000,
		  8'b01111100,
		  8'b01100000,
		  8'b01111110,
		  8'b00000110,
		  8'b00000010,
		  8'b01111110,
		  8'b00000000};
	parameter logic [0:7] map2_b [0:7] =
		'{8'b00000000,
		  8'b01111010,
		  8'b01100000,
		  8'b01111110,
		  8'b00000110,
		  8'b00000010,
		  8'b01111110,
		  8'b00000000};

	parameter logic [0:7] map3_r [0:7] =
		'{8'b00000000,
		  8'b00111110,
		  8'b01011000,
		  8'b01111110,
		  8'b01101110,
		  8'b01011010,
		  8'b00111100,
		  8'b00000000};
	parameter logic [0:7] map3_g [0:7] =
		'{8'b00000000,
		  8'b00111100,
		  8'b00011000,
		  8'b01111010,
		  8'b01101110,
		  8'b01011010,
		  8'b01111000,
		  8'b00000000};
	parameter logic [0:7] map3_b [0:7] =
		'{8'b00000000,
		  8'b00111010,
		  8'b00011000,
		  8'b01111010,
		  8'b01101110,
		  8'b01011010,
		  8'b01111000,
		  8'b00000000};
		  
	bit [2:0] array_column;
	initial
		begin 
			array_column = 0;
			data_r = 8'b11111111;
			data_g = 8'b11111111;
			data_b = 8'b11111111;
			comm = 4'b1000;
		end

  //地圖顯示
	always @(posedge CLK_div_1000)
		begin
			if(array_column >= 8)	array_column = 0;
			else 				array_column = array_column + 1;
			if(now_map == S0)	
				begin
					comm = {1'b1,array_column};
					data_r = map0_r[array_column];
					data_b = map0_b[array_column];
					data_g = map0_g[array_column];
					//加入move
				end
			else if(now_map == S1)	
				begin
					comm = {1'b1,array_column};
					data_r = map1_r[array_column];
					data_b = map1_b[array_column];
					data_g = map1_g[array_column];
				end
			else if(now_map == S2)	
				begin
					comm = {1'b1,array_column};
					data_r = map2_r[array_column];
					data_b = map2_b[array_column];
					data_g = map2_g[array_column];
				end
			else	
				begin
					comm = {1'b1,array_column};
					data_r = map3_r[array_column];
					data_b = map3_b[array_column];
					data_g = map3_g[array_column];
				end
		end		
									
endmodule

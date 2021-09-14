

�ο��̳̣�https://www.chipverify.com/verilog/verilog-tutorial

������ϰ&���棺https://hdlbits.01xz.net/wiki/Problem_sets

# Verilog���

Verilog��һ��Ӳ���������ԣ�Hardware Description Language��HDL���������﷨��C�������ƣ����Ǳ�������ȫ��ͬ�ģ�C���Ա������˳��ִ�е�ָ���Verilog�ۺϣ�synthesize������һ����·�������ԴӲ�ͬ���󼶱�����һ��Ӳ����·��

| level                                  | description                                                  |
| -------------------------------------- | ------------------------------------------------------------ |
| Switch Level�����ؼ�                   | ���Ƹ���mos����ʵ�ֹ���                                      |
| Gate Level���ż�                       | �ø�����ʵ��                                                 |
| Register Transfer Level��RTL���������� | �����Ĵ�����������һ����assign�͸���wireʵ�֣�Ҳ��˵�������ۺϵĶ����Խ���RTL�� |
| Behavior Level����Ϊ��                 | ����ϵͳ�㷨��Ϊ����Ҫ��always��ʵ��                         |

Verilog�ǲ��Ϸ�չ�����ԣ����бȽ���Ҫ����Verilog-1995��Verilog-2001��������׼

# �������ʶ��

�����ı�ʾ��ʽ��`[size]'[base_format][number]`

```verilog
8'b0000_0000
9'h1FA
-6'd3           // ����
"Hello, world!" // �ַ�����ascii��һ���ַ�1�ֽ�

// ���ų���
parameter MSB = 7;

// �������parameter��ģ��
module counter
    #(parameter N = 2) // ���IO����Ҫ��������module�ڲ�����parameter��Ч��һ��
    (
        input clk, rst_n, en,
        output reg[N-1:0] out
    );
    
    always @(posedge clk) begin
        // ����
    end
endmodule

// ʹ�ô���parameter��ģ��
counter #(.N(4)) c0 (.clk(clk), .rst_n(rst_n), .en(en));
```

��ʶ�������֡���ĸ�����ִ�Сд�����»��ߡ�dollar����`$`��ɣ�������dollar�������ֿ�ͷ�������뱣���ؼ�������

# ��������

Verilog��������Ӳ��������������͵Ķ���͸߼����Խ�Ȼ��ͬ

## Net

Net�������������ߵĳ��������ܴ���ֵ��ȡֵȡ��������������·��Driver��������wire����Ϊ���õ�Net���ͣ�wire��Net֮����ʱ���ϸ�����

�����г�����Net���ͣ�

| name      | description                                        |
| --------- | -------------------------------------------------- |
| wire      | ��ͨ����                                           |
| tri       | �ж�����������ߡ���wire��ȫһ������������߿ɶ��� |
| wand, wor | ���루wire and�����߻�wire or��                  |

Net������assign��������ߣ�Variable���У���������<span style="background-color:#FFFF00">assign���ʵ������߼���·</span>

```verilog
wire i1, i2;
wire out;
wire [1:0] data;

assign out = i1 & i2;
assign data = {i2, i1};

// �����һ��net�Ӷ������Դ������ͬ����Դ�ĵ�ƽ��ͬʱ���ߵĵ�ƽΪX
assign out 1'b1;   // ���i1 & i2 == 0��out = X

// implicit assignment
wire out_implicit = i1 & i2;
```

## Variable

Variable��һ���ܴ������ݵĵ�·�ĳ��󣬱���D����������ʱҲ����register����register variable

| name    | description              |
| ------- | ------------------------ |
| reg     | short for register       |
| integer | 32 bits wide             |
| time    | 64 bits wide             |
| real    | floating point�������ۺ� |

Variable��<span style="background-color:#FFFF00">������ֵ���������ֵ</span>�������ӷ�ʽ

```verilog
a <= c;   // non blocking����ͬ��������ֵ����ִ�У�˳������ν
b = c;    // blocking����һ��ִ����֮ǰ����Ĳ���ִ�У�������һ��Ľ����Ӱ��֮���ֵ��
```

* <span style="background-color:#FFFF00">ʱ���·��ʹ�÷�������ֵ</span>

```verilog
always @(posedge clk) begin
    a <= ~a;
    b <= a;
end
```

<img src="./images/verilog_non-blocking.png" style="zoom: 33%;" />

�ɼ�ʱ���������ص���ʱb��D���뻹����һ�����ڵ�a�������ʼ����`b == ~a`

* <span style="background-color:#FFFF00">����߼���·��ʹ��������ֵ</span>

```verilog
always @(*) begin
    a = a & in;
    b = a;
end
```

<img src="./images/verilog_blocking.png" style="zoom:33%;" />

���Υ����������ԭ�򣬱�����ʱ���߼���·��ʹ��������ֵ`a = ~a; b = a;`���ۺϺ�ͱ��a����һ����������b��D�ˣ����Ƚϸ��ӵ�ʱ������һ�������������߼��� / ���ұ�������߼�ʹ�÷�������ֵ������һ��always���л��ʹ�����ָ�ֵͬ��

## Scalar, Vector and Array

Scalar�ǿ��Ϊ1bit�����ݣ�Vector�ǿ�ȴ���1bit�����ݣ�Array�Ƕ�����ݵ�����

```verilog
wire clk;        // ����wire scalar
reg [7:0] addr;  // ����reg vector������������[msb:lsb]
reg y [11:0];    // ����scalar reg array���������Ϊ12��ÿ��1bit��
reg [7:0] mem [1:0][3:0]  // ����vector reg array��2��4�У�ÿ��8bit��

addr[0] = 1;     // ����msb
addr[7:4] = 4'hF // part-select
y[11] = 1'b1;    // ����Array��ע��scalar array�������帳ֵ��vector����
mem[0][0] = 8'haa;  // ��0��0��Ԫ�ظ�ֵ0xAA
```

# ģ��

<span style="background-color:#FFFF00">���ۺϵĴ��붼Ҫ������module��</span>��module������һ�������ض��߼����ܵĵ�·ģ�飬�������������������˿ڣ�Port���Լ��ڲ����߼���·��������һ��ͬ�������D������module��

```verilog
module dff(d, clk, rst_n, q);
    // ����IO
    input d;
    input clk;
    input rst_n;
    output reg q;

    // �߼�����
    always @ (posedge clk) begin
        if (!rst_n)
            q <= 0;
        else
            q <= d;
    end
endmodule
```

<img src="./images/verilog_dff_sync_schematic.png" style="zoom:67%;" />

һ��module�п��԰���������module������������һ��4bit��λ�Ĵ�����

```verilog
module shift_reg(
    // Ҳ�����ڿ�ͷ��������port��IO��������verilog 2001�Ĺ涨
    input d,
    input clk,
    input rst_n,
    output q
);
    // �����ڲ�����
    wire [2:0] q_net;

    // ʵ����(instantiate)D������
    dff u0 (d, clk, rst_n, q_net[0]);   // Port connection by ordered list
    dff u1 (.d(q_net[0]), .clk(clk), .rst_n(rst_n), .q(q_net[1]));  // by name
    dff u2 (.d(q_net[1]), .clk(clk), .rst_n(rst_n), .q(q_net[2]));
    dff u3 (.d(q_net[2]), .clk(clk), .rst_n(rst_n), .q(q));
endmodule
```

<img src="./images/verilog_dff_shift_reg_schematic.png" style="zoom:60%;" />

�˿�����ʱ����������`.name()`�ķ�ʽ��ʾ��������

`input`��`inout`���͵Ķ˿�Ҫ���ⲿ��·��������˲�����reg����

����ʱ������һ��top-level module��������ϵͳ������ģ�鲻�ᱻ������ģ��ʵ������ģ�����ʱʹ�õ�testbenchҲ��һ�ֶ���ģ��

# �����

��C�������������ͬ���ر��ڴ��г�λ�����

| Symbol     | Performance |
| ---------- | ----------- |
| &          | ��          |
| \|         | ��          |
| ~          | ��          |
| ^          | ���        |
| `<<`, `>>` | ���ơ�����  |

λ������������ʹ�ã�����`~&`��ǣ�`~^`ͬ��

��֮ͬ����==, !=����x, zʱ���Ϊx; ===�� !==Ҫ����ȫһ��
λƴ������� {1'b0, 1'b1}, {var_1, 4{var_2}, 2'b10}����1���λ2'b01����2�н�var_1, 4��var_2, 2'b10ƴ�ӵ�һ�𣨱�������λ����
�����������&B����һλ��ڶ�λ�룬��������λ�룬�������ֱ�����һλ

# ������

if, while, forͬC���ԣ�x��z���ж�Ϊ��

case ����c��switch������û��break����Ϊ��·�����ܴ�һ����������������
casez ���ԣ����Ƚ�)z
casex ����z, x

```verilog
case (something)
    value: begin
        //do something while "something==value"
    end
    othervalue: begin
        //do something while "something==othervalue"
    end
    default: begin
        //do something while "something" is none of the above
    end
endcase
```

forever ���       ����ѭ��������д��initial��
repeat(����) ���  �ظ�ָ������

# Always Block

���ź�����ʱ���ᴥ����<span style="background-color:#FFFF00">block�е����˳����У���ͬ��block����ִ��</span>���������������Ǳ������У���������߸����䣩���ߵ�ƽ���У���ƽ�仯�����������е��ź��Ǳ������С��е��ź��ǵ�ƽ����

```verilog
module tff(input d, input clk, input rst_n, output reg q);
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n)
            q <= 1'b0;
        else
            q <= q;
    end
endmodule
```

������always blockʵ������߼���·��

```verilog
module combo (input a, b, c, d, e,
              output reg z);
    always @ ( a or b or c or d or e) begin
        z = ((a & b) | (c ^ d) & ~e);
    end
endmodule
```

<img src="./images/verilog_combo_with_always.png" style="zoom: 33%;" />

��ͼ��Quartus�ۺϵĽ����reg����z�ۺ�Ϊ����߼�������������ǼĴ������ߴ�������ʹ��Vivado�ۺϣ��õ�����һ�����ұ����������Ŀ��Ӳ�����ۺϽ����Ż����в�ͬ��

# Generate Block

Ӧ����һ������ĺ꣬�ۺϵ�ʱ��չ���ɺ��ʵĽṹ

```verilog
genvar i;   // ����generate variable���ۺϺ�����������
generate
    for (i=0; i < 8; i++) begin
        // do something, e.g. instanciate an array of submodules
    end
    
    // Ҳ������generate if��generate case��������Ϊgenerate��չ��֮���ǲ����ڵģ�
    // Ӧ��ֻ����parameter��Ϊ����
endgenerate
```

# ����

## testbench��д

һ�㶨��һ��ר�����������testbench����ģ�飬��������������ź��ṩ�������Ե�ģ�飬�۲��������Ƿ���ȷ�������ֻ���ڷ���ʱ���еĲ����������г����õļ���

```verilog
module testbench();
    wire clk, foo, bar;             // ��������ʱ��Ҫ�õ��ı���
    some_sub_module sub(foo, bar);  // �������ģ��
    
    always #10 clk = ~clk;

    // initial block���ڷ��濪ʼ/Ӳ���ϵ�ʱ����һ��
    // ��������˼Ĵ�����ʼ����������ݾͲ������ۺϣ��������Ҫ��;�Ƿ���
    initial begin
        clk <= 0;        // ����ʱ������������ʾ��ֵ���������������ֵ�ͻ���X
        foo <= 0;
        bar <= 0;
        #10 foo = 1;     // ��ʽ��ʱ�������ۺ�
        #20 bar = 1;
        #400 $finish;    // ��������
    end

    always @(posedge clk) begin
        $display("%d", foo);
        $wrirte("%d", bar);  // display�������Զ����У�write����
    end
endmodule
```

## ModelSimʹ��

1. File - New - Library��ѡ���ļ��У�֮�����������ļ�����������Ҳ���Ը������������ȷ��֮������ļ��л���ʾ��Library���
2. Compile - Compile��ѡ��������Ҫ�����Դ�ļ�������ɹ�֮��Library����»������Ǳ������module
3. ˫��top-level module�����Wave��壬Sim����Objects���
4. ��Objects���ѡ����Ҫ�鿴�Ĳ��Σ���ӵ�Wave��壬Simulate - Run
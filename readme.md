一、dll库包含
2个对外函数：
1）函数1：初始化保温材料参数组，9个参数。
2）函数2：根据指定的方法计算管道保温，24个参数
1个内部数组：
1）数组1：保温材料的8个参数，前5个参数是导热率计算公式a1*（T+a2)^2+b1*(T+b2)+c的5个系数，后面是材料使用密度(kg/m^3), 材料需用温度(℃), 材料单价(元/m^3)

二、使用说明：
1）导入dll库，导入2个函数；
2）使用函数1依次导入3层保温材料的参数，参数将保存在数组1中。
3）使用函数2计算得到保温结果。
4）如果无需更换保温材料回到步骤3，如果需要更换保温材料回到步骤2。

三、dll库数组和函数声明：
//	数组1：保温材料的8个参数，前5个参数是导热率计算公式a1*（T+a2)^2+b1*(T+b2)+c的5个系数，后面是材料使用密度(kg/m^3), 材料需用温度(℃), 材料单价(元/m^3)
double InsRef[3][8] ={
    {0,0,0,0,0,0,0,0},
    {0,0,0,0,0,0,0,0},
    {0,0,0,0,0,0,0,0}
};

// 	函数1：初始化保温材料参数组，9个参数
__declspec(dllexport) void __stdcall InitInsRef(int layer, double ref0, double ref1, double ref2, double ref3, double ref4, double ref5, double ref6, double ref7);

//	函数2：根据指定的方法计算管道保温，18+6个参数
//	Method(计算模式，1为已知保温厚度，2为已知保温分界面温度，3为已知管线散热量和内层保温分界面温度)
//	LayerNum(保温层数，取1-3)
//	PipeOD(管道外径，单位mm)，PipeTemp（管道外表面温度，单位℃）
//	Ins1(最里层保温材料编号)，Thk1(最里层保温厚度，单位mm)，Temp1(最里层保温外表面温度，单位℃)，2代表第二层（如果有），3代表第三层（如果有）
//	EveTemp(环境温度，单位℃), WindSpeed(风速，单位m/s), CoverBlack(外护板黑度)
//	qArea(管道最外层单位面积散热功率，W/m^2), qLine(管道单位长度散热功率，W/m)
//	HeatPrice(热价，元/GJ), ExergyFactor(介质火用系数), CoverPrice(外护板单价，元/m^2), APR(年利率(复利)), YearNum(计息年数), AnnualServiceHours(年运行小时数)
__declspec(dllexport) int __stdcall InsCalc(int Method, int LayerNum, double PipeOD, double PipeTemp,
				   int Ins1, double* Thk1, double* Temp1,
				   int Ins2, double* Thk2, double* Temp2,
				   int Ins3, double* Thk3, double* Temp3,
				   double EveTemp, double WindSpeed, double CoverBlack, 
				   double* qArea, double* qLine, 
				   double HeatPrice, double ExergyFactor, double CoverPrice, double APR, double YearNum, double AnnualServiceHours);

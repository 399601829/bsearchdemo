作业要求：依据项目需求，编写MapReduce程序，实现收视率相关指标的统计分析，可以参考以下步骤

第一步：解析用户原始数据。(map)

第二步：统计每个节目每分钟的收视人数和到达人数。（combiner）

第三步：统计所有节目每分钟当前在播人数。（combiner输出）

第四步：统计每个节目每分钟的收视人数、到达人数、收视率、到达率、市场份额。（reduce）

-------------------------------------------------
iThink(v.1):
1.解析原始数据这一步已经完成
	将原数据解析成	机顶盒编号	时间（年月日）	结束时间	开始时间	频道名称
	
	
2.将这些原始数据压缩，这一步是现在做，还是做好之后再回来做 
	全部结束之后再做 #实战项目：压缩作业输出结果和统计脏数据#
		
//3.统计每个节目每分钟的收视人数和到达人数
	将每个节目的   stbNum@name@time@分钟 -> 123456@发现之旅@2012-09-16@23:56 作为key
	count 即为收视人数
	每分钟的到达人数，即为这1分钟内的人数-在这个频道停留时间没有超过60s的人数
	
//4.统计所有节目每分钟当前在播人数。（combiner输出）
	stbNum@name@time@分钟 -> 123456@发现之旅@2012-09-16@23:56 作为key
	values.size()	为收视率
	结束时间-开始时间<60秒	到达人数
	|-> value就是这个时间内（23:56） 内的收视人数和到达人数 12@12

-------------------------------------------------
iThink(v.2):
1.解析原始数据这一步已经完成
	将原数据解析成	机顶盒编号	时间（年月日）	结束时间	开始时间	频道名称
	
	
2.将这些原始数据压缩，这一步是现在做，还是做好之后再回来做 
	全部结束之后再做 #实战项目：压缩作业输出结果和统计脏数据#
	
	
//3.统计每个节目每分钟的收视人数和到达人数
	第一次筛选的时候力度要粗，提高对单个力度筛选的灵活性
	|-> key = stbNum + "@" + date
	|-> value = sn + "@" + p+ "@" + s + "@" + e + "@" + duration
	
//4.继续构建一个map，统计每个节目每分钟的收视人数和到达人数
	|-> key = 节目名@日期@时间 -> discover@2012-09-16@23:56
	|-> value = stbNum@已经播放的秒数 -> 123456@47
	
-------------------------------------------------
iThink(v.3):
1.解析原始数据这一步已经完成
	将原数据解析成	机顶盒编号	时间（年月日）	结束时间	开始时间	频道名称
	
	
2.将这些原始数据压缩，这一步是现在做，还是做好之后再回来做 
	全部结束之后再做 #实战项目：压缩作业输出结果和统计脏数据#
		
3.统计每个节目每分钟的收视人数和到达人数
	将每个节目的  name@time@分钟 -> 发现之旅@2012-09-16@23:56 作为key
	count 即为收视人数
	每分钟的到达人数，即为这1分钟内的人数-在这个频道停留时间没有超过60s的人数 
	
4.统计所有节目每分钟当前在播人数。（combiner输出）
	name@time@分钟 -> 发现之旅@2012-09-16@23:56 作为key
	values.size()	为收视人数
	set_reachnum.size() 为到达人数
	|-> value就是这个时间内（23:56） 内的收视人数和到达人数 12@12
	
#缘觉时刻#-1： map就是分，reduce就是合。正所谓分久必合合久必分，此可谓之。

	
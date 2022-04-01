版本日志
===========

0.2.1
---------
*发布时间: 2022-03-02*

* 修复了Windows系统中GBK编码无法加载数据的问题。

0.2.0
---------
*发布时间: 2022-02-16*

* 增加了对pcolormesh图的裁剪支持。
* 修复了边界错误的问题。

0.1.11
---------
*发布时间: 2022-02-14*

* 尝试修复安装时出现gbk编码异常的问题。

0.1.10
---------
*发布时间: 2022-02-13*

* 增加功能: cnmaps.get_map函数: 获取地图。
* 增加功能: cnmaps.draw_map函数: 绘制地图。
* 增加功能: cnmaps.MapPolygon类: 地图对象, 包括: 加号(合并)、减号(剪切)、逻辑与(交集)运算符的支持，get_extent方法。
* 增加功能: cnmaps.clip_contours_by_map函数: 基于MapPolygon类对等值线图做裁减。
* 增加功能: cnmaps.sample.load_dem函数: 加载dem样例数据。
* 增加功能: cnmaps.clip_clabels_by_map函数: 基于MapPolygon类对标签做裁减。
* 对cartopy.crs各类投影的支持。
* 对全国中国国界、全国各省(特区/直辖市)地图的预置, 且处理了已知的拓扑错误。
* 集成了travis CI自动化测试。
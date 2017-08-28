---
layout: post
title:  "[V]-最近距离"
date:   2013-10-07 20:00:20 +0800
categories: vyouyue
---
所有商户都有GPS坐标。当给定一个坐标，计算出离给定坐标最近的10个商户，并可再次搜索（再搜时需排除原已搜过的）。

提取算法，数据结构，缓存，存储方式，设计思想？


<table class="table table-striped">
	<tr>
		<td>FIELD</td>
		<td>TYPE</td>
		<td>COLLATION</td>
		<td>NULL</td>
		<td>KEY</td>
		<td>DEFAULT</td>
		<td>Extra</td>
		<td>PRIVILEGES</td>
		<td>COMMENT</td>
	</tr>	
	<tr>
		<td>testId</td>
		<td>INT(11)</td>
		<td>(NULL)</td>
		<td>NO</td>
		<td>PRI</td>
		<td>(NULL)</td>
		<td>AUTO_INCREMENT</td>
		<td>SELECT,INSERT,UPDATE,REFERENCES</td>
		<td>编号</td>
	</tr>		
	<tr>
		<td>testName</td>
		<td>VARCHAR(64)</td>
		<td>utf8_general_ci</td>
		<td>NO</td>
		<td></td>
		<td>(NULL)</td>
		<td></td>
		<td>SELECT,INSERT,UPDATE,REFERENCES</td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td>testLongitude</td>
		<td>DOUBLE</td>
		<td>(NULL)</td>
		<td>YES</td>
		<td></td>
		<td>(NULL)</td>
		<td></td>
		<td>SELECT,INSERT,UPDATE,REFERENCES</td>
		<td>经度</td>
	</tr>                                
	<tr>
		<td>testLatitude</td>
		<td>DOUBLE</td>
		<td>(NULL)</td>
		<td>YES</td>
		<td></td>
		<td>(NULL)</td>
		<td></td>
		<td>SELECT,INSERT,UPDATE,REFERENCES</td>
		<td>纬度</td>
	</tr>	
	<tr>
		<td>testLatitude</td>
		<td>DOUBLE</td>
		<td>(NULL)</td>
		<td>YES</td>
		<td></td>
		<td>(NULL)</td>
		<td></td>
		<td>SELECT,INSERT,UPDATE,REFERENCES</td>
		<td>纬度</td>
	</tr>                                 
	<tr>
		<td>createTime</td>
		<td>DATETIME</td>
		<td>(NULL)</td>
		<td>NO</td>
		<td></td>
		<td>(NULL)</td>
		<td></td>
		<td>SELECT,INSERT,UPDATE,REFERENCES</td>
		<td></td>
	</tr>
</table>


### 获取某经纬度附近dist km的商户信息（肯定要区分商户类别, 经纬度，开始记录，记录数量，范围距离）
```SQL
DROP PROCEDURE IF EXISTS pro_getNearUser
CREATE PROCEDURE pro_getNearUser(IN mylon DOUBLE,IN mylat DOUBLE,IN begin INT,IN counts INT,IN dist INT)  
BEGIN  
	DECLARE mylon DOUBLE;  
	DECLARE mylat DOUBLE;   
	DECLARE lon1 FLOAT;  
	DECLARE lon2 FLOAT; 
	DECLARE lat1 FLOAT; 
	DECLARE lat2 FLOAT;  
	-- calculate lon and lat for the rectangle:
	SET lon1 = mylon-dist/ABS(COS(RADIANS(mylat))*69);  
	SET lon2 = mylon+dist/ABS(COS(RADIANS(mylat))*69); 
	SET lat1 = mylat-(dist/69);   
	SET lat2 = mylat+(dist/69);
	-- run the query:
	SELECT  merchantId,3956 * 2 * ASIN(SQRT(  POWER(SIN((orig.userLon - dest.userLat) * PI()/180 / 2), 2) +  
		COS(orig.userLat * PI()/180) * COS(dest.userLat * PI()/180) * 
		POWER(SIN((orig.userLon -dest.userLon) * PI()/180 / 2), 2)  )) AS distance 
	FROM  users dest,  users orig 
	WHERE  dest.userLon BETWEEN lon1 AND lon2  AND  dest.userLon BETWEEN lat1 AND lat2 
	HAVING distance < dist 
	ORDER BY distance 
	LIMIT begin,counts;  
END
```
---
layout: post_cn
title: "价格计算过程中的小计"
categories: SAP
tag: abap
date: 2014-07-10 23:34:41
---


在价格计算过程中根据需要更改小计的业务逻辑。
位置：SAPLV61A program  FORM XKOMV_BEWERTEN (Include LV61AA55):

	{% highlight abap %}
* L O O P
LOOP AT xkomv.
	...
	* calculate subtotals in KOMP
	IF xkomv-kinak NA 'AKLMXZ'.
		...
		* fill subtotal work fields
		CASE xkomv-kzwiw.        <== The setting for KZWIW
			WHEN ' '.                  is checked in this
			
			WHEN 'A'. CASE statement.
			...
			WHEN 'B'.
			...
			 WHEN '1'.
*{   REPLACE        自己添加的逻辑，在category为5的时候，把基础值传入累积数                                        1
*\          ADD xkomv-kwert TO komp-kzwi1.
		  IF xkomv-kntyp EQ '5'.
			ADD xkomv-kawrt TO komp-kzwi1.
		  ELSE.
			ADD xkomv-kwert TO komp-kzwi1.
		  ENDIF.
*}   REPLACE
			WHEN ...
			...
		ENDCASE.
		...
	ENDIF.
	...
* L O O P
ENDLOOP.
  {% endhighlight %}

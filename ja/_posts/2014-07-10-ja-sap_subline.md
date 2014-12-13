---
layout: post_cn
title: "価格計算の小計ロジックの変更"
categories: SAP
tag: abap
date: 2014-07-10 23:34:41
---


要件により価格決定表にある小計値のロジックを変更する必要があり、下記ようにロジックを変更しました。
普段条件レートと条件金額しか累積できないが、今回の要件で条件基礎値も累積できるように変更：

コード位置：SAPLV61A program  FORM XKOMV_BEWERTEN (Include LV61AA55):

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
*{   REPLACE        ロジックを変更、条件カテゴリが5の場合、条件金額ではなく、条件基礎値を小計値に計上                                        1
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

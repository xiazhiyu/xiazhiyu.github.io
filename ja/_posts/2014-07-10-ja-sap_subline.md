---
layout: post_ja
title: "価格計算の小計ロジックの変更"
categories: SAP関連
tag: abap
date: 2014-07-10 23:34:41
---


今回のプロジェクトの要件で、価格決定表にある小計値ロジックを変更する必要があり、下記ように対応しました。<br/>
普段条件レートと条件金額しか累積できないが、今回の条件基礎値も累積できるようにコードを変更：

位置：<br/>
SAPLV61A program  FORM XKOMV_BEWERTEN (Include LV61AA55):

	{% highlight abap %}
* L O O P
LOOP AT xkomv.
	...
	"calculate subtotals in KOMP
	IF xkomv-kinak NA 'AKLMXZ'.
		...
		"fill subtotal work fields
		CASE xkomv-kzwiw.        "<== The setting for KZWIW is checked in this
			WHEN ' '.                  
			
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

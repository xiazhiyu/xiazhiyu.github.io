---
layout: post_cn
title: "A simple program on ALV OOPS"
date: 2016-04-18 21:41:33
categories: SAP
tag: abap
---

Just a test.

A simple program on ALV OOPS.

	{% highlight abap %}
	
*&---------------------------------------------------------------------*
*& Report  Y_FYG_TEST_001
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT Y_FYG_TEST_001.

DATA:
  T_SFLIGHT TYPE TABLE OF SFLIGHT,
  FS_SFLIGHT TYPE SFLIGHT.

DATA :
  T_CUST TYPE REF TO CL_GUI_CUSTOM_CONTAINER, "CL_GUI_DOCKING_CONTAINER,
  T_GRID TYPE REF TO CL_GUI_ALV_GRID .


SELECT * FROM SFLIGHT INTO TABLE T_SFLIGHT UP TO 50 ROWS.

CALL SCREEN 9000.


*&---------------------------------------------------------------------*
*&      Module  STATUS_9000  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE STATUS_9000 OUTPUT.
  SET PF-STATUS 'ZSTATUS'.
  SET TITLEBAR 'ZTITLE'.

 CREATE OBJECT T_GRID
     EXPORTING
*      I_SHELLSTYLE      = 0
*      I_LIFETIME        =
       I_PARENT          = T_CUST
*      I_APPL_EVENTS     = space
*      I_PARENTDBG       =
*      I_APPLOGPARENT    =
*      I_GRAPHICSPARENT  =
*      I_NAME            =
*      I_FCAT_COMPLETE   = SPACE
*    EXCEPTIONS
*      ERROR_CNTL_CREATE = 1
*      ERROR_CNTL_INIT   = 2
*      ERROR_CNTL_LINK   = 3
*      ERROR_DP_CREATE   = 4
*      others            = 5
      .
  IF SY-SUBRC <> 0.
*   MESSAGE ID SY-MSGID TYPE SY-MSGTY NUMBER SY-MSGNO
*              WITH SY-MSGV1 SY-MSGV2 SY-MSGV3 SY-MSGV4.
  ENDIF.

  CALL METHOD T_GRID->SET_TABLE_FOR_FIRST_DISPLAY
     EXPORTING
*      I_BUFFER_ACTIVE               =
*      I_BYPASSING_BUFFER            =
*      I_CONSISTENCY_CHECK           =
       I_STRUCTURE_NAME              = 'SFLIGHT'
*      IS_VARIANT                    =
*      I_SAVE                        =
*      I_DEFAULT                     = 'X'
*      IS_LAYOUT                     =
*      IS_PRINT                      =
*      IT_SPECIAL_GROUPS             =
*      IT_TOOLBAR_EXCLUDING          =
*      IT_HYPERLINK                  =
*      IT_ALV_GRAPHICS               =
*      IT_EXCEPT_QINFO               =
*      IR_SALV_ADAPTER               =
    CHANGING
      IT_OUTTAB                     = T_SFLIGHT
*      IT_FIELDCATALOG               =
*      IT_SORT                       =
*      IT_FILTER                     =
     EXCEPTIONS
       INVALID_PARAMETER_COMBINATION = 1
       PROGRAM_ERROR                 = 2
       TOO_MANY_LINES                = 3
       others                        = 4
          .
  IF SY-SUBRC <> 0.
*   Implement suitable error handling here
  ENDIF.

ENDMODULE.                 " STATUS_9000  OUTPUT
*&---------------------------------------------------------------------*
*&      Module  USER_COMMAND_9000  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE USER_COMMAND_9000 INPUT.
  DATA LV_UCOMM TYPE SY-UCOMM.
  LV_UCOMM = SY-UCOMM.
  CASE LV_UCOMM.
    WHEN 'CANCEl' OR 'EXIT'.
      PERFORM FREE_OBJECTS.
      LEAVE PROGRAM.
    WHEN 'BACK'.
      PERFORM FREE_OBJECTS.
      SET SCREEN '0'.
      LEAVE SCREEN.
  ENDCASE.
ENDMODULE.                 " USER_COMMAND_9000  INPUT
*&---------------------------------------------------------------------*
*&      Form  FREE_OBJECTS
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
*  -->  p1        text
*  <--  p2        text
*----------------------------------------------------------------------*
FORM FREE_OBJECTS .
  CALL METHOD T_GRID->FREE
    EXCEPTIONS
      CNTL_ERROR        = 1
      CNTL_SYSTEM_ERROR = 2
      OTHERS            = 3.
  IF SY-SUBRC <> 0.
    MESSAGE ID SY-MSGID TYPE SY-MSGTY NUMBER SY-MSGNO
               WITH SY-MSGV1 SY-MSGV2 SY-MSGV3 SY-MSGV4.
  ENDIF.
ENDFORM.                    " FREE_OBJECTS	
	{% endhighlight %}


# 활동링크
유튜브 : https://www.youtube.com/@NETNABI  
reddit : https://www.reddit.com/r/netnabi  
NaverCafe : https://cafe.naver.com/netnabi  

# BareMetal
유튜브에 있는 영상들은 전부 Bare Metal 기반으로 구현되었습니다.  
플래시 16K 메모리가 8K 들은 RTOS를 올리고 구현한 수 있는 자원이  
많이 부족하기 때문에 BareMetal 방식으로 사용 합니다.  

실시간 업데이트가 되며 프로젝트 및 개발을 하면서 보완할 생각 입니다.  

# 코딩 문법 
파스칼케이스 (Pascal Case) 
스네이크 케이스(snake_case) 
카멜 케이스 (camelCase)  
혼합 사용 PSC case 로 정의  

기본 파일 구성은 다음과 같습니다.  
base_uart_type.h  (순수 Define 또는 타입만 선언)  
```c

#ifndef TEST_TYPE_H_
#define TEST_TYPE_H_

#define d_TEST_SETUP   0
/// 외부를 참조할경우 base_uart.h에서 선언  
typedef struct
{
  uint32 v_data1;
  sint32 v_data2;
}ts_Test_Base;

typedef  enum
{
  m_TEST_BASE1,
  m_TEST_BASE2
}te_Test_Base;

typedef struct
{
  te_Test_Base e_test_base;
  ts_Test_Base s_test_base;
}ts_Test_Ctrol;

#endif
```
test_type.h ( 다른소스 타입을 사용시에 선언)
```c
#ifndef TSET_H_
#define TEST_H_

#include "camera_type.h" /// 또는 Camera.h 함수 사용시 
#include "test_type.h"

typedef struct
{
  ts_Camera_base s_camera_base;
  ts_Test_bsase s_tgest_base;
}ts_Test_Moulde;

#endif
```
base_uart.c
```c
#include "uart.h"

/// 개발에 먼저 가독성 한번에 식별이 편하게 만들기 위해 규칙을 정함.  
/// 기본적으로 이름만 보고 이게 어디에 생성 되며 무슨 타입인지 식별을 바로 하기위 작성.  

/// 1. 첫번째 접두어는 항상 소문자.
f_Function;   /// f: 함수
e_enum;       /// e: enum 변수
s_struct;     /// s: 구조체
.... 등등 자세한 접두어는 아래 접두어 항목 확인.

/// 1. 대문자로만 작성 되어야 되는 타입 
1.1 #define d_DEFINE 0    /// 정의
1.2 const cv_PI;          /// 상수
1.3 m_ERROR_TYPE;         /// enum 맴버
1.4 AP, SW, FW ;          /// 약어는 대문자로 표기 변수던 뭐던간에

/// 외부 선언시 C 파일이름 을 접두사로 사용 {프로젝트명}_{기능}.c 예시) v_Base_Uart_Len  
extern uint32 xv_base_uart1_flag;     /// extern 전역변수 경우 소스파일을 접두어로 사용하여 출처를 알림   
extern uint32 xv_base_uart2_flag;     /// extern 전역변수 _소문자 사용    
extern uint32 xv_base_uart3_allClean  /// extern 븉어서 쓸경우 중간에 구분은 대분자로 표현  
extern uint32 xa_test_array[29];      /// extren 배열 변수  

static uint32 gv_timer;               /// static 소스파일 범위 변수  {프로젝트명},{기능} 은 생략 할수 있다.  
static uint32 gv_state;               /// static 소스파일 범위 변수  
static sint32* gp_test_point;         /// 소스파일 범위 포인터 변수  
static sint32* gpa_test_array[20];    /// 소스파일 범위 포인터 배열 변수  

/// 함수 또는 Typdedef 문은 _마다 대문자로 표시  
static ts_Uart_Buff_Ctrol  gs_uart1_buff_ctrol;           /// 소스파일 범위 구조체.  
static te_Error_Typde ge_error_type;  

void f_Test_Int(void)
{
  static uint32 lv_Time;  /// 함수 내 static경우 local 의 의미로 lv_접두어를 사용.
  uint32 v_Buff;          /// {프로젝트명}_{기능} 생략 함수내에만 적용 되므로 불필요
{
void f_Test_Module(void)
{
}

```

nn_camera.c 인 경우 코드 내부 말머리는 다음과 같이 작성합니다.  
f_Pbot_Camera_xxx;  
v_Pbot_Camera_xxx;  
이는 함수의 소스파일 이름을 바로 파악 및 이름 충돌을 피함.  
네이밍 방법은 f_파일이름_대분류_소분류  
파일이름 + 대분류는 다음과 같이 _를 제거 해서 사용
하위집단은 _로 구분.
f_PbotCamera_Read_Send();  
f_PbotCamera_Write_Send();  
  
추가로 Extren은 원소스 헤더파일에서만 사용 다른 코드에서 사용 불가.  
이는 스파게티 방지.  다른 소스코드 에서 사용 하려면 헤더파일 인클루드  
  
# 파일 주석 및 코드 접두사 사용 
코드를 한눈에 파악하기 위해 다음과 같이 선언 합니다. 
```c
/******************************************************************************
* File:    base_schedluer.c
* Author:  NETNABI
* Created: 2024-11-10
*
* Description:
* This is the scheduler manager module.
*
* Revision History:
*   2025-05-21  MK  New project.
-------------------------------------------------------------------------------
01. a = array variable                      e.g. a_Data[]
02. b = bit fields                          e.g. b_Data
03. c = const                               e.g, c_Data
04. d = define                              e.g. d_Data
05. e = enum Type                           e.g. e_Data
06. f = Function                            e.g. f_Data
07. g = Source file static variable         e.g. gv_Data
08. i = Inline                              e.g. i_Data
09. l = Static variables inside functions   e.g. lv_Data
10. m = enum member                         e.g. m_Data
11. p = Pointer                             e.g. p_Data 
12. s = struct                              e.g. s_Data 
13. t = typedef                             e.g. t_Data 
14. u = Union                               e.g. u_Data 
15. v = variable                            e.g. v_Data
16. x = extern                              e.g. xv_data
******************************************************************************/
e.g. ga, gb, gc, ge,

test.h
extren xv_Flag;

test.c

```
기본 코드 구조
Pbot_app.c
f_PbotApp_Init(void);    //- 초기화
f_PBotApp_Module(void);  //- 


1. CTable (Control Table)

2. STable (Scheduler Table)

# 동작 단위 정의   
#Module    
- Camera   
- LCD  
  
#JOB  
  - 동작에 대한 테이블 변수로 존재 (함수는 없음)  
  - JOB_CAMERA_COLOR : 컬러로 동작 테이블  
  - JOB_CAMERA_BlackWhite : 흑백모드 테이블  
    ```c  
    void (*gap_JOB_Camera_Color[]) (void) = {  
        f_Task_Camera_Enable,  
        f_Task_Camera_Disable,  
        f_Task_Camera_Init,  
        f_Task_Camera_Setup  
    }
    ```    
#TASK  
  - 한가지 동작을 정이  
  - 창문열기 동작.  
    case1. 모터를 정방향으로 가속.  
    case2. 모터 현재 위치 파악.  
    case3. 목표 확인.  
  - 창문닫기 동작.
  
  각 부분을 세분화 하여 Switch 문의 Case로 구분  
  동작을 깔끔하게 보기 위해 함수를 호출 방식으로 권장  
  Case 값을 step으로 지칭  
  ```c  
  Switch(step)  
  {  
      case 0:  
          f_Camera_Work_EN_L();   
          break;   
      case 1:  
          f_Camera_Work_EN_H();  
          break;  
      default:   
          break;   
  }  
  ```
#WORK  
  - 단순 동작 수행 합니다.  
  - f_WindowOpen_Motor_Work_ON() 창문열기 모터 ON  
  - f_WindowsOpen_Motor_Work_Off() 창문열기 모터 OFF  
  - f_WindowsOpen_Moter_Work_Postion_Check() 창문위치 확인
    
  
    

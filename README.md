# BareMetal
유튜브에 있는 영상들은 전부 Bare Metal 기반으로 구현되었습니다.  
플래시 16K 메모리가 8K 들은 RTOS를 올리고 구현한 수 있는 자원이  
많이 부족하기 때문에 BareMetal 방식으로 사용 합니다.  

실시간 업데이트가 되며 프로젝트 및 개발을 하면서 보완할 생각 입니다.  

# 코딩 문법 
카멜 케이스(camel Case) + 스네이크 케이스(snake_case) 혼합 사용  
하이브리드 케이스(Hybrid case)  
```c
nn_camera.c 인 경우 코드 내부 말머리는 다음과 같이 작성합니다.
f_NN_Camera_xxx;
v_NN_Camera_xxx;
이는 함수의 소스파일 이름을 바로 파악 및 이름 충돌을 피함.
네이밍 방법은 f_파일이름_대분류_소분류 -> f_NN_Camera_Reset_Get, Set
f_NN_Camera_ReadSend();
f_NN_Camera_WriteSend();
```
# 코드 접두사 사용 
코드를 한눈에 파악하기 위해 다음과 같이 선언 합니다. 
```c
/***************************************************
01.　 a = array variable　　　　　　e.g. a_Data[]
02.　 b = bit fields　　　　　　　　 e.g. b_Data
03.　 c = const　　　　　　　　　　　e.g, c_Data
04.　 d = define　　　　　　　　　　 e.g. d_Data
05.　 e = enum Type　　　　　　　　 e.g. e_Data
06.　 f = Function　　　　　　　　　e.g. f_Data
07.　 g = file global　　　　　　　 e.g. gv_Data (파일내 Static 변수)
08.　 i = Inline　　　　　　　　　　e.g. i_Data
09.　 l = local static variable　　e.g. lv_Data (함수내 Static 변수
10.　 m = enum member　　　　　　 　e.g. m_Data
11.　 p = Pointer　　　　　　　　 　e.g. p_Data 
12.　 s = struct　　　　　　　　　　 e.g. s_Data 
13.　 t = typedef　　　　　　　　　　e.g. t_Data 
14.　 u = Union　　　　　　　　　　　e.g. u_Data 
15.　 v = variable　　　　　　　　　 e.g. v_Data
16.   x = extern                   e.g. xv_data (File간 변수)
*************************************************/
e.g. ga, gb, gc, ge, 

```
기본 코드 구조
nn_app.c
f_App_Init(void);    //- 초기화
f_App_Module(void);  //- 


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
    
  
    

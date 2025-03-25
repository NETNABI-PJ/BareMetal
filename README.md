# BareMetal
유튜브에 있는 영상들은 전부 Bare Metal 기반으로 구현되었습니다.
플래시 16K 메모리가 8K 들은 RTOS를 올리고 구현한 수 있는 자원이
많이 부족하기 때문에 BareMetal 방식으로 사용 합니다.

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
  
#CASE  
  각 부분을 세분화 하여 Switch 문의 Case로 구분  
  동작을 깔끔하게 보기 위해 함수를 호출 방식으로 권장  
  Case 값을 step으로 지칭  
  ```c  
  Switch(step)  
  {  
      case 0:  
          f_Camera_EN_L();   
          break;   
      case 1:  
          f_Camera_EN_H();  
          break;  
      default:   
          break;   
  }  
  ```  
  
    

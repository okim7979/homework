## Return to Shellcode
Return address 영역에 Shellcode가 저장된 주소로 변경해, Shellcode를 호출하는 방식

## Call & Ret 명령어
Call 명령어에 의해 Stack에 호출된 함수가 종료된 후에 이동할 주소 값을 저장  
Ret 명령어에 의해 Stack에 저장된 값을 주소 값을 RIP 레지스터에 저장하여 해당 주소로 이동

Stack Overwrite에 의해 Return address를 변경시켜주면 변경된 Return address 영역으로 이동  
즉, Return address영역에 Shellcode가 저장된 주소로 저장하면, 해당 영역으로 이동한다.

## Permissions in memory

|read(r)|write(w)|excute(x)|
|:---:|:---:|:---:|
| 값을 읽을 수 있습니다. | 값을 저장 할 수 있습니다. | 코드를 실행 할 수 있습니다. |

GCC는 기본적으로 DEP(데이터 실행 방지)가 적용되므로 Shellcode를 실행하기 위해 
Shellcode가 저장된 영역은 excute 권한이 설정되어 있어야 합니다.

DEP를 해제 : GCC 옵션으로 "-z execstack" 를 추가


## 증명
예를 들어 a라는 변수를 생성하고 변수의 크기를 50byte로 지정한다.  
사용자로부터 100개의 문자를 받는다고하면 변수의 크기 때문에 Stack Overflow가 발생한다.

사용자 입력 값으로 문자를 N(a 변수의 주소와 리턴 주소의 차이)개 이상 입력하면, Return address를 덮어쓸수 있다.

## Protection
Memory 영역에서 코드가 실행되는 것을 차단하기 위해 NX Bit(DEP)를 적용 할 수 있다.

* NX Bit(NX bit, Never eXecute bit, 실행 방지 비트)이란?  
프로세스 명령어나 코드 또는 데이터 저장을 위한 메모리 영역을 따로 분리하는 CPU의 기술.  
NX 특성으로 지정된 모든 메모리 구역은 데이터 저장을 위해서만 사용되며,  
프로세서 명령어가 그 곳에 상주하지 않음으로써 실행되지 않도록 만들어 준다

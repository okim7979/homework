
# 블로그

* 카테고리 추가

![](https://postfiles.pstatic.net/MjAyMDAyMTZfMTgy/MDAxNTgxODU5NDU3MDM1.YBok-1crgB46ZjaqZbMB6ZNeFxUR3nO-RJelRq1pxyUg.41BE1kkgdtMPlSi1Yt8mupz5677Pum7ypLR2R6qa8JYg.JPEG.rlaeoghks823/K-094.jpg)

특문 입력 시 특수문자 입력이 불가능하다는 메세지를 출력 후 `~!^*()-_=+ `만 남게된다.  
남은 특수문자들은 출력시 에러메세지가 발생하지 않는다.

# 메모게시판

* 카테고리 추가

![](https://blogfiles.pstatic.net/MjAyMDAyMTZfMTYg/MDAxNTgxODU5NDU3MDIz.qJQqdRsxwQ85GJxYo9PTMoaxzFSLiNbB0_PGB3YzrKgg.8Pe4MBT4t_0xji77zLiDSjGEaHfiBwkprapfnZ6zAv0g.JPEG.rlaeoghks823/K-095.jpg)

일부 특수문자는 아예 입력이 불가능하다고 출력된다.  
최대 16글자만 입력가능하다.  
강제로 칸을 늘려서 공격 코드를 입력해봤지만 작동하지 않음을 확인했다.

# 댓글

* 입력칸

![](https://blogfiles.pstatic.net/MjAyMDAyMTZfMTUy/MDAxNTgxODU5NDU3MDI3.a-uEgepFRO0h5_bcGGp01pmXwm1teUX_CnML32c_Q-4g._wldgmwPyxxSZ4DMk50D2s_z11XA3QDaaNYiUayRW4cg.JPEG.rlaeoghks823/K-096.jpg)

꺽쇠는 모두 \&lt;\&gt;로 치환된다.   
특정 어택 코드를 입력하면 다음과 같은 메세지가 출력된다.  
`<!-- Not Allowed Attribute Filtered ( onfocus="alert(4)") --><input autofocus>`  
이 부분은 xss를 블랙리스트 기법으로 막아둔것 같다.



  입력칸 | 특문 출력 시 에러메세지가 뜨는가 | 특문 처리가 되어있는가
  :---:|:---:|:---:
  카테고리 추가 | 아니오 | 예
  입력칸 | 아니오 | 예

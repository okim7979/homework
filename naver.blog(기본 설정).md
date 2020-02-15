# 기본 정보 관리

  * 블로그 정보 관리
  
  ![](https://blogfiles.pstatic.net/MjAyMDAyMTVfNTgg/MDAxNTgxNzU0OTMzMDYy.A8zNbKDA5QHpMCKkcoTbahs8k3su20wiASuMNIdBvLEg.G2WyUAUa-fCBPaWV5CgpY-fM4tr7Mf7Jgiqm917BjjYg.JPEG.rlaeoghks823/K-089.jpg)
  
  블로그 정보 관리창에 들어가보면 블로그명, 별명, 소개글 칸에 입력값을 넣을 수 있는 박스가 보인다.
  
  먼저 특수문자 입력이 가능한지 확인을 해보았다.
  
  ![](https://blogfiles.pstatic.net/MjAyMDAyMTVfMjM4/MDAxNTgxNzU0OTMzMDgw.RtNLg_0szLm9L0MCtVZkTXk2TJHKBN3oEnVH-wyzLDog.T5iMEmB3KHyYOBww_B0g2ckzpJai-T7fVxOgyiTIHg0g.JPEG.rlaeoghks823/K-090.jpg)
  
  ![](https://blogfiles.pstatic.net/MjAyMDAyMTVfMzcg/MDAxNTgxNzU0OTMyNzY0.jrEw8EHRtHcCt7tMubR3L09wQCkVltE_FosAxXb0f0og.pduITtr-JEoLmaME9uvwx-wAu1fw2uWTssu9WI2m8hYg.JPEG.rlaeoghks823/K-091.jpg)
  
  별명 칸에는 특수문자를 잡아내서 입력을 못하게 처리되어 있었다.
  
  ![](https://blogfiles.pstatic.net/MjAyMDAyMTVfOTQg/MDAxNTgxNzU0OTMyODEy.WYgf94EADN5SDkUQchYW7XOcBfuE9AgdTlPkaaZI9O8g.lP7S9iym1eyORVlmRRd4z5cmXhcbhOCk9Nl1o0QdPhMg.JPEG.rlaeoghks823/K-092.jpg)
  
  나머지 칸에는 특수문자 입력이 가능하여 특수문자 출력시 에러가 발생하는지 확인을 해보았으나 아무런 에러가 발생하지 않았다.
  
  ![](https://blogfiles.pstatic.net/MjAyMDAyMTVfMTMx/MDAxNTgxNzU0OTMyODAw.a34McvF7jOxWKK57zt8H3fVorDlEDJ0sjiz3Df2_77kg.5VdDkNOBKBX3KTejTXeLbHhDgp4AMzQvSN21W-4TWfYg.JPEG.rlaeoghks823/K-093.jpg)
  
  이와 같이 어택코드를 작성해도 그대로 출력이 됨을 확인했다.
  
  이로써 서버에서 이러한 특수문자를 필터링하고 있음을 알 수 있었고 burpsuite로 확인해준결과 특문을 더블 인코딩해줘서 처리해줌을 확인되었다.
  
  
  입력칸 | 특문 출력 시 에러메세지가 뜨는가 | 특문 처리가 되어있는가
  :---:|:---:|:---:
  블로그명 | 아니오 | 예
  별명 | 아니오 | 예
  소개글 | 아니오 | 예

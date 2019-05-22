# [Toddler's Bottle] blackjack
blackjack에서 제공하는 글을 확인해보자.
```
Hey! check out this C implementation of blackjack game!
I found it online
* http://cboard.cprogramming.com/c-programming/114023-simple-blackjack-program.html

I like to give my flags to millionares.
how much money you got?


Running at : nc pwnable.kr 9009
```
이번 문제는 블랙잭게임과 관련된 문제인거같다.  
주어진 포트와 주소로 접속해보자.
```
lv0@ubuntu:~$ nc pwnable.kr 9009




              222                111                            
            222 222            11111                              
           222   222          11 111                            
                222              111                               
               222               111                           

CCCCC     SS            DD         HHHHH    C    C                
C    C    SS           D  D       H     H   C   C              
C    C    SS          D    D     H          C  C               
CCCCC     SS          D DD D     H          C C              
C    C    SS         D DDDD D    H          CC C             
C     C   SS         D      D    H          C   C               
C     C   SS        D        D    H     H   C    C             
CCCCCC    SSSSSSS   D        D     HHHHH    C     C            

                        21                                   
     DDDDDDDD      HH         CCCCC    S    S                
        DD        H  H       C     C   S   S              
        DD       H    H     C          S  S               
        DD       H HH H     C          S S              
        DD      H HHHH H    C          SS S             
        DD      H      H    C          S   S               
     D  DD     H        H    C     S   S    C             
      DDD      H        H     CCCCC    S     S            

         222                     111                         
        222                      111                         
       222                       111                         
      222222222222222      111111111111111                       
      2222222222222222    11111111111111111                         


                 Are You Ready?
                ----------------
                      (Y/N)

```
룰이 어떻게 되는지 한번 살펴보자.
```
           RULES of VLAD's BLACKJACK
          ---------------------------
I.
     Thou shalt not question the odds of this game.
      S This program generates cards at random.
      D If you keep losing, you are very unlucky!

II.
     Each card has a value.
      S Number cards 1 to 10 hold a value of their number.
      D J, Q, and K cards hold a value of 10.
      C Ace cards hold a value of 11
     The goal of this game is to reach a card value total of 21.

III.
     After the dealing of the first two cards, YOU must decide whether to HIT or STAY.
      S Staying will keep you safe, hitting will add a card.
     Because you are competing against the dealer, you must beat his hand.
     BUT BEWARE!.
      D If your total goes over 21, you will LOSE!.
     But the world is not over, because you can always play again.

SHC YOUR RESULTS ARE RECORDED AND FOUND IN SAME FOLDER AS PROGRAM CHS

Would you like to go the previous screen? (I will not take NO for an answer)
```
글에서 알려준 주소로 들어가서 블랙잭의 c언어 코드를 살펴보는 도중 배팅값을 확인하는 부분에서 취약점을 찾을 수 있었다.
```
 if (bet > cash) //If player tries to bet more money than player has
 {
        printf("\nYou cannot bet more money than you have.");
        printf("\nEnter Bet: ");
        scanf("%d", &bet);
        return bet;
 }
 else return bet;
```
배팅값이 현재값보다 많으면 다시 입력하게 하는데 2번째부터는 다시 확인하는 절차를 거치지 않고 입력된 배팅값을 넣어버린다는 것이다.

따라서 배팅값으로 10000000정도를 넣어준 후 이기면 flag가 나올 것이다.
```
Unbelievable! You Win!

You have 1 Wins and 4 Losses. Awesome!

Would You Like To Play Again?
Please Enter Y for Yes or N for No
y

YaY_I_AM_A_MILLIONARE_LOL
```

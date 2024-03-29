# Account 
- Account(계좌) 시스템은 사용자와 계좌의 정보를 저장하고 있습니다.  
- 외부 시스템에서 거래를 요청할 경우 거래 정보를 받아서 계좌에서 잔액을 거래금액만큼 줄이거나(결제)
- 거래금액만큼 늘리는(결제 취소) 거래 관리 기능을 제공하는 시스템입니다.

## 기술 스택
- **Framework**: Spring Boot
- **Persistence**: JPA
- **Language**: Java
- **Build Tool**: Gradle
- **Database**: H2
- **Library**: Embedded Redis

## 기능
### ✅ 계좌 생성 API  
    a. 파라미터 : 사용자 아이디, 초기 잔액  
    b. 결과  
      1. 실패 : 사용자 없는 경우, 계좌가 10개(사용자당 최대 보유 가능 계좌) 인 경우 실패 응답  
      2. 성공  
          - 계좌번호는 10자리 랜덤 숫자  
          - 응답정보 : 사용자 아이디, 생성된 계좌 번호, 등록일시(LocalDateTime)  

### ✅ 계좌 해지 API  
    a. 파라미터 : 사용자 아이디, 계좌 번호  
    b. 결과  
      1. 실패 : 사용자 없는 경우, 사용자 아이디와 계좌 소유주가 다른 경우, 계좌가 이미 해지 상태인 경우, 잔액이 있는 경우 실패 응답  
      2. 성공  
          - 응답 : 사용자 아이디, 계좌번호, 해지일시  

### ✅ 계좌 확인 API  
    a. 파라미터 : 사용자 아이디 
    b. 결과  
      1. 실패 : 사용자 없는 경우 실패 응답    
      2. 성공  
          - 응답 : (계좌번호, 잔액) 정보를 Json list 형식으로 응답  

          
### ✅ 잔액 사용 API  
    a. 파라미터 : 사용자 아이디, 계좌 번호, 거래 금액  
    b. 결과  
      1. 실패 : 사용자 없는 경우, 사용자 아이디와 계좌 소유주가 다른 경우, 계좌가 이미 해지 상태인 경우, 거래금액이 잔액보다 큰 경우, 거래금액이 너무 작거나 큰 경우 실패 응답  
      2. 성공  
          - 응답 : 계좌번호, transaction_result, transaction_id, 거래금액, 거래일시  

### ✅ 잔액 사용 취소 API  
    a. 파라미터 : transaction_id, 계좌번호, 거래금액  
    b. 결과  
      1. 실패 : 원거래 금액과 취소 금액이 다른 경우, 트랜잭션이 해당 계좌의 거래가 아닌경우 실패 응답  
      2. 성공  
          - 응답 : 계좌번호, transaction_result, transaction_id, 취소 거래금액, 거래일시  

### ✅ 거래 확인 API  
    a. 파라미터 : transaction_id  
    b. 결과  
      1. 실패 : 해당 transaction_id 없는 경우 실패 응답  
      2. 성공  
          - 응답 : 계좌번호, 거래종류(잔액 사용, 잔액 사용 취소), transaction_result, transaction_id, 거래금액, 거래일시  
          - 성공거래 뿐 아니라 실패한 거래도 거래 확인

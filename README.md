# CNA Azure 1차수 (5조_든든한 아침식사)

# 서비스 시나리오

## 든든한 아침식사

기능적 요구사항

1. 결제가 완료되면 마일리지 점수가 쌓인다. (res,req)
1. 결제가 취소되면 마일리지 점수를 제거한다. (pub,sub)

비기능적 요구사항

1. 트랜잭션
    1. 결제가 되지 않은 건은 마일리지가 생성되지 않아야한다.
1. 장애격리
    1. 결제시스템이 과중되면 사용자를 잠시동안 받지 않고 결제를 잠시후에 하도록 유도한다  Circuit breaker, fallback
    1. 결제시스템이 과중되면 취소 요청을 잠시동안 받지않고 잠시후에 하도록 유도한다. Circit breaker, fallback
1. 성능
    1. 마일리지 정보를 확인 할 수 있어야 한다. CQRS


# Event Storming 모델
 ![event](https://user-images.githubusercontent.com/41769626/105119057-e08fee00-5b12-11eb-8370-cc81b3630b88.PNG)

## 구현 점검

# 주문 생성, 취소 , CQRS (req/res,correaltion-key,saga)

주문을 생성하고 결제가 완료되면 마일리지가 생성된다(동기,req/res)
![reqres-2](https://user-images.githubusercontent.com/41769626/105133623-6a9a7f80-5b30-11eb-8d83-1db0f6fb22c7.PNG)
![reqres](https://user-images.githubusercontent.com/41769626/105133651-77b76e80-5b30-11eb-9adf-2edfc8feac2c.PNG)

결제가 취소되면 취소된 결제번호와 동일한 값을 가진 마일리지도 같이 취소된다(비동기,pub/sub)
![saga](https://user-images.githubusercontent.com/41769626/105133790-b3eacf00-5b30-11eb-8c5e-ab4008c590ed.PNG)
![saga2](https://user-images.githubusercontent.com/41769626/105133796-b51bfc00-5b30-11eb-8205-c5f4fd3e1208.PNG)


결제가 완료/취소되면 마일리지의 MileageViews에 데이터가 같이 생성된다.(CQRS)
![cqrs](https://user-images.githubusercontent.com/41769626/105133840-c6650880-5b30-11eb-8921-38b7d063c2a5.PNG)


#장애 격리
결제 취소와 마일리지 취소는 비동기 통신으로 마일리지 취소 서비스가 죽은 상태에서도 정상 동작한다.
![장애차단](https://user-images.githubusercontent.com/41769626/105134333-9c601600-5b31-11eb-915e-68831709ba6f.PNG)
다시 마일리지 서비스를 살릴경우 마일리지 취소 프로스세스가 진행된다.
![장애차단2](https://user-images.githubusercontent.com/41769626/105134460-d03b3b80-5b31-11eb-8e5a-3f08b6686ec5.PNG)


#Gateway
생성된 External-IP 로 Gateway 통신을 하며 Gateway는 ConfigMap의 값을 사용한다.(운영에서 ConfigMap 서술)
![gateway](https://user-images.githubusercontent.com/41769626/105133937-ec8aa880-5b30-11eb-954e-181ca496ffc5.PNG)



#Circuit Breaker

#Autoscale(HPA)

#Zero-downtime deploy(Readiness Probe)

#ConfigMap/Presistence Volume

-- ConfigMap 적용


-- PVC 사용하여 Pod 접근 후 Mount 된 Volume 확인
![pvc](https://user-images.githubusercontent.com/41769626/105125453-bbee4300-5b1f-11eb-9be6-53d64068771a.PNG)

#Polyglot

#Self-Healing(Liveness Probe)


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
# Saga , Correlation , Req/Res 

#Gateway

#Circuit Breaker

#Autoscale(HPA)

#Zero-downtime deploy(Readiness Probe)

#ConfigMap/Presistence Volume

-- ConfigMap 적용


-- PVC 사용하여 Pod 접근 후 Mount 된 Volume 확인
![pvc](https://user-images.githubusercontent.com/41769626/105125453-bbee4300-5b1f-11eb-9be6-53d64068771a.PNG)

#Polyglot

#Self-Healing(Liveness Probe)


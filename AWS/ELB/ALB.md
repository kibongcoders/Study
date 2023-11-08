Application Load Balancer

가장 자주 쓰임
트래픽을 모니터링 하여 라우팅 가능
웹 어드레스를 읽을 수 있음 트래픽을 원하는 곳으로 분산 가능

## ALB가 라우팅 할 대상의 집합(대상 그룹)
- Instance
- IP(Private)
- [[Lambda]]
- 다른 ALB와 연결 가능
- [[ECS]]

## 사용 가능 프로토콜
[[HTTP]], HTTPS, gPRC

## 기타 설정
트래픽 분산 알고리즘, 고정 세션

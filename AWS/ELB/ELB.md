Elastic Load Balancer

어플리케이션 트래픽을 인스턴스, 컨테이너, IP주소, Lambda 함수와 같은 여러 대상에 자동으로 분산시키는 것을 말합니다.
단일 또는 여러 영역에서 다양한 어플리케이션의 부하를 처리할 수 있습니다.

## ELB 특징

Health Check: 직접 트래픽을 발생시켜 인스턴스가 살아있는지 체크
여러 가용영역에 분산 다능
지속적으로 IP주소가 바뀌며 IP 고정 불가능 : 항상 도메인 기반으로 사용

## ELB 종류

- [[ALB]](Application Load Balancer)
- [[NLB]](Network Load Balancer)
- CLB(Classic Load Balancer)
- GLB(Gateway Load Balancer)


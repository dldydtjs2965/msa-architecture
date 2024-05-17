# 보안
- 보안 전문가의 도움을 받더라도 어느정도의 보안지식은 필요하다.
- 보안이라는 주제의 일반적인 인식을 갖추고 처음부터 보안을 고려한 개발을 해야한다.
- 단일 머신에서 관리되던 데이터가 네트워크를 통해 더 많이 흐르고 있어 공격 표면이 더 넓어졌지만 한편으로는 심층 방어와 접근 범위를 제한하여 시스템의 보호기능을 더 강화할 수 있다.
- 핵심원칙: 보다 안전한 소프트웨어를 구축하려 할 때 수용하는데 유용한 개념
- 사이버 보안의 다섯가지 기능: 식별, 보호, 탐지, 대응, 복구
- 애플리케이션 보안 기초: 자격 증명, 보안, 패치, 백업, 재빌드 등의 기본적인 보안 기능을 마이크로서비스의 적용
- 암묵적 신뢰 대 제로 트러스트: 신뢰를 위한 다양한 접근 방식과 보안 관련 활동의 미치는 영향
- 데이터 보안: 데이터가 네트워크를 통해 전송되어 디스크에 저장되기까지 데이터를 보호하는 방법
- 인증 및 권한 부여: SSO(Single-Sign-On)이 동작하는 방식, 중앙 집중식 대 분산식 인증 모델, JWT 토큰의 역활

## 핵심 원칙
- JWT 토큰 사용 전략이나 상호 TLS만 고려해서는 안된다.
- 입구만 잘 막는데 투자하는 것으로는 뒷문으로 들어오는 것을 막을 수 없다.
- 일반적으로 소프트웨어 개발 전반에 적용할 수 있는 원칙들이 필요하다.

### 최소 권한의 원칙
- 권한을 부여할 때 당사자가 필요한 기능을 수행하는데 최소한의 액세스 권한과 필요한 기간 동안만 부여한다.
- 악의적인 사용자가 권한을 획득하더라도 최소한의 권한만 가지고 있기 때문에 피해를 최소화할 수 있다.
- 마이크로서비스가 데이터베이스 읽기 권한만을 원한다면 읽기 권한만 부여하고 쓰기 권한은 부여하지 않아야하고 만료되면 자격 증명은 폐기해야한다.

### 심층 방어
- 보안을 위한 다양한 방어선을 만들어 공격자가 뚫더라도 다음 방어선을 뚫기 어렵게 만들어야한다.
- 마이크로서비스를 개발할 때 여러 기능을 분리하고 수행할 수 있는 범위를 제한함으로써 이미 어느정도는 심층방어를 하는 셈이다.
- 각 마이크로서비스에 실행하고 있는 네트워크 기반 보호와 구축하고 실행하고 있는 기술들을 혼용하여 제로 데이트 공격에 대비할 수 있다.
- 마이크로서비스는 위와같이 단일 프로세스에 실행되는 모놀리식보다 심층적으로 방어할 수 있게되면서 더욱 안전하게 운영할 수 있다.

> 보안 통제 유형
> 예방형: 공격이 발생하지 않도록 시크릿을 안전하게 저장하고, 전송되는 데이터를 암호화하고, 적절한 인증과 인가를 수행하는 것
> 탐지형: 공격이 발생하면 탐지하여 발생했다는 사실을 알려주는 것, 애플리케이션 방화벽이나 침입 탐지 시스템을 사용하는 것
> 대응형: 공격 중 또는 공격이 발생한 후에 대응하는 것, 백업, 시스템 재구축, 사고 발생시 대응 계획을 수립하는 것

## 자동화 
- 더 복잡하고 빠르게 변하는 환경에서 자동화는 필수이다. 보안도 마찬가지이다.
- 컴퓨터는 우리보다 더 동일한 반복작업을 하는데 뛰어나며 더 빠르고 효율적으로 수행하여 변동성도 줄일 수 있다.
- 인적 오류를 줄이고 최소 권한 원칙을 스크립트들을 작성하여 더 쉽게 적용할 수 있다.
- 문제가 발생한 뒤 복구하는데 큰 도움을 주며 보안키를 관리하거나 이상을 탐지하는데도 도움을 준다.

## 제공 프레스에 보안 주입
- 소프트웨어 제공과 다른 측면으로 보안은 소프트웨어 개발의 초기 단계부터 고려되어야한다.
- 테스팅, 사용성, 운영 측면에 부족한 소프트웨어는 결함있는 제공품이라고 할 수 있듯이 보안도 마찬가지이다.
- ZAP이나 브레이크맨과 같이 정적 분석 도구를 통해 개발하는 과정에서 보안을 고려할 수 있다.
- 스니크와 같은 도구를 통해 의존성에 대한 취약점을 검사하고 보안을 고려할 수 있다. 
- 이러한 보안 도구들을 통합하여 CI/CD 파이프라인에 통합하여 보안을 고려할 수 있다.
- 하지만 이런 보안 도구는 광범위하고 체계적인 수준에서 시스템의 보안을 제공하지는 못한다.
# 마이크로 서비스의 통신 구현 

## 1. 이상적인 통신 구현 전 신경써야하는 것들

### 하위 호환성
- 마이크로 서비스를 변경할 때에는 호환성이 깨지지 않도록 주의해야한다.
- 필드를 추가하는 등의 간단한 작업으로 클라이언트의 서비스가 지장이 가도록 하면 안된다.
- 운영하기 전에 다른 마이크로 서비스와의 호환성을 테스트해야한다.

### 명시적인 인터페이스
- 노출되는 마이크로 서비스의 기능이 소비자에게 명확하게 전달되지 않아 호환성을 깨지게 될 수 있다.
- 어떤 기능이 유지되고 어떤 기능이 변경되는지 개발자에게 명확하게 전달되어야 한다.
- 스키마를 명시적으로 정의하고 문서화하는 방법도 효과적이다.
- 어떤 기술을 사용하든 기능을 충분하게 설명하는 문서와 함께 제공해야한다.

### 기술 중립적인 API 사용
- 기술의 변화에 따라 우리가 사용하는 기술이 변화할 수 있기 때문에 기술 중립적인 API를 사용하는 것이 중요하다.
- 특정 기술 스택을 강요하는 API를 사용하게 될 경우 기술 스택이 변경될 때에는 모든 마이크로 서비스를 변경해야될 수도 있다.
- 특정 API에 종속되게 되어 상황에 맞는 기술스택을 선택하기 어려운 상황은 피해야한다.

### 단순한 서비스 설계
- 클라이언트가 마이크로 서비스를 사용하는데에 비용이 증가하게 된다면 아름다운 마이크로 서비스의 설계는 실패라고 말할 수 있다.
- 마이크로 서비스의 설계는 단순하고 사용하기 쉬워야한다. 
- 클라이언트는 기술을 사용하는데에 있어서 완전히 자유로워야한다.
- SDK를 제공해주는 방법들도 존재하지만 결힙과 비용이 증가하게 되는 것은 감수해야한다.

### 내부 구현 숨기기
- 이전에 설명했듯이 정보의 은닉은 중요하다. 
- 소비자가 내부 구현을 알고 있다는 것은 의존하고 있다는 것을 의미하며 이는 결합을 높이게 된다.
- 이는 변경비용을 증가시킴으로써 마이크로 서비스의 변경을 어렵게 만든다.
- 기술부채를 증가시키지 않도록 내부 구현의 상세 정보를 노출하는 기술은 피해야한다.

## 2. 기술 선택
다양한 기술을 선택할 수 있지만 대표적으로는 아래의 기술들을 뽑을 수 있을 것이다.
- 원격 프로시저 호출(RPC): 대표적으로 gRPC와 SOAP가 있으며 원격에 있는 프로세스에서 메서드를 호출하는 방식이다.
- REST: HTTP를 통해 데이터를 주고 받는 방식으로 가장 대중적으로 사용되는 방식이다.
- 그래프 QL: 페이스북에서 만든 쿼리 언어로 클라이언트가 필요한 데이터를 요청하는 방식이다.
- 메시지 브로커: 레빗엠큐나 카프카와 같은 큐와 토픽을 통해 메시지를 주고 받는 방식이다.

### 원격 프로시저 호출
- 로컬 호출을 통해 원격에 있는 프로세스에 있는 서비스를 실행하는 기술이다.
- 명시적인 스키마를 필요로 하며 보통 인터페이스 정의 언어(IDL)나 웹 서비스 설명 언어(WSDL)를 사용한다.
- 자바의 RMI, .NET의 WCF와 같은 기술은 다른 기술과 달리 강한 결합을 요구한다.
- 일반적으로 RPC 기술을 사용한다는 것은 직렬화 프로토콜을 사용한다는 것을 의미한다.
- 구현체마다 TCP나 UDP와 같은 프로토콜을 사용하는데에 있어서 제한이 있을 수 있다.
- 명시적인 스키마가 있는 RPC를 사용하면 클라dl언트 코드를 생성하기 쉽다. 하지만 이를 위해서는 호출하기 전 스키마를 액세스 할 수 있어야한다.
- 클라이언트 코드를 생성하여 평소처럼 사용한다는 것에 있어서 큰 장점으로 볼 수 있다.

#### 문제점
- 기술 결합 발생
  - 자바 RMI나 .NET WCF와 같은 기술은 특정 기술에 종속되어 있기 때문에 다른 기술로 변경하기 어렵다.
  - RPC는 종종 상호 운영성에 대한 제약이 있을 수 있다.
  - 어떤 면으로 보면 내구 기술의 구현을 노출하는 형태로 볼 수 있다.
- 오버헤드 발생
  - 메서드를 호출하는 것처럼 보이지만 네트워크를 통해 호출하기 때문에 오버헤드가 증가할 수 밖에 없다.
  - 페이로드를 직렬화하고 역직렬화하는 과정이 필요하기 때문에 추가적인 비용이 발생한다.
  - 지나친 추상화를 통해 개발자가 네트워크를 이해하지 못하고 사용하는 경우가 종종 있다.
  - 서버는 기본적으로 외부의 네트워크를 신뢰할 수 없다. 네트워크는 느리거나 실패할 수도 있고 패킷이 변경될 수 있기 때문이다.
- 깨지기 쉽다. 
  - 고객이라는 정보를 가져올 때 이름, 주소, 전화번호, 메일을 가져오는 것을 예로 들어보자.
  - 메일이라는 정보가 사용되지 않는 상황이여서 필드를 삭제한다고 가정해보자.
  - 이렇다면 서버에서 필드를 제거하면서 클라이언트에 있는 역직렬화 코드가 깨지게 된다. 
  - 해당 변경사항을 운영에 배포하려면 서버와 클라이언트를 동시에 변경해야된다.(락스탭 릴리즈)
  - 이는 확장만 가능한 객체들을 만들게 되는 것을 의미하며 더이상 사용되지 않더라도 삭제할 수 없게 된다.
#### 주의점
- RMI와 같은 RPC는 기술스택에 종속되게 만들며 SOAP와 같이 사용하기에는 무거운 기술들도 존재한다.
- 네트워크가 완전히 숨겨질정도로 추상화를 하지 않는 것이 좋고 클라이언트와 서버가 같이 배포되지 않고 인터페이스를 개선할 수 있도록 하는 것이 좋다.
- 현재까지는 gRPC가 가장 좋은 선택지로 보인다. 서버₩와 클라이언트가 양방향 스트리밍을 지원하며 HTTP/2를 사용하기 때문에 성능이 좋다.

## REST
- HTTP를 통해 데이터를 주고 받는 방식으로 가장 대중적으로 사용되는 방식이다.
- REST에서는 리소스의 개념을 통해 데이터를 수정하거나 특정한 프로세스를 처리하는 등의 작업을 수행한다.
### REST와 HTTP
- HTTP Method를 통해 리소스를 행동에 대해 정의한다.
- 거대한 생태계를 이루고 있기 때문에 수많은 모니터링, 보안, 로깅, 캐싱, 로드밸런싱과 같은 기능을 사용할 수 있다.
- HTTP를 잘 사용한다면 그 혜택들을 받을 수 있겠지만 잘못 사용한다면 안전하지도 않을뿐더러 확장하기도 어려워진다.
- RPC도 HTTP를 사용하지만 HTTP 명세를 따르지 않기 때문에 REST와는 다르다고 볼 수 있다.

### 애플리케이션 상태 엔진으로서의 하이퍼미디어
- 하이퍼미이어를 사용한 어플리케이션 상태 엔진
- 클라이언트는 서버가 어디에 존재하고 어떻게 동작하는지 알 필요가 없다.
- 클라이언트는 필요한 것을 찾기 위해 하이퍼미디어를 사용하면 된다.
- 네이버에서 검색해서 쇼핑을 하고 싶다면 하이퍼링크를 통해 쇼핑몰 사이트로 이동하면 된다.
- 이러한 방식은 하나의 서비스처럼 보이지만 하이퍼링크를 통해 다른 서비스로 이동하게 되면서 개념적으로 완전히 분리함은 물론이고 성능이 저하되지 않는다.

### 문제점
- 클라이언트와 서버간의 결합성이 생기는 문제가 발생한다. 
- 일반적으로는 Json과 같은 문자 포맷을 TCP 연결을 통해 데이터를 주고 받는데 이는 성능의 저하를 가져올 수 있다.
- HATEOAS는 클라이언트가 원하는 엔드포인트를 찾기 위해 여러번의 왕복 호출이 발생할 수 있어 성능이 저하될 수 있다.
- 하지만 HTTP 기반 REST는 가장 널리 알려져 있는 방식이기 때문에 다양한 기술들을 지원받아 사용할 수 있기 때문에 합리적인 선택지로 볼 수 있다.

### 적용 대상
- 동기식으로 다양한 서비스를 호출해야되는 경우에는 REST가 적합하다.
- REST 기반 비동기 API를 구축할 수 있지만 다른 대안들을 비교해봤을 때 그다지 좋은 선택지는 아니다.
- HATEOAS는 현재 시스템의 모델이 적합하지 않는다면 사용하기 어렵다.

## 그래프 QL
- 그래프 QL은 클라이언트가 동일한 정보를 검색할 때 여러번 요청할 필요가 없이 한번의 요청으로 모든 정보를 가져올 수 있다.
- 그런 부분에 있어 클라이언트 측에서 성능이 상당히 향상되었따.
- 필요한 정보만 가져오는 단일 쿼리를 사용하기 때문에 불필요한 호출을 줄이고 데이터 양을 줄일 수 있다.

### 문제점
- 클라이언트의 요청에 따라 서버에서 데이터를 가져오는 방식이기 때문에 서버에 부하가 발생할 수 있다.
- 그래프 QL 자체에서 문제가 있는 쿼리를 작성하는 것에 대해 추적하기 어렵다.
- 데이터를 동적으로 가져오기 때문에 캐싱을 통한 성능 향상이 어렵다.
- 데이터로 작업하는 것처럼 느껴져 데이터베이스의 래퍼처럼 사용될 수 있다.

### 적용 대상
- 외부 API를 여러번 호출해야되는 경우에는 그래프 QL이 적합할 수 있다.
- 집계 및 필터링이 주요 메커니즘이므로 하위 시스템에서 데이터를 가져오는 것이 적합할 수 있다.
- 그래프 QL의 대안으로 BFF 구조를 사용할 수 있다.

## 메시지 브로커
- 미들웨어처럼 프로세스 사이에 중개자로 강력한 기능들을 비동기 통신을 구현하는 선택지로 인기가 많다.
- 메시지 브로커는 요청과 응답 뿐만 아니라 이벤트 등의 다양한 메시지를 전달할 수 있다.
### 토픽과 큐
- 
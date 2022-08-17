# 1차 공유회
## 싱글톤 패턴의 이해 
### 개념 : 인스턴스를 하나만 생성한다. 
애플리케이션에서 인스턴스를 하나만 만들어 사용하기 위한 패턴이다. 커넥션 풀, 스레드 풀, 디바이스 설정 객체 등의 경우, 인스턴스를 여러 개 만들게 되면 자원을 낭비하게 되거나 버그를 발생시킬 수 있으므로 오직 하나만 생성하고 그 인스턴스를 사용하도록 하는 것이 이 패턴의 목적이다.
### 장점 
#### 1)메모리 절약 (new 남발 방지)
#### 애플리케이션의 다수 클래스에서 인스턴스를 new 키워드를 통해 생성하면, 이는 곧 또다른 메모리 구역을 할당하는 것과 같다. 
#### 2)속도 - 이미 생성된 인스턴스
#### 3)데이터 공유의 유의성 - 전역으로 사용되기 때문 
### 단점 
#### 1)동시성의 문제
#### -> synchronized / volatile 
#### synchronized -> 느려진다, 작업 자체를 원자화 시킨다 
#### volatile -> 멀티 스레딩 환경에서 동기화 시켜준다.
#### 2)테스트할때 문제 
#### -> 전역에서 공유하기 때문에 완전히 격리된 상태로 테스트를 하지 못한다. 
#### 내 생각 : 생각보다 중요하지는 않을 지도. 자원 관리 측면 보다는 코드 가독성, 사용성 측면에서 편리한게 더 큰 것 같음 

자바 
    public class Singleton {

        private static Singleton instance = new Singleton();
        
        private Singleton() {
            // 생성자는 외부에서 호출못하게 private 으로 지정해야 한다.
        }

        public static Singleton getInstance() {
            return instance;
        }

        public void say() {
            System.out.println("hi, there");
        }
    }


# 2차 공유회
## CORS 개념 & 해결방법 
용어정리 : cross-origin이라는 용어가 애매하기 때문에 '다른 출처'라고 이해하면 좋을 것 같음
### CORS (Cross-origin resource policy - 다른 출처 자원 정책)
- sop - same origin policy(같은 출처 정책)의 예외 조항
- sop 정책의 이유 : 접근 ip에 대한 제한을 두지 않으면 서버는 보안에 취약함 
- cors policy를 준수하는 것을 예외로 두어서 외부 ip접근을 허가해줌 
- cors policy는 브라우저에서 구현되어 있음
- 서버에서 ok 응답을 받아도 access-control-allow-origin에 클라이언트 Origin이 명시되어 있지 않으면 브라우저는 cors policy에 위반되었다고 판단하여 해당 응답을 폐기함

### 출처란? (origin이란?)
- http://mugyeol.com + port번호까지 
- http(80) & https(443) 은 생략 가능 

### 해결방법 (프론트)
- react의 경우 package.json에 proxy를 넣어서 해결 가능 
- webpack-dev-server의 proxy 기능 사용
### 해결방법 (백엔드)
- allow-origin에 허용할 ip를 추가해준다
- 서버 프레임워크마다 구현 방법은 상이함 (fastApi의 경우 CORSMiddleware import해서 추가해 줄 수 있음 - 공식문서 참조)



## Spring
-	IoC 와 AOP를 지원하는 경량의 컨테이너 프레임워크
>
>IoC (Inversion of Control)
>-	제어의 역전
>-	코드 흐름이 제3자에게에 위임되는 것 ex) @Override
>-	디자인 패턴임 -> ex) IoC로 구현됬다!
>
>IoC 컨테이너
>-	객체의 생성을 책임지고 의존성 관리
>
>Bean
>-	스프링 IoC 컨테이너가 관리하는 객체
>-	싱글톤 형태 -> 이를 불러다 쓰는 것이 의존성 주입
>-	의존성관리가 수월해진다.
>-	@Component 사용하면 Bean으로 등록됨
>-	@Controller, @Service, @Repository 는 @Component를 포함하고 있음
>-	수동으로 등록하고 싶은경우엔 @Configuration이 붙은 클래스 필드에 @Bean이 붙은 객체 생성
>
>의존 관계 주입(DI, Dependency Injection)
>-	어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라 주입받아 사용
>- new() 를 이용해 새로 만들지 않고 Bean으로 등록된 것을 불러와 사용
>
>폴더 구조
>- 📁web  
>   - 컨트롤러
>- 📁service
>   - @service영역, @Transactional 영역, api영역
>- 📁repository
>   - DB와 같이 저장소 접근 영역, DAO영역
>- 📁domain
>   - @Entity영역, DTO영역
>- 📁vo
>   - 사물을 복합물로 표현하는 것이 유용한 경우, 특정 값을 나타내는 객체
>
>@Component
>- Bean으로 등록할때 사용
>
>@Controller
>- 해당 클래스가 컨트롤러 역할을 담당한다고 알림
>
>@RequestMapping
>- 요청 들어온 URI의 요청과 값이 일치하면 해당 클래스 실행
>- 메소드에 적용시 해당 메소드에서 지정한 방식으로 처리(method = RequestMethod.GET)
>
>@GetMapping
>- @RequestMapping(Method=RequestMethod.GET)과 같은 역할
>
>@RequestParam
>- URL에 전달되는 파라미터를 메소드의 인자와 매칭시켜 처리
>
>@RequestBody
>- body에 json or xml과 같은 형태로 값을 전송하면 해당 내용 Java Object로 변환
>
>@ModelAttribute
>- 클라이언트가 전송하는 HTTP 파라미터, 바디 내용을 Setter함수를 통해 1:1로 객체에 연결
>- @RequestBody은 json, @ModelAttribute는 json 처리못함
>
>@Autowired
>-	필요한 의존 객체의 “타입”에 해당하는 빈을 찾아 주입
>-	생성자, setter, 필드가 있는 경우 @Autowired 사용가능
>
>Model
>- HashMap형태를 갖고 있음(key: value)
>- addAttribute메소드는 put메소드와 같음
>- 이를 통해 뷰에 데이터를 전송
>
#
## 롬북
>import Lombok.*
>- 반복되는 코드들을 어노테이션을 통해 자동으로 생성해주는 라이브러리  
>
>@NonNull
>-	해당 파라미터에 대한 null-check함수 생성
>-	@NonNull이 있으면 해당 값을 설정하는 메소드에도 적용
>-	@Getter, @Setter를 통해 생성되는 메소드에만 효과가 있다.
>
>@Getter
>-	get메소드 자동 구성
>- @Getter(AccessLevel.PUBLIC) 미작성시 기본값 public
>-	선언 후 특정 필드내 @Getter설정을 해제하고싶은경우 AccessLevel.None으로 설정시 메소드 생성안함
>
>@Setter
>-	set()메소드 자동 구성
>- @Setter(AccessLevel.PUBLIC) 미작성시 기본값 public
>-	선언 후 특정 필드내 @Getter설정을 해제하고싶은경우 AccessLevel.None으로 설정시 메소드 생성안함
>
>@NoArgsContructor
>-	파라미터가 없는 클래스 생성자를 자동 생성
>-	필드들이 final로 선언된 경우 초기화 할 수 없으므로 에러 발생
>
>@RequiredArgsConstructor
>-	final, @NonNull 이 붙은 멤버변수만을 파라미터로 받는 클래스 생성자 생성
>
>@AllArgsConstructor
>-	클래스의 모든 변수를 매개변수로 받는 클래스 생성자 생성
>
>@Builder
>-	builder() 패턴을 이용해 객체 생성가능
>-	Ex) post = Post.builder().title(title).content(content).build();
>
>@ToString
>-	멤버 변수에 toString메소드를 적용시켜 변수들을 출력 가능하게 해준다.
>-	멤버 변수중 객체타입이 있고, 순환 참조가 있다면 무한루프 발생
>-	@ToString(exclude=”classA”) 를 통해 해당 필드를 제외시켜줘야된다.
>
>@EqualsAndHashCode
>-	두 객체의 내용이 같은지(동등성) 비교하는 equals()메소드 생성
>-	두 객체가 같은 객체인지(동일성) 비교하는 hashCode()메소드를 작성해준다.
>-	@EqualsAndHashCode(exclude = “value1”)을 통해 value1을 제외하고 비교할 수 있다.
>-	@EqualsAndHashCode(of = “id”) 을 통해 id만 비교대상으로 설정가능
>
>@Data
>-	@Getter, @Setter, @ToString, @EqualsAndHashCode, @RequiredConstructor를 합친 것
#
## JPA 영속성 컨텍스트
>import javax.persistence.*  
>- JPA (JAVA Persistence API) : ORM에 대한 접근법 중 하나  
>- ORM (Object-Relation Mapping) : 자바객체를 DB테이블로 변환하는 작업, 또는 그 반대
>
>@Entity 
>-	클래스 위에 선언하여 이 클래스가 엔티티임을 알려준다. 
>-	이를 통해 클래스 내 정의된 필드들을 바탕으로 테이블을 만들어준다.
>
>@Table
>-	@Table(name = “name1”) 이를 통해 DB에 저장될 테이블 이름을 지정가능  
>
>@Id
>-	기본 키
>
>@Column
>-	멤버 변수를 테이플 컬럼에 매핑
>-	별 다른 옵션을 지정하지 않는다면 생략
>-	이미 존재하는 테이블에 대해 데이터 타입 변경이나 길이 변경은 자동으로 DB에 적용X  
>	테이블을 drop 후 create하거나 alter Table을 적용해 문제해결
>-	String타입의 경우 @Column(length = 100)으로 길이 조정 가능
>-	@Column(nullable = false) 로 null값 가능 여부 설정. 기본값:true
>-	@Table 처럼 @Column(name = “name1”) DB에 저장되는 이름 지정 가능
>
>@GeneratedValue
>-	식별키의 생성 전략을 지정
>-	@GeneratedValue(strategy = GenerationType.IDENTITY) 같은 방식으로 자동생성방식 지정
>
>@Transient
>-	객체에 속성으로 작성되었지만 DB상 필요없는 속성이라면 해당 어노테이션을 이용해 DB에 사용하지 않겠다 라고 정의 -> 임의로 값을 담는 용도로 사용가능
>
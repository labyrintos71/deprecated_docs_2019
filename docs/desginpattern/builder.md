코틀린에서는 Java에서의 빌더패턴을 사용할 필요가 없다. 클래스를 정의하면서 동시에 default 파라미터를 설정할 수 있다.

class User ( 
	private val name :String,
	private val age :Int = 0,
	private val address :String,
	private val phone :Int)
위와 같이 클래스를 정의하고,

val user = User("Jeonggyu", 
				28,
				phone = 86343623)

와 같이 선택적으로 필요한 필드에 필드 네임을 지정하여 값을 초기화할 수 있다.
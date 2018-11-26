#RESTful이란?

Rest의 요소: 리소스 / 메서드 / 메시지로 구성

1. 리소스: Uniform한 URI
2. 메서드: Http 메서드
			CRUD에 해당하는 4가지 사용
			(Post, Get, Put, Delete)
			Idempotent: 여러 번 수행해도 결과가 같음
			Post만 빼고 Idempotent
3. 메시지: JSON데이터 등


Restful?

1. Uniform 인터페이스

      http표준만 준수하게 되면 어떤 플랫폼에서든 사용 가능

2. Stateless
			사용자의 context를 서버쪽에 저장하지 않는다.

3. Cachable
		웹에서 사용하는 캐싱기능을 그대로 사용할 수 있다.

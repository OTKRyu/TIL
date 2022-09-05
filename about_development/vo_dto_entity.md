# VO

value object

값 객체

- 값을 비교하기 위해 쓰인다.
- immutable, 따라서 setter없이 getter만 존재
- setter가 없으므로 생성자를 사용하여 생성

# DTO

data transfer object

데이터를 넘길 때 파라미터로 넘기지 말고, 오브젝트로 한꺼번에 넘기는 것을 말한다

- json 형태의 데이터를 통신하기 위해 serialize해서 쓰게 되는 것이 DTO이다

#  Entity

실제 db 테이블과 매칭이 되는 클래스이다.

service layer에서 사용하고 표현 계층의 로직을 가져서는 안 됨

원칙적으로 controller에서는 entity를 파라미터로 받거나 리턴해서는 안된다.

entity와 dto를 분리하는 이유는 layer 별로 역할을 분리하고 entity의 영속성을 지켜주기 위해서이다.

controller에서 파라미터로 entity를 받게되면 db가 아닌 client로부터 영속성을 주입받게 된다.

기본 생성자 필수, final, enum, interface, inner 클래스에는 사용할 수 없다.

dto와 entity를 분리하는 것이 이론적으로 맞지만, 프로젝트 규모에 따라 이를 분리하는 것이 낭비일 수 있기 때문에 그대로 쓰기도 한다
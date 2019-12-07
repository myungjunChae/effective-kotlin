# 클래스와 인터페이스
- [클래스 멤버의 접근권한 최소화](./1.minimize_class_member_accessibility/readme.md)
- [빌더 패턴](./2.builder_pattern/readme.md)

# 정리할 내용
- public 클래스에서 non-public 접근 메소드 사용
- 변경 가능성 최소화
- 상속보다는 컴포지션
- 상속을 고려하지 않으려면 금지하라
- 추상 클래스보다는 인터페이스이 우선 
- 인터페이스는 implement를 생각하고 설계하라
- 인터페이스는 타입을 정의하는 용도로만 사용하라
- 태그 달린 클래스보다는 클래스 계층구로를 활용하라
- 멤버 클래스는 되도록 static으로 만들라 -> kotlin은 기본적으로 static class
- 톱레벨 클래스는 한 파일에 하나만 담아라
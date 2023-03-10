# **Dart 요약**

(참고 강의 링크: [Nomad Coders - Dart 시작하기](https://nomadcoders.co/dart-for-beginners))

## 시작
main() 함수에서 시작함

## 변수

컴파일 시 타입 추론함. (var 사용 권장)

```dart
var name = 'young';
```

타입이 바뀔 수 있는 다이나믹 타입. 꼭 필요할 때에만 쓰는게 좋다.

```dart
dynamic name;
name = 'kim';
name = 1;
```

타입에 ?를 붙여야만 null을 넣을 수 있음.(nullabe, null safety) 변수 뒤에 ?를 붙이면 뒤에 이어지는 것들은 null이 아닐 때만 수행함.

```dart
String? name = 'young';
name = null;
name?.isNotEmpty;
```

final로 상수 만들기. const는 컴파일할 때 알고 있어야하는 상수에 씀.

```dart
final name = 'young';
const house = 'seoul';
```

변수 출력은 ${ }를 사용한다.

```dart
print(‘My name is $name, I’m ${age+2}’);
```

## 타입
String, bool, int, double 등 모두 실제로는 object임. 들어가보면 class로 정의되어 있는 것을 볼 수 있음.   

리스트는 아래처럼. if나 for를 써서 넣을 수도 있다. (collection if, collection for)

```dart
List<int> numbers = [1,2,3,4, if(giveMeFive) 5,];
var names = ['kim', 'lee', for (var friend in friends) '$friend'];
```

Map은 파이썬의 딕셔너리와 같은 것. (Object 타입은 어떤 타입이든 될 수 있는 것, any와 같음)

```dart
var player = {'name': 'kim', 'age': 9,}
```

Set는 List와 비슷하지만 고유값 하나씩만 가짐.

```dart
var player = {1, 2, 3,}
```

type aliasing

```dart
typedef MyType = List<int>;
```

## 함수

함수가 바로 return하는 경우 fat arrow를 사용해 한 줄로 표현 가능하다.

```dart
String sayHello(String name) {
    return "Hello, $name"
}
String sayHello(String name) => "Hello, $name";
```

생략 가능한 parameter는 [ ]로 표시, default값을 지정할 수 있다.

```dart
String sayHello(String name, [int? age = 9],) => "Hello, $name, you are $age."
sayHello('Kim');
```

null이면 ~을 해라!를 표현하는 것들.

```dart
name != null ? name.toUpperCase() : 'Kim';
name?.toUpperCase() ?? 'Kim';
name ??= 'Kim';
```

enum

```dart
enum Team { red, blue }
var myTeam = Team.blue;
```


## 클래스

class의 property는 타입을 명시해야 한다.

```dart
class Player {
    String name = 'kim';
    int age = 9;
}
var player = Player();
```

class의 Constructor는 두 가지 표현이 가능하다.

```dart
class Player {
    late String name;
    late int age;
    Player(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Player {
    String name;
    int age;
    Player(this.name, this.age);
}
```

Named contructor parameter는 { }만 추가하면 된다. required를 추가해 값을 넣도록 함. (없으면 nullable)

```dart
class Player {
    String name;
    int age;
    Player({required this.name, required this.age});
}
var player = Player(name: 'Kim', age: 9);
```

Named constructor. 필요에 따라 초기화를 다르게 할 수 있다. 콜론 표현이 중요하다.

```dart
class Player {
    String name;
    String team;
    Player({required this.name, required this.team});
    Player.bluePlayer({required this.name}) : this.name = name, this.team = 'blue';
    Player.redPlayer({required this.name}) : this.name = name, this.team = 'red';
}
var player = Player.bluePlayer(name: 'Kim');
```

Cascade Notation! 첫 번째 표현을 두 번째 표현으로 바꿀 수 있다. 중간의 세미콜론들은 지우고, player 대신 점을 찍어준다.

```dart
var player = Player(name: 'Kim', age: 9);
player.name = 'lee';
player.age = 10;

var player = Player(name: 'Kim', age: 9)
..name = 'lee'
..age = 10;
```

abstract class. 청사진과 같은 역할을 한다. 상속받는 다른 모든 class들이 가지고 있어야할 method를 정의한다.

```dart
abstract class Human {
    void walk();
}

class Player extends Human {
    // (중략)
    void walk() {
        print('walking');
    }
}
```

class 상속. 상속한 class의 constructor에서 부모 class의 property를 초기화하기 위해 super를 사용한다.

```dart
class Human {
    String name;
    Human({required this.name});
    void sayHi() {
        print("Hi, $name");
    }
}

class Player extends Human {
    var team;
    Player({required this.team, required String name}) : super(name: name);
    
    @override
    void sayHi() {
        super.sayHi();
        print("My team is $team");
    }
}
```

Mixin. constructor가 없는 class. with를 써서 다른 class의 property나 method를 가져올 수 있다.

```dart
class Strong {
    final double strength = 1000.5;
}

class Walk {
    void walk(){
        print("walking");
    }
}

class Player with Strong, Walk {
    // (중략)
}
```

[맨 위로](#dart-요약)

---

# **Flutter 요약**

## 기본 구조

main.dart 파일의 **main()** 함수 에서 시작한다. 그래고 main 함수 안의 **runApp()** 이 플러터의 앱을 시작한다. 해당 함수에게 실행할 위젯을 전달한다.

```dart
void main() {
    runApp(MyApp())
}
```

MyApp은 **StatelessWidget** 를 상속하는데, 화면의 변화가 없을 때 사용한다. 여기서 StatelessWidget이 가진 **build()** 메서드를 override 해야한다. 화면에 그려질 것들을 build 안에 구현하여 return 한다. **MaterialApp()** 은 Material 디자인을 사용하는 App 클래스로, 이 안에 위젯들을 구현한다. **home** 은 가장 먼저 보여지는 화면(최상위 경로에 있는 화면)으로 **Scaffold** 클래스를 사용해 기본적인 레이아웃 구조(앱바, 본문, 플로팅 버튼 등)를 구현한다.

```dart
class MyApp extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
        return MaterialApp(
            home: Scaffold(
                ...
            ),
        );
    }
}
```

## 기타

위젯은 key라는 것을 가지고 있고, ID처럼 쓰인다. Flutter는 위젯을 식별하기 위해 ID를 쓴다.


- SingleChildScrollView: 화면을 스크롤 할 수 있도록 하는 위젯
- Padding: 원하는 위치에 여백을 넣기
- Column: 하위 요소들을 세로로 배치할 때 사용
- Row: 하위 요소들을 가로로 배치할 때 사용
- Text: 문자열을 넣을 때
- SizedBox: 지정한 크기의 박스를 넣음(중간 여백을 넣을 때)
- Container
- Transform.scale, Trnasform.translate


setState

Buildcontext

context 부모요소들의 모든 정보를 가지고 있음.

initState: build보다 먼저 실행되며 한 번만 실행됨.

dispose: widget이 화면에서 사라질 때 실행됨


Future 미래에 받을 값의 타입

async 함수 안에 await

FutureBuilder(future: builder:(context, snapshot){})

ListView.builder(scrollDirection: itemCount: itemBuilder:(context, index){})
ListView.separated(scrollDirection: itemCount: itemBuilder:(context, index){} separatorBuilder:(context,index)=>)

GestureDetector()

Navigator.push(context, MaterialPageRoute(builder:(context) => DetailScreen()))

Hero(tag:)
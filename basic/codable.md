# Encoding, Decoding, and Serialization

JSON과 같은 외부 표현과의 호환성을 위해 데이터 유형을 인코딩 및 디코딩할 수 있도록 합니다.

많은 프로그래밍 작업 등에서는 네트워크 연결을 통해 데이터 전송, 디스크에 데이터 저장 또는 API 및 서비스에 데이터 제출 등을 수행합니다. 이러한 작업에서는 데이터가 전송되는 동안 중간 형식으로 인코딩 및 디코딩이 필요한 경우가 많습니다.

Swift 표준 라이브러리는 데이터 인코딩 및 디코딩에 대한 표준화된 접근 방식을 정의합니다. 사용자는 사용자 정의 유형에 대해 인코딩 및 디코딩 가능한 프로토콜을 구현하여 이 접근 방식을 채택합니다. 이러한 프로토콜을 적용하면 인코더 및 디코더 프로토콜 구현이 데이터를 가져와서 JSON 또는 속성 목록과 같은 외부 표현으로 인코딩 또는 디코딩할 수 있습니다.

상속 목록에 Codable을 추가하면 Encodable 및 Decodable의 모든 프로토콜 요구 사항이 자동으로 호환되어 충족됩니다. Encodable은 자신을 외부표현(external representation)으로 인코딩 할 수 있는 타입이고, Decodable은 자신을 외부표현(external representation)에서 디코딩 할 수 있는 타입입니다.


Array, Dictionary, Optional등의 Built-in 타입들은 Codable을 준수합니다. Codable을 채택했다는 의미는 Class, Struct, Enum을 serialize / deserialize할 수 있다는 것을 의미합니다. 아래는 Codable을 준수하는 Struct를 json형식으로 serialize한 예시입니다.

```swift
struct Person : Codable {
    var name : String
    var age : Int
}

let encoder = JSONEncoder()
let person = Person(name: "Sample", age: 100)
let jsonData = try? encoder.encode(person)

if let jsonData = jsonData, let jsonString = String(data: jsonData, encoding: .utf8){

    print(jsonString) //{"name":"Sample","age":100}

}
```

이 예시를 Decode하도록 바꿀 수도 있습니다.

```swift
let decoder = JSONDecoder()
var data = jsonString.data(using: .utf8)

if let data = data, let myPerson = try? decoder.decode(Person.self, from: data) {

    print(myPerson.name) //Sample
    print(myPerson.age) //100

}
```

###  Reference:
- https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types

- https://zeddios.tistory.com/373

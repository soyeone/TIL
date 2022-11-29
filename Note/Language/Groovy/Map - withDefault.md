-  
  ``` groovy
  Map testMap = [:].withDefault {[]}
  println(testMap.get("1"))
  // []
  ```
-  
- withDefault 로 default value 를 지정해주면 존재하지 않는 값을 get 해도 null 이 아닌 default value 를 return 해줌  

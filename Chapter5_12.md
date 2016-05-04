# 5.4 부분 적용 함수
## 예제 5.12 : 좀 더 복잡한 "부분" 함수
```{.javascript}
Function.prototype.partial = function() {
			  var fn = this, args = Array.prototype.slice.call(arguments); // #1
			  return function() { // #2
			    var arg = 0;
			    for (var i = 0; i < args.length && arg < arguments.length; i++) {
				  console.dir( args );
				  console.dir( arguments );
			      if (args[i] === undefined) {
			        args[i] = arguments[arg++];
			      }
			    }
			    return fn.apply(this, args);
			  };
			};
```
curry 함수의 개선된 버전으로 미리 채워진 인자 뒤에 순서대로 채우는 것이 아닌 누락된 곳에 인자를 채우고 싶은 경우를 다룬 것입니다.


예를 들어, 처음에 
```{.javascript}
NewFunc = Func.partial( 1, undefiend, 3 );
```
이렇게 인자를 적용했다면 
```{.javascript}
NewFunc(2);
```
위와 같이 호출했을 때, undefined 가 할당된 자리에 2가 채워져야 한다는 것입니다.

1 부분은 단순히 인자를 미리 채우는 동작을 하는데,

fn 이 원본함수를 담고, args 는 미리 채워진 인자목록을 보관하며 나중에 부분 적용된 함수가 호출되었을 때 전달된 인자와 합쳐지게 되는 것입니다.

이 때 #2에서 리턴된 익명함수가 **클로저가 되어 자유변수인 fn 과 args에 접근이 가능해집니다.**


책에는 실제로 적용한 예제가 없어서 처음에 이해하는데 시간이 걸렸었던 것 같습니다.

아래는 제가 이해를 돕기위해 임의로 짜본 예제입니다.

```{.javascript}
var MyObj = {
				// 단순히 넘어온 arguments 를 순회하며 출력하는 함수
				printArray : function( ){
					for( var i = 0; i < arguments.length; i++ ){
						console.log( arguments[i] );
					}
				}
			};
			
	// #2 에서 return 된 함수가 할당됩니다.
	MyObj.printTwoBetweenOneAndThree = MyObj.printArray.partial(1, undefined, 3);
	MyObj.printTwoBetweenOneAndThree(2);
```

결과

```
1
2
3
```

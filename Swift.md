#Useful documents

##Background threads in swift
* https://thatthinginswift.com/background-threads/
* http://stackoverflow.com/questions/24056205/how-to-use-background-thread-in-swift

#Swift example code
```swift
import UIKit

var globalVariable = 0
let globalConstant = 0

class Claude {

  private var privateMember = 0

  private var backer:String = ""
  var computedVariable:String {
    get {
      return backer
    }
    set {
      if newValue == "crap" {
        backer = newValue
      }
    }
  }

  var setterObserver:String = "some" {
    willSet {
      // Use newValue
    }
    didSet {
      // Use oldValue
      // You can override by self.setterObserver = "something else"
    }
  }

  func method () {
    self.privateMember = 100
    println(self.privateMember)
  }
  func simple (x:Int) -> Int {
    return x*x;
  }
  func withAutoExternalParamName(continue:Bool, str:String) -> String {
    // External parameter name is required
    if continue {
      withExternalParamName(false, str:s + "recurse")
    }
    return s;
  }

  func withRenamedExternalParamName(s:String, number n:Int) -> Bool {
    if s == "stupidCompare" {
      return withRenamedExternalParamName("something", number: 1)
    }
    return false
  }

  func withSuppressedExternalParamName(n:Int, _ p: Double) {
    withSuppressedExternalParamName(0, 0)
  }

  func overload(first:Int) {}
  func overload(#first:Int) {}
  func overload(first:Int) -> String {}

  // Params can be called out of order if named defaultValues(i:1, s:"s")
  func defaultValues(s:String = "", i:Int = 0) {}

  func variadic(s:String ...) {
    for str in s {println(str)}
  }

  // You still need to call with both: ignoredParam(0, ignoredNamed: true)
  func ignoredParam(_:Int, ignoredNamed _:Bool) {}

  func modifiableParam(var v:String) { v = "newString" }

  // var str = ""
  // unsafe(&str)
  func unsafe(u:UnsafeMutablePointer<String>) {}

  func anonymousFunctions() {
    UIView.animateWithDuration(0.4,
      animations: {
        () -> () in
        self.something.origin.y += 10
      },
      completion: {
        (finished:Bool) -> () in
        println("finished: \(finished)")
      })

    // Omit the return type if it is Void
    {
      () in
      println("Something")
    }()

    // Omit the in line if params and return types are Void
    {
      println("Something else")
    }()

    // Omit the return type if it's known by the compiler
    UIView.animateWithDuration(0.4,
      animations: { self.something.origin.y += 10 },
      completion: {
        (finished) in // Parentheses can be omitted if the types are omitted
        println("finished: \(finished)")
      })

    // Magic params
    UIView.animateWithDuration(0.4,
      animations: { self.something.origin.y += 10 },
      completion: { println("finished: \($0)") })

    // Omit params if they are unused
    UIView.animateWithDuration(0.4,
      animations: { self.something.origin.y += 10 },
      completion: {
        _ in
        println("no params")
      })

    // Trailing function
    UIView.animateWithDuration(0.4,
      animations: { self.something.origin.y += 10 }) {
        _ in
        println("no params")
      }

    // Omit parentheses if called function has only one param and you supply an anonymous function
    // define it
    func some(f:()->()) {
      f()
    }
    // call it!
    some {
      println("success!")
    }

    // Omit return if there is only one statement
    func callee() -> Int {
      return 0
    }
    func callAndPrint(f:()->Int) {
      let i = f()
      pritln(i)
    }
    callAndPrint {
      callee() // Short for "return callee()""
    }
  }
}

struct Steve {

}

enum Enyce {

}

```

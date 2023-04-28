# Workshop 2

## 2.3 Arrays, Dictionaries, and Optionals

1. In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.

```
pub fun main() {
    var favePeople: [String] = ["Tina", "Hugo", "Blake"]
    log(favePeople)
}
```

2. In a script, initialize a dictionary that maps the Strings Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a UInt64 that represents the order in which you use them from most to least. For example, YouTube –> 1, Reddit –> 2, etc. If you’ve never used one before, map it to 0!

```
pub fun main() {
    var socials: {String: UInt64} = {
        "Youtube": 1,
        "Instagram": 2,
        "Reddit": 3, 
        "LinkedIn": 4,
        "Facebook": 5,
        "Twitter": 6
    }
    log(socials)
}
```

3. Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).
The operator unwraps an optional type. If the optional type is nil, it will panic. Otherwise, it returns it's underlying value
For example: 
```
pub fun main() {
    var socials: {String: UInt64} = {
        "Youtube": 1,
        "Instagram": 2,
        "Reddit": 3, 
        "LinkedIn": 4,
        "Facebook": 5,
        "Twitter": 6
    }
    
    var optional: UInt64? = socials["Youtube"]
    var unwrapped: UInt64 = socials["Youtube"]!

    log(optional)
    log(unwrapped)
}
```

4. Using this picture below, explain…
    - What the error message means
    The actual return type and the expected return type is different.
    - Why we’re getting this error
     The code is expecting a String to be returned, but thing[0x03] is an Optional
    - How to fix it
    We can force unwrap `thing[0x03]` as `thing[0x03]!` before returning it

## 2.4 Basic Structs

```
pub contract Jobsite {
  
  pub var employees: {String: Job} 


  pub struct Job {
    pub let position: String 
    pub let description: String 
    pub let pay: Int

    init(_position: String, _description: String, _pay: Int) {
      self.position = _position
      self.description = _description
      self.pay = _pay
    }
  }

  pub fun addJob(position: String, description: String, pay: Int, name: String) {
    var new = Job(_position: position, _description: description, _pay: pay)
    self.employees[name] = new
    log(self.employees)
  }

  init() {
    self.employees = {}
  }

}
```

```
import Jobsite from 0x01

transaction(position: String, description: String, pay: Int, name: String) {

  prepare(signer: AuthAccount) {}

  execute{
    Jobsite.addJob(position: position, description: description, pay: pay, name: name)
    log(Jobsite.employees)
  }
}
```


```
import Jobsite from 0x01

pub fun main(name: String): Jobsite.Job {
    return Jobsite.employees[name]!
}
```
---
tags:
  - "#test"
  - devops
---

## Mocking independent methods

```cpp
// Original Class
class A {
	A() = default;
	~A() = default;

	double Foo(const std::string& name) const;
	int Bar(int num);	
}
```

```cpp
// Mocked Class
class MockA {
	MockA() = default;
	~MockA() = default;

	MOCK_METHOD(double, Foo, (const std::string&), (const));	
	MOCK_METHOD(int, Bar, (num), );	
}
```

```c++
# Usage (in a test for example)
int my_int;

// Instantiate the Mock class
MockA my_obj;

// Implement the behavior of the mocked object
// In this case I'm saying that it will be called only once and it will return `my_int`
EXPECT_CALL(my_obj, Bar)
.WillOnce(testing::Return(my_int));

// Whenever this method is called we'll let the original implementation run
ON_CALL(my_obj, Foo)
// We call the default implementation of the function
.WillByDefault([&my_obj](const std::string& name) -> double {
	return my_obj.A::Foo(name);
});
```


## Mocking the call of a method inside another

```cpp
class B {
	B() = default;
	~B() = default;

	double Foo(const std::string& name) const {
		// something 
		b = Bar(a);
		// something 
	}
	int Bar(int num);	
}
```

> [!Question]
>How to I use a mocked version of `B::Bar` inside `B:Foo`? 

I can't (I think). But with a slight modification I make it possible:

```cpp
class B {
	B() = default;
	~B() = default;

	double Foo(const std::string& name) const {
		// something 
		b = Bar(a);
		// something 
	}
	virtual int Bar(int num);	
}
```

```cpp
// Mocked Class
class MockA {
	MockA() = default;
	~MockA() = default;

	MOCK_METHOD(int, Bar, (num), (override));	
}
```

```c++
# Usage (in a test for example)
int my_int;

// Instantiate the Mock class
MockA my_obj;

// Implement the behavior of the mocked object
// In this case I'm saying that it will be called only once and it will return `my_int`
EXPECT_CALL(my_obj, Bar)
.WillOnce(testing::Return(my_int));
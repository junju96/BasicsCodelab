# BasicsCodelab

08.22: https://developer.android.com/codelabs/jetpack-compose-basics?hl=zh-cn#7

# TODO
* 属性委托的理解与使用；
* 放到

# Notes
## 属性委托
在 Kotlin 中，属性委托是一种强大的语言特性，它允许您将属性的读取和写入操作委托给另一个对象，从而实现更灵活和可重用的代码。属性委托是一种在不改变类的结构的情况下，为属性添加特定行为的方式。

属性委托由两个主要的角色组成：委托者（Delegate）和接收者（Delegate Holder）。委托者负责处理属性的实际读取和写入操作，而接收者则持有委托者对象并将属性的操作委托给它。

Kotlin 中的属性委托可以通过使用 `by` 关键字来实现，例如：`val/var propertyName: PropertyType by delegateInstance`。

以下是一些属性委托的常见用途和示例：

1. **延迟初始化属性**：
   使用 `lazy` 属性委托，您可以在首次访问属性时进行初始化，避免不必要的初始化开销。

```kotlin
val expensiveCalculationResult: Int by lazy {
    // Expensive calculation
    // This block will only be executed once, on the first access
    // The result will be cached for subsequent accesses
    42
}
```

2. **属性修改监听**：
   通过自定义委托者，您可以在属性值被修改时触发相应的操作，比如通知其他部分或执行验证。

```kotlin
class ObservableProperty<T>(initialValue: T) {
    var value: T = initialValue
        set(newValue) {
            field = newValue
            // Perform actions on value change
            // Notify observers, log changes, etc.
        }
}

var myProperty: String by ObservableProperty("initial")
```

3. **属性缓存**：
   您可以使用委托来实现属性的缓存，从而在多次访问属性时避免重复计算。

```kotlin
class CachedProperty<T>(private val compute: () -> T) {
    private var cachedValue: T? = null
    operator fun getValue(thisRef: Any?, property: KProperty<*>): T {
        if (cachedValue == null) {
            cachedValue = compute()
        }
        return cachedValue as T
    }
}

val expensiveResult: Int by CachedProperty { computeExpensiveValue() }
```

4. **自定义属性存储**：
   通过实现自己的委托者类，您可以为属性提供自定义的存储和访问行为。

```kotlin
class CustomDelegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name}' in $thisRef.")
    }
}

var delegatedProperty: String by CustomDelegate()
```

总之，属性委托是一种强大的编程概念，允许您将属性的读写操作分离出来，实现更好的代码复用和灵活性。通过使用内置的委托，或者自定义委托者，您可以在属性上添加自定义的行为，以满足不同的业务需求。


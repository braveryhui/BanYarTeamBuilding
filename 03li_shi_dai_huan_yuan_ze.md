# 03里氏代换原则

继承对于OCP，就相当于多态性对于里氏替换原则(Liskov Substitution Principle，LSP)。
LSP 规定：用超类代替应用程序中使用的对象时，应当不会破坏应用程序。这通常也被称
为“契约式设计(design by contract)”。
回想前面的多态性示例，ComputePay 方法使用了Employee 类型的列表，其中Employee
就是基类型(超类型)。Salary、Hourly 和Seasonal 类都是从Employee 继承而来，因此它们
是Employee 的子类型。
根据LSP，即使已经将列表声明为Employee 的列表，也仍然可以用Salary、Hourly
和Seasonal 的具体实例来填充它。因为有了继承，它们都支持Employee 声明的相同契约(公
共的方法集或API)。应用程序可以对该列表进行迭代，并调用那些在列表中各个项目的
Employee 上定义的方法，不需要知道或特别关心它们都是什么类型。如果它们支持契约，
该调用就是合法的。
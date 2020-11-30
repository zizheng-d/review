## Q：什么是IOC？

A： 
		IOC ( Inversion of Control 控制反转) 并不是一种很厉害的技术，它只是一种设计思想。想要理解IOC就得解决两个问题
问题1: 控制什么？
		没有IOC的时候我们创建对象是需要通过new关键字来创建并获取这个对象的。在引入IOC后对象则是由IOC来创建的，当我们需要使用某个对象的时候，去IOC的容器中即可拿到相关的对象。也就是说IOC控制了对象的创建和对象的获取。
问题2: 反转什么？
		拿Controller中注入Service来举例，在传统编码过程中，如果没有IOC容器，则需要编码人员自己去创建并将创建的对象并注入到Controller中；在引入IOC的情况下，只需要告诉IOC我需要一个什么样的对象即可将该对象注入到Controller中。前者属于正转，我们控制Controller在什么时间主动的去获取Service这个对象，这种情况属于主动注入；后者属于反转，由IOC容器来决定Service对象什么时候注入到Controller中，这种情况属于被动注入。
		总的来说，没有IOC的时候Controller直接依赖Service。当引入IOC的时候，Controller依赖的是IOC容器，IOC负责去创建和注入Service对象。

## Q：什么是依赖注入（DI）？

A：
		DI 即指Dependence Injection，是IOC的另一个角度的描述。依赖注入指的是，当对象A需要某个对象B才能正常执行的时候，IOC容器会在适当的时间将对象B创建出来，并将对象B动态的注入到对象A当中。这也是IOC的核心之一，在程序运行的时候，动态的向某个对象提供该对象所需要的对象。这就是依赖注入。

## Q：IOC的注入方式有哪些？分别都是怎样实现的？

A：

1.  属性注入，即通过类中的Setter方法来注入。在XML文件中需要在<bean>标签下使用<property name="" value="">标签来进行注入

  		2. 构造器注入，即通过类中的构造方法来注入。在XML文件中需要在<bean>标签下使用<constructor-arg value="" index="">标签来进行注入
    		3. 静态工厂注入，静态工厂直接通过调用工厂的静态方法就可以获得需要的对象
      		4. 实例工厂注入，实例工厂需要对工厂进行实例化之后，通过调用工厂对象的获取对象的方法才能获取到所需要的对象

## Q：IOC的优缺点

A：
		优点：实现了程序各个模块之间的解藕，提高了程序的灵活性和可维护性
		缺点：由于IOC创建对象是基于Java的反射来实现的，所以在一定程度上增加了系统的开销，降低了程序的运行效率。
				   创建对象变得复杂，没有IOC的时候可能只需要在需要的地方使用new创建对象即可，但是使用IOC之后创建获取对象需要从IOC中获取，创建对象变的不再那么直观。
				   在Spring实现的IOC中，由于IOC容器创建的对象需要XML文件的支持，所以当类名发生变化时，需要对应的去修改XML文件。当开启包扫描使用注解的时候，该项缺点则不存在。

## Q：IOC中创建的对象有哪些作用域？

A：
		singleton - 单例模式，在任何情况下都只会创建一个对象。所有需要该对象的地方访问的都是同一个对象
		prototype - 多例模式，每次从IOC获取需要的对象的时候，都会返回一个新的对象
		request - 该模式仅对HTTP请求有效，每有一个新的HTTP请求的时候都会创建一个新的对象
		session - 该模式仅对HTTP Session有效，当HTTP请求属于同一个Session时，那么这个时候访问的都是同一个对象
		global-session - 对所有的Session都返回同一个对象。因为Session也有作用域（page、request、session、application），所以该模式不会去管Session的作用域，从而创建一个全局Session都可以访问的一个对象。

## Q：Spring中是如何实现IOC的？

A：
		Spring实现的IOC本质上就是一个BeanFactory来实现的。我们平时使用或者学习的时候使用ClassPathApplicationContext或者FileSystemApplicationContext这两个类都间接的实现了ApplicationContext接口。而ApplicationContext接口又继承了BeanFactory接口。通过ClassPathApplicationContext的构造函数可以看到，在ClassPathApplicationContext的构造函数中使用了refresh()方法。refresh方法中包含了从ClassPath文件夹中读取XML、将XML中配置的Bean解析到BeanFactory中。然后BeanFactory通过反射再来创建对象并注册到容器中。注意refresh方法，在Spring其他的源码中初始化的方法一般使用init。但是这里使用了refresh，也就是说在我们使用的过程中，我们可以通过ClassPathApplicationContext.refresh()来刷新容器内的对象。
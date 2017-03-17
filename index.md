## Welcome to My GitHub Pages

### Junit的断言
JUnit为我们提供了一些辅助函数，他们用来帮助我们确定被测试的方法是否按照预期的效果正常工作，通常，把这些辅助函数称为断言。
断言为Assert类，使用时需静态导入测试类中，可直接使用一下方法：
1、assertArrayEquals(expecteds, actuals)    查看两个数组是否相等。
2、assertEquals(expected, actual)                 查看两个对象是否相等。类似于字符串比较使用的equals()方法
3、assertNotEquals(first, second)                  查看两个对象是否不相等。
4、assertNull(object)                                    查看对象是否为空。 
5、assertNotNull(object)                               查看对象是否不为空。
6、assertSame(expected, actual)                  查看两个对象的引用是否相等。类似于使用“==”比较两个对象
7、assertNotSame(unexpected, actual)         查看两个对象的引用是否不相等。类似于使用“!=”比较两个对象
8、assertTrue(condition)                              查看运行结果是否为true。
9、assertFalse(condition)                             查看运行结果是否为false。
10、assertThat(actual, matcher)                   查看实际值是否满足指定的条件
11、fail()                                                    让测试失败                    
### Junit的注解

@Test还有两种特殊的参数
1、@Test(expected=XXX.class) ：测试异常
2、@Test(timeout=1000) :测试超时
### Junit的测试套件
测试套件就是组织测试类一起运行的，一个作为测试套件的入口类，不能包含其他的方法，更改测试运行器Suite.class，将要测试的类作为数组传入到Suite.SuiteClasses({})
junit的suite类为提供给我们的测试套件类，使用@RunWith(Suite.class)和@Suite.SuiteClasses({Class1,Class2,Class3...})
```java
@RunWith(Suite.class)
@Suite.SuiteClasses({CalculatorTest.class,CalculatorTest2.class})
public class JunitTestCase {}
```
### Junit的参数化设置
Junit提供了参数化测试套件
1、更改默认的测试运行器（在测试类名上方加入@RunWith(Parameterized.class)）
2、声明变量来存放预期值和结果值
3、声明一个返回值为Collection的公共静态方法，并使用@Parameters进行修饰
4、为测试类声明一个带有参数的公共构造函数，并在其中为之声明变量赋值
```java
import main.Calculator;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import static org.junit.Assert.*;
import java.util.Arrays;
import java.util.Collection;        
@RunWith(Parameterized.class)
public class ParametersTest {
    int expected = 0; int input1 = 0; int input2 = 0;
    @Parameterized.Parameters
    public static Collection<Object[]> t(){
        return Arrays.asList(new Object[][]{{3,1,2},{4,2,2}});
    }
    public ParametersTest(int expected, int input1, int input2){
        this.expected=expected;this.input1=input1;this.input2=input2;
    }
    @Test
    public void testAdd(){
        assertEquals(expected,new Calculator().add(input1, input2));
    }
}
```

### Junit的简单例子
首先我先创建个Calculator 的计算类，代码如下，里面有3个方法，分别是两个数相加、两个数相乘、两个数相除；
```java
public class Calculator {
    void calculator(){}
    public int add(int a, int b){
        return  a+b;
    }
    public int multiply(int a, int b){
        return  a*b;
    }
    public int dividor(int a, int b){
        return  a/b;
    }
}```
然后再idea中的操作右键这个类，点击


idea就会给你再test文件夹中同级目录下生成CalculatorTest.java
```java
package test.main;
import org.junit.Test; 
import org.junit.Before; 
import org.junit.After;
public class CalculatorTest {
    @Before
    public void before() throws Exception {}
    @After
    public void after() throws Exception {}
    @Test
    public void testCalculator() throws Exception {
    //TODO: Test goes here...
    }
    @Test
    public void testAdd() throws Exception {
    //TODO: Test goes here...
    }
    @Test
    public void testMultiply() throws Exception {
    //TODO: Test goes here...
    }
    @Test
    public void testDividor() throws Exception {
    //TODO: Test goes here...
    }
} 
```
我们可以编写测试代码，已验证编写的方法
```java
public class CalculatorTest {
    @Before
    public void before() throws Exception {System.out.println("TEST is Begin!");}
    @After
    public void after() throws Exception {System.out.println("TEST is over!");}
    @Test
    public void testAdd() throws Exception {
        Assert.assertEquals("加法测试失败，返回值与预期值不一致1",5,new Calculator().add(2,2),0);
        Assert.assertNotEquals("加法测试失败，返回值与预期值不一致2",4,new Calculator().add(2,2),0.0000);
    }
    @Test
    public void testAddZero() throws Exception{
        Assert.assertEquals("加法测试失败，返回值与预期值不一致3",2,new Calculator().add(1,0),0);
        Assert.assertEquals("加法测试失败，返回值与预期值不一致4",1,new Calculator().add(2,-1),0);
    }
    @Test
    public void testMultiply() throws Exception {
        Assert.assertEquals("乘法测试失败，返回值与预期值不一致1",6,new Calculator().multiply(2,3),0);
        Assert.assertEquals("乘法测试失败，返回值与预期值不一致2",6,new Calculator().multiply(2,2),0);
    }
    @Test
    public void testDividor() throws Exception {
        Assert.assertEquals("除法测试失败，返回值与预期值不一致1",2,new Calculator().dividor(6,3),0);
        Assert.assertEquals("除法测试失败，返回值与预期值不一致2",3,new Calculator().dividor(6,2),0);
    }
    @Test(expected = ArithmeticException.class)
    public void testDividorZero() throws Exception {
        Assert.assertEquals("除法测试失败，除0未抛出异常",2,new Calculator().dividor(6,0),0);
    }
} 
```

此处有个问题，细心的以及发现了测试方法并不是按照我们代码顺序执行，后续我们来讲这个问题。

###Junit测试用例的failure和error
1、failure一般由单元测试使用的断言方法判断失败引起的，这表示测试点发现了问题，就是说程序输出的结果与我们预期的不一样
2、error是由代码异常引起的，它可能产生于测试代码本身的错误，也可能是被测试代码中的一个隐藏的bug
3、测试用例不是用来证明你是对的，而是用来证明你没有错，测试用例用来达到想要的预期结果，但对于逻辑错误无能为力
测试代码见下
```java
public class CalculatorTest2 {
    @Test
    public void testAdd() throws Exception {
        assertEquals(5,new Calculator().add(2,1),0);
    }
    @Test
    public void testDividor() throws Exception {
        assertEquals(2,new Calculator().dividor(6,0),0);
    }
}
```
idea运行后，会出现以下结果
其中黄色表示failure，红色表示error

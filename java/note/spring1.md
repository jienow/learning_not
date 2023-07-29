## 自动装配bean

#### autowire

autowire="byName"会自动在容器内查找，和自己对象set方法后面对顶的beanid

```xml
<bean id="people" class="com.jienow.pojo.People">
    <property name="name" value="zhangsan"/>
    <property name="cat" ref="cat"/>
    <property name="dog" ref="dog"/>
</bean>
```

**注意：一定得是public void setCat(...){...}里面的那个Cat,才能做‘<property’ 里面的那个name的名字，而且他把大小写也给改了**

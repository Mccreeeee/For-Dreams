## 关于MyBatis 3使用时产生的Bug解析

#### 1、使用Mybatis进行数据库数据到实体pojo的映射

遇到的问题：pojo类中存在setter方法和有参构造函数，不存在无参构造函数，其中有参构造函数的参数顺序与数据库列名顺序不同

![user](C:\Users\huqingyu18\Desktop\user.png)

![userdb](C:\Users\huqingyu18\Desktop\userdb.png)

当getuser的时候发现报错，无法正常映射，于是深入剖析，发现关键点在createResultObject函数，如下图：

![createResultObject](C:\Users\huqingyu18\Desktop\createResultObject.png)

发现当不存在默认构造器的时候将会调用createByConstructorSignature函数：

```java
private Object createByConstructorSignature(ResultSetWrapper rsw, Class<?> resultType, List<Class<?>> constructorArgTypes, List<Object> constructorArgs,
      String columnPrefix) throws SQLException {
    for (Constructor<?> constructor : resultType.getDeclaredConstructors()) {
      if (typeNames(constructor.getParameterTypes()).equals(rsw.getClassNames())) {
        boolean foundValues = false;
        for (int i = 0; i < constructor.getParameterTypes().length; i++) {
          Class<?> parameterType = constructor.getParameterTypes()[i];
          String columnName = rsw.getColumnNames().get(i);
          TypeHandler<?> typeHandler = rsw.getTypeHandler(parameterType, columnName);
          Object value = typeHandler.getResult(rsw.getResultSet(), prependPrefix(columnName, columnPrefix));
          constructorArgTypes.add(parameterType);
          constructorArgs.add(value);
          foundValues = value != null || foundValues;
        }
        return foundValues ? objectFactory.create(resultType, constructorArgTypes, constructorArgs) : null;
      }
    }
    throw new ExecutorException("No constructor found in " + resultType.getName() + " matching " + rsw.getClassNames());
  }
```

关键点就在这个for循环，它默认拿构造器参数的顺序和取数据库列的顺序一样的，但是我们的构造器的参数实际上顺序与数据库列不同，那么就会导致在getTypeHandler函数执行的之后找不到真正能够正常对应映射的typeHandler（初步猜测是选择了ObjectTypeHandler），于是后续的getResult继续进行，到了objectFactory.create的时候，猜测是对应参数类型进行转换赋值的时候，然后发现我们的有参构造器中数据库列名为imageUrl和参数score无法进行转换，于是抛出了NumberFormatException，因个人技术原因，无法继续深入研究，仅做理解和参考，后续再回来深入

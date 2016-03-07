# 根据注解注入值
``` java
	 private Map getPojoFiledMap() throws NoSuchMethodException {
        //得到目标类的所有字段列表
        Field[] field = clazz.getDeclaredFields();
        //将所有标有指定annotation的字段，存放到一个map中
        Map fieldMap = new HashMap();
        //循环读取所有字段
        for (int i = 0; i < field.length; i++) {
            Field f = field[i];
            //得到单个字段上的Annotation，Attribute是我指定的注解
            Attribute exa = f.getAnnotation(Attribute.class);
            //如果标识了Annotation的话
            if (null != exa) {
                //构造设置了Annotation的字段的setter方法
                String fieldName = f.getName();
                String setMethodName = "set" + fieldName.substring(0, 1).toUpperCase()
                        + fieldName.substring(1);
                //构造调用的method
                Method method = clazz.getMethod(setMethodName, new Class[]{
                        f.getType()
                });
                //将这个method以annotation的名字为key来存入
                if (!exa.name().equals("")) {
                    fieldMap.put(exa.name(), method);
                } else {
                    fieldMap.put(fieldName, method);
                }
            }
        }
        return fieldMap;
    }
```
使用的方式如下：
``` java
	//t是annotation中的变量值
	if (fieldMap.containsKey(t)) {
         Method setMethod = (Method) fieldMap.get(t);
         Type[] ts = setMethod.getGenericParameterTypes();
         //只要一个参数
         String xClass = ts[0].toString();
         //判断参数类型
         if (xClass.equals("class java.lang.String")) {
	         //真正的注入值
		     setMethod.invoke(tObject, "aaaa");
	     }
    }
```
POJO类：
``` java
	public class Person implements ObjectModel {

	    @Attribute(name = "uid")
	    private String uid;
	    
	    public String getUid() {
	        return uid;
	    }
	
	    public void setUid(String uid) {
	        this.uid = uid;
	    }

   }
```


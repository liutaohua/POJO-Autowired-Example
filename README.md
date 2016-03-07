# ����ע��ע��ֵ
``` java
	 private Map getPojoFiledMap() throws NoSuchMethodException {
        //�õ�Ŀ����������ֶ��б�
        Field[] field = clazz.getDeclaredFields();
        //�����б���ָ��annotation���ֶΣ���ŵ�һ��map��
        Map fieldMap = new HashMap();
        //ѭ����ȡ�����ֶ�
        for (int i = 0; i < field.length; i++) {
            Field f = field[i];
            //�õ������ֶ��ϵ�Annotation��Attribute����ָ����ע��
            Attribute exa = f.getAnnotation(Attribute.class);
            //�����ʶ��Annotation�Ļ�
            if (null != exa) {
                //����������Annotation���ֶε�setter����
                String fieldName = f.getName();
                String setMethodName = "set" + fieldName.substring(0, 1).toUpperCase()
                        + fieldName.substring(1);
                //������õ�method
                Method method = clazz.getMethod(setMethodName, new Class[]{
                        f.getType()
                });
                //�����method��annotation������Ϊkey������
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
ʹ�õķ�ʽ���£�
``` java
	//t��annotation�еı���ֵ
	if (fieldMap.containsKey(t)) {
         Method setMethod = (Method) fieldMap.get(t);
         Type[] ts = setMethod.getGenericParameterTypes();
         //ֻҪһ������
         String xClass = ts[0].toString();
         //�жϲ�������
         if (xClass.equals("class java.lang.String")) {
	         //������ע��ֵ
		     setMethod.invoke(tObject, "aaaa");
	     }
    }
```
POJO�ࣺ
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


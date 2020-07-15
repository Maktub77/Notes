JSON解析 -- 插件

1. Json -lib
2. Gson(google)
3. FastJSON(推荐 -- alibaba)
4. JsckSon

### FastJSON

```java
Daily d = new Daily(1,"今天完成房屋发布",new Date(),null);
		
//将java对象转换为json字符串
String json = JSON.toJSONString(d);
System.out.println(json);

//json字符串转换为json对象
JSONObject jo = JSON.parseObject(json);
String content = jo.getString("content");
System.out.println(content);

//将json字符串转为java对象
JSON.parseObject(json,Daily.class);
System.out.println(d);

//将一个集合转换为json字符串
Daily d1 = new Daily(1,"今天完成项目搭建",new Date(),null);
Daily d2 = new Daily(1,"今天完成登录注册、表单验证",new Date(),null);
Daily d3 = new Daily(1,"今天完成房屋发布",new Date(),null);
Daily d4 = new Daily(1,"今天打酱油！",new Date(),null);

List<Daily> list = new ArrayList<>();
list.add(d1);
list.add(d2);
list.add(d3);
list.add(d4);

//转换
json = JSON.toJSONString(list);
System.out.println(json);

//将一个json字符串解析为java集合
list = JSON.parseArray(json,Daily.class);
System.out.println(list);
```

### Gson

```java
Daily d = new Daily(1,"今天完成房屋发布",new Date(),null);

Gson gson = new Gson();

//将java对象转换为json字符串
String json = gson.toJson(d);
System.out.println(json);

//将json字符串转换为java对象
d = gson.fromJson(json, Daily.class);
System.out.println(d);

//将一个集合转换为json字符串
Daily d1 = new Daily(1,"今天完成项目搭建",new Date(),null);
Daily d2 = new Daily(1,"今天完成登录注册、表单验证",new Date(),null);
Daily d3 = new Daily(1,"今天完成房屋发布",new Date(),null);
Daily d4 = new Daily(1,"今天打酱油！",new Date(),null);

List<Daily> list = new ArrayList<>();
list.add(d1);
list.add(d2);
list.add(d3);
list.add(d4);

//转换
json = gson.toJson(list);
System.out.println(json);

//将一个json字符串解析为java集合
list = gson.fromJson(json, new TypeToken<List<Daily>>(){

}.getClass());
System.out.println(list);
```


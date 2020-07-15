#### EasyExcel的简单使用

1. 导入jar包到项目中

   ```groovy
   dependencies {
       compile('com.alibaba:easyexcel:2.2.3')
   }
   ```

2. 对应excel的实体类

   ```java
   package com.sync.entity;
   
   import com.alibaba.excel.annotation.ExcelProperty;
   
   public class Dept {
   
       @ExcelProperty(index = 0)
       private String name;
   
       @ExcelProperty(index = 1)
       private String deptName;
   
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public String getDeptName() {
           return deptName;
       }
   
       public void setDeptName(String deptName) {
           this.deptName = deptName;
       }
   }
   ```

3. 同步接口

   ```java
   @RestController
   @RequestMapping("sync")
   public class SyncController {
   
       @Autowired
       DeptService deptService;
   
       @Value("${filePath}")
       String filePath;
   
       @GetMapping("/dept")
       public R syncDept(){
           try {
               InputStream is = new FileInputStream(filePath);
               AnalysisEventListener listener = new ExcelDeptDataListener();
               ExcelReader excelReader = EasyExcel.read(is, ExcelDept.class,listener).build();
               ReadSheet readSheet = EasyExcel.readSheet(0).build();
               excelReader.read(readSheet);
               excelReader.finish();
           } catch (FileNotFoundException e) {
               e.printStackTrace();
           }
           return R.ok();
       }
   }
   ```

4. 对应listenter

   ```java
   
   ```

5. listenter获取service

   ```java
   
   ```

   
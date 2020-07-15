### ` equals`

- `equals(Object obj)` 指示其他某个对象是否与此对象“相等”。 

  - 一般实体类需要使用object的equals方法，都需要重写

    ```java
    public boolean equals(Object obj) {
        //判断是否为同一类事物
        if(obj instanceof People){ 
            //将obj强转为people
            People p = (People)obj; 
            //判断属性值是否相等
            if(this.name == p.name && this.age == p.age){ 
                return true;
            }else{
                return false;
            }		
        }else{
            return false;
        }
    }
    ```

### `finalize`

- `finalize()`  当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。

  - 当一个对象被释放时，这个方法被JVM自动调用

    ```java
    protected void finalize() throws Throwable {
        System.out.println("已清空--------------");
        super.finalize();
    }
    ```

  - ```java
    p1 = null;
    p2 = null;
    System.gc();
    ```

### `toString`

- `toString()` 返回该对象的字符串表示。（使用Alt + Shift + S创建`toString`）

  - `toString`方法一般是输出这个对象的详细信息（show）

  - object方法默认输出地址，实体类使用的时候需要重写

    ```java
    public String toString() {
        return "People [name=" + name + ", age=" + age + "]";
    }
    ```

  - ```java
    System.out.println( p1.toString() );
    System.out.println( p1 );
    ```

### `hashCode`

- `hashCode()`返回该对象的哈希码值。

  - ```java
    //重写hsdhCode方法
    @Override
    public int hashCode() {
        return super.hashCode();
    }
    ```

  - `System.out.println( p1.hashCode());`

### `clone`

- `clone()`创建并返回此对象的一个副本。

  - clone方法使用时类必须实现`Cloneable`接口，否则会报`CloneNotSupportedException`克隆异常

  - 使用时clone时要放在一个方法里面

  - ```java
    public class People implements Cloneable{
        public void b(){
            try {
                Object o = clone();
                if(o instanceof People){
                    People p = (People)o;
                    System.out.println(p.age);
                }else{
                    System.out.println("不属于");
                }
            } catch (CloneNotSupportedException e) {
                e.printStackTrace();
            }
        }
    }
    ```

  - `p1.b();`
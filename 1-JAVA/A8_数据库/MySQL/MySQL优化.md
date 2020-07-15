### MySQL优化

1. **explain**

   * 善用explain查看优化sql执行计划
     * **type列**，连接类型。一个好的SQL语句至少要达到range级别。杜绝出现all级别。
     * **key列，**使用到的索引名。如果没有选择索引，值是NULL。可以采取强制索引方式。
     * **key_len列，**索引长度。
     * **rows列，**扫描行数。该值是个预估值。
     * **extra列，**详细说明。注意，常见的不太友好的值，如下：Using filesort，Using temporary。

2. **SQL语句中in包含的值不应过多**

   * MySQL对于IN做了相应的优化，即将IN中的常量全部存储在一个数组里面，而且这个数组是排好序的。但是如果数值较多，产生的消耗也是比较大的。再例如：select id from t where num in(1,2,3) 对于连续的数值，**能用between就不要用in**了；再或者使用连接来替换。 

3. **select语句务必指明字段名称**

   * SELECT*增加很多不必要的消耗（CPU、IO、内存、网络带宽）；增加了使用覆盖索引的可能性；当表结构发生改变时，前断也需要更新。所以要求直接在select后面接上字段名。 

4. **当只需要一条数据的时候，使用limit 1**

   * 这是为了使EXPLAIN中type列达到const类型 

5. **如果排序字段没有用到索引，尽量少排序**

6. **如果限制条件中其他字段没有索引，尽量少用or**

   * or两边的字段中，如果有一个不是索引字段，而其他条件也不是索引字段，会造成该查询不走索引的情况。很多时候使用union all或者是union（必要的时候）的方式来代替“or”会得到更好的效果。 

   > UNION 操作符用于合并两个或多个 SELECT 语句的结果集。 

7. **尽量用union all代替union**

   * union和union all的差异主要是前者需要将结果集合并后再进行唯一性过滤操作，这就会涉及到排序，增加大量的CPU运算，加大资源消耗及延迟。当然，union all的前提条件是两个结果集没有重复数据。 

8. **不使用order by rand()**

   ```sql
   select id from `dynamic` order by rand() limit 1000;
   ```

   优化

   ```sql
   select id from `dynamic` t1 join (select rand() * (select max(id) from `dynamic`) as nid) t2 on t1.id > t2.nidlimit 1000;
   ```

9. **区分in 和 exists、not in和not exists**

   * in语句
     * 只执行一次
     * 确定给定的值是否与子查询或列表中的值相匹配。in在查询的时候，首先查询子查询的表，然后将内表和外表做一个笛卡尔积，然后按照条件进行删选。所以相对内表比较小的时候，in的速度较快。

     ```sql
     select * from student s where s.stuid in(select stuid from score ss where ss.stuid = s.stuid)
     ```

   * exist语句

     * 执行table.length次
     * 指定一个子查询，检测行的存在。遍历循环外表，然后看外表中的记录有没有和内表的数据一样的。匹配上就将结果放入结果集中。

     ```sql
     select * from student s where EXISTS(select stuid from score ss where ss.stuid = s.stuid)
     ```

     执行结果相同，执行流程不同

   * in 和 exists的区别: 

     - 如果子查询得出的结果集记录较少，主查询中的表较大且又有索引时应该用**IN**。
     - 如果外层的主查询记录较少，子查询中的表大，又有索引时使用**exists**。
     - **EXISTS**以外层表为驱动表，先被访问
     - **IN**，那么先执行子查询，所以我们会以驱动表的快速返回为目标，那么就会考虑到索引及结果集的关系了 
     - IN时不对NULL进行处理。
     - in 是把外表和内表作hash 连接
     - exists是对外表作loop循环，每次loop循环再对内表进行查询。一直以来认为exists比in效率高的说法是不准确的。

   * not in 和not exists

   ​	如果查询语句使用了not in 那么内外表都进行全表扫描，没有用到索引；而not extsts 的子查询依然能用到表上的索引。所以无论那个表大，用not exists都比not in要快。

10. **使用合理的分页方式以提高分页效率**

    ```sql
    select id,name from product limit 866613, 20
    ```

    随着表数据量的增加，直接使用limit分页查询会越来越慢。 

    优化

    可以取前一页的最大行数的id，然后根据这个最大的id来限制下一页的起点。比如此列中，上一页最大的id是866612。SQL可以采用如下的写法： 

    ```sql
    select id,name from product where id> 866612 limit 20
    ```

11. **分段查询**

    * 在一些用户选择页面中，可能一些用户选择的时间范围过大，造成查询缓慢。主要的原因是扫描行数过多。这个时候可以通过程序，分段进行查询，循环遍历，将结果合并处理进行展示。

12. **避免在where子句中对字段进行null值判断**

    * 对于null的判断会导致引擎放弃使用索引而进行全表扫描。 

13. **不建议使用%前缀模糊查询**

    * 使用全文索引。

      在我们查询中经常会用到select id,fnum,fdst from dynamic_201606 where user_name like '%zhangsan%'; 。这样的语句，普通索引是无法满足查询需求的。庆幸的是在MySQL中，有全文索引来帮助我们。

      创建全文索引的SQL语法是：

      ```sql
        ALTER TABLE `dynamic_201606` ADD FULLTEXT INDEX `idx_user_name` (`user_name`);
      ```

      使用全文索引的SQL语句是：

      ```sql\
      select id,fnum,fdst from dynamic_201606 where match(user_name) against('zhangsan' in boolean mode);
      ```

      **注意：**在需要创建全文索引之前，请联系DBA确定能否创建。同时需要注意的是查询语句的写法与普通索引的区别。

14. **避免在where子句中对字段进行表达式操作**

    ```sql
    select user_id,user_project from user_base where age*2=36;
    ```

    * 中对字段就行了算术运算，这会造成引擎放弃使用索引，建议改成：

    ```sql
    select user_id,user_project from user_base where age=36/2;
    ```

15. **避免隐式类型转换**

    * where子句中出现column字段的类型和传入的参数类型不一致的时候发生的类型转换，建议先确定where中的参数类型。

16. **对于联合索引来说，要遵循最左前缀发则**

    * 举列来说索引含有字段id、name、school，可以直接用id字段，也可以id、name这样的顺序，但是name;school都无法使用这个索引。所以在创建联合索引的时候一定要注意索引字段顺序，常用的查询字段放在最前面。

17. **必要时可以使用force index来强制查询走某个索引**

    * 有的时候MySQL优化器采取它认为合适的索引来检索SQL语句，但是可能它所采用的索引并不是我们想要的。这时就可以采用forceindex来强制优化器使用我们制定的索引。

18. **注意查询范围查询语句**

    * 对于联合索引来说，如果存在范围查询，比如between、>、<等条件时，会造成后面的索引字段失效。 

19. **关于join优化**

    ![img](https://mmbiz.qpic.cn/mmbiz_jpg/tibrg3AoIJTsfu3nd3L2RGjl6VO7yxISPcKk5H7XnmAaZzuhtOJ4ODpsuBhmBNCic1aF8t9nITOxDRYsRa3A10ew/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

* LEFT JOIN A表为驱动表，INNER JOIN MySQL会自动找出那个数据少的表作用驱动表，RIGHT JOIN B表为驱动表。

  **注意：**

  **1）MySQL中没有full join，可以用以下方式来解决：**

  select * from A left join B on B.name = A.namewhere B.name is nullunion allselect * from B;

  **2）尽量使用inner join，避免left join：**

  参与联合查询的表至少为2张表，一般都存在大小之分。如果连接方式是inner join，在没有其他过滤条件的情况下MySQL会自动选择小表作为驱动表，但是left join在驱动表的选择上遵循的是左边驱动右边的原则，即left join左边的表名为驱动表。

  **3）合理利用索引：**

  被驱动表的索引字段作为on的限制字段。

  **4）利用小表去驱动大表：**

  ![img](https://mmbiz.qpic.cn/mmbiz_jpg/tibrg3AoIJTsfu3nd3L2RGjl6VO7yxISPbiag8y4LggbC1B0VSIAFyoUWeOPzb4dsjdhlz7NAV7zSyFhmtF77xyw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  从原理图能够直观的看出如果能够减少驱动表的话，减少嵌套循环中的循环次数，以减少 IO总量及CPU运算的次数。

  **5）巧用STRAIGHT_JOIN：**

  inner join是由MySQL选择驱动表，但是有些特殊情况需要选择另个表作为驱动表，比如有group by、order by等「Using filesort」、「Using temporary」时。STRAIGHT_JOIN来强制连接顺序，在STRAIGHT_JOIN左边的表名就是驱动表，右边则是被驱动表。在使用STRAIGHT_JOIN有个前提条件是该查询是内连接，也就是inner join。其他链接不推荐使用STRAIGHT_JOIN，否则可能造成查询结果不准确。

  ![img](https://mmbiz.qpic.cn/mmbiz_jpg/tibrg3AoIJTsfu3nd3L2RGjl6VO7yxISPY46CzOXhvOrlSuFKghXic9EnRot4jD3ibzIFS3DQNcuZOojibXoJcXqYw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
###8.2XML数据的存储和管理方式
* 使用CHAR, VARCHAR或者CLOB, BLOB存储XML
* 将XML数据分解并映射到关系表中进行存储
* 纯XML数据的存储和管理
* 混合方式的数据存储和管理
* 使用CHAR, VARCHAR或者CLOB, BLOB的优点
 + 可以在关系表中专门设置一个CHAR, VARCHAR或CLOB,BLOB大对象列来存储XML数据
 + 可以对层次结构的XML数据进行原封不动地保存，不需要将其中的内容分解并映射到关系表（一个或多个）的多个常规列中进行保存
 + 在获取所存储的XML数据时，无论是查询语句的编写还是具体检索过程也都非常简单，能够保证查询的结果与源文档完全一样，而不需要在关系数据库管理系统中进行复杂的连接运算，因为存储时并没有对源XML文档中的数据进行分解
* 使用CHAR, VARCHAR或者CLOB, BLOB的缺点
 + CHAR, VARCHAR或者CLOB, BLOB大对象数据类型并不是专门针对XML数据设计的
 + 在对文档进行搜索和查询检索时，开销非常大，需要设计新的索引技术提高查询检索的效率
 + 大对象的更新操作也非常耗时，因为数据库并没有对大对象数据进行任何解析，即使修改其中很少的内容，也必须更新所有的数据
* 将XML数据分解并映射到关系表中进行存储的优点
 + 可以将XML文档中的数据（元素和属性）分解并映射到关系表（一个或多个）的多个常规列中进行保存，从而解决前面所描述的、使用大对象存储XML数据时碰到的检索和更新的效率问题
 + XML数据中所蕴含的层次关系在分解时必须采用适当的机制予以保存，以便能够在查询检索之后，重新构造出与源XML文档基本一致的数据
 + 仅使用关系模型，用户可以利用现有的SQL工具和代码来直接访问数据库表中存储的信息，而不需要学习新的层次数据的查询语言、以及相关技术，也不需要引入新的索引机制来处理XML数据
* 将XML数据分解并映射到关系表中进行存储的缺点
 + XML数据中包含了大量嵌套关系、以及一些无规则的结构，在对其进行分解存储时，会产生大量的数据库表，并且表与表之间存在复杂的参照关系（通过主键和外键的关系来保存XML数据之间的层次关系），还可能会使得关系表的规范化难以进行、或者导致关系数据库表中出现大量的空值，这些都增加了数据管理的复杂度
 + 在进行查询检索时，一方面需要编写复杂的SQL查询计划（难以开发、以及对其进行优化），以访问分散的数据；另一方面，在执行查询计划时，需要访问大量的数据库表，并进行非常耗时的连接操作，以恢复分解之前数据之间的层次关系，这将使得查询的效率非常低下
* 纯XML数据的存储和管理的优点
 + 要想真正实现XML数据的高效存储和管理，就必须摆脱关系模型的约束和束缚，设计并实现层次数据库
 + 以层次的格式来存储层次的XML数据（以元素或子树为单位，而不是关系数据库中的表或记录），提供新的查询语言（XQuery），引入新的索引机制（值索引、路径索引），从而避免将层次数据模型映射为关系数据模型所带来的性能、模式管理等方面的问题
* NXD（Native XML Database）打破了RDBMS传统数据库一统天下的局面，国外一些机构和公司针对NXD进行了深入地研究开发。NXD从存储、索引、查询等，都围绕XML的特点和标准进行设计
* NXD没有像关系数据库方法那样繁琐的数据转换，又具有中间件所不具有的数据管理功能，如并发控制、备份和恢复等
* NXD兼有一般数据库的特性，例如支持事务，并发控制，查询语言，安全机制，二次开发接口等。唯一的不同之处在于其内部存储模型是基于XML文档树形结构，而非关系模型
 + 文档集合（Document Collection）
 + 查询语言（Query Language）
 + 事务、锁和并发（Transaction、Lock、Concurrent）
 + 索引（Indexed）
 + 应用程序接口
 + 往返（Round-trip）
* 纯XML数据的存储和管理的缺点
 + NXD的发展也遇到了一些困难，因为无法利用现有的关系数据库中所提供的数据存储、管理、索引、优化、事务处理等成熟的技术，所以必须进行彻底的开发
 + XML数据库的理论基础并不像关系数据库理论那么成熟，XML的相关标准也在不断地发展和完善之中
 + 如何实现遗留的关系数据库和NXD之间的数据整合，也是NXD在实际应用过程中所面临的一个挑战
* XML 数据存储管理的要求
 + 需要为XML 数据的处理和访问提供下列支持
    + 查询功能
    + 为XML 数据创建索引
    + 数据修改功能
    + 模式支持
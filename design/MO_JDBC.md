## MO之JDBC功能说明
### 1、Connection（类）：获取数据库连接对象
  功能：类中的方法
  1.获取执行Sql对象Statement
    Statement createStatement();
    Statement prepareStatement(String sql);

  2.管理事务
    开启事务：setAutoCommit(boolean autoCommit):调用该方法设置参数为false，即开启事务
    提交事务：void commit()
    回滚事务：void rollback()

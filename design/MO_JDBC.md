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


|connection接口中的方法|MO目前支持情况|0.6 MO是否支持|
|:-:|:-:|:-:|
| createStatement() | 已支持 | 支持 |
| prepareStatement(String sql) | 暂不支持 | 不支持 |
| prepareCall(String sql) | 暂不支持 | 不支持 |
| nativeSQL(String sql) | 暂不支持 | 不支持 |
| setAutoCommit(boolean autoCommit) | 暂不支持 | 不支持 |
| getAutoCommit() | 暂不支持 | 不支持 |
| commit() | 暂不支持 | 不支持 |
| rollback() | 暂不支持 | 不支持 |
| close() | 暂不支持 | 不支持 |
| isClosed() | 暂不支持 | 不支持 |
| getMetaData() | 暂不支持 | 不支持 |
| setReadOnly(boolean readOnly) | 暂不支持 | 不支持 |
| isReadOnly() | 暂不支持 | 不支持 |
| setCatalog() | 暂不支持 | 不支持 |
| getCatalog() | 暂不支持 | 不支持 |
| setTransactionIsolation(int level) | 暂不支持 | 不支持 |
| getTransactionIsolation() | 暂不支持 | 不支持 |
| getWarnings() | 暂不支持 | 不支持 |
| clearWarnings() | 暂不支持 | 不支持 |
| createStatement(int resultSetType, int resultSetConcurrency) | 暂不支持 | 不支持 |
| prepareStatement(String sql, int resultSetType, int resultSetConcurrency) | 暂不支持 | 不支持 |
| prepareCall(String sql, int resultSetType, int resultSetConcurrency) | 暂不支持 | 不支持 |
| getTypeMap() | 暂不支持 | 不支持 |
| setTypeMap(java.util.Map<String,Class<?>> map) | 暂不支持 | 不支持 |
| setHoldability(int holdability) | 暂不支持 | 不支持 |
| getHoldability() | 暂不支持 | 不支持 |
| setSavepoint() | 暂不支持 | 不支持 |
| setSavepoint(String name) | 暂不支持 | 不支持 |
| rollback(Savepoint savepoint) | 暂不支持 | 不支持 |
| releaseSavepoint(Savepoint savepoint) | 暂不支持 | 不支持 |
| createStatement(int resultSetType, int resultSetConcurrency, int resultSetHoldability) | 暂不支持 | 不支持 |
| prepareStatement(String sql, int resultSetType, int resultSetConcurrency, int resultSetHoldability) | 暂不支持 | 不支持 |
| prepareCall(String sql, int resultSetType, int resultSetConcurrency, int resultSetHoldability) | 暂不支持 | 不支持 |
| prepareStatement(String sql, int autoGeneratedKeys) | 暂不支持 | 不支持 |
| prepareStatement(String sql, int columnIndexes[]) | 暂不支持 | 不支持 |
| prepareStatement(String sql, String columnNames[]) | 暂不支持 | 不支持 |
| createClob() | 暂不支持 | 不支持 |
| createBlob() | 暂不支持 | 不支持 |
| createNClob() | 暂不支持 | 不支持 |
| createSQLXML() | 暂不支持 | 不支持 |
| isValid() | 暂不支持 | 不支持 |
| setClientInfo(String name, String value) | 暂不支持 | 不支持 |
| setClientInfo(Properties properties) | 暂不支持 | 不支持 |
| getClientInfo(String name) | 暂不支持 | 不支持 |
| getClientInfo() | 暂不支持 | 不支持 |
| createArrayOf(String typeName, Object[] elements) | 暂不支持 | 不支持 |
| createStruct(String typeName, Object[] attributes) | 暂不支持 | 不支持 |
| setSchema(String schema) | 暂不支持 | 不支持 |
| getSchema() | 暂不支持 | 不支持 |
| abort(Executor executor) | 暂不支持 | 不支持 |
| setNetworkTimeout(Executor executor, int milliseconds) | 暂不支持 | 不支持 |
| getNetworkTimeout() | 暂不支持 | 不支持 |

### 2、Statement类中的方法
|Statement类中的方法|MO目前支持情况|0.6 MO是否支持|
|:-:|:-:|:-:|
| executeQuery(String sql) | 暂不支持 | 不支持 |
| executeUpdate(String sql) | 暂不支持 | 不支持 |
| close() | 暂不支持 | 不支持 |
| getMaxFieldSize() | 暂不支持 | 不支持 |
| setMaxFieldSize() | 暂不支持 | 不支持 |
| getMaxRows() | 暂不支持 | 不支持 |
| setMaxRows() | 暂不支持 | 不支持 |
| setEscapeProcessing() | 暂不支持 | 不支持 |
| getQueryTimeout() | 暂不支持 | 不支持 |
| setQueryTimeout(int seconds) | 暂不支持 | 不支持 |
| cancel() | 暂不支持 | 不支持 |
| getWarnings() | 暂不支持 | 不支持 |
| clearWarnings() | 暂不支持 | 不支持 |
| setCursorName(String name) | 暂不支持 | 不支持 |
| execute(String sql) | 暂不支持 | 不支持 |
| getResultSet() | 暂不支持 | 不支持 |
| getUpdateCount() | 暂不支持 | 不支持 |
| getMoreResults() | 暂不支持 | 不支持 |
| setFetchDirection(int direction) | 暂不支持 | 不支持 |
| getFetchDirection() | 暂不支持 | 不支持 |
| setFetchSize(int rows) | 暂不支持 | 不支持 |
| getFetchSize() | 暂不支持 | 不支持 |
| getResultSetConcurrency() | 暂不支持 | 不支持 |
| getResultSetType() | 暂不支持 | 不支持 |
| addBatch( String sql) | 暂不支持 | 不支持 |
| clearBatch() | 暂不支持 | 不支持 |
| executeBatch() | 暂不支持 | 不支持 |
| getConnection() | 暂不支持 | 不支持 |
| getMoreResults(int current) | 暂不支持 | 不支持 |
| getGeneratedKeys() | 暂不支持 | 不支持 |
| executeUpdate(String sql, int autoGeneratedKeys) | 暂不支持 | 不支持 |
| executeUpdate(String sql, int columnIndexes[]) | 暂不支持 | 不支持 |
| executeUpdate(String sql, String columnNames[]) | 暂不支持 | 不支持 |
| execute(String sql, int autoGeneratedKeys) | 暂不支持 | 不支持 |
| execute(String sql, int columnIndexes[]) | 暂不支持 | 不支持 |
| execute(String sql, String columnNames[]) | 暂不支持 | 不支持 |
| getResultSetHoldability() | 暂不支持 | 不支持 |
| isClosed() | 暂不支持 | 不支持 |
| setPoolable(boolean poolable) | 暂不支持 | 不支持 |
| isPoolable() | 暂不支持 | 不支持 |
| closeOnCompletion() | 暂不支持 | 不支持 |
| isCloseOnCompletion() | 暂不支持 | 不支持 |
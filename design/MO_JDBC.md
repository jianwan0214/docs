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


| connection接口中的方法 | XX | 0.6 MO是否支持 |
|:-:|:-:|:-:|
| createStatement() | 暂不支持 | 支持 |
| prepareStatement(String sql) | 暂不支持 | 支持 |
| prepareCall(String sql) | 暂不支持 | 支持 |
| nativeSQL(String sql) | 暂不支持 | 支持 |
| setAutoCommit(boolean autoCommit) | 暂不支持 | 支持 |
| getAutoCommit() | 暂不支持 | 支持 |
| commit() | 暂不支持 | 支持 |
| rollback() | 暂不支持 | 支持 |
| close() | 暂不支持 | 支持 |
| isClosed() | 暂不支持 | 支持 |
| getMetaData() | 暂不支持 | 支持 |
| setReadOnly(boolean readOnly) | 暂不支持 | 支持 |
| isReadOnly() | 暂不支持 | 支持 |
| setCatalog() | 暂不支持 | 支持 |
| getCatalog() | 暂不支持 | 支持 |
| setTransactionIsolation(int level) | 暂不支持 | 不支持 |
| getTransactionIsolation() | 暂不支持 | 不支持 |
| getWarnings() | 暂不支持 | 不支持 |
| clearWarnings() | 暂不支持 | 不支持 |
| createStatement(int resultSetType, int resultSetConcurrency) | 暂不支持 | 支持 |
| prepareStatement(String sql, int resultSetType, int resultSetConcurrency) | 暂不支持 | 支持 |
| prepareCall(String sql, int resultSetType, int resultSetConcurrency) | 暂不支持 | 支持 |
| getTypeMap() | 暂不支持 | 不支持 |
| setTypeMap(java.util.Map<String,Class<?>> map) | 暂不支持 | 不支持 |
| setHoldability(int holdability) | 暂不支持 | 不支持 |
| getHoldability() | 暂不支持 | 不支持 |
| setSavepoint() | 暂不支持 | 不支持 |
| setSavepoint(String name) | 暂不支持 | 不支持 |
| rollback(Savepoint savepoint) | 暂不支持 | 不支持 |
| releaseSavepoint(Savepoint savepoint) | 暂不支持 | 不支持 |
| createStatement(int resultSetType, int resultSetConcurrency, int resultSetHoldability) | 暂不支持 | 支持 |
| prepareStatement(String sql, int resultSetType, int resultSetConcurrency, int resultSetHoldability) | 暂不支持 | 支持 |
| prepareCall(String sql, int resultSetType, int resultSetConcurrency, int resultSetHoldability) | 暂不支持 | 支持 |
| prepareStatement(String sql, int autoGeneratedKeys) | 暂不支持 | 支持 |
| prepareStatement(String sql, int columnIndexes[]) | 暂不支持 | 支持 |
| prepareStatement(String sql, String columnNames[]) | 暂不支持 | 支持 |
| createClob() | 暂不支持 | 不支持 |
| createBlob() | 暂不支持 | 不支持 |
| createNClob() | 暂不支持 | 不支持 |
| createSQLXML() | 暂不支持 | 不支持 |
| isValid() | 暂不支持 | 支持 |
| setClientInfo(String name, String value) | 暂不支持 | 不支持 |
| setClientInfo(Properties properties) | 暂不支持 | 不支持 |
| getClientInfo(String name) | 暂不支持 | 不支持 |
| getClientInfo() | 暂不支持 | 不支持 |
| createArrayOf(String typeName, Object[] elements) | 暂不支持 | 不支持 |
| createStruct(String typeName, Object[] attributes) | 暂不支持 | 不支持 |
| setSchema(String schema) | 暂不支持 | 不支持 |
| getSchema() | 暂不支持 | 不支持 |
| abort(Executor executor) | 暂不支持 | 不支持 |
| setNetworkTimeout(Executor executor, int milliseconds) | 暂不支持 | 支持 |
| getNetworkTimeout() | 暂不支持 | 支持 |

### 2、Statement类中的方法
|Statement类中的方法| XX | 0.6 MO是否支持 |
|:-:|:-:|:-:|
| executeQuery(String sql) | 暂不支持 | 支持 |
| executeUpdate(String sql) | 暂不支持 | 支持 |
| close() | 暂不支持 | 支持 |
| getMaxFieldSize() | 暂不支持 | 支持 |
| setMaxFieldSize() | 暂不支持 | 支持 |
| getMaxRows() | 暂不支持 | 支持 |
| setMaxRows() | 暂不支持 | 支持 |
| setEscapeProcessing() | 暂不支持 | 不支持 |
| getQueryTimeout() | 暂不支持 | 支持 |
| setQueryTimeout(int seconds) | 暂不支持 | 支持 |
| cancel() | 暂不支持 | 支持 |
| getWarnings() | 暂不支持 | 不支持 |
| clearWarnings() | 暂不支持 | 不支持 |
| setCursorName(String name) | 暂不支持 | 不支持 |
| execute(String sql) | 暂不支持 | 支持 |
| getResultSet() | 暂不支持 | 支持 |
| getUpdateCount() | 暂不支持 | 支持 |
| getMoreResults() | 暂不支持 | 支持 |
| setFetchDirection(int direction) | 暂不支持 | 支持 |
| getFetchDirection() | 暂不支持 | 不支持 |
| setFetchSize(int rows) | 暂不支持 | 支持 |
| getFetchSize() | 暂不支持 | 支持 |
| getResultSetConcurrency() | 暂不支持 | 支持 |
| getResultSetType() | 暂不支持 | 支持 |
| addBatch( String sql) | 暂不支持 | 支持 |
| clearBatch() | 暂不支持 | 支持 |
| executeBatch() | 暂不支持 | 支持 |
| getConnection() | 暂不支持 | 支持 |
| getMoreResults(int current) | 暂不支持 | 支持 |
| getGeneratedKeys() | 暂不支持 | 支持 |
| executeUpdate(String sql, int autoGeneratedKeys) | 暂不支持 | 支持 |
| executeUpdate(String sql, int columnIndexes[]) | 暂不支持 | 支持 |
| executeUpdate(String sql, String columnNames[]) | 暂不支持 | 支持 |
| execute(String sql, int autoGeneratedKeys) | 暂不支持 | 支持 |
| execute(String sql, int columnIndexes[]) | 暂不支持 | 支持 |
| execute(String sql, String columnNames[]) | 暂不支持 | 支持 |
| getResultSetHoldability() | 暂不支持 | 支持 |
| isClosed() | 暂不支持 | 支持 |
| setPoolable(boolean poolable) | 暂不支持 | 不支持 |
| isPoolable() | 暂不支持 | 不支持 |
| closeOnCompletion() | 暂不支持 | 支持 |
| isCloseOnCompletion() | 暂不支持 | 支持 |

### 2、ResultSet interface中的方法
|ResultSet类中的方法| XX |0.6 MO是否支持|
|:-:|:-:|:-:|
| next() | 暂不支持 | 支持 |
| close() | 暂不支持 | 支持 |
| wasNull() | 暂不支持 | 支持 |
| getString(int columnIndex) | 暂不支持 | 支持 |
| getBoolean(int columnIndex) | 暂不支持 | 支持 |
| getByte(int columnIndex) | 暂不支持 | 支持 |
| getShort(int columnIndex) | 暂不支持 | 支持 |
| getInt(int columnIndex) | 暂不支持 | 支持 |
| getLong(int columnIndex) | 暂不支持 | 支持 |
| getFloat(int columnIndex) | 暂不支持 | 支持 |
| getDouble(int columnIndex) | 暂不支持 | 支持 |
| getBigDecimal(int columnIndex, int scale) | 暂不支持 | 支持 |
| getBytes(int columnIndex) | 暂不支持 | 支持 |
| getDate(int columnIndex) | 暂不支持 | 支持 |
| getTime(int columnIndex) | 暂不支持 | 支持 |
| getTimestamp(int columnIndex) | 暂不支持 | 支持 |
| getAsciiStream(int columnIndex) | 暂不支持 | 支持 |
| getUnicodeStream(int columnIndex) | 暂不支持 | 支持 |
| getBinaryStream(int columnIndex) | 暂不支持 | 支持 |
| getWarnings() | 暂不支持 | 不支持 |
| clearWarnings() | 暂不支持 | 不支持 |
| getCursorName() | 暂不支持 | 不支持 |
| getMetaData() | 暂不支持 | 支持 |
| getObject() | 暂不支持 | 不支持 |
| findColumn() | 暂不支持 | 支持 |
| getCharacterStream() | 暂不支持 | 支持 |
| isBeforeFirst() | 暂不支持 | 支持 |
| isAfterLast() | 暂不支持 | 支持 |
| isFirst() | 暂不支持 | 支持 |
| isLast() | 暂不支持 | 支持 |
| beforeFirst() | 暂不支持 | 支持 |
| afterLast() | 暂不支持 | 支持 |
| first() | 暂不支持 | 支持 |
| last() | 暂不支持 | 支持 |
| getRow() | 暂不支持 | 支持 |
| absolute() | 暂不支持 | 支持 |
| relative() | 暂不支持 | 支持 |
| previous() | 暂不支持 | 支持 |
| setFetchDirection() | 暂不支持 | 支持 |
| getFetchDirection() | 暂不支持 | 支持 |
| setFetchSize() | 暂不支持 | 支持 |
| getFetchSize() | 暂不支持 | 支持 |
| getType() | 暂不支持 | 支持 |
| getConcurrency() | 暂不支持 | 支持 |
| rowUpdated() | 暂不支持 | 支持 |
| rowInserted() | 暂不支持 | 支持 |
| rowDeleted() | 暂不支持 | 支持 |
| update()(一连串数据类型) | 暂不支持 | 支持 |
| updateNull() | 暂不支持 | 支持 |


### 4、ResultSetMetaData 中的方法
| ResultSetMetaData 类中的方法 | XX | 0.6 MO是否支持 |
|:-:|:-:|:-:|
| getColumnCount() | 暂不支持 | 支持 |
| isAutoIncrement() | 暂不支持 | 支持 |
| isCaseSensitive() | 暂不支持 | 支持 |
| isSearchable() | 暂不支持 | 支持 |
| isCurrency() | 暂不支持 | 支持 |
| isNullable() | 暂不支持 | 支持 |
| isSigned() | 暂不支持 | 支持 |
| getColumnDisplaySize() | 暂不支持 | 支持 |
| getColumnLabel() | 暂不支持 | 支持 |
| getColumnName() | 暂不支持 | 支持 |
| getSchemaName() | 暂不支持 | 不支持 |
| getPrecision() | 暂不支持 | 支持 |
| getScale() | 暂不支持 | 支持 |
| getTableName() | 暂不支持 | 支持 |
| getCatalogName() | 暂不支持 | 支持 |
| getColumnType() | 暂不支持 | 支持 |
| getColumnTypeName() | 暂不支持 | 支持 |
| isReadOnly() | 暂不支持 | 不支持 |
| isWritable() | 暂不支持 | 不支持 |
| isDefinitelyWritable() | 暂不支持 | 不支持 |
| getColumnClassName() | 暂不支持 | 支持 |


Mysql各数据类型 DisplaySize、Prec、Scale统计
|数据类型| DisplaySize | Prec |  Scale |
|:-:|:-:|:-:|:-:|
| TINYINT | 4 | 4 | 0 |
| SMALLINT | 6 | 6 | 0 |
| INT | 11 | 11 | 0 |
| BIGINT | 20 | 20 | 0 |
| TINYINT UNSIGNED | 3 | 3 | 0 |
| SMALLINT UNSIGNED | 5 | 5 | 0 |
| INT UNSIGNED | 10 | 10 | 0 |
| BIGINT UNSIGNED | 20 | 20 | 0 |
| DECIMAL64(根据实际情况) | 17 | 15 | 2 |
| DECIMAL128(根据实际情况) | 23 | 21 | 3 |
| FLOAT | 12 | 12 | 31 |
| DOUBLE | 22 | 22 | 31 |
| VARCHAR(根据实际情况) | 100 | 100 | 0 |
| CHAR(根据实际情况) | 100 | 100 | 0 |
| DATE | 10 | 10 | 0 |
| DATETIME | 19 | 19 | 0 |
| TIMESTAMP | 19 | 19 | 0 |
| JSON | 2147483647 | 2147483647 | 0 |

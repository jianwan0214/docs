## MO之Go for mysql功能说明
### 1、DB（类）：获取数据库连接对象
| Driver 接口中的方法 | 0.6 MO是否支持 |
|:-:|:-:|
| func Open(driverName, dataSourceName string) (*DB, error) | 支持 |
| func OpenDB(c driver.Connector) *DB | 支持 |
| func (db *DB) Begin() (*Tx, error) | 支持 |
| func (db *DB) BeginTx(ctx context.Context, opts *TxOptions) (*Tx, error) | 支持 |
| func (db *DB) Close() error | 支持 |
| func (db *DB) Conn(ctx context.Context) (*Conn, error) | 支持 |
| func (db *DB) Driver() driver.Driver | 支持 |
| func (db *DB) Exec(query string, args ...any) (Result, error) | 支持 |
| func (db *DB) ExecContext(ctx context.Context, query string, args ...any) (Result, error) | 支持 |
| func (db *DB) Ping() error| 支持 |
| func (db *DB) PingContext(ctx context.Context) error | 支持 |
| func (db *DB) Prepare(query string) (*Stmt, error) | 支持 |
| func (db *DB) PrepareContext(ctx context.Context, query string) (*Stmt, error) | 支持 |
| func (db *DB) Query(query string, args ...any) (*Rows, error) | 支持 |
| func (db *DB) QueryContext(ctx context.Context, query string, args ...any) (*Rows, error) | 支持 |
| func (db *DB) QueryRow(query string, args ...any) *Row | 支持 |
| func (db *DB) QueryRowContext(ctx context.Context, query string, args ...any) *Row | 支持 |
| func (db *DB) SetConnMaxIdleTime(d time.Duration) | 支持 |
| func (db *DB) SetConnMaxLifetime(d time.Duration) | 支持 |
| func (db *DB) SetMaxIdleConns(n int) | 支持 |
| func (db *DB) SetMaxOpenConns(n int) | 支持 |
| func (db *DB) Stats() DBStats | 支持 |


### 2、Conn（类）：获取数据库连接对象
| Conn 接口中的方法 | 0.6 MO是否支持 |
|:-:|:-:|
| func (c *Conn) BeginTx(ctx context.Context, opts *TxOptions) (*Tx, error) | 支持 |
| func (c *Conn) Close() error | 支持 |
| func (c *Conn) ExecContext(ctx context.Context, query string, args ...any) (Result, error) | 支持 |
| func (c *Conn) PingContext(ctx context.Context) error | 支持 |
| func (c *Conn) PrepareContext(ctx context.Context, query string) (*Stmt, error) | 支持 |
| func (c *Conn) QueryContext(ctx context.Context, query string, args ...any) (*Rows, error) | 支持 |
| func (c *Conn) QueryRowContext(ctx context.Context, query string, args ...any) *Row | 支持 |
| func (c *Conn) Raw(f func(driverConn any) error) (err error) | 支持 |

### 3、ColumnType（类)
| ColumnType 接口中的方法 | 0.6 MO是否支持 |
|:-:|:-:|
| func (ci *ColumnType) DatabaseTypeName() string | 支持 |
| func (ci *ColumnType) DecimalSize() (precision, scale int64, ok bool) | 支持 |
| func (ci *ColumnType) Length() (length int64, ok bool) | 支持 |
| func (ci *ColumnType) Name() string | 支持 |
| func (ci *ColumnType) Nullable() (nullable, ok bool) | 支持 |
| func (ci *ColumnType) DatabaseTypeName() string | 支持 |
| func (ci *ColumnType) ScanType() reflect.Type | 支持 |


### 4、Row、Rows（类)
| Row 接口中的方法 | 0.6 MO是否支持 |
|:-:|:-:|
| func (r *Row) Err() error | 支持 |
| func (r *Row) Scan(dest ...any) error | 支持 |
| func (rs *Rows) Close() error | 支持 |
| func (rs *Rows) ColumnTypes() ([]*ColumnType, error) | 支持 |
| func (rs *Rows) Columns() ([]string, error) | 支持 |
| func (rs *Rows) Err() error | 支持 |
| func (rs *Rows) Next() bool | 支持 |
| func (rs *Rows) NextResultSet() bool | 支持 |
| func (rs *Rows) Scan(dest ...any) error | 支持 |

### 5、stmt（类)
| Stmt 接口中的方法 | 0.6 MO是否支持 |
|:-:|:-:|
| func (s *Stmt) Close() error | 支持 |
| func (s *Stmt) Exec(args ...any) (Result, error) | 支持 |
| func (s *Stmt) ExecContext(ctx context.Context, args ...any) (Result, error) | 支持 |
| func (s *Stmt) Query(args ...any) (*Rows, error) | 支持 |
| func (s *Stmt) QueryContext(ctx context.Context, args ...any) (*Rows, error) | 支持 |
| func (s *Stmt) QueryRow(args ...any) *Row | 支持 |
| func (s *Stmt) QueryRowContext(ctx context.Context, args ...any) *Row | 支持 |

### 5、Tx（类)
| Tx 接口中的方法 | 0.6 MO是否支持 |
|:-:|:-:|
| func (tx *Tx) Commit() error | 支持 |
| func (tx *Tx) Exec(query string, args ...any) (Result, error) | 支持 |
| func (tx *Tx) ExecContext(ctx context.Context, query string, args ...any) (Result, error) | 支持 |
| func (tx *Tx) Prepare(query string) (*Stmt, error) | 支持 |
| func (tx *Tx) PrepareContext(ctx context.Context, query string) (*Stmt, error) | 支持 |
| func (tx *Tx) Query(query string, args ...any) (*Rows, error) | 支持 |
| func (tx *Tx) QueryContext(ctx context.Context, query string, args ...any) (*Rows, error) | 支持 |
| func (tx *Tx) QueryRow(query string, args ...any) *Row | 支持 |
| func (tx *Tx) QueryRowContext(ctx context.Context, query string, args ...any) *Row | 支持 |
| func (tx *Tx) Rollback() error | 支持 |
| func (tx *Tx) Stmt(stmt *Stmt) *Stmt | 支持 |
| func (tx *Tx) StmtContext(ctx context.Context, stmt *Stmt) *Stmt | 支持 |







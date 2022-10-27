## MO之 python for mysql 功能说明
### 1、Engine（类)

| Engine 接口中的方法 | XX | 0.6 MO是否支持 |
|:-:|:-:|:-:|
| clear_compiled_cache() | 暂不支持 | 支持 |
| update_execution_options() | 暂不支持 | 支持 |
| execution_options() | 暂不支持 | 支持 |
| get_execution_options() | 暂不支持 | 支持 |
| name() | 暂不支持 | 支持 |
| dispose() | 暂不支持 | 支持 |
| _optional_conn_ctx_manager() | 暂不支持 | 支持 |
| begin() | 暂不支持 | 支持 |
| _run_ddl_visitor() | 暂不支持 | 支持 |
| connect() | 暂不支持 | 支持 |
| raw_connection() | 暂不支持 | 支持 |
| update_execution_options() | 暂不支持 | 支持 |
| update_execution_options() | 暂不支持 | 支持 |



### 2、Session（类)

| Session 接口中的方法 | XX | 0.6 MO是否支持 |
|:-:|:-:|:-:|
| in_transaction() | 暂不支持 | 支持 |
| in_nested_transaction() | 暂不支持 | 支持 |
| get_transaction() | 暂不支持 | 支持 |
| get_nested_transaction() | 暂不支持 | 支持 |
| info() | 暂不支持 | 支持 |
| _autobegin_t() | 暂不支持 | 支持 |
| begin() | 暂不支持 | 支持 |
| begin_nested() | 暂不支持 | 支持 |
| rollback() | 暂不支持 | 支持 |
| commit() | 暂不支持 | 支持 |
| prepare() | 暂不支持 | 支持 |
| connection() | 暂不支持 | 支持 |
| _connection_for_bind() | 暂不支持 | 支持 |
| _execute_internal() | 暂不支持 | 支持 |
| execute() | 暂不支持 | 支持 |
| scalar() | 暂不支持 | 支持 |
| scalars() | 暂不支持 | 支持 |
| close() | 暂不支持 | 支持 |
| invalidate() | 暂不支持 | 支持 |
| _close_impl() | 暂不支持 | 支持 |
| expunge_all() | 暂不支持 | 支持 |
| _add_bind() | 暂不支持 | 支持 |
| bind_mapper() | 暂不支持 | 支持 |
| bind_table() | 暂不支持 | 支持 |
| get_bind() | 暂不支持 | 支持 |
| query() | 暂不支持 | 支持 |
| _identity_lookup() | 暂不支持 | 支持 |
| no_autoflush() | 暂不支持 | 支持 |
| _autoflush() | 暂不支持 | 支持 |
| refresh() | 暂不支持 | 支持 |
| expire_all() | 暂不支持 | 支持 |
| expire() | 暂不支持 | 支持 |
| _expire_state() | 暂不支持 | 支持 |
| _conditional_expire() | 暂不支持 | 支持 |
| expunge() | 暂不支持 | 支持 |
| _expunge_states() | 暂不支持 | 支持 |
| _register_persistent() | 暂不支持 | 支持 |
| _register_altered() | 暂不支持 | 支持 |
| _remove_newly_deleted() | 暂不支持 | 支持 |
| add() | 暂不支持 | 支持 |
| add_all() | 暂不支持 | 支持 |
| _save_or_update_state() | 暂不支持 | 支持 |
| delete() | 暂不支持 | 支持 |
| _delete_impl() | 暂不支持 | 支持 |
| get() | 暂不支持 | 支持 |
| _get_impl() | 暂不支持 | 支持 |
| merge() | 暂不支持 | 支持 |
| _merge() | 暂不支持 | 支持 |
| _validate_persistent() | 暂不支持 | 支持 |
| _save_impl() | 暂不支持 | 支持 |
| _update_impl() | 暂不支持 | 支持 |
| _save_or_update_impl() | 暂不支持 | 支持 |
| enable_relationship_loading() | 暂不支持 | 支持 |
| _before_attach() | 暂不支持 | 支持 |
| _after_attach() | 暂不支持 | 支持 |
| flush() | 暂不支持 | 支持 |
| _flush_warning() | 暂不支持 | 支持 |
| _is_clean() | 暂不支持 | 支持 |
| bulk_save_objects() | 暂不支持 | 支持 |
| bulk_insert_mappings() | 暂不支持 | 支持 |
| bulk_update_mappings() | 暂不支持 | 支持 |
| _bulk_save_mappings() | 暂不支持 | 支持 |
| is_modified() | 暂不支持 | 支持 |
| is_active() | 暂不支持 | 支持 |
| _dirty_states() | 暂不支持 | 支持 |
| dirty() | 暂不支持 | 支持 |
| deleted() | 暂不支持 | 支持 |
| new() | 暂不支持 | 支持 |


### 3、Query（类)

| Query 接口中的方法 | XX | 0.6 MO是否支持 |
|:-:|:-:|:-:|
| tuples() | 暂不支持 | 支持 |
| _get_options() | 暂不支持 | 支持 |
| statement() | 暂不支持 | 支持 |
| _final_statement() | 暂不支持 | 支持 |
| _statement_20() | 暂不支持 | 支持 |
| subquery() | 暂不支持 | 支持 |
| cte() | 暂不支持 | 支持 |
| label() | 暂不支持 | 支持 |
| as_scalar() | 暂不支持 | 支持 |
| as_scalar() | 暂不支持 | 支持 |
| scalar_subquery() | 暂不支持 | 支持 |
| selectable() | 暂不支持 | 支持 |
| only_return_tuples() | 暂不支持 | 支持 |
| is_single_entity() | 暂不支持 | 支持 |
| enable_eagerloads() | 暂不支持 | 支持 |
| _with_compile_options() | 暂不支持 | 支持 |
| with_labels() | 暂不支持 | 支持 |
| get_label_style() | 暂不支持 | 支持 |
| set_label_style() | 暂不支持 | 支持 |
| enable_assertions() | 暂不支持 | 支持 |
| whereclause() | 暂不支持 | 支持 |
| _with_current_path() | 暂不支持 | 支持 |
| yield_per() | 暂不支持 | 支持 |
| get() | 暂不支持 | 支持 |
| _get_impl() | 暂不支持 | 支持 |
| lazy_loaded_from() | 暂不支持 | 支持 |
| _current_path() | 暂不支持 | 支持 |
| correlate() | 暂不支持 | 支持 |
| autoflush() | 暂不支持 | 支持 |
| populate_existing() | 暂不支持 | 支持 |
| _with_invoke_all_eagers() | 暂不支持 | 支持 |
| with_parent() | 暂不支持 | 支持 |
| add_entity() | 暂不支持 | 支持 |
| with_session() | 暂不支持 | 支持 |
| _legacy_from_self() | 暂不支持 | 支持 |
| _set_enable_single_crit() | 暂不支持 | 支持 |
| _from_selectable() | 暂不支持 | 支持 |
| values() | 暂不支持 | 支持 |
| with_entities() | 暂不支持 | 支持 |
| add_columns() | 暂不支持 | 支持 |
| add_column() | 暂不支持 | 支持 |
| options() | 暂不支持 | 支持 |
| with_transformation() | 暂不支持 | 支持 |
| get_execution_options() | 暂不支持 | 支持 |
| execution_options() | 暂不支持 | 支持 |
| with_for_update() | 暂不支持 | 支持 |
| params() | 暂不支持 | 支持 |
| where() | 暂不支持 | 支持 |
| _last_joined_entity() | 暂不支持 | 支持 |
| _filter_by_zero() | 暂不支持 | 支持 |
| filter_by() | 暂不支持 | 支持 |
| order_by() | 暂不支持 | 支持 |
| group_by() | 暂不支持 | 支持 |
| having() | 暂不支持 | 支持 |
| _set_op() | 暂不支持 | 支持 |
| union() | 暂不支持 | 支持 |
| union_all() | 暂不支持 | 支持 |
| intersect() | 暂不支持 | 支持 |
| intersect_all() | 暂不支持 | 支持 |
| except_() | 暂不支持 | 支持 |
| except_all() | 暂不支持 | 支持 |
| join() | 暂不支持 | 支持 |
| outerjoin() | 暂不支持 | 支持 |
| reset_joinpoint() | 暂不支持 | 支持 |
| select_from() | 暂不支持 | 支持 |
| __getitem__() | 暂不支持 | 支持 |
| slice() | 暂不支持 | 支持 |
| limit() | 暂不支持 | 支持 |
| offset() | 暂不支持 | 支持 |
| distinct() | 暂不支持 | 支持 |
| all() | 暂不支持 | 支持 |
| from_statement() | 暂不支持 | 支持 |
| first() | 暂不支持 | 支持 |
| one_or_none() | 暂不支持 | 支持 |
| one() | 暂不支持 | 支持 |
| scalar() | 暂不支持 | 支持 |
| _get_bind_args() | 暂不支持 | 支持 |
| column_descriptions() | 暂不支持 | 支持 |
| instances() | 暂不支持 | 支持 |
| merge_result() | 暂不支持 | 支持 |
| exists() | 暂不支持 | 支持 |
| count() | 暂不支持 | 支持 |
| delete() | 暂不支持 | 支持 |
| update() | 暂不支持 | 支持 |
| _compile_state() | 暂不支持 | 支持 |
| _compile_context() | 暂不支持 | 支持 |


### 3、Column（类)
| Column 接口中的方法 | XX | 0.6 MO是否支持 |
|:-:|:-:|:-:|
| references() | 暂不支持 | 支持 |
| append_foreign_key() | 暂不支持 | 支持 |
| _set_parent() | 暂不支持 | 支持 |
| _setup_on_memoized_fks() | 暂不支持 | 支持 |
| _on_table_attach() | 暂不支持 | 支持 |
| copy() | 暂不支持 | 支持 |
| _merge() | 暂不支持 | 支持 |
| _make_proxy() | 暂不支持 | 支持 |


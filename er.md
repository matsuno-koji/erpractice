**こんな風に書いたらいいかも**

- PK = \+ Primary Key 主キー
- CPK = \+ Composite Primary Key 複合主キー
- FK = \# Foreign Key 外部キー
- ユニーク制約 = \*

**カーディナリティ**

```
------ :1
----|| :1 and only 1
----o| :0 or 1
-----{ :many
----|{ :1 or more
----o{ :0 or many
```

```puml
@startuml
/'色指定'/
!define MASTER_MARK_COLOR AAFFAA

package "会社" as companies_group {
    entity "会社" as companies {
    + id [PK]
    ==
    name: varchar(128)
  }
}

package "従業員" as system_table {
  entity "従業員" as employees {
    + id [PK]
    ==
    # company_id [FK]
    # office_id [FK]
    * employee_code: varchar(16)
    name: varchar(128)
  }

  entity "営業所" as offices <<M,MASTER_MARK_COLOR>> {
    + id [PK]
    ==
    name: varchar(128)
  }


}
package "資格" as license_group {
  entity "資格" as licenses <<M,MASTER_MARK_COLOR>> {
    + id [PK]
    ==
    name: varchar(128)
  }

  entity "従業員保有資格" as license_holders <<M,MASTER_MARK_COLOR>> {
  + employee_id [CPK, FK]
  + license_id [CPK, FK]
  }
  /'コメント'/
  note left of license_holders : 従業員・資格の中間テーブル
}
/'リレーション '/
employees ||--|| offices
companies --o{ employees
license_holders --o{ employees
license_holders --o{ licenses

@enduml
```

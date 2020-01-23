# RDB

## サロゲートキー(代理キー)

- キーそのものに意味がない
- 例えば
    - AUTO INCREMENTなどの連番
    - ランダムなハッシュ値

## ナチュラルキー(自然キー)

- キーそのものに意味がある
- 例えば
    - 商品コード
    - エリアコード

## 自己参照型のリレーションシップ

- 親-子-孫のような階層構造を持つ関係を1つのテーブルで表現する

例えば、エリア分け

```sql
CREATE TABLE `areas` (
    `area_code` VARCHAR(128) NOT NULL,
    `parent_area_code` VARCHAR(128) DEFAULT NULL,
    `area_name` VARCHAR(128) NOT NULL,
    PRIMARY KEY (`area_code`),
    FOREIGN KEY (`parent_area_code`) REFERENCES `areas` (`area_code`) 
) ;
```

```
INSERT INTO `areas` ( `area_code`, `parent_area_code`, `area_name` ) VALUES
('PREF1', NULL, 'Tokyo'),
('CITY1', 'PREF1', 'Chiyoda');
```

自己参照型でないと、以下のような構造になる

```sql
CREATE TABLE `prefectures` (
    `code` VARCHAR(128) NOT NULL,
    `name` VARCHAR(128) NOT NULL,
    PRIMARY KEY (`code`)
);

CREATE TABLE `cities` (
    `code` VARCHAR(128) NOT NULL,
    `prefecture_code` VARCHAR(128) NOT NULL,
    `name` VARCHAR(128) NOT NULL,
    PRIMARY KEY (`code`),
    FOREIGN KEY (`prefecture_code`) REFERENCES `prefectures` (`code`) 
);
```

---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL Generate Test Data","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"MySQL Generate Test Data","File Folder with relative path":"database/MySQL/Doc/MySQL Tutorial","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-tutorial/my-sql-generate-test-data/","dgPassFrontmatter":true}
---


### 编辑dbs
```bash
./dsdgen -DIR  /app/tpcds/datas/ -SCALE 1 -parallel 4 -child 1 
```
### 设置数据导入的路径
```bash
#secure_file_priv=/var/lib/mysql-files/
secure_file_priv=''
```
### 由于导入数据中有空值
#### 设置会话级参数。
```bash
use tpcds;
set sql_mode=''; --由于会话级变量，取消所有限制。
load data infile '/app/tpcds/datas/call_center_1_4.dat' into table  call_center COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/catalog_page_1_4.dat' into table  catalog_page COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/catalog_returns_1_4.dat' into table catalog_returns COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/catalog_sales_1_4.dat' into table catalog_sales COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/customer_1_4.dat' into table  customer COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/customer_address_1_4.dat' into table customer_address COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/customer_demographics_1_4.dat' into table customer_demographics COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/date_dim_1_4.dat' into table  date_dim COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/dbgen_version_1_4.dat' into table dbgen_version COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/household_demographics_1_4.dat' into table household_demographics COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/income_band_1_4.dat' into table  income_band COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/inventory_1_4.dat' into table  inventory COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/item_1_4.dat' into table  item COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/promotion_1_4.dat' into table  promotion COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/reason_1_4.dat' into table  reason COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/ship_mode_1_4.dat' into table  ship_mode COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/store_1_4.dat' into table  store COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/store_returns_1_4.dat' into table store_returns COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/store_sales_1_4.dat' into table  store_sales COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/time_dim_1_4.dat' into table  time_dim COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/warehouse_1_4.dat' into table  warehouse COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/web_page_1_4.dat' into table  web_page COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/web_returns_1_4.dat' into table  web_returns COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/web_sales_1_4.dat' into table  web_sales COLUMNS TERMINATED BY '|' ;
load data infile '/app/tpcds/datas/web_site_1_4.dat' into table  web_site COLUMNS TERMINATED BY '|' ;
```
## 生成sql 语句
```bash
 ./dsqgen -output_dir /part2/tpcds/querydata/ -input ../query_templates/templates.lst -scale 1 -dialect oracle -directory ../query_templates
```
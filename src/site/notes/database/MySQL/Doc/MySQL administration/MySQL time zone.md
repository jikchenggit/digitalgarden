---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL time zone","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:06","tags":"MySQL time zone","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-time-zone/","dgPassFrontmatter":true}
---




##  MySQL local time 设置.
`lc_time_names` 不会影响 `STR_TO_DATE()` 或者 `GET_FORMAT()` 函数.
`lc_time_names` 的值也不会影响FORMAT() 函数的返回结果.但是这个参数可以接受第三个参数,这个参数可以用于设置时间区域.并使用 小数点,千位分隔符分割.
    
使用的地区列表请参照:
[http://www.iana.org/assignments/language-subtag-registry](http://www.iana.org/assignments/language-subtag-registry)
  
  测试:
  设置对应的`set NAMES utf8`
```sql

mysql> SET NAMES 'utf8';
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> SELECT @@lc_time_names;
+-----------------+
| @@lc_time_names |
+-----------------+
| en_US           |
+-----------------+
1 row in set (0.00 sec)

mysql> SELECT DAYNAME('2010-01-01'), MONTHNAME('2010-01-01');
+-----------------------+-------------------------+
| DAYNAME('2010-01-01') | MONTHNAME('2010-01-01') |
+-----------------------+-------------------------+
| Friday                | January                 |
+-----------------------+-------------------------+
1 row in set (0.00 sec)

mysql> SELECT DATE_FORMAT('2010-01-01','%W %a %M %b');
+-----------------------------------------+
| DATE_FORMAT('2010-01-01','%W %a %M %b') |
+-----------------------------------------+
| Friday Fri January Jan                  |
+-----------------------------------------+
1 row in set (0.00 sec)

mysql> SET lc_time_names = 'es_MX';
Query OK, 0 rows affected (0.03 sec)

mysql> SELECT @@lc_time_names;
+-----------------+
| @@lc_time_names |
+-----------------+
| es_MX           |
+-----------------+
1 row in set (0.00 sec)

mysql>  SELECT DAYNAME('2010-01-01'), MONTHNAME('2010-01-01');
+-----------------------+-------------------------+
| DAYNAME('2010-01-01') | MONTHNAME('2010-01-01') |
+-----------------------+-------------------------+
| viernes               | enero                   |
+-----------------------+-------------------------+
1 row in set (0.00 sec)

mysql> SELECT DATE_FORMAT('2010-01-01','%W %a %M %b');
+-----------------------------------------+
| DATE_FORMAT('2010-01-01','%W %a %M %b') |
+-----------------------------------------+
| viernes vie enero ene                   |
+-----------------------------------------+
1 row in set (0.00 sec)

```
  

|                Locale Value                 |                Meaning                |
| ------------------------------------------- | ------------------------------------- |
| `ar_AE`: Arabic - United Arab Emirates      | `ar_BH`: Arabic - Bahrain             |
| `ar_DZ`: Arabic - Algeria                   | `ar_EG`: Arabic - Egypt               |
| `ar_IN`: Arabic - India                     | `ar_IQ`: Arabic - Iraq                |
| `ar_JO`: Arabic - Jordan                    | `ar_KW`: Arabic - Kuwait              |
| `ar_LB`: Arabic - Lebanon                   | `ar_LY`: Arabic - Libya               |
| `ar_MA`: Arabic - Morocco                   | `ar_OM`: Arabic - Oman                |
| `ar_QA`: Arabic - Qatar                     | `ar_SA`: Arabic - Saudi Arabia        |
| `ar_SD`: Arabic - Sudan                     | `ar_SY`: Arabic - Syria               |
| `ar_TN`: Arabic - Tunisia                   | `ar_YE`: Arabic - Yemen               |
| `be_BY`: Belarusian - Belarus               | `bg_BG`: Bulgarian - Bulgaria         |
| `ca_ES`: Catalan - Spain                    | `cs_CZ`: Czech - Czech Republic       |
| `da_DK`: Danish - Denmark                   | `de_AT`: German - Austria             |
| `de_BE`: German - Belgium                   | `de_CH`: German - Switzerland         |
| `de_DE`: German - Germany                   | `de_LU`: German - Luxembourg          |
| `el_GR`: Greek - Greece                     | `en_AU`: English - Australia          |
| `en_CA`: English - Canada                   | `en_GB`: English - United Kingdom     |
| `en_IN`: English - India                    | `en_NZ`: English - New Zealand        |
| `en_PH`: English - Philippines              | `en_US`: English - United States      |
| `en_ZA`: English - South Africa             | `en_ZW`: English - Zimbabwe           |
| `es_AR`: Spanish - Argentina                | `es_BO`: Spanish - Bolivia            |
| `es_CL`: Spanish - Chile                    | `es_CO`: Spanish - Colombia           |
| `es_CR`: Spanish - Costa Rica               | `es_DO`: Spanish - Dominican Republic |
| `es_EC`: Spanish - Ecuador                  | `es_ES`: Spanish - Spain              |
| `es_GT`: Spanish - Guatemala                | `es_HN`: Spanish - Honduras           |
| `es_MX`: Spanish - Mexico                   | `es_NI`: Spanish - Nicaragua          |
| `es_PA`: Spanish - Panama                   | `es_PE`: Spanish - Peru               |
| `es_PR`: Spanish - Puerto Rico              | `es_PY`: Spanish - Paraguay           |
| `es_SV`: Spanish - El Salvador              | `es_US`: Spanish - United States      |
| `es_UY`: Spanish - Uruguay                  | `es_VE`: Spanish - Venezuela          |
| `et_EE`: Estonian - Estonia                 | `eu_ES`: Basque - Basque              |
| `fi_FI`: Finnish - Finland                  | `fo_FO`: Faroese - Faroe Islands      |
| `fr_BE`: French - Belgium                   | `fr_CA`: French - Canada              |
| `fr_CH`: French - Switzerland               | `fr_FR`: French - France              |
| `fr_LU`: French - Luxembourg                | `gl_ES`: Galician - Spain             |
| `gu_IN`: Gujarati - India                   | `he_IL`: Hebrew - Israel              |
| `hi_IN`: Hindi - India                      | `hr_HR`: Croatian - Croatia           |
| `hu_HU`: Hungarian - Hungary                | `id_ID`: Indonesian - Indonesia       |
| `is_IS`: Icelandic - Iceland                | `it_CH`: Italian - Switzerland        |
| `it_IT`: Italian - Italy                    | `ja_JP`: Japanese - Japan             |
| `ko_KR`: Korean - Republic of Korea         | `lt_LT`: Lithuanian - Lithuania       |
| `lv_LV`: Latvian - Latvia                   | `mk_MK`: Macedonian - FYROM           |
| `mn_MN`: Mongolia - Mongolian               | `ms_MY`: Malay - Malaysia             |
| `nb_NO`: Norwegian(Bokmål) - Norway         | `nl_BE`: Dutch - Belgium              |
| `nl_NL`: Dutch - The Netherlands            | `no_NO`: Norwegian - Norway           |
| `pl_PL`: Polish - Poland                    | `pt_BR`: Portugese - Brazil           |
| `pt_PT`: Portugese - Portugal               | `rm_CH`: Romansh - Switzerland        |
| `ro_RO`: Romanian - Romania                 | `ru_RU`: Russian - Russia             |
| `ru_UA`: Russian - Ukraine                  | `sk_SK`: Slovak - Slovakia            |
| `sl_SI`: Slovenian - Slovenia               | `sq_AL`: Albanian - Albania           |
| `sr_RS`: Serbian - Yugoslavia               | `sv_FI`: Swedish - Finland            |
| `sv_SE`: Swedish - Sweden                   | `ta_IN`: Tamil - India                |
| `te_IN`: Telugu - India                     | `th_TH`: Thai - Thailand              |
| `tr_TR`: Turkish - Turkey                   | `uk_UA`: Ukrainian - Ukraine          |
| `ur_PK`: Urdu - Pakistan                    | `vi_VN`: Vietnamese - Viet Nam        |
| `zh_CN`: Chinese - China                    | `zh_HK`: Chinese - Hong Kong          |
| `zh_TW`: Chinese - Taiwan Province of China |                                       |

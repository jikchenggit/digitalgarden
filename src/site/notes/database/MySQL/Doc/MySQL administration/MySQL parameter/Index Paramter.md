---
{"author":"aming","email":"jikcheng@163.com","title":"Index Paramter","creation_date":"2022-06-27 15:57","Last modified date":"2022-12-12 13:47","tags":"Index Paramter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/index-paramter/","dgPassFrontmatter":true}
---




##  全文索引
全文索引有以下几个参数.
```sql

mysql> show variables like 'ft_%';
+--------------------------+----------------+
| Variable_name            | Value          |
+--------------------------+----------------+
| ft_boolean_syntax        | + -><()~*:""&| |
| ft_max_word_len          | 84             |
| ft_min_word_len          | 4              |
| ft_query_expansion_limit | 20             |
| ft_stopword_file         | (built-in)     |
+--------------------------+----------------+
```
## [`ft_max_word_len`]

|          Property          |         Value         |
| -------------------------- | --------------------- |
| **Command-Line Format**    | `--ft-max-word-len=#` |
| **System Variable**        | `[ft_max_word_len]`   |
| **Scope**                  | Global                |
| **Dynamic**                | No                    |
| **[SET_VAR] Hint Applies** | No                    |
| **Type**                   | integer               |
| **Minimum Value**          | `10`                  |

对于MyISAM 表,此参数设置为索引中最大的字符串长度.

::: alert-info
Note
如果更改了选项,myisam 表上全文索引,需要使用``REPAIR TABLE *`tbl_name`*QUICK`` 重新生成索引.
:::


## [`ft_min_word_len`]

|          Property          |         Value         |
| -------------------------- | --------------------- |
| **Command-Line Format**    | `--ft-min-word-len=#` |
| **System Variable**        | `[ft_min_word_len]`   |
| **Scope**                  | Global                |
| **Dynamic**                | No                    |
| **[SET_VAR] Hint Applies** | No                    |
| **Type**                   | integer               |
| **Default Value**          | `4`                   |
| **Minimum Value**          | `1`                   |
对于myisam 的全文索引最小字符串长度

::: alert-info
Note
更改此变量后,需要用``REPAIR TABLE *`tbl_name`*QUICK`` 重建myisam 索引.
:::

##  [`ft_query_expansion_limit`]

|          Property          |             Value              |
| -------------------------- | ------------------------------ |
| **Command-Line Format**    | `--ft-query-expansion-limit=#` |
| **System Variable**        | `[ft_query_expansion_limit]`   |
| **Scope**                  | Global                         |
| **Dynamic**                | No                             |
| **[SET_VAR] Hint Applies** | No                             |
| **Type**                   | integer                        |
| **Default Value**          | `20`                           |
| **Minimum Value**          | `0`                            |
| **Maximum Value**          | `1000`                         |

使用全文索引和`WITH QUERY EXPANSION`的最大匹配数.
查询括展时取最相关的几个值用作二次查询



##  [`ft_stopword_file`]


|          Property          |             Value              |
| -------------------------- | ------------------------------ |
| **Command-Line Format**    | `--ft-stopword-file=file_name` |
| **System Variable**        | `[ft_stopword_file]`           |
| **Scope**                  | Global                         |
| **Dynamic**                | No                             |
| **[SET_VAR] Hint Applies** | No                             |
| **Type**                   | file name                      |

对于全文索引，MySQL会从 ft_stopword_file 变量指定的文件中读取不进行全文索引的过滤词表, 一行一个。若将该变量设置为空字符串(”)则禁用过滤词表。对于过滤的词表请参照storage/myisam/ft_static.c源代码
```
a's           able          about         above         according
accordingly   across        actually      after         afterwards
again         against       ain't         all           allow
allows        almost        alone         along         already
also          although      always        am            among
amongst       an            and           another       any
anybody       anyhow        anyone        anything      anyway
anyways       anywhere      apart         appear        appreciate
appropriate   are           aren't        around        as
aside         ask           asking        associated    at
available     away          awfully       be            became
because       become        becomes       becoming      been
before        beforehand    behind        being         believe
below         beside        besides       best          better
between       beyond        both          brief         but
by            c'mon         c's           came          can
can't         cannot        cant          cause         causes
certain       certainly     changes       clearly       co
com           come          comes         concerning    consequently
consider      considering   contain       containing    contains
corresponding could         couldn't      course        currently
definitely    described     despite       did           didn't
different     do            does          doesn't       doing
don't         done          down          downwards     during
each          edu           eg            eight         either
else          elsewhere     enough        entirely      especially
et            etc           even          ever          every
everybody     everyone      everything    everywhere    ex
exactly       example       except        far           few
fifth         first         five          followed      following
follows       for           former        formerly      forth
four          from          further       furthermore   get
gets          getting       given         gives         go
goes          going         gone          got           gotten
greetings     had           hadn't        happens       hardly
has           hasn't        have          haven't       having
he            he's          hello         help          hence
her           here          here's        hereafter     hereby
herein        hereupon      hers          herself       hi
him           himself       his           hither        hopefully
how           howbeit       however       i'd           i'll
i'm           i've          ie            if            ignored
immediate     in            inasmuch      inc           indeed
indicate      indicated     indicates     inner         insofar
instead       into          inward        is            isn't
it            it'd          it'll         it's          its
itself        just          keep          keeps         kept
know          known         knows         last          lately
later         latter        latterly      least         less
lest          let           let's         like          liked
likely        little        look          looking       looks
ltd           mainly        many          may           maybe
me            mean          meanwhile     merely        might
more          moreover      most          mostly        much
must          my            myself        name          namely
nd            near          nearly        necessary     need
needs         neither       never         nevertheless  new
next          nine          no            nobody        non
none          noone         nor           normally      not
nothing       novel         now           nowhere       obviously
of            off           often         oh            ok
okay          old           on            once          one
ones          only          onto          or            other
others        otherwise     ought         our           ours
ourselves     out           outside       over          overall
own           particular    particularly  per           perhaps
placed        please        plus          possible      presumably
probably      provides      que           quite         qv
rather        rd            re            really        reasonably
regarding     regardless    regards       relatively    respectively
right         said          same          saw           say
saying        says          second        secondly      see
seeing        seem          seemed        seeming       seems
seen          self          selves        sensible      sent
serious       seriously     seven         several       shall
she           should        shouldn't     since         six
so            some          somebody      somehow       someone
something     sometime      sometimes     somewhat      somewhere
soon          sorry         specified     specify       specifying
still         sub           such          sup           sure
t's           take          taken         tell          tends      
th            than          thank         thanks        thanx
that          that's        thats         the           their
theirs        them          themselves    then          thence
there         there's       thereafter    thereby       therefore
therein       theres        thereupon     these         they
they'd        they'll       they're       they've       think
third         this          thorough      thoroughly    those
though        three         through       throughout    thru
thus          to            together      too           took
toward        towards       tried         tries         truly
try           trying        twice         two           un
under         unfortunately unless        unlikely      until
unto          up            upon          us            use
used          useful        uses          using         usually
value         various       very          via           viz
vs            want          wants         was           wasn't
way           we            we'd          we'll         we're
we've         welcome       well          went          were
weren't       what          what's        whatever      when
whence        whenever      where         where's       whereafter
whereas       whereby       wherein       whereupon     wherever
whether       which         while         whither       who
who's         whoever       whole         whom          whose
why           will          willing       wish          with
within        without       won't         wonder        would
wouldn't      yes           yet           you           you'd
you'll        you're        you've        your          yours
yourself      yourselves    zero
```

:::alert-warning
Note
更改ft_stopword_file 变量之后,对于MyISAM 表必须使用``REPAIR TABLE *`tbl_name`* QUICK`` 重建全文索引.
:::

##  RTREE 索引
have_rtree_keys={YES|NO}
mysqld支持RTREE索引则为YES，否则为NO。RTREE索引用于MyISAM表的空间索引
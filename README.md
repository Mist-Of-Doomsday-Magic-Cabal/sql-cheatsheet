# SQL-Cheat-Sheet

## Basics
<table>
  <tr>
    <td>Create Table</td>
    <td>create table table1( id varchar(300) primary key, name varchar(200) not null);</td>
  </tr>
  <tr>
    <td>Insert </td>
    <td>insert into table1 (id,name) values ('aa','bb');</td>
  </tr>
  <tr>
    <td>Delete</td>
    <td>delete from table1 where id ='cc';</td>
  </tr>
  <tr>
    <td>Update</td>
    <td>update table1 set id = 'bb' where id='cc';</td>
  </tr>
  <tr>
    <td>Query</td>
    <td>select id,name (case gender when 0 then 'male' when 1 then ‘femal’ end  ) gender from  table1</td>
  </tr>
  <tr>
    <td>Delete Table  </td>
    <td>drop table table1;</td>
  </tr>
  <tr>
    <td>Change Table Title </td>
    <td>alter table table1 rename to table2;</td>
  </tr>
  <tr>
    <td>Copy Table Content</td>
    <td>insert into table1 (select * from table2);</td>
  </tr>
  <tr>
    <td>Copy Table Structure</td>
    <td>create table table1 select * from table2 where 1&gt;1;</td>
  </tr>
  <tr>
    <td>Copy Table Content andStructure</td>
    <td>create table table1 select * from table2;</td>
  </tr>
  <tr>
    <td>Copy Selected Field</td>
    <td>create table table1 as select id, name from table2 where 1&gt;1;</td>
  </tr>
</table>

## Math Functions

<table>
  <tr>
    <td>Absolute value: abs()</td>
    <td>select abs(-2) value from dual;</td>
    <td>2</td>
  </tr>
  <tr>
    <td>Ceiling: ceil()</td>
    <td>select ceil(-2.001) value from dual;</td>
    <td>-2</td>
  </tr>
  <tr>
    <td>Florr: floor()</td>
    <td>select floor(-2.001) value from dual;</td>
    <td>-3</td>
  </tr>
  <tr>
    <td>Trunc: trunc()</td>
    <td>select trunc(-2.001) value from dual;</td>
    <td>-2</td>
  </tr>
  <tr>
    <td>Round-off: round()</td>
    <td>select round(1.234564,4) value from dual;</td>
    <td>1.2346</td>
  </tr>
  <tr>
    <td>N-th Power: power(m,n)</td>
    <td>select power(4,2) value from dual;</td>
    <td>16</td>
  </tr>
  <tr>
    <td>Square Root: SQRT()</td>
    <td>select sqrt(16) value from dual;</td>
    <td>4</td>
  </tr>
  <tr>
    <td>Random Number: dbms_random(minvalue,maxvalue)</td>
    <td>select dbms_random.value() from dual;<br>select dbms_random.value(2,4) value from dual;</td>
    <td>  </td>
  </tr>
  <tr>
    <td>Sign: Sign()</td>
    <td>select sign(-3) value from dual;</td>
    <td>-1</td>
  </tr>
  <tr>
    <td>Greatest Value: greatest(value)</td>
    <td>select greatest(-1,3,5,7,9) value from dual;</td>
    <td>9</td>
  </tr>
  <tr>
    <td>LeastValue: least(value)</td>
    <td>select least(-1,3,5,7,9) value from dual;</td>
    <td>-1</td>
  </tr>
  <tr>
    <td>Deal with NULL</td>
    <td>select  nvl(null,10) value from dual;</td>
    <td>10</td>
  </tr>
</table>

## Rownum

<table>
  <tr>
    <td>Selectfrom top n rows.(Oracle Don’t Support Selecttop)</td>
    <td>select * from student where rownum &lt;3;</td>
  </tr>
  <tr>
    <td>Select from table but top n rows.</td>
    <td>select * from(select rownum rn ,id,name from student) where rn&gt;2;<br>select * from (select rownum rn, student.* from student) where rn &gt;3;</td>
  </tr>
  <tr>
    <td>Select from a region</td>
    <td>select * from (select rownum rn, student.* from student) where rn &gt;3 and rn&lt;6;</td>
  </tr>
  <tr>
    <td>Sort and select from top.</td>
    <td>select * from (select rownum rn, t.* from ( select d.* from DJDRUVER d order  bydrivernumber)t )p where p.rn&lt;10;</td>
  </tr>
  <tr>
    <td>Sort and select fromregion.</td>
    <td>select * from (select rownum rn, t.* from ( select d.* from DJDRIVER d order by DJDRIVER_DRIVERTIMES)t )p where p.rn&lt;9 and p.rn&gt;6;</td>
  </tr>
  <tr>
    <td>Sort and select from region,another way.</td>
    <td>select * from (select rownum rn, t.* from ( select d.* from DJDRIVER d order by DJDRIVER_DRIVERTIMES)t where rownum&lt;9 )p where p.rn&gt;6;</td>
  </tr>
</table>

## Paging Query (10 terms a page)

### Without sorting

<table>
  <tr>
    <td>low efficiency</td>
    <td>select * from (select rownum rn, d.* from DJDRIVER d  )p where p.rn&lt;=20 and p.rn&gt;=10;<br>select * from (select rownum rn, d.* from DJDRIVER d  )p where p.rn between 10 and 20;</td>
  </tr>
  <tr>
    <td>high efficiency</td>
    <td>select * from (select rownum rn, d.* from DJDRIVER d where rownum&lt;=20 )p where p.rn&gt;=10;</td>
  </tr>
</table>

### With Sorting

<table>
  <tr>
    <td>Sort and query a region</td>
    <td>select * from (select rownum rn, t.* from ( select d.* from DJDRIVER d order by DJDRIVER_DRIVERTIMES)t )p where p.rn&lt;=20 and p.rn&gt;=10;</td>
  </tr>
  <tr>
    <td>(low efficiency)</td>
    <td>select * from (select rownum rn, t.* from ( select d.* from DJDRIVER d order by DJDRIVER_DRIVERTIMES)t )p where p.rn between 10 and 20;</td>
  </tr>
  <tr>
    <td>Sort and query a region(high efficiency) </td>
    <td>select * from (select rownum rn, t.* from ( select d.* from DJDRIVER d order by DJDRIVER_DRIVERTIMES)t where rownum&lt;=20 )p where p.rn&gt;=10;</td>
  </tr>
</table>
 
## String Manipulation

<table>
  <tr>
    <td>Substring(start from 1)</td>
    <td>substr('abcdefg',1,5)</td>
    <td>Abcde</td>
  </tr>
  <tr>
    <td>Search substring</td>
    <td>instr('abcdefg','bc')</td>
    <td>TRUE</td>
  </tr>
  <tr>
    <td>Append strings</td>
    <td>'Hello'||'World' </td>
    <td>HelloWorld</td>
  </tr>
  <tr>
    <td>Deletewhitespace</td>
    <td>trim('  Wish ') </td>
    <td>Wish</td>
  </tr>
  <tr>
    <td>Deletewhitespace before the string</td>
    <td>rtrim('Wish  ') </td>
    <td>Wish</td>
  </tr>
  <tr>
    <td>Deletewhitespace after the string</td>
    <td>ltrim('  Wish') </td>
    <td>wish</td>
  </tr>
  <tr>
    <td>Delete prefix</td>
    <td>trim(leading 'w' from 'wish')</td>
    <td>ish</td>
  </tr>
  <tr>
    <td>Delete trailing</td>
    <td>trim(trailing 'h' from 'wish') </td>
    <td>wis</td>
  </tr>
  <tr>
    <td>Delete </td>
    <td>trim('w' from 'wish')</td>
    <td>ish</td>
  </tr>
  <tr>
    <td rowspan="2">Ascii convert</td>
    <td>ascii('A') </td>
    <td>65</td>
  </tr>
  <tr>
    <td>ascii('a')</td>
    <td>97</td>
  </tr>
  <tr>
    <td rowspan="2">Character convert</td>
    <td>chr(65)</td>
    <td>A</td>
  </tr>
  <tr>
    <td>chr(97)</td>
    <td>a</td>
  </tr>
  <tr>
    <td>length </td>
    <td>length('abcdefg')   </td>
    <td>7</td>
  </tr>
  <tr>
    <td rowspan="3">Capitalize</td>
    <td>lower('WISH') </td>
    <td>wish</td>
  </tr>
  <tr>
    <td>upper('wish') </td>
    <td>WISH</td>
  </tr>
  <tr>
    <td>initcap('wish')</td>
    <td>Wish</td>
  </tr>
  <tr>
    <td>Replace</td>
    <td>replace('wish1','1','youhappy') </td>
    <td>wishyouhappy</td>
  </tr>
  <tr>
    <td rowspan="2">Translate(string,from_str,to_str).<br>Replace every character in string that appeared in from_str to appropriateone in to_str.</td>
    <td>translate('wish1','1','y') </td>
    <td>wishy</td>
  </tr>
  <tr>
    <td>translate('wish1','sh1','hy')</td>
    <td>wihy</td>
  </tr>
  <tr>
    <td>Connect</td>
    <td>concat('11','22')</td>
    <td>1122</td>
  </tr>
</table>

## Aggregate Function

<table>
  <tr>
    <td>Term number</td>
    <td>count (distinct|all)</td>
  </tr>
  <tr>
    <td>Average</td>
    <td>avg (distinct|all)</td>
  </tr>
  <tr>
    <td>MaximumValue</td>
    <td>max (distinct|all)</td>
  </tr>
  <tr>
    <td>MinimumValue</td>
    <td>min (distinct|all)</td>
  </tr>
  <tr>
    <td>Standard Deviation</td>
    <td>stddev(distinct|all)</td>
  </tr>
  <tr>
    <td>Sum</td>
    <td>sum(distinct|all)</td>
  </tr>
  <tr>
    <td>Median</td>
    <td>median(distinct|all)</td>
  </tr>


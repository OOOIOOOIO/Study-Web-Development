# Query Method

## 쿼리 Subject Keyword
메서드 처음 시작 위치에 오는 키워드로서 어떤 작업을 할지, 어떤 데이터 타입을 리턴할지 결정한다.
- findBy : List<Member>와 같이 Collection 타입으로 리턴되거나, 조회 대상이 PK라면 단일 객체로 리턴한다.
- findFirst(숫자)By : findFirstBy..과 같이 숫자가 없다면 제일 첫 번째 데이터 1개가 리턴되고, 숫자가 있다면 Collection 타입으로 리턴
- existBy : boolean 타입으로 리턴
- countBy : int/long 등으로 해당되는 데이터 수 리턴
- deleteBy : void 타입 혹은 int/long 등으로 삭제된 데이터 수 리턴

<hr>

## 메서드명 내 키워드
메서드명 By 뒤에 작성되는 키워드로 SQL의 Where절 내 명령어와 같은 역할을 한다.
[공식 레퍼런스]([https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods))

<table style="border-collapse: collapse; width: 100%;" border="1" data-ke-align="alignLeft" data-ke-style="style9">
<tbody>
<tr>
<td style="text-align: center; width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;키워드&quot;}"><span style="font-family: 'Nanum Gothic';"><b>키워드</b></span></td>
<td style="text-align: center; width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;샘플&quot;}"><span style="font-family: 'Nanum Gothic';"><b>샘플</b></span></td>
<td style="text-align: center; width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;JPQL&quot;}"><span style="font-family: 'Nanum Gothic';"><b>JPQL</b></span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Distinct&quot;}"><span style="font-family: 'Nanum Gothic';">Distinct</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findDistinctByLastnameAndFirstname&quot;}"><span style="font-family: 'Nanum Gothic';">findDistinctByLastnameAndFirstname</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;select distinct …​ where x.lastname = ?1 and x.firstname = ?2&quot;}"><span style="font-family: 'Nanum Gothic';">select distinct …​ where x.lastname = ?1 and x.firstname = ?2</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;And&quot;}"><span style="font-family: 'Nanum Gothic';">And</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByLastnameAndFirstname&quot;}"><span style="font-family: 'Nanum Gothic';">findByLastnameAndFirstname</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.lastname = ?1 and x.firstname = ?2&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.lastname = ?1 and x.firstname = ?2</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Or&quot;}"><span style="font-family: 'Nanum Gothic';">Or</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByLastnameOrFirstname&quot;}"><span style="font-family: 'Nanum Gothic';">findByLastnameOrFirstname</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.lastname = ?1 or x.firstname = ?2&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.lastname = ?1 or x.firstname = ?2</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Is, Equals&quot;}"><span style="font-family: 'Nanum Gothic';">Is, Equals</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByFirstname,findByFirstnameIs,findByFirstnameEquals&quot;}"><span style="font-family: 'Nanum Gothic';">findByFirstname,findByFirstnameIs,findByFirstnameEquals</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.firstname = ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.firstname = ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Between&quot;}"><span style="font-family: 'Nanum Gothic';">Between</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByStartDateBetween&quot;}"><span style="font-family: 'Nanum Gothic';">findByStartDateBetween</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.startDate between ?1 and ?2&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.startDate between ?1 and ?2</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;LessThan&quot;}"><span style="font-family: 'Nanum Gothic';">LessThan</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAgeLessThan&quot;}"><span style="font-family: 'Nanum Gothic';">findByAgeLessThan</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age < ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age &lt; ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;LessThanEqual&quot;}"><span style="font-family: 'Nanum Gothic';">LessThanEqual</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAgeLessThanEqual&quot;}"><span style="font-family: 'Nanum Gothic';">findByAgeLessThanEqual</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age <= ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age &lt;= ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;GreaterThan&quot;}"><span style="font-family: 'Nanum Gothic';">GreaterThan</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAgeGreaterThan&quot;}"><span style="font-family: 'Nanum Gothic';">findByAgeGreaterThan</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age > ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age &gt; ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;GreaterThanEqual&quot;}"><span style="font-family: 'Nanum Gothic';">GreaterThanEqual</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAgeGreaterThanEqual&quot;}"><span style="font-family: 'Nanum Gothic';">findByAgeGreaterThanEqual</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age >= ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age &gt;= ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;After&quot;}"><span style="font-family: 'Nanum Gothic';">After</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByStartDateAfter&quot;}"><span style="font-family: 'Nanum Gothic';">findByStartDateAfter</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.startDate > ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.startDate &gt; ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Before&quot;}"><span style="font-family: 'Nanum Gothic';">Before</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByStartDateBefore&quot;}"><span style="font-family: 'Nanum Gothic';">findByStartDateBefore</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.startDate < ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.startDate &lt; ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;IsNull, Null&quot;}"><span style="font-family: 'Nanum Gothic';">IsNull, Null</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAge(Is)Null&quot;}"><span style="font-family: 'Nanum Gothic';">findByAge(Is)Null</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age is null&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age is null</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;IsNotNull, NotNull&quot;}"><span style="font-family: 'Nanum Gothic';">IsNotNull, NotNull</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAge(Is)NotNull&quot;}"><span style="font-family: 'Nanum Gothic';">findByAge(Is)NotNull</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age not null&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age not null</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Like&quot;}"><span style="font-family: 'Nanum Gothic';">Like</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByFirstnameLike&quot;}"><span style="font-family: 'Nanum Gothic';">findByFirstnameLike</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.firstname like ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.firstname like ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;NotLike&quot;}"><span style="font-family: 'Nanum Gothic';">NotLike</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByFirstnameNotLike&quot;}"><span style="font-family: 'Nanum Gothic';">findByFirstnameNotLike</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.firstname not like ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.firstname not like ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;StartingWith&quot;}"><span style="font-family: 'Nanum Gothic';">StartingWith</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByFirstnameStartingWith&quot;}"><span style="font-family: 'Nanum Gothic';">findByFirstnameStartingWith</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.firstname like ?1 (parameter bound with appended %)&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.firstname like ?1 (parameter bound with appended %)</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;EndingWith&quot;}"><span style="font-family: 'Nanum Gothic';">EndingWith</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByFirstnameEndingWith&quot;}"><span style="font-family: 'Nanum Gothic';">findByFirstnameEndingWith</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.firstname like ?1 (parameter bound with prepended %)&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.firstname like ?1 (parameter bound with prepended %)</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Containing&quot;}"><span style="font-family: 'Nanum Gothic';">Containing</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByFirstnameContaining&quot;}"><span style="font-family: 'Nanum Gothic';">findByFirstnameContaining</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.firstname like ?1 (parameter bound wrapped in %)&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.firstname like ?1 (parameter bound wrapped in %)</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;OrderBy&quot;}"><span style="font-family: 'Nanum Gothic';">OrderBy</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAgeOrderByLastnameDesc&quot;}"><span style="font-family: 'Nanum Gothic';">findByAgeOrderByLastnameDesc</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age = ?1 order by x.lastname desc&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age = ?1 order by x.lastname desc</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;Not&quot;}"><span style="font-family: 'Nanum Gothic';">Not</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByLastnameNot&quot;}"><span style="font-family: 'Nanum Gothic';">findByLastnameNot</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.lastname <> ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.lastname &lt;&gt; ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;In&quot;}"><span style="font-family: 'Nanum Gothic';">In</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAgeIn(Collection<Age> ages)&quot;}"><span style="font-family: 'Nanum Gothic';">findByAgeIn(Collection&lt;Age&gt; ages)</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age in ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age in ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;NotIn&quot;}"><span style="font-family: 'Nanum Gothic';">NotIn</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByAgeNotIn(Collection<Age> ages)&quot;}"><span style="font-family: 'Nanum Gothic';">findByAgeNotIn(Collection&lt;Age&gt; ages)</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.age not in ?1&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.age not in ?1</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:4,&quot;4&quot;:1}"><span style="font-family: 'Nanum Gothic';">TRUE</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByActiveTrue()&quot;}"><span style="font-family: 'Nanum Gothic';">findByActiveTrue()</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.active = true&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.active = true</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:4,&quot;4&quot;:0}"><span style="font-family: 'Nanum Gothic';">FALSE</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByActiveFalse()&quot;}"><span style="font-family: 'Nanum Gothic';">findByActiveFalse()</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where x.active = false&quot;}"><span style="font-family: 'Nanum Gothic';">… where x.active = false</span></td>
</tr>
<tr>
<td style="width: 107.109px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;IgnoreCase&quot;}"><span style="font-family: 'Nanum Gothic';">IgnoreCase</span></td>
<td style="width: 323.828px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;findByFirstnameIgnoreCase&quot;}"><span style="font-family: 'Nanum Gothic';">findByFirstnameIgnoreCase</span></td>
<td style="width: 377.062px;" data-sheets-value="{&quot;1&quot;:2,&quot;2&quot;:&quot;… where UPPER(x.firstname) = UPPER(?1)&quot;}"><span style="font-family: 'Nanum Gothic';">… where UPPER(x.firstname) = UPPER(?1)</span></td>
</tr>
</tbody>
</table>
